
# SmartBasketball

AI basketball broadcasting system


## Features

- Real-time goal detection (FPS 300~)
- Real-time basketball broadcasting system (Auto-Framing, FPS 100~)
- Deploy on amateur basketball game

<img src="assert/smart_basketball_demo.gif" alt="drawing" width="480"/>
<img src="assert/_flowchart.png" alt="drawing" width="800"/>

## Pre Requirements
- CUDA = 10.2 or 11.x
- TensorRT = 8.4.1
- PyTorch = 1.13.0
- OpenCV (GStreamer) = 4.x.x
- YOLOv7
## Installation

Install python package

```bash
    pip install -r requirements.txt
```

Initial YOLO trt Weight
```bash
    sh yolo/init_trt_weight.sh
```
## Usage/Examples

#### 1. Genertate player density heatmap and PTZ Controller
```python
import sys
sys.path.append('[ROOT_OF_PATH]')
from smart_basketball_controller import SmartBasketballController

smart_basketball = SmartBasketballController(base_jump_size=2, vis=False)

count = 0
while True:
    # generate sync right_frame, left_frame
    ...

    # generate "2d_court_heatmap" and "update PZT director shot"
    court_with_heatmap, left_court_shot_full_time, right_court_shot_full_time = smart_basketball[[frame_left, frame_right]]

    cv2.namedWindow('court_with_heatmap',0)
    cv2.imshow('court_with_heatmap',court_with_heatmap)
    if cv2.waitKey(1) & 0xff == ord('q'):
        break
```

#### 2. Goal and shoot detect 
```python
import sys
sys.path.append('[ROOT_OF_PATH]')
from basketball_shoot_detector import BasketballShootDetector

cam_id = 211 # [211,214]
shoot_detector = BasketballShootDetector(cam_id, vis=False, save_log=False)

while True:
    # generate frame and frame_timestamp
    ...

    shoot_interval_info, shoot_info = shoot_detector.recvive(frame,file_time)
    # shoot_interval_info：當前幀投籃區間資訊,
    if shoot_interval_info is not None:
        [shoot_interval_start,shoot_interval_end], is_goal = shoot_interval_info
    
    # shoot_info：當前幀投籃進球資訊
    num_of_total_shoot, num_of_goal_shoot = shoot_info
```
