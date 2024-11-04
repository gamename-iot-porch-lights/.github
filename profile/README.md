
This is an iot implementation that controls lighting of my front porch. Lighting is keyed off of sunrise and sunset times. 

All of the repos in this organization are prefixed with "GIPL" for Gamename Iot Porch Lights.  Since I have lots of repos and project, this helps me identify what repo is associated with which project at a glance.

This is a hybrid implementation.  The cloud controls when the lights are turned on/off. A local esp32 (called the "GIPL-device") controls the actual LED strip.  The esp32 is an AWS IoT Thing. It communicates with AWS via MQTT topics. 

On a daily basis, an AWS EventBridge scheduler fires off the lambda "GIPL-Scheduler" function. This function does 2 things. First, it queries US Naval Observatory for sunrise and sunset times based on my longitude and latitude. Then, it schedules 2 invocations of the "GIPL-illuminator" lambda (one each for sunrise/sunset timestamps). 

Each of the 2 schedules will fire once per day at the appointed times. The "GIPL-illuminator" lambda, which is invoked by this scheduled run, sends a message to the local "GIPL-device" (esp32) specifying whether to turn on or off the light strip.  

The esp32 is dumb. It only responds to two things: switching lights on/off or running an OTA firmware deployment upgrade.  
