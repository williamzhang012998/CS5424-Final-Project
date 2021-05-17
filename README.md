# CS5424-Final-Project
# InSafe: Auto-alarm band, Your Safety Companion
## 1. Introduction

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/logo1.png)

**InSafe: This auto-alarm band is a wearable device to protect the user from emergency situations. It is connected with an accelerometer and a camera. The two sides of the device are parallel to each other. When the user is encountering dangerous circumstances, for instance, robbery, the user raises arms, and the sensors will be perpendicular to the ground. The camera will start facial recognition and taking pictures of the robber. Simultaneously when the accelerometer start sensing that the arm is raised perpendicularly to the ground, it will first trigger voice recognition. When the user mentions the “money”, “purse”, “police”, and “shoot” the device will trigger an emergency call at the end.**

## 2. Concept

### a. State Diagram

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/state%20diagram.jpg)

### b. Verplank Diagrams

![logo1](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/verplank%20diagram.jpg)

## 3. Building the System

This project uses a Raspberry Pi Camera to stream video, a MPU6050 accelerometer and a microphone. 

### a. Hardware Design

When we design the band, we try to make the two sides of the device that are connected to the camera parallel to each other, so when the user raises arms, the camera will start taking picture of the robber's face, and the sensor will have voice recognition triggered by sensing that the device is perpendicular to the ground.

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

https://drive.google.com/file/d/1OOAwmOg_YR7x5JOqOMqGnst5wNP6gMo3/view?usp=sharing

## 4. Reflection

**Takeaways:**
Initially, we try to make something portable to solve some real pain point. The integration of of the camera and the accelerometer was taking consideration of the hand movement when people face dangerous situations, like robbery, they will need to raise their arms. So we make the natural movement as our interaction to trigger the sensors on the device.

The idea of deploying the accelerometer to trigger voice recognition was inspired by the healthcare tracking device Jeanne made in Lab 3. In Lab 3, "The most amazing part of the system is that the the Speech2Text could recognize speech input and print it out. The app.py in the demo also inspired me that I could integrate the accelerometer with the speech recognition function. The accelerometer could sense the angle of the device and visualize it via x, y, z. So I could harness the accelerometer to trigger Speech2Text. Therefore, I think of a wearable device that could recognize and analyze voice when one raises his or her arm and when the device is placed flat." However, the Sppech2Text model didn't work effectively as expected in Lab 3.Tthe texts it printed out are not identical with what was being recognized, which makes it super difficult to trigger the next step -- presenting corresponding images on the TFT screen. In this project, we fixed the model, and the voice recognition workes pretty well.

**Limitations:**
Jeanne is responsible for prototype design and developing the accelorometer functionality with Speech2Text. And William is responsible for developing the camera model and merge the two models into one. And we work together on other documentation parts. Since we are working remotely, when we integrate the sensors to the 3D-printing prototype, sometimes it was hard to test the functionalities on the device.

Further, the ultimate goal of the smart alert band is to not only record the robber's face but also call emergency. But at this stage, we don't know how to connect the device to a real phone and make real phone call, so we could just use the voice recognition to trigger the screen display. However, we see great potential in the further development of connecting the device to make a real emergency call. Such a mechanism would also work for other emergency situations, for instance, when an elderly adult slips down.

Future implementation - Connect to the phone app or just making a phone call:

![logo2](https://github.com/williamzhang012998/CS5424-Final-Project/blob/main/logo2.png)

## 5. References

https://www.hackster.io/hackershack/smart-security-camera-90d7bd
