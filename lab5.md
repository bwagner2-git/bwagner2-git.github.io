<!-- # ECE 5960 -->
<style>
.topnav {
  background-color: #333;
  overflow: hidden;
}

/* Style the links inside the navigation bar */
.topnav a {
  float: left;
  color: #f2f2f2;
  text-align: center;
  padding: 10px 7px;
  text-decoration: none;
  font-size: 15px;
}

/* Change the color of links on hover */
.topnav a:hover {
  background-color: #ddd;
  color: black;
}

/* Add a color to the active/current link */
.topnav a.active {
  background-color: #1FD2D5;
  color: white;
}
</style>

<div class="topnav">
  <ul>
  <a href="/">Home</a>
  <a href="/lab1"> Lab 1 </a>
  <a href="/lab2">Lab 2</a>
  <a href="/lab3"> Lab 3</a>
  <a href="/lab4">Lab 4</a>
  <a class="active" href="/lab5">Lab 5</a>
  <a href="/lab6">Lab 6</a>
  <a href="/lab7">Lab 7</a>
  <a href="/lab8">Lab 8</a>
  <a href="/lab9">Lab 9</a>
  <a href="/lab10">Lab 10</a>
  <a href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a href="/lab13">Lab 13</a>
  </ul>
</div>

## Lab 5

### Pre-lab
By reading the datasheet for the motor driver, we see that we are supposed to hook up the red cable of the battery to Vin and the ground of the battery to GND. We also short the ground of the Artemis to the ground of the motor drivers. This common ground is important because we will be sending PWM signals from the Artemis to the motor controller so they need to have the same reference voltage. Each motor driver has two sets of inputs and two sets of outputs. We couple these in parallel so that we can provide a sufficient amount of current to make our robot fast. To parallel couple we short AIN1 and BIN1 and feed them a PWM signal, and we short AIN2 and BIN2 and feed them the other PWM signal. One PWM signal drives the motor in one direction while the other drives it in the other direction. We then short BOUT1 and AOUT1 together and BOUT2 and AOUT2 together and feed one of them to the black wire of one of the motors and the other to the red wire of the same motor. One chip controls one motor and each chip receives 2 PWM signals, one for each direction as previously mentioned.
Because of this, we need 4 PWM signals coming from the Artemis. I chose to send the PWM signals from pins 1, 3, 14, and 16 because these pins were all PWM enabled.
Below is a picture displaying how I hooked up my motor drivers to the Artemis and to the motors.
<br>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/blob/main/screenshots/lab5/Screen%20Shot%202022-03-07%20at%2011.06.04%20PM.png" height="800"/>
<br>
I put all of my motor drivers in the battery component and sealed it with duck tape hoping that I will rarely have to open it again for repairs. This will make fixing any of the wiring for the sensors much easier as I am not dealing with the clutter from the wires for the motor drivers. I stuck with the standard color coting of red being the positive terminal and black being ground. I made the PWM wires blue.

### 1.
Based on a discussion I had with one of the TA's I soldered my motor drivers directly in the car and thus did not test them with an external power source. The Artemis Nano is a 3.3V device and thus when generating a PWM signal using the function generator to test the motors 3.3V should be considered high. Additionally, the batteries we are using are 3.7V so it would make sense to set the external power source to 3.7V as well.

### 2.
To demonstrate control of the PWM I wrote the following below and hooked it up to the oscilloscope. This produced the results in the video below.

```
void setup() {
   pinMode(1,OUTPUT);
   analogWrite(1,50);
   delay(10000);
   analogWrite(1,100);
}
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/SjTUtLHeQDk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 3.
The picture below shows I have successfully taken apart my car. 
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab5/20220307_232130.jpg" height="800"/>

### 4.
Below you can see that I am successfully able to use analog write and a set of PWM signals to drive a motor in both directions.
<iframe width="560" height="315" src="https://www.youtube.com/embed/n2XKL7TtEQs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
To do this I wrote a simple script that sent a pwm signal along one of the lines to the motor driver, waited, and then zeroed the first PWM signal and sent another PWM signal along the other line to the same motor driver causing it to reverse its direction. As you can see in the video, the motors are also being powered by an external battery.

### 5. 

I found that When a PWM signal of 30 out of 255 is given to the left and a pwm signal of 40 out of 255 is given to the right, the robot is just barely able to move itself. Left and right refer to the robot having the black side up and the side originally with the lights being designated as the front.
<br>

To make the robot go straight I used the following in setup
```
   pinMode(1,OUTPUT);
   pinMode(14,OUTPUT);
   analogWrite(14,40);//right
   analogWrite(1,65);//left
 ```
 The results of this are shown below
 
<iframe width="560" height="315" src="https://www.youtube.com/embed/JVFt6z8-Al4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
The tape is 8ft long and the requirement was to go 6ft still over the tape.

<br>
The video below shows untethered open loop control of the robot. In the video, the robot goes approximately straight, turns left, turns back right, and then stops and spins in both directions.
<iframe width="560" height="315" src="https://www.youtube.com/embed/FfuhH2_QdEk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
I used the following code to accomplish this. As you can see each maneuver is executed for approximately 2 seconds.
```
void setup(){
//set up the pins
   //initialize it to go straight
   analogWrite(14,40);//right 
   analogWrite(1,65);//left
   
}
//black side up and foward is side with lights
//16 and 14 are right 14 is forward 16 is backward
//1 and 3 are left 



void loop() {
  long startTime=millis();
  long endTime;
  while(1){
    endTime=millis();
    if (endTime-startTime>10000){
      analogWrite(14,0);
      analogWrite(1,0);
      analogWrite(16,0);
      analogWrite(3,0);
      
    }
    if (endTime-startTime>2000 && endTime-startTime<4000){
      analogWrite(14,80);
      analogWrite(1,50);
    }
    if (endTime-startTime>4000 && endTime-startTime<6000){
      analogWrite(14,50);
      analogWrite(1,120);
    }
    if (endTime-startTime>6000 && endTime-startTime<8000){
      analogWrite(1,0);
      analogWrite(3,150);
      analogWrite(14,150);
    }
    if (endTime-startTime>8000 && endTime-startTime<10000){
      analogWrite(14,0);
      analogWrite(1,150);
      analogWrite(3,0);
      analogWrite(16,150);
    }
  }
```



### 5960 Extra Questions
1. The motors do not respond very quickly to changes in the signal. The analog write generates a PWM signal that acts as a sort of pseudo analog signal for the motor, and a higher frequency PWM signal is not going to provide any benefits as the steps are already unoticeable. The motor is an sense acts as a sort of low pass filter. 
2. 

