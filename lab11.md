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
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/1sxVmoNviI4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
