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
  <a class="active" href="/lab4">Lab 4</a>
  <a href="/lab5">Lab 5</a>
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

## Lab 4


### Car Dimensions
* wheel diameter: 7.5 cm
* wheel width 2.8 cm
* tire thickness 1 cm
* body length 16.5 cm
* body width 7 cm 
* body height 3.5 cm

<br>
I chose to measure car dimensions becuase I thought that they might be useful in many of the phyics type equations that might come up throughout the course. It seems like the car will be rotating a lot throughout the course and the dimensions seem like they would be very helpful for calculating things like the moment arms/moments of interia. We also talked about how we would need to change the reference frame of many of the senors when handling incoming data, and having a good feel for the cars dimensions seemed like it would be helpful for this.
Knowing the wheel dimensions also seemed like it would be really helpful down the road. For exampe, becuase I now know the wheel diameter, if I know how fast the car is going on a non slippery surface, I can now get a good estimate how many rpms the motor is outputting. This knowledge could be helpful when trying to fine tune things such as the duty cycle of a pwm signal I am sending out. Knowing the wheel width seemed like it could be useful for some type of measurement involving slippage. One thing that I noticed, is that the tires are very easily compressible and it seems like the wheel might conform to different shapes under relatively high forces. Knowing the tire thickness could help quantify this phenomenon and give some indication of the possible shapes the tire could take on.
I just used a simple ruler to take car dimensions.
### Car Weight
The car weighs 1.15 pounds or 0.5216312 kg with the batter inside. The battery that was inside was the one that came with the car. Again this information seemed like it would be useful in any physics that we bring into the our process later in the class such as moments of interia. The information seems like it will couple nicely with the car's dimmensions moving forward.
I measured the cars weight by taking it to a local post office and asking them to weigh it.

### Car Drift
I came up with the idea to measure car drift. When I say drift I mean how far the car drifts left or right for some amount of distance that it drives forward. I observed that when the car was on the blue side, it experienced significant and consistent drift to the left as the first video below demonstrates. However, when the car was driving with the black side up it experienced less drift and sometimes it drifted right and other times it drifted left as the second video below indicates. In the videos I am holding the forward button only. Each tile on the floor is 1 foot by 1 foot. To measure drift I set the car at a predefined starting point and aligned the two wheels of the car along the side of the tile to ensure that it was pointing directly ahead. I then pressed and held forward on the remote and drove until it drifted into the wall. I recorded where it hit the wall as the third video below shows. I then counted the squares it traveled forward and the squares it traveled left and compared the two to calculate drift. When on the blue side, on average, the car drifted .2 feet to the left for every foot it traveled forward. In future labs, we will give the car some command and expect it to do something. What it actually does will undoubtedly be different to some degree than the expected behavior, but we will have some feedback loop to adjust the discrepency. However, the better that we can predict the actual behavior caused by our command, the faster our model will converge to expected behavior. I through that gaining a better understanding this consistent drift that I observed would help me make better predict what commands will lead to what behaviors and potentially help to spped up how quickly my model achieves expected behavior in the future.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QvRM8i6N2uM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/J3W0LbY_uIs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ce2wIUCW_j8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>




### Tricks
For my last characterization, I chose to look at some tricks. There is whole future lab dedicated to stunts and so I thought it would be helpful for that lab. Additionally, tricks are cool and I wanted to play around with the car. I wanted to know what combinations of inputs led to the car consistently performing a trick. The first video shows what I am calling a half flip. You dirve the car either forward at full speed one way, and then you press full speed backwards and the car stops, flips, and comes back the other way. Not really shown in the video is the drift. For this trick, you get the car going forward at full speed and then you tap either right or left and the car slides a bit and then you can keep driving forward or go into donuts. For donuts, you just hold either left of right and the car spins. My favorite trick, which is shown in the video, is what I call the penny spin because it looks like a penny when you have spun it and it starts to settle back to the flat surface. In this trick the car essentially does a donut on 2 wheels before just completely starting to flip around. To do this trick you hold forward and either left or right, which starts the car doing donuts, the you release either left or right and continue to hold forward. I think that it would be really interesting to see if you could keep the car in the 2 wheel donut shape and not have it flip out by seniding it forward pulses instead of a continuous forward command.
<iframe width="560" height="315" src="https://www.youtube.com/embed/q8cyodO3pFQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The last trick I did was a jump with a half flip. The car naturally did the half flip when it left the ramp. I think this happened becuase for a brief moment the cars back wheels were driving the car and the front wheels were in the air causing the front of the car to kick up. It would be interesting to see when the front wheels left the ground, if you stopped driving the back wheels, would the car flip? The video of the jump is shown below.
<iframe width="560" height="315" src="https://www.youtube.com/embed/EpmchlkbECo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Again these tricks were helpful because it gave me an idea of what commands I need to send the car to reliably do some trick.

