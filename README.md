# Deploy-Edge-Impulse-Object-Detection-Model-on-ESPCAM
This project demonstrates how to deploy an object (egg) detection model trained using Edge Impulse on an ESPCAM module.

Edge Impulse simplifies the process of creating machine learning models for edge devices. This project focuses on deploying an object detection model to detect eggs using the ESPCAM.

## Requirements

### Hardware
- ESP32-CAM module
- FTDI programmer (or similar) for flashing firmware
- USB cable
- Breadboard and jumper wires (optional for wiring setup)

- ### Software
- Edge Impulse Studio account
- Arduino IDE (with ESP32 board package installed)
- Required libraries: `ESP32`, `Edge Impulse Inferencing SDK`

 ## Project Setup

### 1. collect images 
3. take suffucion amount of images with deffrent angles of your object (in my case egg) with your phone or with your ESP-CAM for more efficient result
    ## to take a pic with esp-cam follow this steps:
      ### . Hardware Setup
1. Connect FTDI to ESP32-CAM:
   - **GND → GND**, **5V → 5V**, **RX → TX**, **TX → RX**
   - **IO0 → GND** (for flashing)
2. Insert a formatted microSD card (optional for saving images).

      ### . Install ESP32 Board
1. Open Arduino IDE, go to `File > Preferences`.
2. Add this URL in "Additional Board Manager URLs":  
   ```
   https://dl.espressif.com/dl/package_esp32_index.json
   ```
3. Go to `Tools > Board > Boards Manager`, search "ESP32," and install.

      ### . Upload Code
1. Open `File > Examples > ESP32 > Camera > CameraWebServer`.
2. Update:
   - **Wi-Fi Credentials**:
     ```cpp
     const char* ssid = "YourWiFiSSID";
     const char* password = "YourWiFiPassword";
     ```
   - Uncomment `#define CAMERA_MODEL_AI_THINKER`.
3. Select:
   - **Board**: `AI Thinker ESP32-CAM`
   - **Port**: Correct COM port.
    ![image](https://github.com/user-attachments/assets/dc599413-6bca-4809-bc93-be0329f8419e)

4. Press **Reset Button** on the ESP32-CAM.
5. Click **Upload** in Arduino IDE.
   
      ### . Run the Web Server
1. Disconnect **IO0 → GND**, then reset ESP32-CAM.
2. Open the Serial Monitor (`115200` baud) to get the IP address.
3. Enter the IP address in a browser to access the camera interface.
![image](https://github.com/user-attachments/assets/fdf90e7b-0b6d-4f51-9b6f-f8f0803d9fc4)
4. take pictures of your object and save them

### 2. train the maodel
1- go to https://studio.edgeimpulse.com/signup  and creat an acout 
-  go to data acqusition and upload the data  :
![image](https://github.com/user-attachments/assets/417eb6c0-2cb7-4f62-bbd3-9d3291f39340)
- it would be split by default to 80% training and 20% for testing 

2- label the data : go to the labeling and draw a box on the object you want to detect on all the images and name the label on what the object is (egg in my case)
3- configure your model: go to creat impulse and configure your model 
![image](https://github.com/user-attachments/assets/6f6fd528-1b98-4df1-9582-39828b2e63cb)
4- create features :  where the images are transformed into a set of meaningful numerical representations. These features are used as inputs to train machine learning models.
- go to images and click create feature 
![image](https://github.com/user-attachments/assets/9f7fc942-213b-429e-98fb-df1a71cf4648)
![image](https://github.com/user-attachments/assets/89c3b068-a80f-46cd-bb7d-0106a1897533)

5- set up the neural network :  the model for object detection in edge impulse is FOMO (Faster Objects, More Objects) MobileNetV2 0.35 , set up the Number of training cycles and train the model
![image](https://github.com/user-attachments/assets/5bc5636b-107c-4094-93dd-d45b1eec655e)

6- deploy the model : go to deployment and download the arduino library, it would be downlad as zip file
![image](https://github.com/user-attachments/assets/4eb00c8b-9523-4598-a99d-c6d43e969515)

### 4. Deploy the Model
1. Include the exported Edge Impulse library in your Arduino project.
- go to sketch - inculde library - manage library - add zip - then add the zip file you uplad from the edge impulse 
![image](https://github.com/user-attachments/assets/461ef573-679f-401b-9b0f-a36731c456d5)
2. go to exemples and choose the library you include - esp32 - camera
  ![image](https://github.com/user-attachments/assets/244a7790-050b-4493-bfdb-c083f2e68b96)
3. in our case we are using ESP-CAM
  uncomment CAMERA_MODEL_AI_THINKER
  ![image](https://github.com/user-attachments/assets/02383b66-e764-4e6f-9026-4e2051b379e0)
4. upload to your ESP-CAM
  ![IMG_20241213_202820 1](https://github.com/user-attachments/assets/49978039-f9ac-4909-b167-ba27e953abb4)
5. take off the **IO0 → GND** and open the terminal and observe the result
  ![image](https://github.com/user-attachments/assets/fe496b5a-c064-483f-bf5d-96a62bae0c61)

