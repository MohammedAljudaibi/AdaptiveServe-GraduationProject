
# AdaptiveServe (Graduation Project)
**AdaptiveServe** is an AI-powered table tennis training system that adapts its serves based on the player’s skill. It uses computer vision to track the ball and machine learning models to adjust serve parameters in real time.

https://github.com/user-attachments/assets/852a487f-3bc0-4e23-a4a9-6fa43c61f11d

----------

## Overview

The system works in a loop:

1. A serve is made using a commercial ping pong robot (Huipang HP-07).
    
2.  A high-speed camera captures the player’s return.
    
3.  The video is analyzed in three steps:
    I. YOLOv8 detects the ball's location in each frame.
    II. Missing detections are filled using interpolation.
    III. Ball landing frames are identified to assess performance.
4.  A linear regression model analyzes the player's performance.
    
5.  Serve parameters (topspin/backspin) are adjusted accordingly.
    

The system sends these updates to a Raspberry Pi, which triggers relay switches to simulate pressing physical buttons on the serve machine’s controller.

----------

## Usage

### Laptop (main system)

```bash
python main.py
```

This:

1.   Starts with a test phase to measure skill then sets serve parameters
    
1.   Records video after every 5 serves
    
1.   Updates parameters based on performance
    
1.   Saves annotated video outputs in `/output_videos/`

>  Loops back to step  2
    

### Raspberry Pi (serve machine)

The `network.py` file opens a TCP socket to receives commands like `Tops+`, `Back-`, etc. Each message activates a relay switch to adjust the robot.

---

| Command | Action |
| :---: | :---: |
|Freq+ / Freq-|Increase or Decrease Frequency of Balls Served per Minutes|
|Tops+ / Tops- |Increase or Decrease Topspin of Served Balls|
|Back+ / Back- |Increase or Decrease Backspin of Served Balls|
|Osci+ / Osci- |Increase or Decrease Oscillation Speed (Horizontal Direction)|

## Requirements

-   Python 3.8+
    
-   Raspberry Pi with GPIO pins
    
-   Relay module
    
-   Camera (ideally 60fps or more)
    
-   Huipang HP-07 serve machine
    
----------

## Example

A GUI appears showing current serve parameters, after the first 16 serves, the system logs:

```
Total Balls Hit: 10
Hit Percentage: 62.5%
```

Then adjusts future serves accordingly and keeps adapting by sending instructions to the machine.

----------
