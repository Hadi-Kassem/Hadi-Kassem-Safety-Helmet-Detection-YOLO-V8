# 🦺 Hardhat Detection with YOLOv8

This project implements a **hardhat and safety compliance detection system** using the **YOLOv8s** object detection model.  
The goal is to detect whether construction workers are wearing safety helmets, as well as identifying heads and persons without helmets.  

---

## 📂 Dataset

- **Source:** [Kaggle – Hard Hat Detection](https://www.kaggle.com/datasets/andrewmvd/hard-hat-detection) 
- **Content:**  
  - ~**5,000 images**  
  - Bounding box annotations in Pascal VOC format  
  - Classes: **helmet**, **head**, **person** 


---

## 🛠️ Data Preparation

1. **Annotation Format**: YOLO format (`.txt` files with bounding boxes).
2. **Preprocessing**:
   - Resized images to standard input size (YOLOv8 default 640×640).
   - Normalized bounding boxes.
3. **Augmentation**: 
   - Random horizontal flips
   - Random scaling
   - Color jitter
   - Mosaic augmentations (YOLO built-in)

---

## ⚙️ Model Training

We trained a **YOLOv8s** model using Ultralytics’ framework.

**Training Settings:**
- **Model**: YOLOv8s
- **Epochs**: 30
- **Batch Size**: 16
- **Optimizer**: AdamW
- **Learning Rate**: `6e-05`
- **Hardware**: NVIDIA Tesla T4 GPU (15 GB VRAM)

**Training Losses:**
- **Box loss**: 1.0959 → 1.2239 (val)
- **Class loss**: 0.5106 → 0.5744 (val)
- **DFL loss**: 1.0672 → 1.1422 (val)

---

## 📊 Validation Performance

The trained model was evaluated on the validation set.  
Here are the results:

### Overall Metrics

<table>
  <tr>
    <th>Metric</th>
    <th>Value</th>
  </tr>
  <tr>
    <td>Precision</td>
    <td>0.957</td>
  </tr>
  <tr>
    <td>Recall</td>
    <td>0.591</td>
  </tr>
  <tr>
    <td>mAP@50</td>
    <td>0.636</td>
  </tr>
  <tr>
    <td>mAP@50-95</td>
    <td>0.424</td>
  </tr>
</table>

---

### Per-Class Metrics

<table>
  <tr>
    <th>Class</th>
    <th>Precision</th>
    <th>Recall</th>
    <th>mAP@50</th>
    <th>mAP@50-95</th>
  </tr>
  <tr>
    <td>Helmet</td>
    <td>0.949</td>
    <td>0.904</td>
    <td>0.960</td>
    <td>0.644</td>
  </tr>
  <tr>
    <td>Head</td>
    <td>0.921</td>
    <td>0.868</td>
    <td>0.921</td>
    <td>0.612</td>
  </tr>
  <tr>
    <td>Person</td>
    <td>1.000</td>
    <td>0.000</td>
    <td>0.027</td>
    <td>0.015</td>
  </tr>
</table>

---

## 🚀 Results Interpretation

- The model performs **very well** on detecting **helmets** and **heads** with high precision and recall.
- **Helmet detection** achieves **96% mAP@50** → reliable in detecting safety compliance.
- **Head detection** is also strong (**92% mAP@50**).
- **Person detection** struggled due to **data imbalance** (few samples for the `person` class).  
  → This can be improved with **more training data** or **class balancing**.

