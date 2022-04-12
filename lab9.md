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
  <a href="/lab5">Lab 5</a>
  <a href="/lab6">Lab 6</a>
  <a href="/lab7">Lab 7</a>
  <a href="/lab8">Lab 8</a>
  <a class="active" href="/lab9">Lab 9</a>
  <a href="/lab10">Lab 10</a>
  <a href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a href="/lab13">Lab 13</a>
  </ul>
</div>

## Lab 9

### Scanning the Room
To begin this lab I developed and implemented a method to rotate the car as slowly as I could around a point. I chose to do this using orientation control and what essentially ended up being just an I controller. To do the control, I first passed in a target angular speed at which I wanted my car to rotate at over bluetooth. I then looked at the gyroscope value of the car and compared it to the target angular velocity. I subtracted the two and added this to a term which I then scaled and set as the PWM for the motor. This is summarized below.
```
error=desiredAngularVelocity-measuredAngularVelocity;
turnRate+=I*error; //adjust the turnrate
turnCar(turnRate); //adjust the pwm according to the turnrate
```
In effect this led to the car staying still for a bit while it "accumulated enough PWM" to begin moving, but once it began moving, it moved very slowly and fairly smoothly which was th goal as it allowed me to take a lot of distance measurements over the course of one rotation. I did not want to have a ton of measurements where the car was just standing still because it was still building up enough power to start turning, so I implemented a check to only take a measurement when the measured PWM was a certain percentage of the desired PWM. The result of my implementation is shown in the videos below.

<br>
INSERT VIDEOS HERE
<br>
