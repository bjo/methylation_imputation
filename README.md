# COS424 Homework 2, Methylation Data Imputation

# bed file format

In the data directory, you will find a set of .bed files for each chromosome. These files are tab-separated files that are of this format:

| Column      | Name          | Description   |
| ----------- | ------------- | ------------- |
| 1           | chromosome              | chromosome of the methylation site |
| 2           | start position          | position of the methylated C (inclusive, think of this as the start of methylation site) |
| 3           | end position            | position of the G following the methylated C (exclusive, think of this as the end of methylation site) |
| 4           | strand                  | + or - depending on whether it is on 5’-3’ or 3’-5' |
| 5 ~ (5+k-1) | methylation value calls | corrected methylation value for each sample |
| 5 + k       | hg                      |  0 or 1, 1 if the position defined by columns 1,2,3 is present on the Illumina 450K chip |

# Input data

We have provided data from 5 chromosomes: 1, 2, 6, 7 and 11. For each chromosome, you are given 3 files:

### intersected_final_chrN_cutoff_20_train.bed (Training data)

The training data contains the aggregated information from 33 samples (corresponding to columns 5,6,...,37).
These samples correspond to different tissues and/or conditions under which the data has been collected. The bed file contains data for k = 33 processed, aggregated samples.

For example,

<code>awk -F'\t' '{print NF; exit}' intersected_final_chr6_cutoff_20_train_revised.bed</code>

outputs 38, corresponding to 33 samples.

### intersected_final_chrN_cutoff_20_sample_partial.bed

### intersected_final_chrN_cutoff_20_sample_full.bed

The sample_partial.bed file contains partial information from a held-out sample. You will notice that the methylation value calls are made only in rows in which the last column entry is 1. However, not all rows for which the last column entry is 1 may have defined methylation values. You can think of this situation as observing a new sample from the Illumina 450K chip, and trying to infer all the methylation values that are missing in the chip, using previous knowledge about methylation patterns, coming from the training data. The values you are trying to predict are given in sample_full.bed file.

# Prediction task

Your goal in this assignment is to use the information from the train.bed file and the positions defined (not 'nan') in the sample_partial.bed file in order to predict the values that are provided in sample_full.bed file. 

Alternatively, although we have held out one sample out of 34, and separated it into sample_partial and sample_full files, you may look at the performance of your methods in other samples in the training set as well (using values from only the rows for which the last column is 1). You can also think about separating the 34 samples into two sets (training and test sets), except in this case you will have partial information on the test set (information from the 450K chip, or the rows in which the last column value is 1). As you may have guessed by now, our choice of training vs. test sample is arbitrary, and you are free to provide us with additional metric to evaluate your performance, as long as you don't cheat (i.e. use information from the test set other than the allowed rows). The terms of honor code are outlined in further detail below.

# Handling missing data

The naming convention "intersected_final_chrN_cutoff_20_" comes from the processing pipeline. “cutoff_20_” means that a quality control has been performed such that across all the positions collected (for every row), at least 80% of the values are well defined (not 'nan'). However, even with this quality control, there are still entries in the training data that contain 'nan' values. Try to come up with good ways of dealing with these missing data, and think about how different ways of dealing with missing data will affect the quality of your final predictions.

# Honor code

In this exercise, you are free to use the sample that we provided as the prediction benchmark, or select other samples (or a set of samples) to use as your prediction benchmark. In the test set, you are also allowed to use information from the rows for which the last column entry is 1 (you observe them from the 450K chip). In addition, you are also allowed to use other genomic annotations ([GENCODE annotations](http://www.gencodegenes.org/releases/19.html) and [ENCODE annotations](https://www.encodeproject.org/data/annotations/) are good places to start.)

### The information you are NOT permitted to use are information from your test set from rows where the last column value is 0. In the sample_partial.bed file, these values are masked for you. You SHOULD also mask these values from other samples if you intend to use them as your test sample. You are also not permitted to use any previous scholarly work that report methylation values directly in certain loci (and in a lot of cases, this information will hurt you rather than help you. Think about why that would be).

We have checks in place to make sure that you're not:

* Copying code from other groups or from online resources
* Using sources of information you're not permitted to use

Since we have a pretty good idea of how well your prediction task will work, once we see an unreasonable prediction result, we will make sure that there is no foul play. In other words, just be honest. This is not a Kaggle competition, but a place for you to learn the necessary skills for applying machine learning to a variety of exciting problems.

# Most importanty, have fun!
