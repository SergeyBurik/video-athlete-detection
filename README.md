# video-athlete-detection

# Athlete Detection and Tracking
## The main goal is to automatically:
- locate the athlete in the video (without prior knowledge of their position),
- track the athlete’s movements throughout the video, even when camera angles change,
- estimate the bounding box and 2D skeleton.

Tasks include selecting a detection method that finds only the athlete, choosing a tracking method, selecting a bounding box and 2D skeleton estimation method, and implementing the system’s inference pipeline.

Two solutions are described below.

## Solution 1
This solution implements a system for automatic athlete detection, tracking, and pose estimation in video.
It uses the **Grounding DINO** model for text-based detection at a rate of 5 times per second. After detection, the **CSRT tracker** is activated to track the athlete between detections.
To improve speed, detection runs on a **resized version** of the frame, with coordinates scaled back to the original size.
Once the athlete is detected or tracked, the corresponding region is cropped and passed to the **YOLO-Pose** model for pose estimation.
The detected keypoints are displayed as red dots on the original frame.
The processed video is saved as an output file.
The code also supports GPU acceleration with mixed precision for faster computation.

## Solution 2
This solution is an optimized version of the previous approach. **Grounding DINO** is used for initial detection, running at **a low frequency** (2 times per second), using resized frames for speed. The detected coordinates are scaled back to the original resolution. **Between DINO calls**, a **YOLO model** (or a similar lightweight model) is used to **refine** or update the athlete's position within an **expanded area around the last known location**. This region is dynamically enlarged horizontally and vertically to stabilize detection. If YOLO successfully finds the athlete, the coordinates are saved for further processing. Once the athlete’s region is defined, it is cropped and passed to the YOLO Pose model for body keypoint detection. The detected keypoints are then overlaid on the original frame as red dots, and the processed frames are saved as an output video.



![example1](https://github.com/user-attachments/assets/f17883f9-f5f2-40c5-a0c3-07639893c5ff)
![example2](https://github.com/user-attachments/assets/9beb311f-9cb2-4631-8a1f-d12222458eb2)
![example3](https://github.com/user-attachments/assets/afb86ae9-224a-4466-8b7d-a2177c9b20d2)
![example4](https://github.com/user-attachments/assets/a84bc3b5-6253-4091-97de-73c5441c531d)

## more examples in output_examples


