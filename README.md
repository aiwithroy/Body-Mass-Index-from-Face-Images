# Project Title: Body Mass Index from Face Images
### Technologies: Deep Computer Vision
### Domain: Healthcare

## Problem Statement:
Building a model to predict the BMI (body mass index) of a person from an image of their
face. This project borrows from another project made to classify the age and gender of
a person using the input of their face, including the weights of a trained model and a
script used to dynamically detect a user’s face with their webcam. In addition to being
an interesting machine learning problem, predicting BMI in this way could be a useful
tool in medical diagnostics.

### The main objective here is -
1. Building a model to predict the BMI (body mass index) of a person from an image of their
face.
2. Face Detection is also required.
3. Maintaining a database to store each and every data.
   
### USE As Marketing Strategy
Gyms, Fitness Trainers and Fitness Apps can use this as their marketing strategy. On Social Media Platforms, they can prompt the potential customers to check their BMI by just uploading their image and if the BMI is not in the healthy range, they can alert the customer about the problems they might have to face due to excess weight and tell them to join their Gym or take their app subscription so that they can live a healthy lifestyle!

## HOW TO RUN THE WEB APP
Just run all the cells of Body-Mass-Index-from-Face-Images.ipynb. This will make your back end ready and also links it to the front end web app hosted on anvil. Web App Link : https://facetobmi.anvil.app/

### Presentation Link
Please Find PPT Here :
# Face detection and BMI/Age/Sex prediction

The model provides end-to-end capability of detecting faces and predicting the BMI, Age and Gender for each person.

The architecture of the model is described as below:

![](https://github.com/aiwithroy/Body-Mass-Index-from-Face-Images/blob/master/facetobmi/img/model_structure.jpg)


## Face detection

Face detection is done by `MTCNN`, which is able to detect multiple faces within an image and draw the bounding box for each faces.  

It serves two purposes for this project:

### 1) preprocess and align the facial features of image.

Prior model training, each image is preprocessed by `MTCNN` to extract faces and crop images to focus on the facial part. The cropped images are saved and used to train the model in later part.

Illustration of face alignment:
![](https://github.com/aiwithroy/Body-Mass-Index-from-Face-Images/blob/master/facetobmi/img/mtcnn_face_alignment.jpg)

### 2) enable prediction for multiple persons in the same image.

In inference phase, faces will be detected from the input image. For each face, it will go through the same preprocssing and make the predictions.

Illustration of ability to predict for multiple faces:

![](https://github.com/aiwithroy/Body-Mass-Index-from-Face-Images/blob/master/facetobmi/img/detect_predict_multi_faces.png)

## Multi-task prediction

In vanilla CNN architecture, convolution blocks are followed by the dense layers to make output the prediction. In a naive implementation, we can build 3 models to predict BMI, age and gender individually. However, there is a strong drawback that 3 models are required to be trained and serialized separately, which drastically increases the maintenance efforts.

|   |
|---|
|`[input image] => [VGG16] => [dense layers] => [BMI]`|
|`[input image] => [VGG16] => [dense layers] => [AGE]`|
|`[input image] => [VGG16] => [dense layers] => [SEX]`|

Since we are going to predict `BMI`, `Age`, `Sex` from the same image, we can share the same backbone for the three different prediction heads and hence only one model will be maintained.

|    |
|----|
|`[input image] => [VGG16] => [separate dense layers] x3 => weighted([BMI], [AGE], [SEX])`|

This is the most simplified multi-task learning structure, which assumed independent tasks and hence separate dense layers were used for each head. Other research such as `Deep Relationship Networks`, used `matrix priors` to model the relationship between tasks.

![](https://ruder.io/content/images/2017/05/relationship_networks.png)
_A Deep Relationship Network with shared convolutional and task-specific fully connected layers with matrix priors (Long and Wang, 2015)._

## Reference

 * MTCNN: [https://github.com/ipazc/mtcnn](https://github.com/ipazc/mtcnn)
 * VGGFace: [https://github.com/rcmalli/keras-vggface](https://github.com/rcmalli/keras-vggface)
