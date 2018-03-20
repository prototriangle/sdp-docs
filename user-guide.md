# Assessment (COPIED FROM COURSE DOCS, DELETE LATER)
The user guide for your group should be between 4 and 6 pages long. It is intended to provide information on the design and implementation of your robot which will be useful to anyone who wants to use your system. 

The document should be targeted towards somebody at the level of a first year CS student, and should let this person fully understand how to take your code and your robot and get it demonstrate what it is capable of. It should include: 

-   How to install your code, and any dependencies, onto a standard DiCE machine 
-   An overview of your robot from the perspective of a potential user, including 
-   structural information such as how it can move, what sensors it is equipped with, and other functionality such as grabbers if appropriate 
-   usage information such as how to turn it on, swap batteries, etc. This should include all the issues that a user might have to deal with when using your robot. 
-   An overview of your DiCE-resident software, including how to get it running, including any necessary calibration, and how to actually operate it in order to play a match 
-   A troubleshooting guide (perhaps best presented as a table) 
    
You are encouraged to include diagrams and tables in your report in order to make things clear, rather than just use textual descriptions. Reports may contain appendices beyond the strict page limits but markers are not required to read these appendices and in any case you are advised to use them sparingly. Large illustrations and references will not count towards the page limits. 

The user guide will contribute 15% of the marks for the course, and will be marked out of 15. 
The marking will be based on the following elements: 

-   up to 3 marks for clarity of the installation instructions 
-   up to 6 marks for comprehensiveness of the usage information 
-   up to 2 marks for the troubleshooting guide 
-   up to 4 marks for the quality of the presentation.

> Good overall organisation, easy to find specific
information
> General appearance (consideration of page limit, good
use of diagrams and tables, nice layout, suitable use of
appendices)
> Correct spelling and grammar, good expression,
readability
> Adequate citation of background material/sources if
relevant




# Introduction
Project Hermes is a delivery and inventory management system. It is optimized for a factory setting however could be adapted for other environments as well. There are four main components in the system: the robot, the lines, the shelves, and the interface.  

The robot uses two wheels, paired with two castors to move around. It has two colour sensors for line detection. A lift mechanism using hall effect sensors. On-board is an EV3 intelligent brick (micro-computer) and an Arduino (micro-controller). 

There is a template for the lines included with the system that are to be printed out, although if it is more convenient for the user, it is possible to use tape on the floor.  

The system must have at least one shelving unit, where boxes are stored, and at least one drop-off point, where boxes can be put by the robot or when a box is to be entered or returned to the system. 

The software for the robot and server can be downloaded and instructions follow on this.

# About Hermes
## Hardware
> Is there an overview of the robot’s construction and
components?
[robot pictures with labels on them]

[shelves]

## Software

