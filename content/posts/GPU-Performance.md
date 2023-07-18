---
title: "MT5 finetuning & GPU Performance"
date: 2023-07-15T23:37:29-07:00
draft: false
---


# Mt5 Fine tuning & GPU Analysis

# Intro

The mt5 is a transformer based langauge model developed by Google Research(Xue et al. 2021). It is an extension of the existing T5 series, the text-text transfer models, however this version is trained on 101 different langugaes.  Every task  handled by the model is considered  text-text which means it can handle translation, summarization, question answering etc. Although not was popular as the latest GPT models(GPT4), the wide range of langauge capabilities makes Mt5 stand out. Please note that the repo in this model is from the HuggingFace transformers library. As such the Mt5 model out of the box is not that useful, and requires fine tuning.


# Purpose

The purpose of this repo is to fine tune the Mt5 langauge model to perform question answering in Spansih. I am specifically working with the [Squad_es](https://huggingface.co/datasets/squad_es) dataset present on HuggingFace.



# Methods

To complete this work, I leveraged cloud GPUs from PaperSpace, due to access to direct CUDA kernel statistics unlike Google Colab. Access to Nvidia tools was critical during this development to better glean insights on optimal hyper parameter tuning. The most time intensive task, was the data cleaning and processing before being able to be fine tuned.

This project involved a number of steps that utilized deep learning techniques, data preprocessing, GPU analysis, and fine-tuning of the MT5 model for the specific task of question-answering in Spanish.

Initially, the large SQuAD_es dataset was loaded and preprocessed to fit the format required for the HuggingFace [Trainer class](https://huggingface.co/docs/transformers/main_classes/trainer). This preprocessing stage involved combining the context and question into a single string and appropriately formatting the answer. The preprocessed data was then tokenized using the T5TokenizerFast from the HuggingFace transformers library, preparing it for input into the MT5 model.

The main portion of this project is the fine-tuning of the MT5 model. I ran this on GPUs supplied by [Paperspace](https://www.paperspace.com/core) since they already have CUDA installed, and allow direct access to GPU statistics, unlike Google Colab pro+. Availability of these detailed GPU performance metrics was a key factor in guiding the fine-tuning process. Analyzing the GPU utilization and the time spent accessing memory helped in optimizing the hyperparameters for the model. 



# Results

Below are the results from the fine tuning of the model to now be able to perform question answering in Spanish based off the SQUAD dataset.


### GPU Utilization

#
<img src="/posts/images/cuda_statistics.png" width="75%">

The table above is the output from the nsys Nvidia GPU profiler. I am showing this in order to get a better udnerstandign of how exactly the GPU spends time. Let's break down what these actually mean.

#### cudaStreamSynchronize(49.9%):

This workflow blocks the CPU until all previously issued commands in the CUDA stream have been completed. Here we can see that a lot of time is spent communicating between the CPU & GPU indicating that the code has inefficies with the GPU sitting idle. The current implementation is in PyTorch which could be revisted to see if there is a better framework to use.

#### cudaLaunchKernel(27.4%)
 
This command above is used to launch cuda kernals which accounts for a quarter of time. This is higher than expected, altough it might be due to my small batch size. A future fix to reduce overhead could be to do a kernel fusion in order to do more work per kernel.

#### cudaMemcpyAsync (24.2%)

The above workflow indicates that there are asynchronous data transfers occuring in small but frequent manner. This is likley due to my small batch size, compared to the total amount of data. I'm not sure how I can optimize this, since if I increase the batch size there are errors within the exectuion, needs further analysis.





### GPU Utilization
#
<img src="/posts/images/GPU_utilization.png" width="75%">

The above graph showcases the GPU utilization across time, this metric was key in hyper paramter tuning to make sure I wasn't overloading the GPU, and using it efficiently. This graph changed from a previous hyper-parameters when the GPU utilization was too low.
#

### Time spent accessing memory
#
<img src="/posts/images/GPU_time _spent_acc_mem.png" width="75%">

One of the most time consuming processes within a GPU is the time it takes to transfer data between the disk and GPU. I wanted to graph this out to make sure this wasn't a huge bottleneck, that I needed to address.
#


### Train/loss
#
<img src="/posts/images/best_train_loss.png" width="75%">

Above you can see the training loss over time, wtih a slight increase in loss tofwatd the 1k mark for the inputs. After the training we can see the fine tuning seems to be successful, the last check is to make sure that the model did not overfit the data.

# Citations and sources

[Nvidia profiler](https://docs.csc.fi/computing/nsys/)

[Mt5 paper](https://arxiv.org/abs/2010.11934)

[Mt5 HuggingFace model](https://huggingface.co/docs/transformers/model_doc/mt5)



