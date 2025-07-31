# KISS-Matcher

## Requirements
We have tested this on Ubuntu 22.04, Cuda 12.04, Nvidia RTX 3060 and Python 3.10.

## Setup

Create a virtual environment and compile the packages.

```
python3 -m venv kiss_matcher_venv
source kiss_matcher_venv/bin/activate
pip3 install --upgrade pip setuptools wheel scikit-build-core ninja cmake build
```

## Installation

And then, run the following command:

```
cd python
pip3 install -e .
```

If something goes weird, you can add `verbose` option like this:

```
cd python
pip3 install -e . --verbose
```

## How to Run

### Example A. Small Object Dataset
```
cd python
python3 examples/run_kiss_matcher.py   \  
    --src_path /home/arghya/ihmc-repos/ihmc-humanoid-labeler/humanoid_data/2d_3d_data/workspace_mug/meta_data/mug_mesh_orig_0.pcd   \  
    --tgt_path /home/arghya/ihmc-repos/ihmc-humanoid-labeler/humanoid_data/2d_3d_data/workspace_mug/meta_data/mug_mesh_roi_0.pcd   \  
    --resolution 0.2
```
### Example A. KITTI 10m Benchmark

### Example B. Registration from scan-level to map level

First, download files via:

```
python3 utils/download_datasets.py
```

Then, all datas are downloaded in `data/` directory.

**Usage**: Run `examples/run_kiss_matcher.py` following template:

```
python3 examples/run_kiss_matcher.py \
    --src_path <src_pcd_file> \
    --tgt_path <tgt_pcd_file> \
    --resolution <resolution> \
    --yaw_aug_angle <yaw_aug_angle in deg (Optional)>
```

### Example B-0. Perform registration two point clouds from different viewpoints of Velodyne 16 at MIT campus

```
python3 examples/run_kiss_matcher.py \
    --src_path data/Vel16/src.pcd \
    --tgt_path data/Vel16/tgt.pcd \
    --resolution 0.2
```

**Result**

![Image](https://github.com/user-attachments/assets/c7d57fd1-24e7-458e-84a8-9b3578cc12dd)

### Example B-1. Perform registration two point clouds from different viewpoints of Velodyne 64 in KITTI dataset

```
python3 examples/run_kiss_matcher.py \
    --src_path data/Vel64/kitti_000540.pcd \
    --tgt_path data/Vel64/kitti_001319.pcd \
    --resolution 0.3
```

**Result**

![Image](https://github.com/user-attachments/assets/2adfa6d8-d3cb-4bd3-9283-26926f171806)

### Example B-2. Perform registration KITTI07 (Orange) and KITTI00 (Cyan) map clouds

```
python3 examples/run_kiss_matcher.py \
    --src_path data/KITTI00-to-07/kitti07.pcd \
    --tgt_path data/KITTI00-to-07/kitti00.pcd \
    --resolution 2.0
```

**Result**

![Image](https://github.com/user-attachments/assets/5716f629-19cc-4aa8-b715-70178cca8f20)

### Example B-3. Perform registration KITTI00 (Orange) and KITTI360-09 (Cyan) map clouds

```
python3 examples/run_kiss_matcher.py \
    --src_path data/KITTI00-to-KITTI360/kitti00.pcd \
    --tgt_path data/KITTI00-to-KITTI360/kitti360_09.pcd \
    --resolution 2.0
```

**Result**

![Image](https://github.com/user-attachments/assets/4e569bac-9264-457f-9e85-2664b3b76ed7)

### Example B-4. Perform registration heterogeneous LiDAR point cloud maps in `KAIST05` of the HeLiPR dataset

We're so excited in that initial transformation problem between Heterogeneous LiDAR SLAM now has been solved via KISS-Matcher!
By setting `<yaw_aug_angle>`, we can check whether it works even in the presence of huge pose discrepancy.

- Aeva-to-Livox

```
python3 examples/run_kiss_matcher.py \
    --src_path data/HeLiPR-KAIST05/Aeva.pcd \
    --tgt_path data/HeLiPR-KAIST05/Livox.pcd \
    --resolution 2.0 \
    --yaw_aug_angle 180
```

- Aeva-to-Ouster

```
python3 examples/run_kiss_matcher.py \
    --src_path data/HeLiPR-KAIST05/Aeva.pcd \
    --tgt_path data/HeLiPR-KAIST05/Ouster.pcd \
    --resolution 2.0 \
    --yaw_aug_angle 180
```

- Aeva-to-Ouster

```
python3 examples/run_kiss_matcher.py \
    --src_path data/HeLiPR-KAIST05/Livox.pcd \
    --tgt_path data/HeLiPR-KAIST05/Ouster.pcd \
    --resolution 2.0 \
    --yaw_aug_angle 180
```

**Result**

![Image](https://github.com/user-attachments/assets/9427e089-44f1-40e3-bc44-cc66f0c8e17c)

### Example B-5. Perform registration `Collosseo test0` (Orange) and `train0` (Cyan) sequence maps of the VBR dataset

In this example, there are non-negligible pitch and roll rotation. So, please set

```
python3 examples/run_kiss_matcher.py \
    --src_path data/VBR-Collosseo/test0.pcd \
    --tgt_path data/VBR-Collosseo/train0.pcd \
    --resolution 2.0
```

**Result**

![Image](https://github.com/user-attachments/assets/2e84608e-b1c8-4706-8ea9-f1149697cc4b)

> :warning: We have confirmed that the PCL visualizer triggers a segmentation fault when no GPU is available. In this case, all warped clouds are saved as ${SRC_NAME}\_warped.pcd, and we recommend using other visualization tools, such as CloudCompare, for visualization.
