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

## Lab 2

### Demo
To start this lab I loaded up the provided demo code both on the Artemis and in Jupyter Lab after I had done all of the necessary installations. I then followed the demo code as instructed. Screenshots below show that this was accomplished.
<img src="https://raw.githubusercontent.com//bwagner2-git/bwagner2-git.github.io/blob/main/screenshots/mac%20address%20advertising.png" height="200" ALIGN="left" style="padding-right:20px"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/blob/main/screenshots/connected.png" height="200" ALIGN="left" style="padding-right:20px"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/blob/main/screenshots/connected2.png" height="200" ALIGN="left" style="padding-right:20px"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/blob/main/screenshots/connected3%202%20ints.png" height="200" ALIGN="left" style="padding-right:20px"/>
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/blob/main/screenshots/disconnect.png" height="200" ALIGN="left" style="padding-right:20px"/>

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
<img src="https://raw.githubusercontent.com/bwagner2-git/bwagner2-git.github.io/main/screenshots/lab2/augmented%20echo.png" height="200" ALIGN="left" style="padding-right:20px"/>


