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
<br>
To implement my debugger, I first implemented a struct that kept track of all of the information that I wanted to be sent over to my computer. This information included a TOF sensor values, roll, pitch, yaw, and a timestamp for the entry. I also implemented a global variable array which would store these structs. I then created a "logIt" method for my bot that created one of these structs everytime it was called and added it to the global log array. When an entry was successfully added, to the array, the logIt method returned a 1. When an entry was not successfully added, this meant the the array was filled up and logIt returned a 0. This allowed me record data for an entire run and then stop the robot at a given point in time. The length of the run could be controlled by lengthening the global log array (because it would take longer to fill it up).  Next, I created a function that took on of these "info" structs and formed an EString where all of the values were separated by commas. Lastly, I created a function that iterated throught the log array, converted each entry to an EString, and sent it off to my computer via a BLE notification. The order at which this was done was important. First, the robot completed its run logging senosr values and timestamps locally in the log array, after the log array was finished and the robot's run was complete, the robot spent the time to send over the log to my computer via BLE. It would have slowed down the critical loop if I sent over the sensor info during the bot's run as it was being collected. I then used the following code to receive updates on the bluetooth notification.
```
log=[]
def updateValue(uuid,value):
    global log
    log.append(value)    
ble.start_notify(ble.uuid['RX_STRING'],updateValue)
```


### PID
After I was able to log sensor data and send it over BLE, I started to implement the PID controller. Before implementing the controller, I came up with a way to adjust the P, I, and D coefficients without having to reprogram the Artemis every time. I accomplished this using the send three floats that I implemented a few labs back. I also had the car wait until it had received this command until it started its "run".
```
Initialize P I and D to 0
while(p==0) receive commands on the Artemis from Python and update P, I, and D appropriately
while (log is not full) run PID controller
send over run information
```

For this lab I implemented a PD controller. It seems obvious that one should include the P term in this case. I chose to include a D term becuase I wanted my robot to be able to determine if it was approaching the wall too fast and slow down. For the derivative term I stored the past 4 errors and I subtracted the most recent error from the error from 3 times through the loop ago. I thought that this would make my D term much more resistant to variations caused by noise in the sensor readings. The code below shows my PD controller.
```
theerror=myBot.front-300;
          pe[0]=pe[1]; //previous error array
          pe[1]=pe[2];
          pe[2]=pe[3];
          pe[3]=theerror;
          myBot.forward(p*float(theerror)-d*(abs(pe[3]-pe[0])));
```
The video below shows my PID controller when it was tuned. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/plTJmalr32Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
As you can see the robot slows down as it is approaching the wall and backs up to the appropriate distance with minimal oscillation. Below you can see a graph of the TOF sensor values over one PID run.
<br>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab6/front%20sensor.png" height=800 />
<br>
You often see consecutive repeats in TOF sensor data because I used non-blocking code. My IMU is running much faster than the TOF sensors and in order to sample the IMU as fast as possible I create multiple data entries in the time that it takes the TOF sensor to update its reading from one value to the next. Based on this TOF data, I set the PWM value and feed that to bot.forward(). The forward method takes a float argument. Any argument greater than 1 or less than -1 will cause the bot to try and drive full speed in one direction. "Full speed" is capped to a PWM of around 65 in my example. Any value between 1 and -1 will be multiplied by 65, cast to an int and set as the PWM value. Here is how the value that is fed to PWM changes over a run in my example. 
<br>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab6/pwm.png" height=800/>
<br>
Here is another example of PID executing on my bot, though not quite as well tuned as the previous video.
<iframe width="560" height="315" src="https://www.youtube.com/embed/Aw_2fhKwGZE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Finally, here is another example of a very slow PID controller. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/nUG_5GN2eTo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
I am not sure whether the bot's run ended or whether it did not have enought power to get itself out of the deadband on the example below, but either way it did make me think about the benefits of adding an integral term to my controller and how I might cleverly do that. I think that it would be smart to add an I term that can get the bot very close to moving, but not move the bot on its own. In this way if there was any error the bot would have just enough power to inch forward thanks to the I term overcoming the deadband and the P term (due to the error) pushing it forward just enough. However, if the bot is in the right position and there is no error, the I term would not have the power to move the bot based on the accumulation of error it has seen in the past.

