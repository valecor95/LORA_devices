# LoRa Devices

In this section, you will submit an experiment with one LoRa node on IoT-LAB, flash a RIOT firmware for LoRaWAN and connect the node to your application in the TTN network.

Connect to the Saclay site host:

      ssh username@saclay.iot-lab.info

Start an experiment with 1 node called riot_ttn

      iotlab-auth -u <login> 
      iotlab-experiment submit -n riot_ttn -d 60 -l 1,archi=st-lrwan1:sx1276+site=saclay

Remember the experiment identifier returned by the last command. It’ll be used in the commands shown below, <exp_id>. The requested experiment duration is 60 minutes. Wait a moment until the experiment is launched (state is Running) and get the nodes list. For the next steps of this tutorial we suppose that you obtained st-lrwan1-1.saclay.iot-lab.info

      iotlab-experiment get -i <exp_id> -s
      iotlab-experiment get -i <exp_id> -r

Note: you can also use the RIOT development code (e.g the master branch) but this will be at your own risk: this tutorial may not fully work. Important: RIOT doesn’t support the arm gcc version installed by default on the SSH frontend, e.g. 4.9. So we provide arm gcc 7.2 in /opt/gcc-arm-none-eabi-7-2017-q4-major. Use the following command to use gcc 7.2 by default:

      export PATH=/opt/gcc-arm-none-eabi-7-2018-q2-update/bin:$PATH

Use the CLI-Tools to flash the ST LoRa node with the LoRaWAN firmware that you have just built. Here we use st-lrwan1-1 but it may change in your case:

      iotlab-node --update tests/LORA_devices/bin/b-l072z-lrwan1/tests_LORA_devices.elf -l saclay,st-lrwan1,11

You can now access the RIOT shell running on your node using netcat:

      nc st-lrwan1-1 20000

Now find the Device EUI, Application EUI and Application key information in the Overview tab of the iotlab-nodedevice on the TTN web console.Then set them to the RIOT firmware (replace the values with yours):

      loramac set deveui 0000000000000000
      loramac set appeui 0000000000000000
      loramac set appkey 00000000000000000000000000000000

You can also set a fast datarate, e.g. 5, corresponding to a bandwidth of 125kHz and a spreading factor of 7, since the nodes are very close to the gateway:

      loramac set dr 5

Now that the node has the required information set, it is time to join it to the network using the OTAA

      loramac join otaa
