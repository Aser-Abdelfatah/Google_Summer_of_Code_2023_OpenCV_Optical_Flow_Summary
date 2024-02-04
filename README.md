# Google Summer of Code 2023: Adding Optical Flow Model to OpenCV Library
- This is a summary of my contributions to the OpenCV library under Google Summer of Code 2023.
- I'd like to thank my mentor [Yuantao Feng
](https://github.com/fengyuentau) for his invaluable guidance and support throughout the project. His help was absolutely instrumental to the project's success. 
## Project goal: Adding a lightweight deep-learning optical flow model to OpenCV. 

### Problem Summary 
Optical flow is the problem of estimating the motion of objects in an image or video sequence. Optical flow is pivotal to many computer vision applications such as object tracking, video stabilization, and motion analysis, providing essential information about the dynamics of a scene or an object. With the upsurge in the use of deep neural networks, DNN models were developed to solve the optical flow problem, achieving state-of-the-art results by learning to estimate motion from image data. However, DNNs can become computationally expensive making it infeasible to be deployed on embedded systems. This problem is tackled by the development of lightweight DNN models. However, the OpenCV model zoo hasn’t had any implementation of an optical flow lightweight model yet. Thus, this project aims to find the best lightweight optical flow model in terms of model size, speed, and accuracy to introduce to the OpenCV model zoo.


### Phase 1: Identifying the best lightweight DNN optical flow model:
Using KITTI Vision Benchmark Suite, I shortlisted three models based on their accuracy - measured in Fl-All[^1] - and runtime. The models were RAFT-it+_RVC, GMFlow, and MatchFlow. In consultation with the mentor, we decided to use the RAFT[^2] model for two main reasons. 
- First, the RAFT model is the basis for various optical flow models, such as ScaleRAFTRBO, CamLiRAFT, and RAFT-3D++. Therefore, the RAFT model is multi-purpose and modular and can be modified  by OpenCV users to meet their various needs, especially given that it is supported across different libraries and frameworks. 
- Second reason is the open-source availability of the model's ONNX version, which facilitates loading the model with OpenCV DNN.

### Phase 2: Implementing required operators to load the model with OpenCV DNN
To convert RAFT from ONNX to run with OpenCV DNN, the GatherElements operator had to be added to the OpenCV DNN module. The first [pull request](https://github.com/opencv/opencv/pull/24092) to OpenCV was implementing the operator in the OpenCV DNN module. Then, a [second pull request](https://github.com/opencv/opencv_extra/pull/1082) was made to OpenCV_extra to test and validate the implementation of GatherElements. 
### Phase 3: Introducing a demo for the RAFT model to OpenCV Model Zoo using ONNX
Simultaneous to implementing and testing the GatherElements operator, a [third pull request](https://github.com/opencv/opencv_zoo/pull/197) was submitted to opencv_zoo to add the RAFT model along with a demo and example for inputs and outputs using the ONNX version of the RAFT model.
### Phase 4: Loading the RAFT model with the OpenCV DNN
After the GatherElements operator passed the tests, the [pull request to opencv_zoo](https://github.com/opencv/opencv_zoo/pull/197) was updated to run the demos for the RAFT model with OpenCV DNN instead of ONNX. In addition, I solved the twinkling issue of the visualization module. An example of the updated output:
<p align="center"> <img src="https://github.com/Aser-Abdelfatah/Google_Summer_of_Code_2023_OpenCV_Optical_Flow_Summary/assets/47282229/51ee3612-c478-45fe-a398-7b2036f37f4d" />

### Phase 5: Testing the RAFT model with OpenCV DNN
 Finally, the [initial OpenCV pull request of the GatherElements implmentation](https://github.com/opencv/opencv/pull/24092) was updated to test the RAFT model loaded with OpenCV DNN. And the [second OpenCV_extra pull request](https://github.com/opencv/opencv_extra/pull/1082) was updated to add the data needed for testing the RAFT model. 


[^1]: Fl-All refers to the percentage of pixels with endPoint Error over the complete frames (EPE) more than 3 pixels or larger than 5% of the ground truth.
[^2]: The Recurrent All-Pairs Field Transforms (RAFT) model was introduced in 2020 by Zachary Teed and Jia Deng
of Princeton University. The model is available in open source on [the GitHub repo RAFT](https://github.com/princeton-vl/RAFT). And the research paper is published on [arXiv with the title RAFT: Recurrent All-Pairs Field Transforms for Optical Flow.](https://arxiv.org/pdf/2003.12039.pdf)

