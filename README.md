# Polyp Instrument Segmentation Using DeepLabv3+ with Custom Layers

## 1. Introduction and Motivation

Accurate instrument segmentation in endoscopic imagery is vital for automated surgical systems, assisting in tool tracking, scene understanding, and minimizing invasiveness. This project explores a custom implementation of DeepLabv3+ with a ResNet50 backbone, enhanced with handcrafted convolution and dilated spatial pyramid pooling layers. It focuses on robust polyp tool segmentation from the Kvasir-Instrument dataset using advanced preprocessing and evaluation strategies.

---

## 2. Research Objectives

* Develop an efficient DeepLabv3+ architecture with custom layers.
* Improve segmentation robustness through preprocessing techniques like histogram equalization and dilation.
* Evaluate performance using Dice Coefficient and Jaccard Index.
* Visualize qualitative performance under varying prediction thresholds.

### Key Research Questions

* Does preprocessing improve segmentation quality in medical instrument images?
* How effective is the custom DSPP module for fine-grained instrument boundary detection?
* Can we reach near real-time inference without compromising accuracy?

---

## 3. Dataset

* **Source**: Kvasir-Instrument Dataset
* **Inputs**: RGB endoscopy images (512x512)
* **Masks**: Binary instrument masks
* **Split**: 531 training, 80 validation, 59 test images

---

## 4. Image Preprocessing

* **Images**: Histogram equalization + Gaussian smoothing (kernel = 4x4)
* **Masks**: Binary conversion and dilation (kernel = 2x2)
* **Libraries**: OpenCV, NumPy

![Preprocessing Comparison](https://github.com/user-attachments/assets/b93bb934-4ef1-47ea-8254-ea42ce2ed3ab)

---

## 5. Model Architecture

### 5.1 Custom Layers

* **ConvBlock**: Modular convolutional layer with optional batch normalization and activation.
* **DilatedSpatialPyramidPooling**: Captures multi-scale context using dilated convolutions and global pooling.

### 5.2 DeepLabv3+ Backbone

* **Encoder**: ResNet50 pretrained on ImageNet.
* **ASPP**: Custom DSPP module with dilation rates \[1, 6, 12, 18].
* **Decoder**: Concatenation with low-level features + ConvBlocks + UpSampling2D
* **Output**: 1-channel sigmoid for binary segmentation

### 5.3 Model Details

* **Input Shape**: (512, 512, 3)
* **Loss**: Binary Crossentropy
* **Metrics**: Dice Loss, Dice Coefficient, Jaccard Index
* **Optimizer**: Adam

---

## 6. Training Strategy

* **Epochs**: 25
* **Batch Size**: 8
* **Learning Rate**: Default Adam
* **Callback Strategy**: No early stopping, manual validation

### Training Metrics

| Epoch | Val Dice | Val Jaccard |
| ----- | -------- | ----------- |
| 15    | 0.8672   | 0.7685      |
| 21    | 0.9725   | 0.9466      |
| 25    | 0.9731   | 0.9477      |

![__results___23_1](https://github.com/user-attachments/assets/d20f2f43-919b-4287-a300-69acb7140821)

---

## 7. Evaluation

| Split      | Dice   | Jaccard | Loss   |
| ---------- | ------ | ------- | ------ |
| Train      | 0.9781 | 0.9571  | 0.0071 |
| Validation | 0.9761 | 0.9534  | 0.0079 |
| Test       | 0.8649 | 0.7761  | 0.1596 |

---

## 8. Prediction and Visualization

* Evaluated predictions at two thresholds: 0.3 and 0.99
* Observed robustness in boundary detection across tools of varying shape and brightness

![Sample Predictions](https://github.com/user-attachments/assets/d6f41393-9d3b-4827-8981-9584eb6ca554)

---

## 9. Conclusion

This project demonstrates the utility of combining handcrafted preprocessing with custom deep semantic segmentation models. It highlights:

* The effectiveness of histogram equalization + dilation for medical imagery
* Custom DSPP’s ability to capture fine spatial detail
* Competitive segmentation scores with minimal overfitting

### Future Work

* Apply post-processing (e.g., CRF, morphological refinements)
* Explore multi-class segmentation across different tools
* Deploy model in real-time surgical assistance setups

---
