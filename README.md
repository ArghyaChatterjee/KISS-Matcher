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

### Usage 
Run `examples/run_kiss_matcher.py` following template:

```
python3 examples/run_kiss_matcher.py \
    --src_path <src_pcd_file> \
    --tgt_path <tgt_pcd_file> \
    --resolution <resolution> \
    --yaw_aug_angle <yaw_aug_angle in deg (Optional)>
```

### Small Object Dataset
Run KISS-Matcher for 2 small pointclouds:
```
cd python
python3 examples/run_kiss_matcher.py   \  
    --src_path /home/arghya/KISS-Matcher/input_data/mug_mesh_orig_0.pcd   \  
    --tgt_path /home/arghya/KISS-Matcher/input_data/mug_mesh_roi_0.pcd   \  
    --resolution 0.2
```


