# Medical Image Reconstruction and Anomaly Detection Using VAE and Tri-VAE Models

This repository contains two deep learning models for MRI reconstruction and anomaly detection. The first model is a standard variational autoencoder (VAE) trained on IXI T2-weighted brain MRI slices. The second model is a Tri-VAE equipped with skip connections, gated cross-skip attention, triplet regularization, and multi-scale reconstruction outputs. Both models are evaluated on BraTS2020 tumor segmentation data using reconstruction error and Dice similarity.

## Overview

The project explores generative modeling for medical imaging with a focus on reconstruction quality and the ability to detect abnormal tissue. The workflow includes dataset loading, preprocessing, model definition, training, and evaluation. Reconstruction error is used to approximate tumor regions, enabling anomaly detection without supervised labels.




## VAE Model

The first model is a conventional variational autoencoder consisting of:
- Three convolutional layers in the encoder  
- Fully connected layers outputting mean and log variance  
- A reparameterization module  
- A decoder with transpose convolutions  
- L1 reconstruction loss and KL divergence

The VAE is trained on the IXI dataset and evaluated by computing residuals on BraTS slices. A threshold on the residual map is used to approximate tumor masks, and Dice scores are computed against the ground truth segmentation.

## Tri-VAE Model

The Tri-VAE architecture extends the standard VAE with additional mechanisms:

- Encoder with three convolutional stages producing feature maps  
- Decoder with multi-scale reconstruction (coarse and full)  
- A Gated Cross Skip (GCS) connection that injects encoder features into the decoder  
- Triplet loss to enforce separation between clean, positive, and artificially corrupted negative samples  
- SSIM loss applied on the predicted negative reconstruction  
- KL divergence applied to anchor and positive embeddings  
- Multi-stage learning rate schedule for stable optimization

The Tri-VAE is trained on IXI slices and evaluated extensively on BraTS data. A reconstruction error map is thresholded to generate predicted tumor masks, and Dice coefficients are computed across slices and patients.

## Data Loading and Preprocessing

Two datasets are used:

### IXI Dataset
Used for training both models. PNG slices are loaded from subdirectories, converted to grayscale, resized to 256×256, and normalized.
<img width="758" height="244" alt="image" src="https://github.com/user-attachments/assets/9cac7a72-f456-4062-948b-19ebc7a1ab51" />

### BraTS2020 Dataset
Used for testing anomaly detection. T2-weighted images and segmentation masks are loaded using nibabel. Each 3D volume is processed slice-by-slice. A resized ground truth mask is compared to the predicted one to compute Dice scores.
<img width="1182" height="802" alt="image" src="https://github.com/user-attachments/assets/6d6e7d20-3e6d-4dff-bf71-0a81c24fb0a6" />
## Loss Functions

The models use multiple losses depending on the architecture:

- L1 reconstruction loss for full and coarse outputs  
- KL divergence for latent regularization  
- Triplet loss to ensure distinct embeddings  
- SSIM loss for negative reconstruction consistency  

These components enable robust reconstruction of healthy tissue and highlight abnormalities.

## Evaluation
<img width="1590" height="390" alt="image" src="https://github.com/user-attachments/assets/6d077b55-c1f0-4b55-95a6-b840a671b857" />

The evaluation pipeline includes:

- Reconstruction on individual BraTS slices  
- Residual (error) map computation  
- Thresholding to obtain predicted masks  
- Dice coefficient computation for each slice  
- Average Dice score across patients  
- Random slice testing for qualitative inspection  

This provides insight into each model’s capability to detect anomalies through reconstruction differences.

## Summary

This project demonstrates two approaches to medical image reconstruction and anomaly detection: a standard VAE and a more advanced Tri-VAE with skip connections and contrastive regularization. Using IXI for training and BraTS for evaluation, the models highlight the potential of generative modeling for unsupervised tumor detection. The Tri-VAE improves reconstruction fidelity and produces more consistent anomaly localization.
