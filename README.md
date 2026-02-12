# ts-tokenizer-thesis

## Project Timeline

| Stage | Time | Description | Deliverables and Milestones |
|------|------|-------------|-----------------------------|
| **Stage 1: Setup and Preparation** | Jan 16, 2026 – Feb 27, 2026 | First, we will set up a working environment, and put time into understanding the problem and plan for the project. We will also implement downstream models as well as a baseline compression framework to have a starting point for comparison. | - Project plan document (13.02.)<br>- Two regression downstream models trained on Volvo data and public data (20.02.)<br>- Two classification downstream models trained on Volvo data and public data (27.02.)<br>- Baseline compression framework implementation (27.02.) |
| **Stage 2: Development** | Feb 28, 2026 – Mar 27, 2026 | Second, we will focus on developing the core components of the proposed compression approach leveraging tokenization. We will also start writing the theory chapters of the thesis and formulize a halftime report. | - Encoder component implementation (13.03.)<br>- Quantization / Tokenization module implementation (13.03.)<br>- Decoder component implementation (13.03.)<br>- Completed theory chapters of the final thesis (20.03.)<br>- Halftime report (27.03.) |
| **Stage 3: Experiments** | Mar 28, 2026 – Apr 17, 2026 | Third, we will focus on evaluating the performance of the proposed compression method against the baseline model. | - Six compressed datasets using two compression methods (08.04.)<br>- Evaluation results based on compression (08.04.)<br>- Trained downstream models (17.04.)<br>- Evaluation results based on downstream task performance (17.04.) |
| **Stage 4: Finalizing** | Apr 18, 2026 – May 15, 2026 | Finally, we will conduct an ablation study to understand the contribution of different components of the proposed method, and finalize the thesis document for a first draft. | - Ablation study results (01.05.)<br>- Experiment, Results, Discussion chapter of the thesis (10.05.)<br>- Complete draft of the final thesis (15.05.) |

## ToDo's
- Stage 1: 
  - Setup environment: Volvo Chatgpt, VPN, Kubernetes, Repository for Signet. 
  - Setup the Repo for Compression (Tim).
  - Work on the implementation of the downstream task.
    - Test Dataloader for trino data. (Tim)
    - Implement Dataloader for .csv data. (Wait for Szabolcs)
    - Implement Dataloader for public data. (Emrik)
    - Test regression model. 
    - Implement anomaly detection model. 
- Stage 2:
  - ...










