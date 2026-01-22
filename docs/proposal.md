---
layout: default
title: Proposal
---

## Summary of the Project 

Our project aims to build a wolf detector in Minecraft. The goal is to automatically identify the location of all wolves in a given game frame with a bounding box in real time (actual gameplay). The input to our system will be visual data captured from an automated data generation harness in Minecraft, which automatically generates screenshots of wolves and the corresponding 2D bounding box coordinates. The output will be a list of axis-aligned bounding boxes indicating the location of wolves in the frame. In practice, the model should be able help players detect wolves in realtime gameplay.

## AI/ML Algorithms 

We will be using supervised machine learning for image object detection, namely convolutional neural networks (CNNs).

## Evaluation Plan

To evaluate our project quantitatively, we will measure standard classification and detection metrics such as accuracy, precision, recall, and F1 score on a validation set of Minecraft images, separate from the training set. As a baseline, we will compare against a naive, purely algorithmic classifier that draws bounding boxes around all vaguely white objects. We expect our model to significantly outperform this baseline, ideally achieving high precision and recall in varied environments (e.g., different biomes, lighting conditions, and distances). Qualitatively, we will visualize detection results directly in Minecraft screenshots to verify correct behavior, test edge cases such as partially occluded wolves or visually similar mobs. We will also be testing bounding boxes  Our “moonshot” scenario is running the game at 60 fps and capturing wolves accurately.




(no ai was used in this proposal)
