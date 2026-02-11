# ts-tokenizer

## Plan
- Stage 1, Setup and Preparation: (Week 1-5, Deadline: 20.02.)
  - Create a project plan. (Deadline: 13.02.)
  - Create a working environment. 
  - Train a downstream ML model, representative of a defined subset of industry-relevant tasks,
  on uncompressed data to quantify loss in predictive utility.
  - Implement a baseline model based on established neural compression frameworks such as https://www.sciencedirect.com/science/article/pii/S1568494623008153.
- Stage 2, Development: (Week 6-10, Deadline: 27.03.)
  - Develop a learnable tokenization module that discretizes data into semantically meaningful
  units optimized for downstream tasks.
  - Develop lightweight entropy modeling and coding schemes tailored to the tokenized representations.
  - Develop a decoder.
  - Write the halftime report. 
  - Write the theory part of the final report.
- Stage 3, Experiments: (Week 11-13, Deadline: 17.04.)
  - Train the model from Task 1 on the compressed data using both compression methods. 
  - Evaluate baseline model and proposed tokenization + lightweight entropy model framework based on rate-utility and computational efficiency.
- Stage 4, Finalizing: (Week 14-17, Deadline: 15.05.)
  - Ablation study of the proposed tokenization + lightweight entropy model framework. 
  - Write the methodology, results, discussion, and conclusion part fo the final report. 

## ToDo's
- Stage 1: 
  - Convert timeline into table, Setup Repository in Volvo, Setup the Repo for Compression, Setup the repo for Report (Tim).
  - Work on the implementation of the downstream task.
    - Test Dataloader for trino data. (Tim)
    - Implement Dataloader for .csv data. (Wait for Szabolcs)
    - Implement Dataloader for public data. (Emrik)
    - Test regression model. 
    - Implement anomaly detection model. 
- Stage 2:
  - ...

## Dataset
[hyundai dataset](https://ieee-dataport.org/open-access/x-canids-dataset-vehicle-signal-dataset)










