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
  <a class="active" href="/lab10">Lab 10</a>
  <a href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a href="/lab13">Lab 13</a>
  </ul>
</div>
## Lab 10

### Open-Loop Square
What is the duration of a velocity command? 
<br>

As the code shows below the duration of a velocity command is 4 seconds for both the translational and rotational velocity commands.
<br>
<br>
Does the robot always execute the exact same shape?
<br>

  A close look at the true ground truth of the robot (the green plot in the video below) over time seems to suggest that it consistently executes the same shape. Over many many runs a visible difference might occur. Additionally if you sped the robot up and reduces the asynio sleep times drastically, then I think that you would begin to see it not follow the same path every time. This is because the simulation seems to be running on a different process than the Python script. When the Python script sleeps, it may not sleep for exactly the same amount of time every time a sleep is called especially if this sleep is a much smaller number. Thus the Python script may set the velocity in the simulation at irregular times causing the shape the robot follows to vary slightly from time to time.
  
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/xJTUtsG4OWQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  


```
while cmdr.sim_is_running() and cmdr.plotter_is_running():
    pose, gt_pose = cmdr.get_pose()
    cmdr.plot_odom(pose[0], pose[1])
    cmdr.plot_gt(gt_pose[0], gt_pose[1])
    while True:
        cmdr.set_vel(.1,0)
        
        await asyncio.sleep(4);
        pose, gt_pose = cmdr.get_pose()
        cmdr.plot_odom(pose[0], pose[1])
        cmdr.plot_gt(gt_pose[0], gt_pose[1]) 
        cmdr.set_vel(0,math.pi/8)
        
        await asyncio.sleep(4);
        pose, gt_pose = cmdr.get_pose()
        cmdr.plot_odom(pose[0], pose[1])
        cmdr.plot_gt(gt_pose[0], gt_pose[1])
        cmdr.set_vel(.1,0)
        
        await asyncio.sleep(4);
        pose, gt_pose = cmdr.get_pose()
        cmdr.plot_odom(pose[0], pose[1])
        cmdr.plot_gt(gt_pose[0], gt_pose[1])
        cmdr.set_vel(0,math.pi/8)
        
        await asyncio.sleep(4);
        pose, gt_pose = cmdr.get_pose()
        cmdr.plot_odom(pose[0], pose[1])
        cmdr.plot_gt(gt_pose[0], gt_pose[1])
        cmdr.set_vel(.1,0)
        
        await asyncio.sleep(4);
        pose, gt_pose = cmdr.get_pose()
        cmdr.plot_odom(pose[0], pose[1])
        cmdr.plot_gt(gt_pose[0], gt_pose[1])
        cmdr.set_vel(0,math.pi/8)
        
        await asyncio.sleep(4);
        pose, gt_pose = cmdr.get_pose()
        cmdr.plot_odom(pose[0], pose[1])
        cmdr.plot_gt(gt_pose[0], gt_pose[1])
```

<br>
### Obstacle Avoidance
My virtual object avoidance robot is shown in a video below.
<iframe width="560" height="315" src="https://www.youtube.com/embed/eFcNkHTbkGc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
The heart of the code I used to accomplish this is below.

```
while True:
      cmdr.set_vel(.1,0)
      while True:
          if cmdr.get_sensor()[0]>.3:
              cmdr.set_vel(.1,0)
          else:
              cmdr.set_vel(0,math.pi/8)
              await asyncio.sleep(4)
          pose, gt_pose = cmdr.get_pose()
          
          await asyncio.sleep(.001)
```



<br>
By how much should the virtual robot turn when it is close to an obstacle?
<br>
<br>
  I found that the obstacle avoidance works well when you instruct the robot to rotate in 90 degree intervals. If told it to rotate in really small intervals, it would often collide with the wall because it would turn until the TOF beam went past and edge and would drive forward. Becuase the TOF beam does not account in anyway for the width of the robot, the side of the robot, would then clip the corner.
 <br>
 <br>
At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
<br>
I made the robot go fairly slow at a speed of only .1m/s. This seemed to work very well from the standpoint of avoiding collisions. I was able to make the robot go much faster. I increaesd the rate at which it turned and decreased the duration of the turn by the inverse of the increase. I also made the robot go much faster translationally and increased the distance from the wall at which it turned by about a factor of 3. This is shown in the video below. In the simulation this was easy, but because of the dynamics of the robot, in real life speeding up obstacle avoidance would be much more challenging. 
<br>

<iframe width="560" height="315" src="https://www.youtube.com/embed/WjG37AZl46k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
<br>
How close can the virtual robot get to an obstacle without colliding?
<br>
It is not always apparent that the robot has run into the wall so this is hard to do visually. In order to figure this problem out, I wrote the following python code which produced the result shown in the image.

<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab10/Screen%20Shot%202022-04-14%20at%204.54.31%20PM.png" height=500/>
<br>
I ran the simulator and recorded sensor measurements as the robot sat still over 15 seconds. I found that the standard deviation of the sensor measurements is about .034 meters. In order to rarely crash into the wall, it would be a good idea to stay 3 standard deviations away from the wall or .102 meters away. If you do this, then you can be pretty sure you will not crash into the wall and if you want to be even more sure, you can start to turn away from the wall at an even farther distance away. This does not necessarily accound for the width of the car which you may need to take into accound when turning so you do no clip the side of the robot on the wall. Additionally, I am assuming that the standard deviation of the sensor values are about the same at any distance in the simulator. In the real robot, it might be the case that the standard deviation of the sensor is greater at greater distances.
<br>
<br>
Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?
<br>
The obstacle avoidance code seems to be pretty reliable when I simulate it. However, it does seem to eventually settle into a repeating pattern that does aviod obstacles but is less interesting. To avoid entering into this repeating pattern, I could add in some randomness into the amount that it turns when it gets to a wall. 


<br>