A single computer on the intranet runs the server. The server is in 2 parts: One serves the user interface to the web browser and the other manages the robots. Each robot runs a client that manages communication with the server.
![Diagram of high-level system overview](https://i.imgur.com/DouMh8p.png)

# System Setup
> Are all processes described comprehensively (i.e. how
  to install code, the requirements and dependencies)?
> Is there an appropriate level of technical language and
  explanation (“1st year CS student”)?

## Line network setup
Hermes robots navigate using a line network that forms a spanning tree over the working area. The edges of the network are connected by junctions.
### Line specification
The lines for the network are black with a width between 2.5cm and 3.5cm. The black lines are directly bordered by white lines with width of at least 5cm. [See diagram]
### Junction specification
Junctions join together the lines. The junctions use lines of the same thickness as specified above. The lines must fork between 25cm and 40cm from the edge of a central circle with diameter greater than 50cm [see diagram]. No more than 4 lines may connect to a single junction.
### Marker specification
Markers must be placed around the map at specific points.
#### Junction markers
 <img style="float: right; width: 40%;" src="https://i.imgur.com/TjQmWMR.png">Robots traverse the junctions clockwise (entering on the left-hand side of a fork and exiting on the other).There are black junction markers on the left and right of the line incident to a junction (traveling towards the junction). These are 0cm and 20cm from the fork, respectively (See diagram). These are at least 4cm in length. There are coloured markers inside the circle with length of at least 8cm (see diagram) and around the outside of the circle with length of at least 5cm. Each "exit" of the junction must use a unique colour with paired colours as depicted.


##### Endpoint markers
At endpoints (workstations and storage shelves) there are 2 markers

## Server installation
How to install the server and the front-end:
  1. Extract the provided server tarball file
  `tar -xvzf server.tar.gz`
  2. Inside the extracted server directory install the required npm packages
  `sudo npm install --save`
  3. Change to the front-end client directory
  `cd client`
  4. Install the required npm packages for the front-end
  `sudo npm install --save`

## Robot connection
Make sure the server is running on the same network as the robot.
>\> ssh   [robot@ev3dev.local]
>\> ./client.py --ip {server ip -\> default: 127.0.0.1} --start {starting locations (int) -\> default: 0} 
```
If you are using a different line system in the same format you will also need to calibrate the robot to run on the lines effectively. These values can be used for every robot added to the system so this only needs to be performed once at the set up of the network or whenever lines are laid in different colours. ?????? How to update light sensitivity . SSH into the robot using the terminal command 

>\> ssh   [robot@ev3dev.local]

Place the robot on the background of your choosing perpendicular to the line you will be using.  

>\> python3 rangefinder.py 

This will find the maximum and minimum values defining the light range. 

>\> nano linefollower.py 

In this file, locate the variables minRng and maxRng. Update these to the exact values printed to the terminal by rangefinder. The first value printed by rangefinder is minRng and the second is maxRng. 

The robot has a toggleable setting telling it whether it is following a bright line on a dark background or vice-versa (always on the left). 

>\> ssh   [robot@ev3dev.local]
>\> nano linefollower.py 

In linefollower.py locate the 'direction' variable. Set it to 1 for a dark line and light background, and –1 for vice versa.
```

# System Usage
> Is the usage information clearly structured?
> Are all physical interactions with the robot clear, e.g. how
to turn it on, swap batteries, etc.?
> Are all software interactions clear, e.g. how to carry out
calibration, operate the robot, etc.?
> Are all the functional capacities of the robot and its
expected behaviour in different situations explained?
> Are good usability decisions in the design of the robot
and software apparent?

## Server
### Startup
To run the server go to the server directory and start yarn
```
yarn dev
```
### User interface
Users can access the interface by accessing [http://*{server-ip}*:3000/](#do-nothing) using any web browser. 
Where *{server-ip}* is the local ip address of the sever. You can find the server ip by following this guide on the computer running the server: [How to Find Your IP Address in Ubuntu Linux](https://www.howtogeek.com/howto/17012/how-to-find-your-ip-address-in-ubuntu/).


-   Select/Change users 
Each user should select their workstation number from the top-right corner, before making any requests. (Workstation 1 -> User 1)

-   Requesting an item 
A user who wants an item should issue a retrieve request. The requested item will be then fetched from storage by the robot.
Note: A retrieve request will be changed to a transfer request when a user requests an item that is not in storage (on loan) and the user that has it makes a store request.
-   Cancelling a request 
A request can be cancelled before it is processed by the robot. If it is cancelled after the robot starts processing it, all items will be returned to their previous position. 

-   Returning an item 
A user that currently holds an item that wants it stored, should issue a store command.
-   Queueing requests 
Request are being processed by the robots on a first-come first-served basis.

-   Adding item to the storage 
When a user wants to add a new item to the storage, the add item tab should be used. An empty box is then retrieved from storage for the user to add the item inside.  When the user has the box with the item ready, should issue a store request for that item. 
If there are no spare boxes then the following error will be returned. 
`All bases are full, cannot store new items`

-   Locating the Robot 
The map tab can be used to approximately see where each robot is located. Alternatively the requests tab shows the location of each robot that's processing requests.

-   Removing Item from Storage 

TODO:Only an admin can remove an item from storage? Ask alex 


Light statuses 

\+ Red = Startup, Updating, Shutdown + Red pulsing = Busy + Orange = Alert, Ready + Orange pulsing = Alert, Running \+ Green = Ready + Green pulsing = Running Program
"

### Administration
## Robot
### Startup and shutdown
"
Starting up
Press the centre button on  the EV3, and the red light will turn on. Wait until light turns green.
"
"
Shutting down
Press the separate button on the top left corner to bring up the power options. Select 'Shutdown'.
>> sudo poweroff
Can be used instead.
"
"
Restarting the robot
Press the separate button on the top left corner to bring up the power options. Select 'Reboot'.
"

### Batteries and charging
[pictures of swapping batteries]

## System expansion
### Additional robots
Repeat the same steps from [Robot connection](##Robot-Connection).
### Additional storage
Buy additional shelving units.
Follow the [Line Network Setup](#line-network-setup) guide for adding them.
Fill the shelving unit with empty shelves.
Update the map using the admin user.
### Additional drop-off points
Buy additional drop-off points.
Follow the [Line Network Setup](#line-network-setup) guide for adding them.
Update the map using the admin user.
### Line network expansion
Follow the [Line Network Setup](#line-network-setup) guide for adding them.
Update the map using the admin user.

# Troubleshooting
> Are all key potential problems identified?
> Are useful solutions presented?

## The robot doesn't turn on

If the robot doesn't turn on then it's a battery fault most likely. In this situation proceed to do the following steps:

- Remove batteries from the EV3 Brick;
- If the batteries are normal AA batteries:
-- Replace them with new AA batteries.
- If the batteries are part of the EV3 Rechargeable Battery pack:
-- Check the [EV3 User Guide Page 8](https://abc.com), EV3 Brick section to read on how to recharge and install the batteries.

## Server is shut off

Robot will stop and say *"Lost connection to server"* four times and then stay idle. 
Then the [Robot Connection](#robot-connection) instructions can be run again after the server is turned on.


## Request is not being processed
- No available robots to process the request
- If the request is a retrieve request, it won't be processed if the item requested in on loan. The retrieve request will then be changed to a transfer request when the user that has the item that is on loan makes a store request.


## Robot fails to establish a connection with the server
- Make sure the server startup instructions were followed correctly before trying to do the robot connection.
- Ensure the server and the robot are in the same local network.

# other
-   Object collision prevention reset 
-   Battery warning/replacement 
  
-   At set level the robot will emit a noise??? Yet to be determined. 

## Recovering lost robot 
If the robot is still working you can locate it from the map tab.
Otherwise, check the error log for the last seen location of the robot.

## Robot stopped while processing request
Check request history to determine which request was being processed by the robot.
If the item the robot was holding needs to be stored back, it can be done by issuing a *store* request for that item from any workstation s
-   Wobbly Robot


