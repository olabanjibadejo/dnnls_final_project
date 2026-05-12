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
(Input frames + descriptions) -> (Image encoder + text encoder) -> Fusion -> GRU temporal modeling -> Attention -> (Predict next image + next text)


## Baseline Evaluation
The following metrics were used to evaluate the baseline model and the improved model.
**MSE** (Mean Squared Error) and **SSIM** (Structural Similarity Index Measure) - Image quality assesment

**BLEU** (Bilingual Evaluation Understudy) and **ROUGE** (Recall-Oriented Understudy for Gisting Evaluation) - Text generation  quality

I wrote a **evaluate_model(...)** class to calculate these metrics all through the experiments.
- **[Code Screeshot](baseline.ipynb)** — evaluate_model class


## EXPERIMENT 1 — VISUAL TRANSFER LEARNING

The baseline visual encoder was trained from scratch. Although suitable for reconstruction, it may not learn strong semantic visual features.

To improve this, the convolutional encoder was replaced with a pretrained ResNet-18 backbone initialized with ImageNet weights.

- **[Code Screeshot](baseline.ipynb)** — Visual Encoder with resnet backbone

**Key Insight**
Although the pretrained encoder improved visual feature quality, text generation performance degraded. This suggested that stronger visual representations alone were insufficient without proper multimodal alignment.

## EXPERIMENT 2 — GLOBAL CONTRASTIVE ALIGNMENT

The baseline architecture concatenated image and text embeddings without explicitly enforcing semantic consistency between modalities.

To address this, a global contrastive alignment objective was introduced and included in the training loop

- **[Code Screeshot](baseline.ipynb)** — Global Alignment Loss
  
**Initial Finding**
Applying contrastive alignment too aggressively during early training destabilized text learning, causing text loss to increase significantly. - **[Training Log](baseline.ipynb)**

**Lesson Learned**
This revealed a conflict between sequence prediction objectives and alignment objectives during early optimization.

## EXPERIMENT 3 — STABILIZED CONTRASTIVE TRAINING

The baseline architecture concatenated image and text embeddings without explicitly enforcing semantic consistency between modalities.

To stabilize training, the contrastive objective was delayed until later epochs and its weighting was reduced.
- **[Code screenshot](baseline.ipynb)** - Delayed Contrastive Loss
- **[Training Log](baseline.ipynb)** 

## Final Results

The stabilized contrastive model significantly improved BLEU score while maintaining comparable image reconstruction quality. 

- **[Final Results](baseline.ipynb)** - Table comparison for metrics across experiments

MSE increased by
SSIM inceased by
BLEU increased by
ROUGE increased by

## EXPLAINABLE AI

I modified the Attention class to return weights across frames. These weights were used to generate an attention heatmap/plot that helped to show which frames contributed the most in generating the predicted image.

- **[Code screenshot](baseline.ipynb)** - Attention Visualization

