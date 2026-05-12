# Multimodal Story Prediction with ResNet Transfer Learning and Contrastive Alignment

## QUICK LINKS

- **[Final Notebook](notebooks/Final_assessment_template_with_explainableAI.ipynb)** — Full experimental workflow
- **[Final Results](results/tables/final_results.csv)** — Quantitative evaluation metrics
- **[Figures](results/plots)** — Attention maps, similarity distributions, qualitative predictions

## PROJECT OVERVIEW

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

## BASELINE PIPELINE
(Input frames + descriptions) -> (Image encoder + text encoder) -> Fusion -> GRU temporal modeling -> Attention -> (Predict next image + next text)


## BASELINE EVALUATION
The following metrics were used to evaluate the baseline model and the improved model.
**MSE** (Mean Squared Error) and **SSIM** (Structural Similarity Index Measure) - Image quality assesment

**BLEU** (Bilingual Evaluation Understudy) and **ROUGE** (Recall-Oriented Understudy for Gisting Evaluation) - Text generation  quality

I wrote a **evaluate_model(...)** class to calculate these metrics all through the experiments.

- **[Code Screeshot](code_snapshots/evaluate_model.png)** — evaluate_model class


## EXPERIMENT 1 — VISUAL TRANSFER LEARNING

The baseline visual encoder was trained from scratch. Although suitable for reconstruction, it may not learn strong semantic visual features.

To improve this, the convolutional encoder was replaced with a pretrained ResNet-18 backbone initialized with ImageNet weights.

- **[Code Screeshot](code_snapshots/resnet.png)** — Visual Encoder with resnet backbone

**Key Insight**

Although the pretrained encoder improved visual feature quality, text generation performance degraded. This suggested that stronger visual representations alone were insufficient without proper multimodal alignment.

## EXPERIMENT 2 — GLOBAL CONTRASTIVE ALIGNMENT

The baseline architecture concatenated image and text embeddings without explicitly enforcing semantic consistency between modalities.

To address this, a global contrastive alignment objective was introduced and included in the training loop

- **[Code Screeshot](code_snapshots/global_alignment.png)** — Global Alignment Loss
  
**Initial Finding**

Applying contrastive alignment too aggressively during early training destabilized text learning, causing text loss to increase significantly. - **[Training Log](experiments/exp2_alignment/training_log.txt)**

**Lesson Learned**

This revealed a conflict between sequence prediction objectives and alignment objectives during early optimization.

## EXPERIMENT 3 — STABILIZED CONTRASTIVE TRAINING

The baseline architecture concatenated image and text embeddings without explicitly enforcing semantic consistency between modalities.

To stabilize training, the contrastive objective was delayed until later epochs and its weighting was reduced.

- **[Code screenshot](code_snapshots/contrastive_loss.png)** - Delayed Contrastive Loss
- **[Training Log](experiments/exp2_alignment_stable/training_log.txt)** 

## FINAL RESULTS

The stabilized contrastive model significantly improved BLEU score while maintaining comparable image reconstruction quality. 

- **[Final Results](results/tables/final_results.csv)** - Table comparison for metrics across experiments

MSE increased by 
SSIM inceased by
BLEU increased by
ROUGE increased by

## EXPLAINABLE AI

**Attention Visualization Plot**

I modified the Attention class to return weights across frames. These weights were used to generate an attention heatmap/plot that helped to show which frames contributed the most in generating the predicted image.

- **[Code screenshot](code_snapshots/attention_visualization.png)** - Attention Visualization Code
- **[Figure](results/plots/attention_heatmap_final.png)** - Attention Visualization Plot

**Similarity Distribution**

Although the embedding space remained highly compressed, positive image-text pairs consistently exhibited slightly higher cosine similarity than negative pairs. This suggests weak but meaningful multimodal alignment.

- **[Code screenshot](code_snapshots/embedding_similarity.png)** - Similarity plotting function
- **[Figure](results/plots/similarity_distribution.png)** - Similarity plotting 

## FINAL BAR PLOT

The final comparison plot highlights the trade-offs between visual reconstruction quality and multimodal alignment performance across experiments.

- **[Code screenshot](code_snapshots/bar_plot.png)**
- **[Figure](results/plots/final_comparison_plot.png)** 

## CHALLENGES FACED

1. Understanding the provided baseline architecture

2. Integrating pretrained ResNet without breaking latent dimensions

3. Balancing multimodal objectives

4. Contrastive instability

## CONCLUSION

This project demonstrated that improving visual feature quality alone does not guarantee better multimodal sequence prediction performance.

While transfer learning improved image representations, explicit alignment between image and text embeddings was necessary to translate these improvements into stronger narrative generation.

The experiments also revealed the importance of training stability when combining multiple learning objectives in multimodal systems.

## REFERENCES

- Oliveira & Matos. StoryReasoning Dataset (2025)
- He et al. Deep Residual Learning for Image Recognition (2016)
- Radford et al. Learning Transferable Visual Models From Natural Language Supervision (CLIP)

