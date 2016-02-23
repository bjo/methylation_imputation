# COS424 Homework 2, Imputing Methylation Status

# bed file format

In the data directory, you will find a set of .bed files for each chromosome. These files are tab-separated files that are of the following format:

| Column      | Name          | Description   |
| ----------- | ------------- | ------------- |
| 1           | chromosome              | chromosome of the methylation site |
| 2           | start position          | position of the methylated C (inclusive, think of this as the start of methylation site) |
| 3           | end position            | position of the G following the methylated C (exclusive, think of this as the end of methylation site) |
| 4           | strand                  | + or -, depending on whether the site is on the 5’-3’ or 3’-5' strand |
| 5 ~ (5+k-1) | methylation value calls | methylation value calls for each sample |
| 5 + k       | hg                      | 0 or 1, 1 if this position is present on the Illumina 450K chip |

# Input data

We have provided data for 5 chromosomes: 1, 2, 6, 7 and 11. For each chromosome, you are given 3 files:

### intersected_final_chrN_cutoff_20_train.bed (Reference/Training data)

The training data contains the information from 33 training samples (corresponding to columns 5,6,...,37).
These samples correspond to different tissues and/or conditions under which the data has been collected. 

For example,

<code>awk -F'\t' '{print NF; exit}' intersected_final_chr6_cutoff_20_train_revised.bed</code>

outputs the total number of columns as 38, corresponding to 33 samples.

### intersected_final_chrN_cutoff_20_sample_partial.bed

### intersected_final_chrN_cutoff_20_sample_full.bed

The sample_partial.bed file contains your testing data.  The testing data consists of a simgle sample where the majority of genomic sites contain nan values to impute.  The last column contains a 0/1 indicating whether this site is included on the Illumina 450K chip.  You will notice that the sites that contain methylation values only occur in rows in which the last column entry is 1. However, not all rows for which the last column entry is 1 may have defined methylation values. You can think of this situation as observing a new sample from the Illumina 450K chip, and your task is to infer all the methylation values that are missing in the chip, using previous knowledge about methylation patterns, coming from the training data. The actual methalation values for the testing data are given in sample_full.bed file.

# Prediction task

Your goal in this assignment is to use the information from the train.bed file and the positions that do not contain nans in the sample_partial.bed file to predict the values that are provided in sample_full.bed file. 

Our choice of training vs. test sample was put in place to help you, but you are still free to provide us with additional metrics to evaluate your performance, as long as you don't cheat (i.e. use information from the test set other than the allowed rows where the last column contains a 1). Alternatively, you may create your own testng data from the other samples in the training set to evaluate the performance of your methods.  Although we have only held out a single sample out of 34, and separated it into sample_partial and sample_full files, you may look at the performance of your methods in other samples in the training set as well (using values from only the rows for which the last column is 1).  To do this for a single sample, you would select a sample (column) from the training data and replace the methelation values with nan for all the rows corresponding to a value a 0 in the last column. You can do this for arbitrary number of samples to partition the 34 samples into a testing and training set.

# Handling missing data

The naming convention "intersected_final_chrN_cutoff_20_" comes from the processing pipeline. “cutoff_20_” means that a quality control has been performed such that across all the positions collected (for every row), at least 80% of the values are well defined (not 'nan'). However, even with this quality control, there are still entries in the training data that contain 'nan' values. Try to come up with good ways of dealing with these missing data, and think about how different ways of dealing with missing data will affect the quality of your final predictions.

# Word of Caution

In this exercise, you are free to use all the information provided in the training set, or choose an alternate set of samples to use as your training set. In the test set, you are also allowed to use information from the rows for which the last column entry is 1 (you observe them from the 450K chip). In addition, you are also allowed to use other genomic annotations ([GENCODE annotations](http://www.gencodegenes.org/releases/19.html) and [ENCODE annotations](https://www.encodeproject.org/data/annotations/) are good places to start.)

The information you are NOT permitted to use is the information from your test set from the rows where the last column value is 0. In the sample_partial.bed file, these values are masked for you. You SHOULD also mask these values from other samples if you intend to use them as your test sample.

As a reminder, we have checks in place to make sure that you're not:

* Copying code from other groups or from online resources
* Using sources of information you're not permitted to use

We have a pretty good idea of how well your prediction task will work.  If we see an unreasonable prediction result, we will make sure that there is no foul play. In other words, just be honest. This is not a Kaggle competition, but a place for you to learn the necessary skills for applying machine learning to a variety of exciting problems.

# Most importanty, have fun!
