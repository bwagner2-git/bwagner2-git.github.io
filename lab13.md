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
  <a href="/lab9">Lab 9</a>
  <a href="/lab10">Lab 10</a>
  <a href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a class="active" href="/lab13">Lab 13</a>
  </ul>
</div>

## Lab 13

### Our Team
I worked with Anya and Chidera on this lab.

### Our Approach
We gave our robot a list of points to hit on the map and our robot moved in steps to try and hit each one of those points. At the beginning of each step the robot would do a scan of the arena. The scan consisted of a 360 degree turn in which the robot took a ToF distance measurement every 20 degrees. The 18 distance measurement angle pairs were then sent over to the computer via bluetooth. The computer received this information and ran the localization code from lab 12 to find out where the robot most likely was on the map.
<br>
<br>
Once the robot had an idea of where it was, it either decided that it was close enough to the point that it was targeting on its last step in which case it set its new goal to the next point in the list of target points, or the robot decided that it was too far away form the point that it was currently targeting. In this case it did not change the point that it was targeting.  In this way if the robot missed a point, it would try to make its way back before continuing onto the next point.
<br>
<br>
Once the computer knew where the was going and where the robot was (with some uncertainty because this was done via the localization code) it calculated an angle that it would need to turn and a distance that it would need to travel in order to make it to its target point. The computer then sent these instruction back to the robot via the send three floats command we have been using throughout the class. After the robot completed its scan it started sitting there and waiting for a command. When it received the three floats command from the computer, it updated global variables that stored the angle it needed to turn and the distance it needed to drive in the current step. It then turned by the angle amount, and drove forward that distance before stopping and going back into the scan in order to repeat the process. It used the front ToF sensor to measure the distance and it integrated the IMU gyro values to measure changes in angle. The code for a step is shown at the bottom of the page.
<br>
<br>
In order to make our system work well, we tried to reduce error during the movement process. One source of error was the robot not driving straight. In order to combat this, we implemented a PID controller with the IMU gyroscope value as the input to get the robot to drive straight when moving between points. The code for this lived in the “driveStraight” function shown below. We achieved some very strong results with this code as the videos below show.

