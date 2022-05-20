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
We gave our robot a list of points to hit on the map and our robot moved in steps to try and hit each one of those points. At the beginning of each step the robot would do a scan of the arena. The scan consisted of a 360 degree turn in which the robot took a ToF distance measurment every 20 degrees. The 18 distance measurment angle pairs were then sent over to the computer via bluetooth. The computer received this information and ran the localization code from lab 12 to find out where the robot most likely was on the map. Once the robot had an idea of where it was, it either decided that it was close enough to the point that it was targeting on its last step in which case it set its new goal to the next point in the list of target points, or the robot decided that it was too far away form the point that it was currently targeting. In this case it did not change the point that it was targeting. Once the computer knew where the was going and where the robot was (with some uncertainty because this was done via the localization code) it calculated an angle that it would need to turn and a distance that it would need to travel in order to make it to its target point. The computer then sent these instruction back to the robot via the send three floats command we have been using throughout the class. After the robot completed its scan it started sitting there and waiting for a command. 


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
