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

# 1.
In lab 6, I drove one of my motors at a PWM value of 65/255 and the other at a PWM value of 55/255. Because of this I drove the car at the wall with the crash pad using the same PWM values (without PID) and recorded the run so that I could analyze the step response. The way I designed my bot, I am easily able to change the timing of the run. I played around with this time and found a run length that would get it up approximately to full speed for a given PWM value and would stop right after that so that it did not hit the wall too fast.
<br>
My data is recorded in entries and is sent over to my computer. However, my TOF code is non-blocking and the IMU runs much faster than the TOF sensors, thus I multiple entries often contain the same TOF sensor reading. Becuase of this, I filtered out data log entries with repeat front TOF sensor readings. 
<br>
I eventually came up with the following data.

