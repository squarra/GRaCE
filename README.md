> [!NOTE]
> I couldn't get it to run quite yet. Here are some notes

- A cuda capable GPU is needed (Compiling Pointnet2_PyTorch needs it)
- Don't use the conda environment, I've come further by using python3.12
    - Use the most recent versions of all the libraries
    - Especially if you run into the `libtorch.so` error, use a recent torch version
- Can't use python3.13 yet, because open3d is not compiled for it
- `graspflow-simulator` repo doesn't exist or is private, just comment it out in the `.gitmodules` file or init every module on it's own
- 
- Symlinking fixed the Eigen error for the franke module 

```sh
sudo ln -s /usr/include/eigen3/Eigen /usr/include/Eigen
```

# GRaCE

## Introduction

This paper addresses the multi-faceted problem of robot grasping, where multiple criteria may conflict and differ in importance. We introduce Grasp Ranking and Criteria Evaluation (GRaCE), a novel approach that employs hierarchical rule-based logic and a rank-preserving utility function to optimize grasps based on various criteria such as stability, kinematic constraints, and goal-oriented functionalities. Additionally, we propose GRaCE-OPT, a hybrid optimization strategy that combines gradient-based and gradient-free methods to effectively navigate the complex, non-convex utility function. Experimental results in both simulated and real-world scenarios show that GRaCE requires fewer samples to achieve comparable or superior performance relative to existing methods. The modular architecture of GRaCE allows for easy customization and adaptation to specific application needs.

    <img align="center" alt="GraspFlow" src="figs/main_fig3.png" width="600" height="300" />

This project has a lot of submodules. We recommend to use forked versions of submodules given in this repository. First, create and activate conda environment and install modules given in environment.yaml:

```
conda env create -f environment.yml
```

## Section 1: GraspNet from NVidia

We use Pytorch version of the Graspnet. 
### Prerequisits:
1. Pointnet2_PyTorch (given as a submodule in this repo) - PointNet++ library for GraspNet's backbone.
2. franka_analytical_ik  - solves analytical IK for Panda Robot.

### Installation
We mainly follow same installation as in. However, we also extended it to add additional filtering capabilites. Please install IK submodule and copy generated library to pytorch_6dof-graspnet module. Details are given in [this link](https://github.com/tasbolat1/franka_analytical_ik.git).

*Note:* Generally any grasp sampler can be used. In our paper, we have also tested on [GPD sampler](https://github.com/tasbolat1/gpd.git).


## Section 2: Grasp Optimization via GRaCE

### Prerequisites
1. franka_analytical_ik - solves analytical IK for Panda Robot.
2. differentiable-robot-model -  differentiable robot model used for E-classifier to calculate FK of the robot.
3. Task-Grasp - Classifier for grasps with respect to the intent provided by a user
4. JointBERT - Intent classification model for grasp queries from a user

Each of the submodule has own documentation for the setup.
### Usage
To optimize grasps, go to GoES folder:

```
cd GoES/GoES
```

and run:
```
python refine_isaac_grasps.py --cat <cat> --idx <idx>\
            --grasp_folder ../experiments/test \
            --sampler graspnet --grasp_space SO3 --device 0 --max_iterations 30 \
            --method GraspOptES --classifier <classifier> --experiment_type E_TYPE \
            --cfg configs/graspopt_isaac_params.yaml --batch_size 15
```
For list of parameters for the above function, please check settings.txt file.

### Simulation
Please read README under ``graspflow-simulator`` folder.

## BibTeX

To cite this work, please use:

```
TODO
```