```
void driveStraight(float imuVal){
  leftWheel+=round(float(.3)*imuVal);
  rightWheel-=round(float(.3)*imuVal);
  if (leftWheel>80) leftWheel=80; //cap the speed
  if (rightWheel>80) rightWheel=80; 
  if (leftWheel<50) leftWheel=50; //cap the lower limit based on physical properties of the robot to keep it moving
  if (rightWheel<50) rightWheel=50;
  analogWrite(16,0);
  analogWrite(13,0);
  analogWrite(7,rightWheel); //rightwheel
  analogWrite(11,leftWheel);//leftwheel
  
}
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/dh3mkPWNMF0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>
I found that the initial values you pass for right wheel and left wheel are important because it takes some amount of time for the robot to converge on a ratio that will get the robot to drive straight. If the pwm  values are set far away from this perfect ratio, the car will turn for a bit and then drive straight, but at this point it is driving straight in the wrong direction because of that initial turn. If you set the ratio such that the car will drive relatively straight initially, then the PID controller helps the car STAY driving straight in the desired direction. Another essential part of the controller is using the round function when converting from float too int. When casting from a float to an int, Arduino truncates the decimal which means that it always rounds down. This causes the PID favor turning the car in one direction more than the other and the car drifts in that direction. Using the round function evens out this imbalance and helped the controller’s performance tremendously.

<br>
<br>

Another imperfection in the car’s movement is that it does not turn around a single point when it is rotating either for the scan or when it is turning to go to the next point. We found a pwm ratio that made the car spin over a relatively fixed location and hardcoded that into the turn and the scan. Originally, we were also doing a second rotation when we got to a new point i.e. one step consisted of a turn, drive forward, and another turn. The second turn was meaningless and added more error to the system so we got rid of it.
 <br>
<br>
Another hack we used to improve performance was to “hardcode” in the first step. In order to get the robot from the first point to the second point we just told it to drive forward at a certain speed for a set amount of time rather than using the ToF sensor to measure distance. We did this because, when the robot was on the first point and was pointing toward the second point, the wall was far away and we did not trust out distance sensor measurements at that distance. 
<br>
<br>
Another problem we originally had was the robot consistently overshooting the desired point. We did two things to counteract this problem. First, we implemented active braking instead of just killing the motors and letting the robot coast. Next, we actually had the Python script scale back the distance between two points that it sent to the robot. For example, if the Python script determined that the robot needed to drive forward .5 meters to the next point, it would tell the robot to drive forward .33 meters instead.
<br>
<br>
Below is the results of our implementation.

INSERT THE GOOD TWO VIDEOS HERE 

<br>
Here is an example of the robot getting lost and finding its way back to the path.

INSERT ROBOT FINDING ITS WAY BACK VIDEO

### Why we chose to do it the way we did
Ultimately, we might have been able to hit all of the points in this path more accurately by using more PID and hardcoding in a very specific sequence of instructions for the robot to follow. However, had we done it this way however, our robot would have been constrained to that specific path. If we wanted to change the path our robot took, we would have to figure out and hardcode in a new set of very specific instructions. Our solution on the other hand allowed us to send in a list of points, our robot would work with the computer to figure out a way to achieve that path. Thus switching up our robot’s path required minimal effort which would be beneficial in a lot of real world applications involving this technology! Additionally, our robot was more robust in the sense that if it got off path for whatever reason, it could often find its way back as the video above demonstrated. In order to demonstrate the power of our solution, we did the course backwards by simply reversing the original list of points!

INSERT BACKWARDS VIDEO

### When its 1 AM and you robot finally works
<iframe width="560" height="315" src="https://www.youtube.com/embed/aiY8pAgs5rU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


### Thanks for a great semester!


INSERT BACKWARDS VIDEO


#### Relevant Code
```
    //////DRIVE STRAIGHT//////
        int beginTime=millis();
        while (millis()-beginTime<1300) motor_forward(60);

        motor_brake();
        
         beginTime =millis();
         while(millis()-beginTime<4000) motor_brake;
         
         while (1) {
          
          /////DO A SCAN//////////
          int updateSuccess;
          angle=0;
          endIntegral=micros();
          updateSuccess=myBot.updatePosition();
          while (central.connected() && CR==1) {  
            angle+=abs(myBot.gX)*(float((micros()-endIntegral))/1000000);
            endIntegral=micros();
            updateSuccess=myBot.updatePosition();
  
//            Serial.println(myBot.gX);
            e = abs(myBot.gX) - 15;
            pwm_val = 105 - Kp*e;
//            motor_spin( limit_range(pwm_val), limit_range(pwm_val)+15 );  
            motor_spin(100,115);
//            motor_spin(
            if (measurement>=18){  //reintialize for next loop send over turn log and move on
              motor_stop();
              angle=0;
              increment=0;
              measurement=0;
              sendMapLog();
              CR=0;
              break;
            }
            if (angle-increment>=20){
              struct turn t;
              t.predictedAngle=angle;
              t.frontTOF=myBot.front;
              t.sideTOF=myBot.side;
              measurements[measurement]=t;
              measurement+=1;
              increment=angle;
            }
        }
         /////DO A SCAN//////////

        ////////receive an instruction///////
        while (central.connected()){
//          Serial.println("waiting for a command");
          if (rx_characteristic_string.written()) {
                Serial.println("handled");
                handle_command();   
                break;     
          }
        }
        ////////receive an instruction///////
        
        
        /////////TURN AN ANGLE//////
          angle=0;
          endIntegral=micros();
          updateSuccess=myBot.updatePosition();
//          if (pi>180) pi=360-pi;
          while (central.connected()) { 
            angle+=abs(myBot.gX)*(float((micros()-endIntegral))/1000000);
            endIntegral=micros();
            updateSuccess=myBot.updatePosition();
            if (angle>=int(pi)) {
              angle=0; //reset for next time
              motor_stop();
              break;   /// i is second angle
            }
            motor_spin( 100, 115 ); //just use last pwm value
          }
        /////////TURN AN ANGLE//////


        //////DRIVE STRAIGHT//////
          myBot.updatePosition();
          startingPoint=myBot.front;
          
          Serial.print("starting point is ");
          Serial.println(startingPoint);
          
          while (startingPoint-myBot.front<int(p) && central.connected()){ //use this
            myBot.updatePosition();
            driveStraight(myBot.gX);
            myBot.pright=rightWheel;
            myBot.pleft=leftWheel;
          }
          motor_stop();
          
          Serial.print("the last one was ");
          Serial.print(startingPoint-myBot.front);
          Serial.println("starting turn");

         beginTime = millis();
         while (central.connected()){
          motor_brake();
          if (millis()-beginTime>1000) break;
         }
         
         //////DRIVE STRAIGHT//////



        beginTime = millis();
         while (central.connected()){
          motor_brake();
          if (millis()-beginTime>1000) break;
         }
         CR=1;

      }
```
