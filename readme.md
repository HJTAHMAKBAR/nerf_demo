## NeRF复现记录

> NeRF代码的官方实现是使用TensorFlow编写的，为了加强对其原理的理解以及后续使用的方便，故使用Pytorch进行改写复现。

### 主要模块

渲染过程

- 给定任意一个位姿

- 根据这个位姿以及成像平面上的像素坐标构造出射线

- 经过两个模型(coarse, fine)分层采样推理出RGB和σ

- 以此推广到每一帧，连续起来形成视频

#### 数据集

- NeRF Synthetic数据集
  - camera_angle_x: 水平视场角
  - frame：每一帧的照片
    - file_path：每一帧照片
    - rotation：旋转没有用到
    - transform_matrx：变换矩阵；相机坐标系到世界坐标系的转换矩阵；相机在世界坐标系下的位姿
      - R：旋转矩阵
      - T：平移矩阵，坐标系的中心
- 坐标系
  - 相机坐标系：[right, up, backward]即[x, y, z]
  - COLMAP：[right, down, forward]即[x, -y, -z]

#### NeRF模型结构

> NeRF有两个模型：coarse、fine

- coarse
  - 输入：rays, view_dir
  - 输出：σ, RGB_coarse
- fine
  - 输入：σ, RGB_coarse
  - 输出：RGB_fine

 

