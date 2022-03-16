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
  <a class="active" href="/lab6">Lab 6</a>
  <a href="/lab7">Lab 7</a>
  <a href="/lab8">Lab 8</a>
  <a href="/lab9">Lab 9</a>
  <a href="/lab10">Lab 10</a>
  <a href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a href="/lab13">Lab 13</a>
  </ul>
</div>

## Lab 6

### Prelab
At the beginning of this lab, I spent a signficant amount of time making a solid debugger which I am hoping will pay off throughout the rest of the course. Not only did I implement a debugger, but I modularized my code by creating a "bot" class. This helps me keep my code extremely clean and easy to debug when building functioality around the robot. Instantiating the robot takes three arguments. The first to arguments are pointers to TOF sensor objects and the third argument is a pointer to the IMU object. The bot then calls bot.update() to update its front sensor value, side sensor value, roll, pitch, and yaw. I changed the sensor code so that it was non-blocking. To do this I used a flag to indicate whether or not a TOF sensor was already ranging. A small piece of the updated sensor code is shown below to expound upon how I accomplished this.
```
if (!oneRanging){
        dOne->startRanging(); //Write configuration bytes to initiate measurement
        oneRanging=true;
//        Serial.println("sensor one started ranging");
      } else if (dOne->checkForDataReady()) {
        front = dOne->getDistance(); //Get the result of the measurement from the sensor
        dOne->clearInterrupt();
        dOne->stopRanging();
        oneRanging=false;
      }
```
In this example oneRanging is the flag that describes whether a sensor is already ranging. 

