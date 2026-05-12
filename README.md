# Multimodal Story Prediction with ResNet Transfer Learning and Contrastive Alignment

## Quick Links

- **[Experiments Notebook](baseline.ipynb)** — Full experimental workflow
- **[Final Results](results/final_results.csv)** — Quantitative evaluation metrics
- **[Figures](results/)** — Attention maps, similarity distributions, qualitative predictions

## Project Overview

This project improves a multimodal grounded sequence prediction model using the StoryReasoning dataset.

The task involves predicting the next image frame and textual description from a sequence of previous frames and descriptions.

The baseline model combines:
- a convolutional visual autoencoder
- an LSTM text encoder
- a GRU temporal sequence model
- an attention mechanism for temporal context aggregation

The goal of this project was to improve multimodal representation quality through:
1. Visual transfer learning using a pretrained ResNet-18 encoder
2. Image-text alignment using contrastive learning

## Baseline Pipeline
Input frames + descriptions
        ↓
Image encoder + text encoder
        ↓
Fusion
        ↓
GRU temporal modeling
        ↓
Attention
        ↓
Predict next image + next text

## Baseline Evaluation
The following metrics were used to evaluate the baseline model and the improved model.
**MSE** (Mean Squared Error) and **SSIM** (Structural Similarity Index Measure) - Image quality assesment
**BLEU** (Bilingual Evaluation Understudy) and **ROUGE** (Recall-Oriented Understudy for Gisting Evaluation) - Text generation  quality
