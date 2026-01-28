## Canonical Lossy Compression Pipeline

x
 → Encoder (transform)
 → z (continuous latent)
 → Quantizer
 → q (discrete symbols)
 → Entropy model P(q)
 → Entropy coder
 → bitstream

## Implementation Details

### Why use TCRAE as baseline? 
The TCN–RNN autoencoder is a representative of state-of-the-art continuous-latent neural time-series compression (discrete-latent compression methods barely exist for time-series data. And if they do (Deep Dict) they are not really reproducible). Compared to vanilla autoencoders, TCN–RNN architectures are explicitly designed for long temporal dependencies and have demonstrated stronger rate–distortion performance in recent peer-reviewed work. This allows us to evaluate our tokenizer-based approach against a competitive neural compressor rather than a toy model. This methods is also suitable because of its costly architectural design: deep temporal convolutional stacks with large receptive fields, recurrent (often sequential) decoding, attention mechanisms, and dense continuous latent representations. These components typically lead to increased FLOPs, higher activation memory, and longer inference latency compared to discrete, token-based compression methods. 

### Implementation Overview

| Compression Stage | TCRAE (TCN–RNN AE) | Tokenization-based Compression |
|------------------|-------------------|--------------------------------|
| Input signal | Continuous time series | Continuous time series |
| Transform coding | Deep TCN encoder + RNN | Lightweight learned encoder |
| Continuous latent `z` | High-precision, dense | Intermediate embeddings |
| Explicit quantization | ❌ Absent (implicit only) | ✔️ Present (tokenizer / codebook) |
| Discrete symbols `q` | ❌ None | ✔️ Finite token vocabulary |
| Entropy modeling | ❌ Not used | ✔️ Learned symbol distribution |
| Entropy coding | ❌ Not used | ✔️ Arithmetic / ANS coding |
| Compression control | Architectural bottleneck | Token granularity + entropy |
| Output representation | Latent tensors | Bitstream |

### Detailed Implementation: TCRAE (TCN–RNN Autoencoder)

1. Input handling and preprocessing
- Multivariate time-series data are segmented into fixed-length windows.
- Each channel is normalized independently using statistics computed on the training set.
- Windows are treated as independent samples during training and evaluation.

2. TCN encoder (transform coding stage)
	•	The encoder consists of a stack of Temporal Convolutional Network (TCN) blocks with increasing dilation factors.
	•	Each TCN block contains:
	•	A 1D convolution operating along the temporal dimension
	•	Normalization (BatchNorm or LayerNorm)
	•	A nonlinear activation function (ReLU or LeakyReLU)
	•	Optional dropout for regularization
	•	A residual connection to stabilize training
	•	Temporal downsampling is introduced via strided convolutions or pooling layers to reduce sequence length.
	•	The encoder maps the input signal to a lower-resolution, higher-dimensional latent sequence, forming the continuous bottleneck.

Outcome:
A compressed continuous latent representation whose size is controlled by the downsampling factor and channel width.

⸻

3. RNN bottleneck (sequence compression)
	•	The encoder output is processed by a recurrent neural network (LSTM or GRU).
	•	The RNN captures longer-range temporal dependencies that are not easily modeled by convolutions alone.
	•	The hidden state dimensionality and number of layers define the strength of the bottleneck.
	•	The RNN produces a sequence of hidden states (or a reduced representation) that serves as the core compressed signal.

Interpretation:
Compression is achieved implicitly by restricting temporal resolution and latent dimensionality rather than by discretization.

⸻

4. RNN decoder
	•	A symmetric RNN decoder reconstructs a latent sequence from the bottleneck representation.
	•	The decoder mirrors the bottleneck RNN in depth and hidden size.
	•	No probabilistic modeling or entropy estimation is performed at this stage.

⸻

5. TCN decoder (signal reconstruction)
	•	A mirrored TCN stack upsamples the latent sequence back to the original temporal resolution.
	•	Transposed convolutions or interpolation-based upsampling are used.
	•	A final linear projection maps features back to the original channel dimension.

Training objective:
	•	Reconstruction loss (e.g., MSE or MAE) between input and reconstructed signal.

⸻

6. Compression characteristics (TCRAE)
	•	Compression rate is defined structurally via:
	•	Temporal downsampling factor
	•	Latent dimensionality
	•	No explicit quantization, entropy modeling, or bitstream generation is performed.
	•	Reported compression rates are proxy metrics derived from latent size.

⸻

### Detailed Implementation: Tokenization-Based Compression

#### Layer 1: Representation Learning

1. Input handling and preprocessing
- Identical preprocessing and windowing as used for TCRAE to ensure comparability.
- Normalization, segmentation, ...

2. Lightweight encoder 
- A shallow neural encoder (e.g., small CNN or MLP applied per timestep) maps the input signal to intermediate embeddings.
- The encoder is intentionally kept lightweight to avoid introducing heavy representational power before tokenization.
- The encoder implements the learned transform stage of a lossy compression pipeline. Its purpose is not to predict labels or reconstruct the input per se, but to reshape the data into a representation that can be efficiently discretized. Specifically, the encoder learns a mapping that:
 - Removes redundancy and correlations present in the raw time series
 - Concentrates important information into a low-dimensional, stable representation
 - Aligns the geometry of the representation space with the constraints of quantization
 - The objective is usually joined with the quantization and decoder stages.
 - In contrast to autoencoder-based methods, where the encoder is free to produce high-entropy continuous latents, the encoder here is explicitly shaped to produce quantization-friendly representations that support discrete tokenization and entropy coding.

3. Vector quantization / tokenization
 - Intermediate embeddings are discretized using a learned codebook. The codebook very simply looks like this: codebook = nn.Parameter(torch.randn(K, D)), with K = number of tokens and D = embedding dimension.
 - Each embedding vector is replaced by the index of its nearest codebook entry.
   - distances = ||z - codebook||² then q = argmin(distances) then z_q = codebook[q]; q is an integer index (the token), z_q has the same shape as z (continuous vector) and "replaces" z. 
 - This step introduces the only explicit source of information loss.
 - The output is a sequence of discrete token IDs drawn from a finite vocabulary.
 - How backpropagation/ learning works:
   - x → encoder → z
   - z → nearest neighbor → q
   - q → codebook lookup → z_q
   - z_q → head → y_hat (the head is completely replaceable; decoder, if we want reconstruction; downstream head, if we want downstream task error)
   - Threeway loss computation:
     - Loss => head: Gradients flow normally into the head parameters.
     - Loss => z_q: 

#### Laysr 2: Entropy modeling & coding

4. Entropy modeling
 - A learned probabilistic model estimates the distribution of token sequences.
 - What architecture is commonly used/ should be used here ???
 - The entropy model defines the expected bitrate of the representation.

5. Entropy coding
 - Tokens are compressed into a bitstream using a standard entropy coder (e.g., arithmetic coding or ANS).
 - Frequently occurring tokens receive shorter codes, resulting in efficient compression.
 - The resulting bitstream represents the final compressed output.

