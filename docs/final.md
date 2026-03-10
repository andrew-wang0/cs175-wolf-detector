---
layout: default
title: Final Report
---

# Final Video

<video width="100%" controls>
  <source src="./img/vid.mp4" type="video/mp4">
</video>

<div style="height: 220px; margin: 1.5rem 0; border: 2px dashed #bdbdbd; border-radius: 10px; display: flex; align-items: center; justify-content: center; color: #666; text-align: center; padding: 1rem;">
  <strong>Leave space for live gameplay demo.</strong>
</div>

# Project Summary

Our project aims to build a real-time wolf detector for Minecraft that can run during gameplay. The input to the system is an image frame captured from Minecraft, and the output is a set of 2D bounding boxes with confidence scores indicating where wolves appear in the frame.

A large part of the engineering was put into getting a reliable data pipeline to train our models with. We wrote a NeoForged mod that spawns wolves, captures screenshots, and converts each wolf's in-game 3D hitbox into a YOLO-format 2D annotation. That let us generate training data directly from Minecraft instead of labeling images by hand.

The biggest struggle we ran into was that good results were very dependent on the data quality and data diversity that we gave it. Our early models looked promising in a simple setup, but struggled in a more complex environment. We used a mostly fixed camera, a predictable floor, and no meaningful distractor mobs. In the final stage of the project, we made the task harder in three targeted ways. We changed camera angles, changed the floor across ~100 multicolored Minecraft block types, and added sheep to the scene so the detector had to ignore lookalike mobs.

# Approach

<img src="./img/pipeline.png" width="100%" alt="Pipeline for Minecraft data generation, training, and evaluation">

## Minecraft Environment

We chose to generate our training data directly inside Minecraft so that the model would learn from the same visual environment it would later run in. This helped ensure the wolf detector would work in the game across different terrains and situations.

For the initial experiments, we used a simple superflat world and repeatedly generated scenes with wolves from the player’s point of view. This made it easier to build and test the data pipeline before expanding to more varied environments.

