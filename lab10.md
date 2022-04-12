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
Make your robot follow a set of velocity commands to execute a “square” loop anywhere in the map.
Plot and analyze the ground truth and odometry of the robot.
What is the duration of a velocity command?
Does the robot always execute the exact same shape?
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/xJTUtsG4OWQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
<br>
### Obstacle Avoidance
Design a simple controller in your Jupyter notebook to perform a closed-loop obstacle avoidance.
By how much should the virtual robot turn when it is close to an obstacle?
At what linear speed should the virtual robot move to minimize/prevent collisions? Can you make it go faster?
How close can the virtual robot get to an obstacle without colliding?
Does your obstacle avoidance code always work? If not, what can you do to minimize crashes or (may be) prevent them completely?
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/eFcNkHTbkGc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

