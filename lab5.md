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




To demonstrate control of the PWM I wrote the following below and hooked it up to the oscilloscope. This produced the results in the video below.

```
void setup() {
  // put your setup code here, to run once
   pinMode(1,OUTPUT);
   analogWrite(1,50);
   delay(10000);
   analogWrite(1,100);
}
```


When a pwm signal of 30 out of 255 is given to the left and a pwm signal of 40 out of 255 is given to the right, the robot is just barely able to move itself.




To make the robot go straight I used the following in setup
```
   pinMode(1,OUTPUT);
   pinMode(14,OUTPUT);
   analogWrite(14,40);//right
   analogWrite(1,65);//left
 ```
 The results of this are shown below
 INSERT YOUTUBE VIDEO HERE

The tape is 8ft long and the requirement was to go 6ft still over the tape.
