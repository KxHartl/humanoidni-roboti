# Humanoid robots and biomimetic systems: exercises and assignments

**Humanoidni roboti i biomimetički sustavi**

> Course exercises – Humanoid Robots and Biomimetic Systems (Humanoidni roboti i biomimetički sustavi), FSB, University of Zagreb, 2026

![C++](https://img.shields.io/badge/C%2B%2B-OpenCV%20%C2%B7%20PCL-00599C)
![Python](https://img.shields.io/badge/Python-3776AB)
![RealSense](https://img.shields.io/badge/camera-Intel%20RealSense-0071C5)
![YOLO](https://img.shields.io/badge/detection-Ultralytics%20YOLO-111F68)
![Open3D](https://img.shields.io/badge/point%20clouds-Open3D-4b5563)
![MuJoCo](https://img.shields.io/badge/simulation-MuJoCo-1f6feb)

The lab exercises and the three individual assignments of the course, collected in one
repository. The work follows one thread: an RGB-D camera mounted on a Universal Robots arm sees a
table with 3D-printed fruit, and the robot has to find each piece and pick it up. Along the way
come camera capture, camera and hand-eye calibration, object detection and segmentation, point
cloud processing, trajectory generation and joint and task-space control in simulation.

## What's inside

| Folder | Contents |
|---|---|
| `zadaca_01/` | Assignment 1: a C++ program that captures RGB images and organised point clouds from an Intel RealSense camera, configured from a RealSense Viewer JSON file |
| `Vision_systems/` | the same capture program in its later form, with the captured image and point cloud pairs of single fruit (apple, lemon, tomato) and of fruit groups |
| `object_detection_and_pose_estimation/` | exercise: an interactive C++ pipeline (OpenCV and PCL) that segments an RGB image by colour, applies the same mask to the point cloud and fits a sphere, a cylinder or oriented bounding boxes to estimate the object's centre |
| `camera_calibration/` | exercise in six tasks: intrinsic parameters, collecting image and robot pose pairs, ChArUco board pose, hand-eye calibration, locating a clicked object in the robot base frame, registering a point cloud in the robot base |
| `vjezba_trajektorija/` | exercise: first-order and fifth-order polynomial trajectories in joint space, a task-space trajectory with velocity and acceleration limits, and sending the trajectories to the robot in simulation |
| `zadaca_02/` | Assignment 2: instance segmentation of 3D-printed fruit with a YOLOv8 nano model trained on a dataset the course's students annotated together, and pose estimation from the RGB-D data, with the report |
| `object_detection/` | working scripts for detection and 3D localisation used on the way from assignment 2 to assignment 3 |
| `control/` | exercise in MuJoCo: P, PD and PID joint control of a 3-DOF arm with a step and a sine reference and with a disturbance, then task-space and impedance control of a Franka Panda, with the report |
| `pick_and_place/` | working scripts for the pick-and-place trajectory (HOME, APPROACH, PICK, APPROACH_PLACE, PLACE) |
| `zadaca_03/` | Assignment 3: the full pick-and-place pipeline, described below |

## Assignment 3: perception and manipulation pipeline

`zadaca_03/` takes the earlier pieces and runs them as one pipeline:

1. capture the scene from three camera poses on the robot,
2. segment the fruit in each RGB image with YOLO and map the masks to 3D through the depth data,
3. filter, register (ICP) and merge the point clouds,
4. compute the centroid of each object and transform it into the robot base frame with the
   hand-eye calibration,
5. plan a quintic task-space trajectory for the pick-and-place cycle and send it to the robot.

The code is in `zadaca_03/src/` and is started from `zadaca_03/`:

```bash
pip install -r requirements.txt
python src/main.py --help
```

`src/main.py` has a `--mock` option that runs without the robot and the camera, and `--offline`
with `--existing-captures <folder>` that runs the perception part on captures saved earlier, such
as those in `data/raw/outputs/`.

## Reports

- Assignment 2: [`zadaca_02/final_report/Kresimir_Hartl_Z2_Final.pdf`](zadaca_02/final_report/Kresimir_Hartl_Z2_Final.pdf)
- Assignment 3: [`zadaca_03/dist/report.pdf`](zadaca_03/dist/report.pdf)
- Control exercise: [`control/docs/seminar/izvjesce.pdf`](control/docs/seminar/izvjesce.pdf)

All reports are in Croatian.

## Build the C++ programs

The C++ folders (`zadaca_01/Realsense_humanoidna_cpp_project/Realsense_humanoidna/`,
`Vision_systems/`, `object_detection_and_pose_estimation/`) are CMake projects that need OpenCV
and PCL, and the capture programs also need librealsense2.

```bash
cd Vision_systems
cmake -S . -B build
cmake --build build
```
