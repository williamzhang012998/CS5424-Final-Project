# CS5424-Final-Project
# InSafe: Auto-alarm band, Your Safety Companion
## 1. Introduction

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/logo1.png)
![logo2](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/logo2.png)

**This auto-alarm band is a wearable device to protect the user from emergency situations. It is connected with an accelerometer and a camera. The two sides of the device are parallel to each other. When the user is encountering dangerous circumstances, for instance, robbery, the user raises arms, and the sensors will be perpendicular to the ground. The camera will start facial recognition and taking pictures of the robber. Simultaneously when the accelerometer start sensing that the arm is raised perpendicularly to the ground, it will first trigger voice recognition. When the user mentions the “money”, “purse”, “police”, and “shoot” the device will trigger an emergency call at the end.**

## 2. Concept

### a. State Diagram

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/state%20diagram.jpg)

### b. Verplank Diagrams

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/verplank%20diagram.jpg)

## 3. Building the System

This project uses a Raspberry Pi Camera to stream video, a MPU6050 accelerometer and a microphone. 

### a. Hardware Design

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/wearable0.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/wearable.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/wearable1.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/wearable2.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/wearable3.jpg)

**tesing the reading of the accelerometer to find a pattern when it is placed perpendicular VS not perpendicular**

When the accelerometer is placed perpendicular (in real scenarios, the user raises arms), the webserver shows that **9 < x < 11**.

Creating the webserver for the controller on a remote device:
```
pi@ixe00:~/$ python server.py
 * Serving Flask app "server" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 162-573-883
```

To run the app to see the accelerometer pattern remotely:

`(woz) pi@yourHostname:~/Interactive-Lab-Hub/Final-Project $ python app.py`

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer0.jpg)

Voice recognition triggered by placing the accelerometer perpendicular. The device will start recording speech and recognize "money", "purse", "police", and "shoot". When the user mentions these words, the screen will display an image of "calling 911".

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer1.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer2.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer3.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer4.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer5.jpg)
![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/accelerometer6.jpg)

### b. How to run Program

Before running the program, make sure to configure the raspberry pi camera on your device.

SSH into your pi and run

```
sudo raspi-config
```

Select `Interface Options`, then `Pi Camera` and enable it. Press `Finish` and reboot.

### Installing Dependencies

This project uses openCV for object detection. Install the corresponding openCV version for your python version. The python version used for this project is 3. The installation should take a few hours only if you are lucky.

Next install the dependencies for the project

```
pip install -r requirements.txt
```

Next to get emails alerts, edit the `mail.py` file.

```
nano mail.py
```

Edit the section where it asks you the email you want to send the update from and email you want to send the update to

```
# Email you want to send the update from (only works with gmail)
fromEmail = 'InSafe@gmail.com'
fromEmailPassword = 'IDDisthebest123'

# Email you want to send the update to
toEmail = 'anotheremail@gmail.com'
```

The `mail.py` file uses a gmail SMTP server and sends an email with an picture of the object detected by the camera. 

Some other modifications you can make are in `main.py`


```
email_update_interval = 600 # sends an email only once in this time interval
video_camera = VideoCamera(flip=True) # creates a camera object, flip vertically
object_classifier = cv2.CascadeClassifier("models/fullbody_recognition_model.xml") # an opencv classifier
```

Here you can choose different types of objects you want to detect by switching for example from `"models/fullbody_recognition_model.xml"` to `"models/facial_recognition_model.xml"`

There are 3 models available in the models directory.

```
facial_recognition_model.xml
fullbody_recognition_model.xml
upperbody_recognition_model.xml
```

## Running the Project

To run the program

```
python main.py
```
To view live stream of camera, go to `<raspberrypi_ip>:5000` in your browser.

## 3. Prototype


## 4. Reflection


## 5. References
