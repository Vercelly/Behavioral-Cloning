Behavioral-Cloning-P3-USDC
In this project we train our computer to drive like us using simulator provided by Udacity Windows which helps us generate images through driving it! These images are then feeded into model and the simulator is used to simulate our model.

Requirements
Keras
OpenCV
Numpy
Pandas
Sklearn
Model Training
I first generated images using the training mode inside the simulator and drove the car using PS3 Joystick. It was difficult to drive using Keyboard since the sensitivity of angles are important for the project and for this reason , PS3 Controller using Better DS3 was used for training of Data. Model was trained only for first track (Not the jungle track).

The Images obtained by training
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/c17b52d9-e7ba-4198-b419-e6f0a6c47878)


Techniques and Augmentation
It was important to realize what i required for the model to train. Scenery doesn't affect the way the car drives in long shot but the instantaneous decision based on the road. To draw an analogy we tell our car that don't try to look beyond the horizon.We also normalize like the way we've done in all our previous projects!
So the techniques used here are ,

Cropping : The images which we recieve through training are basically a full sized image which includes scenery etc , So as mentioned before we are telling our car "don't look beyond the horizon" so we crop our image from the top and sides to focus on the roads and not get influenced by others. After cropping the Image
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/9e45eb3e-204a-4450-91b6-2772514012af)

Resize image :- Depending upon our model we resize image such that we have easier training with lesser time training the model. a. For VGG Model - 224 , 49 b. For NVIDIA model - 200 , 66

Brightness :- We want our model to make the most of the data which are available but we need to keep in mind that we can't feed the same images and train it since that would mean it could overfit , Secondly we need to realize that our simulator has constant lightening and hence , Inspired by this post. After Brightness HSV
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/e3ff2f5b-f1b2-4a83-8b3b-4644b3ec1c8b)
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/b1c7a958-2b49-4b9c-abf3-2f894767b942)

Flipping :- We must realize that in our track we have a lot of left turn in comparision to the right turns. So we use Horizontal flipping , Horizontal flipping enables us to have more data and few unique data!. The basic idea is to create a mirror image and reverse the angles. "angle = angle*-1.

Model
In this case , I have tried two architectures and both code along with their output's are present in the repository. The first model is going to be a modified End to End Learning for Self Driving Car and the second model is going to be a modified VGG. I tried both the models and VGG outperforms NVIDIA model. But this could be due to the fact that NVIDIA model needs more training as it fails only at one particular curve from which it recovers and drives fairly right. On the other hand VGG model works perfectly for the track1 and fails at the track 2.

Model Architecture NVIDIA
There are few changes made on the model's end as opposed to the original NVIDIA end to end architecture.

The summary of the architecture i used is this :- Architecture used for the project
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/d2021331-f6ff-4c08-8a23-6cc2e8d22d60)

As opposed to the original architecture which follows NVIDIA End to End for Self Driving Car
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/072b86b8-a47e-4259-97de-5a88d5623bf8)

Here , I've used the Lambda layer to Normalize the image and feed through the network. The model was trained for 3 epochs with batch size being 128. Adam optimizer was used for the model with Mean Squared Error to check for loss.

Model Architecture VGG16 based.
The idea was to build a model which could run in the lowest possible epoch due to machine constraint. I used VGG16 Architecture at first but it was really slow and the problem is more of what degree to steer instead of what direction to steer , So few changes were to be made. For this reason , I used 4 Layer convolutional network with relu along with 3 Fully connected layer's as my final output.I used normalized image and trained the data again on 3 epochs with batch size being 128 , loss function of mean squared error. Here's the architecture i used ,

Architecture used for the Project
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/9dd52008-eb35-411e-ae4c-5935ccc2d724)
The original VGG16 Architecture
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/249c735e-bb06-4f26-ab76-c64e7220e21b)

VGG16

I first had the idea to use Dropout after every layer but it was making the training slow and was pretty unnecessary as the Dropout at the end of Convolutions did the trick.

Training
I trained the model on my computer with NVIDA 940MX 4GB GPU along with I5 7200U processor. I trained the model for 3 epochs which perfectly did the trick for the first track. With 10% Validation set.

Running the Model
The model took around 15 minutes to train. There were changes which were to be made inside the drive.py file so that it doesn't throw index out of bounds error.

To run the model in NVIDIA Architecture -
python modelnv.py

Which would result in generation of h5 file which we are going to use for running.

python drive1.py modelnv.h5

To run the model in VGG inspired Architecture -
python model.py python drive.py model.h5

Video of the car driving
![image](https://github.com/Vercelly/Behavioral-Cloning/assets/161895992/22ca2f68-3544-4b23-993b-08ca39ea99c3)

Using VGG at Speed 17 Running of the car at 17kmpl The model performed better at Slower speed using VGG best being achieved at speed limit set as 12 in the drive.py file.

Unfortunately NVIDIA model doesn't make the cut as it leaves the circuit and then returns back at one particular curve (U-Turn). But i happen to infer that maybe a more epoch could do the trick , Which i leave it for future.

How to improve the output ?
As stated earlier , I have trained the model using 3 epochs which in it's own way is less and would require atleast 5-6 to be a completely accurate model. On the other hand , It needs more data apart from the desert data. I feel that augmentation done allows the model to train precisely on the basis of the known curves that being said , Newer set of data could always improve the model in this case.

Future Extension
The simulator doesn't actually have any other car's or obstruction apart from turns and steering a car in real life also depends upon the car's going at the front. Hence maybe trying to generate data using car games and running it sounds like a good option. Also , Throttle and Speed are predefined hence , We actually don't do anything to it , Maybe playing around with it could help example - In case of NVIDA architecture there were a lot of late turns leading to drifting of car.
