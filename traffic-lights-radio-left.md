### @activities true
### @explicitHints true

# ERPS STEM WEEK :: Traffic Lights - Radio Controlled - Left

## Introduction
### Introduction Step @unplugged
In this challenge, we are going to use two traffic lights, with radio controls to alternate traffic light sequences. This will require two BBC micro:bits!  
The code in this tutorial will be for one traffic light, with code in the paired tutorial being for the other light.
This form of radio control will be using a 'passing a token method' of control system.  
This code is for what we will call the 'left' traffic light.

## Basic radio trigger
### Step 1
micro:bit radios talk in groups. We need to set a radio group for the two micro:bits so that they will communicate with each other. The group can be any number from 1-255 - you should use any of the numbers assigned to you. If there are several micro:bit radio pairs then each pair should use a different number.  
From the ``||radio:Radio||`` cateogry, place the ``||radio:set group||`` block into the ``||basic: on start||`` section and set the group to 1.

#### ~ tutorialhint
```blocks
radio.setGroup(1)
```

### Step 2
We want to send a radio message to the other traffic light to do something, such as start the traffic light sequence on a button press. When sending any radio messages we will send a string. The name acts as a 'token', so the reciever knows when to start the traffic light sequence.  
Add an ``||input:on button A||`` block. To send the message, from the ``||radio:Radio||`` category add a ``||radio:send string||`` block. This will let us send some text to the receiving traffic light. The string will be an instruction to "Start Sequence".

#### ~ tutorialhint
```blocks
input.onButtonPressed(Button.A, function () {
    radio.sendString("Start Sequence")
})
```

## Traffic Light sequence
### Step 3
Now let's add in some code so that the lights can do the traffic light sequence.  
Add a ``||Kitronik_STOPbit.Make Traffic Light state||`` block after ``||radio:set group||`` and set it to ``||Kitronik_STOPbit.Stop||``.

#### ~ tutorialhint
```blocks
radio.setGroup(1)
Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Stop)
```

### Step 4
We now need to be able to write the code to receive our 'token'. From the ``||radio:Radio||`` category, drag in the ``||radio:on radio receivedString||`` block - this will have a text input within the block.

#### ~ tutorialhint
```blocks
radio.onReceivedString(function (receivedString) {
})
```

### Step 5
Within the ``||radio:on radio receivedString||`` block, we need to check the received message is what we are expecting. If the message is something we know about we can handle it - we call the code that does something a handler.  
Add an ``||logic:if||`` block inside the block and check the received message is "Start Sequence". Place an ``||logic:" " = " "||`` compare block into the ``||logic:if||`` statement. From the ``||radio:on radio receivedString||``, drag ``||variables:receivedString||`` into the first text section, and then type "Start Sequence" into the comparison text box. (**Note:** Make sure the spelling and letter cases are identical to the sent message).

#### ~ tutorialhint
```blocks
radio.onReceivedString(function (receivedString) {
    if (receivedString == "Start Sequence") {
    	
    }
})
```

### Step 6
Next, create a variable called ``||variables:Start_Lights||``, and inside the ``||logic:if||`` section, ``||variables:set Start_Lights to||`` ``||logic:true||``.  
Once we have our 'token', we can set the variable ready to start.

#### ~ tutorialhint
```blocks
radio.onReceivedString(function (receivedString) {
    if (receivedString == "Start Sequence") {
        Start_Lights = true
    }
})
```

### Step 7
Now in the ``||basic:forever||`` loop, add an ``||logic:if||`` block which will be checking whether ``||variables:Start_Lights||`` ``||logic:= true||``.
#### ~ tutorialhint
```blocks
basic.forever(function () {
    if (Start_Lights == true) {
    	
    }
})
```

### Step 8
Inside this ``||logic:if||`` bracket, create the traffic light sequence. At the start of our code, we set the light to be at "Stop" already, so do not include that again.  
Our sequence will be shifted by one and goes "Get ready" **->** "Go" **->** "Ready to stop" **->** "Stop". Add this sequence, along with the required ``||basic:pauses||`` at the start and inbetween each step.

#### ~ tutorialhint
```blocks
basic.forever(function () {
    if (Start_Lights == true) {
        basic.pause(2000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.GetReady)
        basic.pause(500)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Go)
        basic.pause(2000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.ReadyToStop)
        basic.pause(1000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Stop)
    }
})
```

### Step 9
After we have finished our light sequence, we need to make sure the code does not run again until a new message has been received.  
Insert ``||variables:set Start_Lights to||`` ``||logic:false||``.

#### ~ tutorialhint
```blocks
basic.forever(function () {
    if (Start_Lights == true) {
        basic.pause(2000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.GetReady)
        basic.pause(500)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Go)
        basic.pause(2000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.ReadyToStop)
        basic.pause(1000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Stop)
        Start_Lights = false
    }
})
```

### Step 10
Currently we have two traffic lights with the traffic light sequence which will run when they receive a message. Normally when one traffic light is finished it will trigger the next one to start.  
We can do this by adding a ``||radio:send string||`` block at the end of the traffic light code, with the message "Start Sequence".

#### ~ tutorialhint
```blocks
basic.forever(function () {
    if (Start_Lights == true) {
        basic.pause(2000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.GetReady)
        basic.pause(500)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Go)
        basic.pause(2000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.ReadyToStop)
        basic.pause(1000)
        Kitronik_STOPbit.trafficLightState(Kitronik_STOPbit.LightStates.Stop)
        Start_Lights = false
        radio.sendString("Start Sequence")
    }
})
```

### Step 11
Connect your transmitter BBC micro:bit and click ``|Download|``.