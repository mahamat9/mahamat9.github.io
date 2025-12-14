---
layout: page
title: Deep image processing projects
description: Object detection and image segmentation
img: assets/img/cv_illustration.jpg
importance: 2
category: academic
giscus_comments: true
github: https://github.com/mahamat9/deep-processing-images
---

## [Object Detection](https://github.com/mahamat9/deep-processing-images/tree/main/TP%20Object%20Detection)

**Implementation of detection pipelines**
- Pre-trained model: YOLO11
- Foundation model: DINO
- Fine-tuning YOLO on MinneApple dataset
- Comparative analysis between classical and foundation models

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/object_detection_yolo.jpg" title="YOLO Detection Results" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/object_detection_dino.jpg" title="DINO Detection Results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: YOLO11 detection results on MinneApple dataset. Right: DINO foundation model detection comparison.
</div>

## [Blood Cell Image Segmentation](https://github.com/mahamat9/deep-processing-images/tree/main/TP%20Segmentation)

**Segmentation pipeline development**
- U-Net architecture
- VGG16 with transfer learning
- Data augmentation techniques
- Evaluation: Dice score and inverse IoU

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blood_cell_unet.jpg" title="U-Net Segmentation" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blood_cell_vgg16.jpg" title="VGG16 Segmentation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: U-Net architecture segmentation results. Right: VGG16 with transfer learning results.
</div>