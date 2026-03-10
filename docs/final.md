---
layout: default
title: Final Report
---

# Final Video

# Project Summary

Our project aims to build a real-time wolf detection system for Minecraft that locates wolves in a live gameplay environment. The input to the system consists of image frames captured from Minecraft.

The input are images are collected through a semi-automated data generation process that takes screenshots and associates each wolf with 2D bounding box coordinates in YOLO format through a custom made Minecraft mod.

The output of the model is a set of axis-aligned bounding boxes (AABB), each corresponding to a detected wolf in the frame, along with a confidence score. Each bounding box specifies the predicted location of a wolf in image coordinates.

In practice, running our trained model in a live environment will be able to detect wolves in gameplay frames, enabling live visual highlighting of wolves during gameplay.


# Approach

<img src="./img/pipeline.png" width="100%">

# Evaluation

# Resources Used