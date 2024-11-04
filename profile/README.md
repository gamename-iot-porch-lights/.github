
This is an iot implementation that controls lighting of my front porch. Lighting is keyed off of sunrise and sunset times. 

This is a hybrid implementation.  The cloud controls when the lights are turned on/off. A local esp32 controls the actual LED strip.  The esp32 is an AWS iot thing. It is mqtt attached to AWS. 

On a daily basis, an AWS EventBridge scheduler fires off a lambda function which queries the US naval observatory for sunrise and sunset times for my longitude and latitude. Once those timestamps are obtained, there are two new lambda-based jobs scheduled; one each for sunrise and sunset. 

The two jobs fire once per day at the appointed times. The lambda, which is invoked by this scheduled run, sends a message to the local esp32 specifying whether to turn on or off the light strip.  

The esp32 is dumb. It only responds to two things: turning on/off lights or running an OTA firmware deployment upgrade.  




