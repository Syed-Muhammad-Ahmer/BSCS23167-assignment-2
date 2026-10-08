# Fine-Tuning ResNet-50 and U-Net on Small Image Datasets

Student Roll Number: BSCS23167 
Repository: Syed Muhammad Ahmer

## Repository Structure

```text
/
├── notebook/
│   └── assignment_resnet_unet.ipynb
├── results/
│   ├── resnet_results.png
│   ├── resnet_confusion_matrix.png
│   ├── unet_curves.png
│   └── unet_predictions.png
└── README.md

Datasets

The datasets are not tracked in this repository due to size constraints. They
can be downloaded directly from the official sources:

  - Oxford Flowers-102: https://www.robots.ox.ac.uk/~vgg/data/flowers/102/
      - Subset: 5 classes, 40 images per class (200 images total).
  - Oxford-IIIT Pet: https://www.robots.ox.ac.uk/~vgg/data/pets/
      - Subset: 150 image and binary mask pairs.


Methodology

Part A: ResNet-50 Classification

  - Pretrained weights: ImageNet-1K (ResNet50_Weights.DEFAULT).
  - Fully connected layer replaced to output 5 classes.
  - Phase 1: Backbone frozen, classifier trained for 10 epochs (AdamW, lr=1e-3,
    weight_decay=1e-4).
  - Phase 2: layer4 unfrozen, trained for 10 epochs (AdamW, lr=1e-4, Cosine
    Annealing scheduler).
  - BatchNorm layers maintained in evaluation mode during training.

Part B: U-Net Segmentation

  - Architecture: U-Net with ImageNet-pretrained ResNet-50 encoder
    (segmentation_models_pytorch).
  - Target: Binary pet segmentation (foreground vs. background).
  - Loss: Combined Binary Cross-Entropy with Logits and Soft Dice Loss.
  - Phase 1: Encoder frozen, decoder and segmentation head trained for 15 epochs
    (AdamW, lr=1e-3).
  - Phase 2: Encoder layer3 and layer4 unfrozen, trained for 15 epochs using
    discriminative learning rates (encoder lr=1e-4, decoder lr=3e-4, Cosine
    Annealing scheduler).

Results

ResNet-50 (Classification)

  - Split: 70% Train (140), 15% Validation (30), 15% Test (30)

| Metric              | Score |
| ------------------- | ----: |
| Training Accuracy   | 1.000 |
| Validation Accuracy | 0.967 |
| Test Accuracy       | 1.000 |
| Macro Precision     | 1.000 |
| Macro Recall        | 1.000 |
| Macro F1-Score      | 1.000 |

U-Net (Segmentation)

  - Split: 70% Train (105), 15% Validation (22), 15% Test (23)

| Metric              | Score  |
| ------------------- | -----: |
| Training Loss       | 0.1281 |
| Validation Loss     | 0.2034 |
| Test Dice Score     | 0.9328 |
| Test IoU            | 0.8795 |
| Test Pixel Accuracy | 0.9459 |

Requirements and Execution

  - Python 3.10+
  - PyTorch 2.x
  - Torchvision
  - segmentation-models-pytorch
  - scikit-learn
  - matplotlib
  - pillow
  - scipy

To run the notebook:

1.  Open notebook/assignment_resnet_unet.ipynb in Google Colab or a local
    Jupyter environment with a CUDA-enabled GPU.
2.  Install required packages:
    pip install segmentation-models-pytorch
3.  Execute the cells sequentially from top to bottom.
