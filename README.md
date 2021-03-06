# **Traffic Sign Recognition** 

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./writeup_imgs/distr.png "Distribution"
[image2]: ./writeup_imgs/vis.png "Visualization"
[image3]: ./newSigns_32x32x3/11_RigtoffWayatNextIntersection.jpg "Traffic Sign 1"
[image4]: ./newSigns_32x32x3/12_PriorityRoad.jpg "Traffic Sign 2"
[image5]: ./newSigns_32x32x3/13_Yield.jpg "Traffic Sign 3"
[image6]: ./newSigns_32x32x3/17_NoEntry.jpg "Traffic Sign 4"
[image7]: ./newSigns_32x32x3/31_WildAnimalsCrossing.jpg "Traffic Sign 5"
[image8]: ./writeup_imgs/accuracy.png "Validation Accuracy"
[image9]: ./writeup_imgs/top5.png "Top 5 Probabilities"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it!

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the numpy library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. 

![alt text][image2]

It is a bar chart showing that the distribution of classes in the training, validation and test set.

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

In order to pre-process the data set, traffic sign images are converted grayscale for reducing the complexity of the classification problem. Then, the grayscale images normalized to `[-1 1]` using `(pixel - 128)/ 128`.


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

Architecture of the model based on the LeNet model architecture and it is modified by adding dropout layers to prevent overfitting problem. My final model consisted of the following layers:

| Layer                 |     Description                               | 
|:---------------------:|:---------------------------------------------:| 
| Input                 | 32x32x1 Grayscale image                       | 
| Convolution 5x5       | 1x1 stride, valid padding, outputs 28x28x6    |
| RELU                  |                                               |
| Max pooling           | 2x2 stride,  outputs 14x14x6                  |
| Convolution 5x5       | 1x1 stride, valid padding, outputs 10x10x16   |
| RELU                  |                                               |
| Max pooling           | 2x2 stride, outputs 5x5x16                    |
| Flatten               | outputs 400                                   |
| Dropout               |                                               |
| Fully connected       | outputs 120                                   |
| RELU                  |                                               |
| Dropout               |                                               |
| Fully connected       | outputs 84                                    |
| RELU                  |                                               |
| Dropout               |                                               |
| Fully connected       | outputs 43                                    |
| Softmax               |                                               |

#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, Adam Optimizer is used and hyperparameters are selected as follows:

* Number of Epochs: 125
* Batch Size = 128
* Keeping Probability of the Dropout = 0.5
* Learning Rate = 0.0005
* Variables are initialized as truncated normal with mu = 0 and sigma = 0.1 

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

First of all, well known original LeNet structure is used as a model architecture. In order to prevent the overfitting problem, dropout layers are added to the original architecture before each fully connected layer. Finally, model hyperparameters are tuned by using trial and error procedure. Validation accuracy of the model is shown in the following figure with respect to the Epoch number:

![alt text][image8]

My final model results were:
* training set accuracy of 99.4%
* validation set accuracy of 96.9% 
* test set accuracy of 94.5%
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image3] ![alt text][image4] ![alt text][image5] 
![alt text][image6] ![alt text][image7]

The wild animals crossing and the right of way at the next intersection traffic signs have same triangular background which is the common background for different traffic signs and it might lead to classification difficulties.  

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction (and also shown in cell 18 in the notebook):

| Image                 |     Prediction                                | 
|:---------------------:|:---------------------------------------------:| 
| No Entry              | No Entry                                      | 
| Yield                 | Yield                                         |
| Wild Animals Crossing | Wild Animals Crossing                         |
| Right of Way          | Right of Way                                  |
| Priority Road         | Priority Road                                 |


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. This compares favorably to the accuracy on the test set of 94.5%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making top 5 softmax probabilities on my final model is located in the 21th cell of the Ipython notebook. Following figure shows the top 5 softmax probabilities of the selected traffic signs:

![alt text][image9]


