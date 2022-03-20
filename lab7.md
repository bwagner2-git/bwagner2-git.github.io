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
  <a class="active" href="/lab7">Lab 7</a>
  <a href="/lab8">Lab 8</a>
  <a href="/lab9">Lab 9</a>
  <a href="/lab10">Lab 10</a>
  <a href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a href="/lab13">Lab 13</a>
  </ul>
</div>

## Lab 7

### 1. Step Response
In lab 6, I drove one of my motors at a PWM value of 65/255 and the other at a PWM value of 55/255. Because of this I drove the car at the wall with the crash pad using the same PWM values (without PID) and recorded the run so that I could analyze the step response. The way I designed my bot, I am easily able to change the timing of the run. I played around with this time and found a run length that would get it up approximately to full speed for a given PWM value and would stop right after that so that it did not hit the wall too fast.
<br>
My data is recorded in entries and is sent over to my computer. However, my TOF code is non-blocking and the IMU runs much faster than the TOF sensors, thus I multiple entries often contain the same TOF sensor reading. Becuase of this, I filtered out data log entries with repeat front TOF sensor readings. 
<br>
I eventually came up with the following data.
#### Motor Step Response
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab7/motorInput.png" />
#### TOF Sensor Readings
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab7/TOF%20step.png" />
#### Calculated Speed
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab7/speed%20step%20response%2050%20and%2065.png" />
<br>
To generate the speed graph, I looked at successive TOF sensor readings, found the distance between them, and then divided by the time between the readings.
<br>
By looking at the speed graph I can see that it looks like my robot gets up to about 1.1 m/s and it took it about 2.3 seconds to rise to 90% of this value. It would have been better to let the robot run a little bit longer and see the curve flatten out to a more constant speed, but the curve is defintitely starting to flatten out and I did not want to crash my robot into a wall.
<br>
Using this information allowed me to compute my A and B matrices. 
Using d=u/v where I am using 1 as u to represent the unity step response, and v is velocity, I can see that steady state drag, d, is 1/1000mm/s or d=.001
<br>
Then using <img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab7/Screen%20Shot%202022-03-20%20at%204.05.41%20PM.png" /> to calculate m, and the 90 percent rise time of 2.3 seconds, I get m=.0009988773
<br>
### 2. Kalman Filter Setup

In this section I specify process noise and sensor noise. 
```
sig_u=np.array([[sigma_1**2,0],[0,sigma_2**2]]) //We assume uncorrelated noise, therefore a diagonal matrix works.
sig_z=np.array([[sigma_4**2]])
```
My C matrix is a m by n matrix where n is the number of dimensions in my state space and m is the number of states I actually measure. There are 2 dimensions in my state space x and v and there I actually only measure x so my C matrix looks like
```
C=np.array([[-1,0]])
```
