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

For the initial experiments, we used a simple superflat world and repeatedly generated scenes with wolves from a birds-eye view. This made it easier to build and test the data pipeline before expanding to more varied environments.


## Data Generation

Our data generation pipeline was built as a custom NeoForged mod. At runtime, the mod:

- Spawns wolves around a center point at randomized positions.
- Teleports the player around the center point so that the wolves are seen from different camera angles.
- Replaces the floor with one of 78 differently colored Minecraft blocks.
- Spawns sheep into the scene.
- Captures a screenshot from the players viewpoint.
- Reads each wolf's 3D axis-aligned bounding box from the game engine.
- Writes one YOLO annotation per visible wolf in the format `<class_id> <center_x> <center_y> <width> <height>`.
- Clear and reset the scene with the steps above

The sheep were intentionally included but not labeled, since this remained a one-class wolf detector. That made them useful hard negatives. If the detector started drawing wolf boxes around sheep, it meant the model was still relying on visual cues such as whiteness instead of actually distinguishing wolf features.

One small detail we kept in the dataset was the platform frequently appearing as two types of blocks instead of one. This happens because when we replace a large amount of blocks, the game doesn't re-render them all right away. We chose not to fix this bug since they added more variation of the environment to the dataset.

To avoid labeling wolves that the player could not actually see, we added a visibility check using ray tracing from the camera to the bounding-box corners. If the wolf was fully occluded by terrain or another mob, we excluded it from the labels. This was less useful for our superflat scenario but would've been useful to correctly label occluded wolves in the future.

The different datasets ended up having a large impact on the behavior of the final model. Each step of variation that was added forced the detector to rely less on simple shortcuts and more on the actual visual features of wolves. Each change exposed new weaknesses in earlier models and helped us guess what the detector was really learning from the data.

We also considered generating training data in the Minecraft overworld so the model could learn from more natural terrain and occlusion. However, we didn't have a way to reliably vary the enviroment. In our superflat environment, we were able to control the color and angle of our samples consistently. Because of this, we chose to keep training data generation in the controlled platform for now.

<img src="./img/varied_samples.jpg" width="100%" alt="varied data examples">


# Resources Used

- Ultralytics YOLO documentation for model training and inference: https://docs.ultralytics.com
- NeoForged for Minecraft mod development and in-game data generation: https://neoforged.net
- OpenCV (`cv2`) for image processing and drawing evaluation outputs: https://opencv.org
- Minecraft as the environment for data generation and testing: https://www.minecraft.net
- Python standard library modules such as `argparse`, `pathlib`, `random`, `shutil`, and `datetime`
- ChatGPT for code clarification and minor implementation support; the system design, project decisions, and main implementation work were completed by the team