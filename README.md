**Objective:**

In this assignment, we are tasked with building a miniature version of GPT-2 from scratch, experimenting with different architectures and hyperparameters. As part of the project, we used Andrej Karpathy’s llm.c as the baseline. The trained model is then evaluated on HellaSwag, with the goal of surpassing the baseline score of 0.31 on the HellaSwag benchmark.

**Methodology:**

As part of this assignment, I experimented with the following architectures. Initially, I ran the models on Colab, where I spent $30 to perform initial tests, running up to 5,000 iterations per architecture to gauge early performance for specific hyperparameter settings. After identifying the model that performed comparatively better, I transferred it to the HPRC for further training. However, I encountered several issues and bottlenecks, which I will discuss under the challenges section. The architectures I tested are as follows:
  1.	SwiGLU + RMS Prop + GroupedQuery + RoPE
  2.	RoPE (No Positional Encoding) + Grouped Attention + GeLU
  3.	RoPE (No Positional Encoding) + GeLU
  4.	Baseline (Andrej Karpathy’s llm.c with modified hyperparameters and context embedding size)

**Standard Hyperparameters:**
1.	batch_size - 16
2.	dtype – float16
3.	weight_decay – 0.1
4.	zero_stage – 1
5.	learning_rate – 0.004
6.	warmup_iters – 1500
7.	learning_rate_decay_frac – 0.85
8.	overfit_single_batch – 0

**Performance Comparison for minimum runs:**

| Architecture                                | #Iterations | Performance (Val)                 | Performance                       |
|---------------------------------------------|-------------|-----------------------------------|-----------------------------------|
| SwiGLU + RMS + GroupedQuery + RoPE           | 5000        | 7.79 and hella accuracy: 0.2501   | Worst Performance                 |
| RoPE + GELU                                 | 5000        | 3.39 and hella accuracy: 0.261    | Comparable to Baseline            |
| RoPE + Grouped Query                        | 5000        | 3.57 and hella accuracy: 0.2741   | Slightly better than baseline     |


Apart from this, we also increased the context window to 2048 and the embedding size to 1024 for both the baseline and the RoPE + Grouped Query architectures. As a result, we observed a significant increase in HellaSwag accuracy. However, due to resource and time constraints, and since this idea was explored at the last minute, we were only able to run 9000 iterations. At 9000 iterations, we achieved an accuracy of 0.314 for the Grouped Query + RoPE architecture and 0.302 for the baseline model. I am really excited to see how the results unfold after reaching 19,650 iterations.


**SWIGLU+ROPE+RMS: (Sample Output Screen)**

![image](https://github.com/user-attachments/assets/707431d6-4e1b-4bf4-bd18-d38d0ae588d7)

**RoPE+GroupedQuery – (Sample Output Screen)**

![image](https://github.com/user-attachments/assets/706b4b13-9688-44e9-8081-0cc4f2420b2e)

**Rope+GELU (No POS Encoding) (Sample Output Screen)**

![image](https://github.com/user-attachments/assets/521b0b09-6825-40d4-b4f6-835d1bdccc32)

**Performance of Grouped Query+RoPE Architecture (With same context window size and Embedding)**

Training Loss:

<img width="587" alt="image" src="https://github.com/user-attachments/assets/8166fc25-7bd7-43af-b3f2-d228ff9e0b8e">

Validation Loss:

<img width="589" alt="image" src="https://github.com/user-attachments/assets/1b90442a-fcf3-4d65-a7ab-57ca14ab2c6d">

HellaSwag Accuracy:

<img width="583" alt="image" src="https://github.com/user-attachments/assets/46ccd27b-5e27-4618-b234-f4ede710139c">

**Performance of Modified Baseline with modified context and embedding size)**

Training Loss:

<img width="563" alt="image" src="https://github.com/user-attachments/assets/1355302b-530d-4c92-8293-2ef1fd6052b1">

Validation Loss:

<img width="563" alt="image" src="https://github.com/user-attachments/assets/4d3c669e-f315-44c5-94f0-293cd34602a2">

HellaSwag Loss:

<img width="557" alt="image" src="https://github.com/user-attachments/assets/476fb903-e1a5-4932-9f24-b2cfeb98753e">

Using Grouped_Query+Rope+Modified context and embedding size would have been even better as we got even better results (0.3224 accuracy at 10k iterations vs 0.302 accuracy at 19650 of baseline) but sadly we lost as runtime occured resulting in crash of our log data.













