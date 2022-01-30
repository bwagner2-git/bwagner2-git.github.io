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
  <a class="active" href="/lab1"> Lab 1 </a>
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
  <a href="/lab13">Lab 13</a>
  </ul>
</div>
## Lab 1

### Part 1: Blink
The first part of this lab consisted of uploading the blink example code to the Artemis Nano and observing the behavior. Below is a video demonstrating that this was completed successfully. This example code set the onboard LED as an output in the setup loop. It then repeatedly set the LED high, waited, set the LED low, waited, and then set the LED high again starting the sequence over and causing the LED to blink repeatedly.
<iframe width="560" height="315" src="https://www.youtube.com/embed/Rr4qf5RxXmI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Part 2: Serial
The next part of this lab again consisted of uploading example code to the Artemis Nano. This time we uploaded the serial example and demonstrated data being sent to and from the Artemis Nano over the cable. This was analyzed on the serial monitor provided by the Arduino IDE. A video of this is shown below. When something is typed into the terminal and sent to the Artemis Nano, it echoes it back.
<iframe width="560" height="315" src="https://www.youtube.com/embed/gsTndgzi1MY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Part 3: Temperature Sensor
The next part of the lab consisted of uploading the analog read example code to the Artemis Nano. In this example, the Artemis Nano reported on data that it captured from its onboard temperature sensor. This was then displayed in the serial monitor.
<iframe width="560" height="315" src="https://www.youtube.com/embed/XALgurxx6d4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Part 4: Loudest Frequency
For this part of the lab I uploaded example code. This code used the pulse density microphone on the Artemis board to print out values representing the loudest frequency present in real-time. It utilized an FFT to accomplish this. In the example below, I whistle towards the board and the values, which are being printed on the serial monitor, change significantly.
<iframe width="560" height="315" src="https://www.youtube.com/embed/zzwfq-nGOgk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



### Part 5: Whistle Blink
In this part of the lab, I was tasked with lighting up the onboard LED when I whistled at the Artemis and turning it off otherwise. I did this by modifying the "loudest frequency" example code. I first set the onboard LED to an output in the setup loop. I then looked at the serial monitor during the loudest frequency example. I saw that when I was not whistling, the loudest frequency value was consistenly below 1500. However, when I whistled the values jumped above 1500. Because of this I thought that this would be an appropriate threshold value to determine whehter or not I was whistling. I then set the LED based on how the loudest frequency value compared to this threshold value. The pseudo code is as follows:
```
if loudestFrequency>1500:
  turn the LED on
else:
  turn the LED off
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/DCdwJuWWQZU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


