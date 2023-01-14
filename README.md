# YOLO-Argus2

[Argus2](https://www.frontiersin.org/articles/10.3389/fnins.2018.00584/full) is an retinal implant approved for treating blindness based on a 6x10 grid of electrodes.
Upon activation, each electrode is capable of eliciting in the patient a visual cue akin to a bright light in the part of the field of view(FOV) corresponding to the stimulating
electrode on the retina.

Due to the extremely low spatial resolution of the device, filtering is ineffective for preserving useful information in the FOV. When reduced to a 6x10 representation,
the acquired image will have lost all of its information. 

This repos is a YOLO-powered algorithm with the following workflow:
1) the acquired image is run through the YOLO network, who outputs the bounding boxes of the reckognized objects in the frame
2) The centers of the bounding boxes are displayied in the corresponding grid-cell (this is only for the purpose of visualization, the true action would be activating the
corresponding electrode in the Argus2's grid
3) if the camera(patient) stays focused on a scene for 3 seconds(this period can be adjusted), the encoding and display of the detected objects begins. Details below

Below a sample frame to illustrate.

![SampleFrame](/data/SampleImage.png)

## Workflow
- During all this procedure the camera/patient has to stay focused. looking away will stop it
- The order with which objects are encoded is of increasing y-coordinates in the grid (in standard plt notation, where origin is in top-left angle of the image and y-axis increase in the bottom direction
  - if two objects have the same y-coordinate in the grid, the one with lowest X-coordinate goes first
- First, the object to be encoded blinks for 3 seconds. This gives the patient a clear understanding of which object's encoding is about to be shown
- Then, the symbol corresponding to that object is displayied for 3 seconds.
- If the patient is still focused, repeat the previous 2 steps.

Here a short video to better showcase how it works

![Video](/data/combinedVideo.avi)

## Encoding strategy
the YOLO network used was trained on the [Coco](https://cocodataset.org/#home) dataset. Of the 80 categories, an encoding was defined for the most useful ones for everyday life. 
For each of them, an unique encoding was defined. A stylized representation of the object was chosen when possible. 
For example, for "laptop", the shown pattern would be:

<img src="/data/laptop.png" width="250" height="280">


## Usage
- YOLO weights are not included in this repo. you can download them from [darknet](https://pjreddie.com/darknet/yolo/)
- Slight modifications are needed in the handling of the in-stream if it is to be run online by a camera. For HW constraints of my machine, these were not included in the code


