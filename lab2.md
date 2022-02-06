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
  <a class="active" href="/lab2">Lab 2</a>
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

## Lab 2: Bluetooth Low Energy

### Demo
To start this lab I loaded up the provided demo code both on the Artemis and in Jupyter Lab after I had done all of the necessary installations. I then followed the demo code as instructed. Screenshots below show that this was accomplished.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/mac%20address%20advertising.png" height="200"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/connected.png" height="200"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/connected2.png" height="300"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/connected3%202%20ints.png" height="300"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/disconnect.png" height="200"/>


### Tasks
Next I modified and added to this base code to accomplish the tasks. The first task was to "Send an ECHO command with a string value from the computer to the Artemis board, and receive an augmented string on the computer." I implemented this in such a way that the user would send a string to the Artemis, then the Artemis would add a winky-face, ;), and send it back for the computer to print out. On the Artemis side, I found the switch case where it was handling the ECHO command. There was already an array that had the character values of the string in it called char_arr. To append the winky face, I created a pointer that pointed to the first character in the array. I then moved the pointer forward until it pointed to a NULL value signalling I had reached the end of the string. I then changed the value pointed to by the pointer to a ";" and incremented the pointer by 1 and changed that value to a ")". 
```
clear tx_estring
char *pointer = address of first element in char arr
while *pointer is not NULL:
  move the pointer to the next char in the array
//the pointer now points to the first NULL in the array
*pointer=";" //changes the value at the address pointed to by the pointer
*(pointer+1)=")"
//the char array now has the winky face appended to it
append the char array to tx_estring
write tx_estring to the tx_characteristic_string //now it will be sent back to the computer by another piece of the code
```
This allowed me to send echo commands with an arbitrary string (not too long) and receive an augmented echo as shown by the following screenshot.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/augmented%20echo.png" height="200"/>

Next, I was tasked with developing the send three floats function on the Artemis side. The function was already filled out on the Jupyter Lab side. In order to develop the Artemis side of the function, I looked at the SEND_TWO_INTS function for inspiration. I first declared three floats. I then used already defined robot_cmd.get_next_value(); function three times to populate the three float variables with the proper values before printing them out on the serial monitor. The result is shown in the video below.
<iframe width="560" height="315" src="https://www.youtube.com/embed/eKT1XxwJOm0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Next I set up a notifcation handler in Python to constantly receive updates of the BLEFloatCharactersitic from the Artemis. To store this information I added a "time" attribute to the BaseBLEController class. I then created a callback function that updated this attribute when it was called and used the start_notify method of the ble object to trigger this function to run when an update of BLEFloatCharactersitic was received. Below you can see the result. Notice I am never explicitly updating ble.time, it is being updated automatically. 
<br>


In the Artemis code, strings of characters to be sent to Python are written to the BLECStringCharactersitic where as float values that are to be sent are written to the BLEFloatCharactersitic. Both are sent as bytes and received as byte objects in Python. The strings that are sent over can simply be decoded which then converts them from a byte type to a string type. However, the floats that are sent over need to be unpacked as a float value. This action takes them from an object of type bytes to an object of type float. Information about unpacking can be found here  https://docs.python.org/3/library/struct.html. This is evident if you dive into the part of the library shown below. <img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/Screen%20Shot%202022-02-06%20at%203.25.32%20PM.png" height="200"/>

So to go from BLECStringCharactersitic to a Python float you would go BLECStringCharactersitic->bytes object->string->float. Going from a string to float could be done with a simple cast. To go from BLEFloatCharactersitic to float you would go BLEFloatCharactersitic->bytes object->float. This is shown below.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/Screen%20Shot%202022-02-06%20at%203.20.23%20PM.png" height="200"/>
If you try to read in the float characteristic as a string or the string characteristic as a float you get the errors shown below. Essentially, if something is sent as a float characteristic it should be read in as a float and if something is sent as a string characteristic it should be read as a string and then it can be converted to whatever after that.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/Screen%20Shot%202022-02-06%20at%203.50.02%20PM.png" height="400"/>

To test the data rate I defined the function shown below. Essentially it creats some bytes object to send to the Artemis and have the Artemis echo it back. I set the echo command back to its original state so that it did not augment whatever it was sent for this exercise. The amount of bytes that are sent are 2 times the original message length because the bytes need to be sent there and back. I measured the total round trip time from the time I started sending them to the time I received all of the message back and used this to determine data rate. I then found an example on stackoverflow on how to plot my points. This plot is also shown below.
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/test%20data%20rate%20function.png" height="200"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/testing%20data%20rate.png" height="400"/>
Notice how the data rate tends to increase with the number of bytes you send. I think that this is because the amount of time spent setting up the connection (versus actually sending bytes) is a bigger portion of the total time when the number of bytes is lower than when you send a lot of bytes. The plot shows number of bytes on the x-axis and time on the y-axis. This plot ended up looking nice, but it seemed that the time of these transactions had a good amount of variation between runs. The effective data rates are also shown in the screenshot as requested.
<br>
In the Artemis code, there is a variable called interval which determines how often the Artemis updates its float value and thus sends a notification to the Python notification handler. To increase the rate at which the artemis sent data to the Python script, I made this interval value smaller. I then had the notification value print out the value of ble.time when it was updated so I could see if any values were missed. If no values were missed I should see every value increase by .5 from the previous value. I expected that the computer would drop some data, but I pushed the value of interval all the way down to 1, and it did not drop any data as the video shows below. This was suprising to me, but it was the results I got. Perhaps if I sped it up even more, I would reach a point where it did drop some of the data. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/3rMDMhQOTAk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
That concludes lab 2. Thanks for reading!









