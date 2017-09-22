# DroneInspectionDemo
Drone - Crack Inspection Demo
(This script does not include the instructions for running on Raspberry Pi and Taking a picture on the Pi)


Scenario

This demo illustrates a simple scenario, potentially based on drones and automatically inspecting images collected from drone cameras.

It is a variation of some work I found from the Watson IOT team:
Hari Narasimhamurthy
Greg Knowles

Images are accessed from an written in node-red which calls Watson Visual Recognition which has very minimal training to detect cracks. A simple node red dashboard shows the Visual Recognition scores. The drone data and VR scores are then sent to Watson IOT which shows a map view of the drone and other Watson IOT ‘boards’ with drone and VR data 

For this demo, the images are hardcoded. These can either be stored locally if using a local instance of node red, or are available in an objectstore repository.  With simple customisation, these can be replaced

I am planning a simple extension to the demo using a Raspberry Pi with Sensehat and using the camera on th Pi to take images. This will require the hardware to show, but will be simple to do and a more complete demonstration.


Access to Working Demo
It is best to open the dashboard and Watson IOT screens together, side by side on your screen so that you can see the integration between the 2 environments

1. Access the Dashboard and working demo at https://ocodemo-2.eu-gb.mybluemix.net/ui/#/0

2. The Watson IOT Dashboard can be accessed here https://ewhce0.internetofthings.ibmcloud.com/dashboard/#/boards/d8e9e64c-9702-42a7-bb55-b43b110903d0

Seems to work best in Firefox - chrome and safari on my mac have a tendency to truncate the url

You may need to load this twice if it loses the URL after logging in

(****  IMPORTANT you will not be able to access my dashboard unless I add your IBM ID to my member list - send a note to mcgarrie@uk.ibm.com)

For completeness the Node-Red Flow is here  
https://ocodemo-2.eu-gb.mybluemix.net/ui/#/0


Creating Your Own System

You can either use Node-Red running locally or in bluemix.

I will cover in Bluemix - sim for local, although there is also the option to access images from the local file store which is what the previous version of this demo did. If so, replace the objectstore nodes with FileIn nodes and point to the specific files

In Bluemix create e.g. a node-red starter app.

The following nodes may need to be installed in Node-red
node-red-contrib-ibm-watson-iot
node-red-dashboard
node-red-node-base64


You will need the following services. Create if you don’t have already 
- Object Storage
- Watson Visual Recognition
- Internet of Things Platform (Watson IOT)

Training the VR classifier

All the Images I used to create my classifier and in the app are available here
https://ibm.box.com/s/rdobmwdyk88gf22d29s1nwe04bogjfqo

The easiest way to create a custom classifier is to use  the ‘Visual Recognition Tool’. Access this from the VR service that you have or directly from  https://visual-recognition-tooling.mybluemix.net/created, 
Make sure you associate the tool with the correct API key (if it doesn’t ask you for a key when launching the tool, this can be ‘updated’ - top right corner’)

Create a classifier . . . Give it name, e.g. CrackDetection
Create a class - ‘yescracks’. Add a zip file of positive crack images - get them from google or wherever. You can use mine
Delete the other class - we don’t need it
Add a zip file or negative images . . .harder to find on google, I created my own by photographing close up from around my home and garden
Once complete, create the classifier and wait until ‘ready’. You will require the Classifier ID in the NR Flow


Configuration Steps
ObjectStore Nodes. Create credentials. Configuring the node-red nodes, should be straightforward. Most of the configuration parameters are available in the credentials.
Load images into objectstore repository that correspond with the names in the Node-red flow

Classify Image. all that is needed is the API key. You will need to add your ClassiferID to the VR Inputs Function Node


Setting up Watson IOT

Launch the Watson IOT Dashboard
- Under Devices, Create a DeviceType e.g. DroneInspectionDevice. Take all the default
- Under devices, Add a device, e.g. drone-4, take defaults again, but make note of the credentials. You will need this to configure the Watson IOT node in NR. Optionally, create some other devices, although the current flow only uses 1 which sends data (although the demo could be easily extended to show more)

In your node-red flow, configure Watson IOT, connect as Device/Registered
Event Type = drone_event
Create Credentials: Map the Watson IOT organisation, server, DeviceType, Device and Auth Token 
Exit and check that node red is connected to Watson IOT

On Watson IOT, create some cards. Either design your own or copy mine. Best is the map, device list and drone properties.
These should be fairly self explanatory how to create and easily positioned on screen. The key is using the drone-event structure which Watson IOT should find if everything is correctly configured.

NOTE: The map works best if either you don’t allow it to use your home location when you start it, OR you change the latitude and longitude in the node red flow so that you can display a smaller area. You will find the lat and long hardcoded in the ‘Retrieve VR scores and Drone Data Function Node’. This will also show the randomiser code that makes the values change, for demo purposes.

Node-Red Flow Below

Enjoy and Extend(!)

Questions:  Douglas McGarrie, mcgarrie@uk.ibm.com

