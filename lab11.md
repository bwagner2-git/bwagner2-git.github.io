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
  <a class="active" href="/lab11">Lab 11</a>
  <a href="/lab12">Lab 12</a>
  <a href="/lab13">Lab 13</a>
  </ul>
</div>

## Lab 11
In this report I will take you through my implementation of each of the steps of the Bayes filter and show you the associated code.
<br>
### Compute Control
```
def compute_control(cur_pose, prev_pose):
    """ Given the current and previous odometry poses, this function extracts
    the control information based on the odometry motion model.

    Args:
        cur_pose  ([Pose]): Current Pose
        prev_pose ([Pose]): Previous Pose 

    Returns:
        [delta_rot_1]: Rotation 1  (degrees)
        [delta_trans]: Translation (meters)
        [delta_rot_2]: Rotation 2  (degrees)
    """
    delta_rot_1=np.degrees(np.arctan2(cur_pose[1]-prev_pose[1],cur_pose[0]-prev_pose[0]))-prev_pose[2]
    delta_trans=np.sqrt((cur_pose[1]-prev_pose[1])**2+(cur_pose[0]-prev_pose[0])**2)
    delta_rot_2=cur_pose[2]-prev_pose[2]-delta_rot_1

    return mapper.normalize_angle(delta_rot_1), delta_trans, mapper.normalize_angle(delta_rot_2)
```
<br>
The compute control function essentially just tells you the control sequence necessary to take the robot from one pose to another. Every transition from one pose to another has 3 associated actions, the first rotation, a translation, and a second rotation. The figure below from the slides in class does a nice job of illustrating this. 
<br>
<img src='https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab11/motion.png' height=400 />
<br>
The compute control takes in as arguments the robots current pose and the robots previous pose and returns the first rotation, the translation, and the second rotation that was used to take the robot from the previous pose to the current pose.
### Odometry Motion Model
```
def odom_motion_model(cur_pose, prev_pose, u):
    """ Odometry Motion Model

    Args:
        cur_pose  ([Pose]): Current Pose
        prev_pose ([Pose]): Previous Pose
        u=(rot1, trans, rot2) (float, float, float): A tuple with control data in the format 
                                                   format (rot1, trans, rot2) with units (degrees, meters, degrees)


    Returns:
        prob [float]: Probability p(x'|x, u)
    """
    
    ###you have the map and compute control tells you what control input gets you from one spot on the map to another ideally based on the map
    ###^^^this is is the output of compute control function and is x
    ###the mu is what you think actually happened based on your internal sensors
    ###thus you are generating a gaussian with what you think happened being the average and seeing how likely it is that 
    ###you care about the distance from mu to x and where x is on the gausian about where you think you are
    ###thus you are finding the probability that you are where you think you are given where you were and some control input
    delta_rot_1_hat, delta_trans_hat, delta_rot_2_hat=compute_control(cur_pose,prev_pose) ###look at lecture 18 
    
    # u is the non hat version from the slide with delta_rot_1 delta_rot_trans and delta_rot_2
    p1=loc.gaussian(mapper.normalize_angle(delta_rot_1_hat-mapper.normalize_angle(u[0])),0,loc.odom_rot_sigma) #you are shifting the whole gaussian curve
    p2=loc.gaussian(delta_trans_hat,u[1],loc.odom_trans_sigma)
    p3=loc.gaussian(mapper.normalize_angle(delta_rot_2_hat-mapper.normalize_angle(u[2])),0,loc.odom_rot_sigma)
    
    prob=p1*p2*p3 ###probability for rotation 1 and translation and rotation 2
    
    return prob
```
The odometry motion model basically says, given what you think you did and where you think you were, what is the probability you are at a given place now. You break down the problem into the three actions involved with going from one pose to another (rotation 1, translation, rotation 2). You generate a gaussian with what you think you did based on your odometry sensors as the mean. The sigma of this gaussian, which relates to how spread out it is, depends on your sensors and is given to us in the code as loc.odom_rot_sigma for rotation and loc.odom_trans_sigma for translation. You also pass an 'x' to the gaussian function and the function inputs x to the function it generates and returns the output of the generated gaussian function given that input. You use the previously described compute control function to generate the "x's" that you pass into the gaussian function. This essentially allows you to figure out step by step the probability that each one of the three actions did took you where you think it did. You then multiply all of these probabilities to figure out the probability that the total movement took you where you think it did.
<br>
Note in my code I often use x-mu as my 'x' input and 0 as the mu input to the gaussian which essentially just shifts the gaussian around 0 and has no effect on what I actually care about.

<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/1sxVmoNviI4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
