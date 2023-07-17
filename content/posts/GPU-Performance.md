---
title: "MT5 finetuning & GPU Performance"
date: 2023-07-15T23:37:29-07:00
draft: false
---




# Intro

The mt5 is a transformer based langauge model developed by Google Research(Xue et al. 2021). It is an extension of the existing T5 series, the text-text transfer models, however this version is trained on 101 different langugaes.  Every task  handled by the model is considered  text-text which means it can handle translation, summarization, question answering etc. Although not was popular as the latest GPT models(GPT4), the wide range of langauge capabilities makes Mt5 stand out. Please note that the repo in this model is from the HuggingFace transformers library. As such the Mt5 model out of the box is not that useful, and requires fine tuning.


[Github link](https://github.com/jprivera44/Mt5-model-comparison)

# Purpose

The purpose of this repo is to fine tune the Mt5 langauge model to perform question answering in Spansih. I am specifically working with the [Squad_es](https://huggingface.co/datasets/squad_es) dataset present on HuggingFace.



# Methods

To complete this work, I leveraged cloud GPUs from PaperSpace, due to access to direct CUDA kernel statistics unlike Google Colab. Access to Nvidia tools was critical during this development to better glean insights on optimal hyper parameter tuning. The most time intensive task, was the data cleaning and processing before being able to be fine tuned.


# Results

Below are the results from the fine tuning of the model to now be able to perform question answering in Spanish based off the SQUAD dataset.



<img src="/posts/images/best_train_loss.png" width="75%">


<img src="/posts/images/GPU_utilization.png" width="75%">

Below are further results of GPU utils.

<img src="/posts/images/GPU_time _spent_acc_mem.png" width="75%">


# Citations and sources


