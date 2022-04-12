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
<li> What is the duration of a velocity command? 
 <br>
As the code shows below the duration of a velocity command is 4 seconds for both the translational and rotational velocity commands.
<li> Does the robot always execute the exact same shape?
 <br> 
  A close look at the true ground truth of the robot (the green plot in the video below) over time seems to suggest that it consistently executes the same shape. Over many many runs a visible difference might occur. Additionally if you sped the robot up and reduces the asynio sleep times drastically, then I think that you would begin to see it not follow the same path every time. This is because the simulation seems to be running on a different process than the Python script. When the Python script sleeps, it may not sleep for exactly the same amount of time every time a sleep is called especially if this sleep is a much smaller number. Thus the Python script may set the velocity in the simulation at irregular times causing the shape the robot follows to vary slightly from time to time.
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/xJTUtsG4OWQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
 <br>
#### Code for Square Maneuver

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
<br>
<li> By how much should the virtual robot turn when it is close to an obstacle?
<li> At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
<li> How close can the virtual robot get to an obstacle without colliding?
<li> Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/eFcNkHTbkGc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

