# Statistical Analysis of SummaReranker


The following project focused on analysing the Rouge scores that were obtained by the [SummaReranker](https://arxiv.org/abs/2203.06569) on the Reddit TIFU Dataset. SummaReranker is a text summarization system developed by Ravaut et al. 2022. It consists of two stages: candidate generation by the base language model and then the SummaReranker is applied as part of the second stage to pick the best summary for a given text. The aim of this project is to have a closer look at the summaries that are generated and to determine whether there is a statistical difference between the baseline model PEGASUS and the SummaReranker. 

 
 ## SummaReranker
 
Follow the steps as presented in this [repository](https://github.com/Ravoxsg/SummaReranker-ACL-22-) in order to obtain the results that are needed for this project's evaluation. To perform this experiment you will need: baseline and SummaReranker generated summaries, the scores obtained for each summary, the original texts and the test labels.  The required files can be also found in the data folder of this repo: `statistical_tests_summaReranker/data`

Once all the evaluation data is generated [pymer4](https://eshinjolly.com/pymer4/) can be installed. Pymer4 is a library that allows you to use linear mixed effects model in a python setting. The installation page provides instruction on how to install the library and the library. However, during the installation problems might arise. You can follow the steps bellow to ensure the successful installation on linux.

## Installation steps for Linux

Firstly, install [Anaconda](https://www.anaconda.com/products/distribution). It is recomended to install Miniconda as it contains all the necessary packages that are needed and doens't include other unnecessary packages that are included in Anaconda. 
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash ./Miniconda3-latest-Linux-x86_64.sh
```
Follow the instructions and complete the installation. Then go to your projects folder and type the following:
 
```
cd statistical_tests_summaReranker
conda install -c conda-forge mamba
mamba  create --name pymer4 -c ejolly -c conda-forge -c defaults pymer4 notebook
conda activate pymer4

```
This enviroment will allow you to import the LMEMs and performed the desired tests. 


### Visualization of the evaluation data

The notebook: `statistical_tests_summaReranker/evaluation_analysis.ipynb` contains all the experiments of this project. With the help of the notebook you can visualize how the scores correlate to different variables, like the Flesch reading ease score, word difficulty, word number and sentence lenght. 
An example of the a graph can be seen below:


<img src="https://github.com/tashamina/statistical_tests_summaReranker/blob/main/visualization/sentence_length.png" width="500" height="500">

For instance, from the above graph we can see the relationship between the Rouge 1 Score and the amount of sentence present in the original text. The graph shows that the highest Rouge-1 scores are for those texts that have less sentences. We can also notice that the SummaReranker perfoms better than the baseline as the sentence count increases. 

<img src="https://github.com/tashamina/statistical_tests_summaReranker/blob/main/visualization/readability.png" width="700" height="500">

The above graph shows the median score for Rouge 1 score against the Flesch Readability Ease score. We can see that overall the summaReranker is able to achieve higher scores. And it could be seen that as the text becomes easier to read, the scores all become higher. It is interesting to note though how around 90 Flesch Readabilty Ease score the Rouge score suddenly drops, but then it goes back up for the SummaReranker but stays low for the baseline.

### Experimental Settings

Null Hypothesis: The expected Rouge scores are equal for both systems, there is no statistically significant difference between the two systems.  
Alternative Hypothesis: The expected Rouge scores are not equal for both systems. And one system performs better then that other one. 

- Finetuned Pegasus Baseline vs SummaReranker
- Pegasus Base model (no fine-tuning) vs SummaReranker

2 LMEMs were fitted on the data:

 - differentMeans model: assumes that the rouge scores are not equal from both systems 
 - common Means model: assumes that the rouge scores are equal from both system

Then Generalized Likelihood Ratio Test gives us the p-value that helps in determining whether we can reject the null hypothesis or not.

## Results

GLRT results:

| Models  | P-value |
| ------------- | ------------- |
| Finetuned Pegasus Baseline vs SummaReranker  | 5.830698124320577e-07  |
| Pegasus Base model (no fine-tuning) vs SummaReranker | 1.999688226117513e-08 |

Pairwise Comparison

| Contrast |	Estimate |	2.5_ci |	97.5_ci |	SE |	DF |	T-stat |	P-val |	Sig|
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |------------- |
| Finetuned Pegasus Baseline - SummaReranker  | 0.031 |	0.02 |	0.041 |	0.005 |	42.0 |	5.84 |	0.0 	| ***|
| Pegasus Base model (no fine-tuning) - SummaReranker | 0.121 |	0.085 |	0.156 |	0.018 |	42.0 |	6.849 |	0.0 |	***|

## Discussion 

Based on the p-value that were calculated, the null hypothesis can be rejected and we can see that the difference betweeen the baseline results and those of the SummaReranker are statistically significant.

## References 

Ravaut, Mathieu, Shafiq Joty, and Nancy F. Chen (2022). SummaReranker: A Multi-Task Mixture-of-Experts Re-ranking Framework for Abstractive Summarization. doi: 10.48550/ARXIV.2203.06569. [url](https://arxiv.org/abs/2203.06569.)
