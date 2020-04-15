# ESPHome IRremote Hassio project
I have been trying to capture raw-data an old IR-remote sends with an IR-receiver and NodeMCU. After the capture of all the button presses, I bound a binary-sensor so when a button is pressed, it is linked to a Boolean that turn ON and of OFF after that. This binary-sensor I can link to Home Assistant to use as a trigger. 

I used a NodeMCU v3 with a IR-receiver on pin D5 to receive the codes the IR-remote is sending. 
