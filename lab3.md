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
  <a class="active" href="/lab3"> Lab 3</a>
  <a href="/lab4">Lab 4</a>
  <a href="/lab5">Lab 5</a>
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

## Lab 3

Below is my circuit diagram showing how I hooked up the three sensors so that they could communicate to the Artemis using I2C. I am choosing to set the shutdown pin one on of the TOF sensors, change the address of the other, and then re-enable the TOF sensor instead of shuting one down before every transaction. I do all of this in setup so that it is only run once. Here is the pseudo code:
```
declare sensor 1;
declare sensor 2;
pull shutdown pin on sensor 1 low;
change I2C address of sensor 2;
set shutdown pin on sensor 1 high; // re-enabling it
interact with sensor 1;
interact with sensor 2;
```
The benefit of doing it this way is that I use less wires, because I would need to have a shutdown wire going to sensor 2 if not. This implementation is also more efficient because I only need to spend time setting a shutdown pin once at the beginning and do not need to worry about it any more. Toggling between the two sensors shutting one down and reading from the other, would likely be more memory efficient as I would likely only need one sensor object in my program.
I will place one TOF sensor on the front of the robot and one on the side. It seems that I might miss objects that are in between these two sensors field of views as well as on my blind side and in the back.

<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/circuit%20diagram.png" height="800"/>


### part 3a Time of Flight Sensors
1. I experienced the same common error when trying to read the I2C address of the time of flight sensor using the example script. The TA instructed me to paste a copy of my results showing the bug which was the common one experienced in the class. 
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/I2C%20address%20bug.png" height="800"/>
2. Looking at the data sheet shows that the benefit of using the short range mode on the time of flight sensor over the long range mode is "better ambient immunity". This means that at shorter ranges, the sensor will be less sensitive to impoerfections and inconsistencies in the environment. The benefit of using the long range mode is obviously that you can see further and the drawback is that the sensor will have more error in certain environments. Because we are operating our cars in the fairly controlled enviroment of the lab, it will likely make sense to use the long range mode so that we can see further out. The medium mode obviously experiences both the pros and cons of each mode to a less extreme degree.
3. To determine the ranging time of the sensor I utilized the millis() function which returns the number of milliseconds since the system was started. Since this value is always positive and can get quite big, I defined two unsigned long variables to store a start time and an end time. To determine the ranging time I set start time right before the sensor started ranging, and set end time right after the sensor actually got data. The ranging time was 3ms. Some pseudo-code and serial monitor results are shown below.
```
unsigned long startTime;
unsigned long endTime;
startTime=millis();
start sensor ranging;
///other stuff
sensor.getDistance;
endTime=millis();
sensor.clearInterupt;
sensor.stopRanging;
print endTime-startTime;
```
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/ranging%20time.png" height="800"/>

To measure reliability of the sensor, I made some modifications to the code so that it would calculate the squared standard deviation of the first one hundred measurements that distance sensor one took and print them out to the screen. To do this I defined the following global variables.
```
//test reliability of the sensor
float reliability[100];
int times=0;
float total=0;
float sumOfs=0;
```
I then added the following to the loop function.
```
  //test reliability of the sensor
  if (times<100){
    reliability[times]=distanceOne;
    times+=1;
    total+=distanceOne;
  } else{
    int p;
    float average=total/100;
    for(p=0;p<100;p++){
      sumOfs+=(reliability[p]-average)*(reliability[p]-average);    
    }
    Serial.print("standard deviation squared is ");
    Serial.println(sumOfs/100);
    delay(10000);
  }
```
I got the following at close range.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/Screen%20Shot%202022-02-21%20at%2012.51.49%20PM.png" height="800"/>

I got the following at a further range.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/Screen%20Shot%202022-02-21%20at%2012.53.06%20PM.png"/>

As we would intuitively expect the sensor seems to get less reliable at farther ranges.



The video below shows that I successfully hooked up and read from the 2 ToF sensors.
<iframe width="560" height="315" src="https://www.youtube.com/embed/uF7pzUjcBoQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### 5690 Questions
1. There are many types of IR sensors. 2 prevalent types are Amplitude based IR sensors and IR Time of Flight sensors. Amplitude based IR sensors work by having a transmitter send out infared light and then you have a receiver measure amplitude of the reflected light which will change with the distance that the light was reflected from. Time of Flight sensors also send out an infared signal and they measure the time it takes to see the reflection which gives you information about how far away the surface the light reflected off of is. These sensors do not technically measure the time between sending and receiving the signal, but instead they measure the phase difference between the sent signal and the reflected signal which is coorelated to the time.
##### Amplitude Based IR Sensors
###### Pros
* they are really cheap and have a small form factor
###### Cons
* affected by reflectivity of target
* does not work in settings with high amounts of ambient light
##### IR Time of Flight
###### Pros
* small form factor
* Not sensitive to target color, texture, or ambient light
###### Cons
* relatively expensive
* complicated processing
* they have a low sampling frequency (typically about 7-30Hz)

2. According to the documentation (https://cdn.sparkfun.com/assets/e/1/8/4/e/VL53L1X_API.pdf), the .setIntermeasurementPeriod(); function is used affects the sensor's delay between two ranging operations. The .setTimingBudgetInMs(); function is the time that the sensor is allowed to perform one ranging operation.
The functions that were provided however, do not work with this specific sensor as it is not the official Sparkfun library sensor. The screenshot below demonstrates that I tried this exercise. I also tried to experiment with this command VL53L1_SetInterMeasurementPeriodMilliSeconds(&myICM,1000 ); that I found in the aforementioned manual, but it did not work either. For now I am just going to stick with the default values, but it will be nice to know that there might be a way to change these settings moving forward and I can ask the TAs for help on how to accomplish this. Given that the TOF sensor is fairly slow, it seems that as we are in a fast robotics course and we are not extremely power constrained, that we might want to decrease the intermeasurement period and try to ramp up our sampling rate.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/Screen%20Shot%202022-02-21%20at%2012.20.33%20PM.png"/>

3. When I rapidly moved the sensor I received some measurements that were flagged as invalid. This is shown below. This might affect our robot when it is moving really fast. In future labs when we are dealing with measurements at high speeds, it might be wise to look at this signal and sigma and throw out invalid measurements.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/Screen%20Shot%202022-02-21%20at%203.14.37%20PM.png
" height=800/>

### part 3b
1. I experienced the same common error when trying to read the I2C address of the time of flight sensor using the example script. The TA instructed me to paste a copy of my results showing the bug which was the common one experienced in the class. 
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/I2C%20address%20bug.png" height="800"/>


<iframe width="560" height="315" src="https://www.youtube.com/embed/PWmsGQvqPLI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>






