# ESPHome IRremote Hassio project
I have been trying to capture raw-data an old IR-remote sends with an IR-receiver and NodeMCU. After the capture of all the button presses, I bound a binary-sensor so when a button is pressed, it is linked to a Boolean that turn ON and of OFF after that. This binary-sensor I can link to Home Assistant to use as a trigger. 
I used a NodeMCU v3 with an IR-receiver on pin D5 to receive the codes the IR-remote is sending.
# How to capture the raw-data?
1.	Add a new node in the Home Assistant module ESPHome
2.	Compile the bin and put it on the NodeMCU (or other MCU you are using)
3.	When the node is online in ESPHome, edit the YAML
4.	Put the remote_receiver module in the YAML as described on the wiki: https://esphome.io/components/remote_receiver.html
```
remote_receiver:
  pin: D5
  dump: all
```
5.	Upload the YAML to the node
6.	When uploaded go to “Show Logs” of the node
7.	When everything is correct, you can press a button on the IR-remote and capture its raw-data. Example output when I press the MUTE-button:
[17:00:57][D][remote.raw:028]: Received Raw: -4514, 4526, -549, 1698, -549, 1697, -549, 1698, -549, 577, -546, 576, -548, 575, -548, 575, -549, 574, -549, 1699, -549, 1698, -549, 1699, -547, 576, -548, 576, -548, 575, -548, 576, -547, 575, -548, 1699, -549, 1696, -551, 1698, -548, 
[17:00:57][D][remote.raw:041]:   1698, -549, 576, -547, 578, -546, 575, -548, 577, -547, 576, -547, 575, -549, 577, -547, 575, -549, 1699, -548, 1699, -548, 1701, -546, 1698, -548
8.	As you see it is not possible for ESPHome to put all the raw-data on one string, so we need to divide it:
“-4514, 4526, -549, 1698, -549, 1697, -549, 1698, -549, 577, -546, 576, -548, 575, -548, 575, -549, 574, -549, 1699, -549, 1698, -549, 1699, -547, 576, -548, 576, -548, 575, -548, 576, -547, 575, -548, 1699, -549, 1696, -551, 1698, -548, 1698, -549, 576, -547, 578, -546, 575, -548, 577, -547, 576, -547, 575, -549, 577, -547, 575, -549, 1699, -548, 1699, -548, 1701, -546, 1698, -548”
9.	Now we got the raw-data of the MUTE-button we can assign a binary-sensor to it so Home Assistant will recognize it as an entity. Leave the logs and edit the YAML-file again
10.	Put the binary-sensor module in the file with the raw-data captured:
```
binary_sensor:
  - platform: remote_receiver
    name: "IR 1"
    raw:
      code: [-4514, 4526, -549, 1698, -549, 1697, -549, 1698, -549, 577, -546, 576, -548, 575, -548, 575, -549, 574, -549, 1699, -549, 1698, -549, 1699, -547, 576, -548, 576, -548, 575, -548, 576, -547, 575, -548, 1699, -549, 1696, -551, 1698, -548, 1698, -549, 576, -547, 578, -546, 575, -548, 577, -547, 576, -547, 575, -549, 577, -547, 575, -549, 1699, -548, 1699, -548, 1701, -546, 1698, -548]
```
11.	Save the file and upload it again
12.	Now the file is uploaded, you can go the Home Assistant and use it as an entity to trigger things. Example: I use Node-RED to use the trigger to put on my lamps and fans. 
