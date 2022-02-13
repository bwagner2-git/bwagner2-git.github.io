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




### part 3b
1. I experienced the same common error when trying to read the I2C address of the time of flight sensor using the example script. The TA instructed me to paste a copy of my results showing the bug which was the common one experienced in the class. 
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab3/I2C%20address%20bug.png" height="800"/>






