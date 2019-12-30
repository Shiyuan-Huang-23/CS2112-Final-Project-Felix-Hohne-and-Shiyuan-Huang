## Fall 2019: Critterworld 
### CS 2112: Honors Object Oriented Design and Data Structures at Cornell University 
### by Shiyuan Huang and Felix Hohne 

___
**Abstract**

Our Final Project required approximately 10,000 lines of Java code. 

It involved creating a simulation of a world of animals called critters that can move around, eat food, attack one another, reproduce, and evolve. These critters move on a 2-D hexagonal grid; when they move or perform actions, they use energy. When they run out of energy, they die and become food for other critters. The genome of each critter includes a program that determines what each critter does each turn under specific conditions. When critters mate or bud, this program is copied over to its offspring, possibly with mutations. Depending on the situation, Manna from Heaven, food added at random intervals to random tiles, will be available to critters. 


We implemented: 

+ A Parser, Interpreter, and Fault Injector for a context-free grammar with 16 productions that represent the critter genomes
+ A simulator and Graphical User Interface to view the Critter World
+ A method that calculates the closest available food to a critter using Dijkstra’s Algorithm with a Priority Queue implemented using a Binary Heap
+ A multi-threaded, distributed application based on the Model-View-Controller design pattern that communicates over HTTP and using JSON, with the GUI running on a client computer and querying the server about the state of the world, and the model containing the simulation logic, parser, interpreter, and fault injector on a server and responding to client requests via HTTP

While we are unable to show our actual code in order to protect academic integrity, we provide several examples of features we implemented.

___
**Spiral Critter** 

Here is an example of a critter program we wrote that causes a critter to spiral on a map, with some slight changes to protect academic integrity. Our Parser parses the program written in the critter language, converts it into an Abstract Syntax Tree, then interprets the instructions based on conditions occurring in the world, as computed by the simulator. 

<img width="1059" alt="newSpiral" src="https://user-images.githubusercontent.com/58995473/71490416-12a92080-282b-11ea-9e7a-65bb0c230dd5.png">

The Spiral Critter in Action: 

![SpiralVideo](https://user-images.githubusercontent.com/58995473/71511468-c5f93000-2892-11ea-8c83-155208a09357.gif)

___
**Graphical User Interface**

A key part of our assignment was to visualize our simulation. When we originally wrote our simulation logic, we worked with a Command Line Interface. Here, the numbers represent critters and the direction they are pointing in, # represent rock tiles, F represent food tiles, and _ represent empty tiles. 

<img width="176" alt="NewCUIImage" src="https://user-images.githubusercontent.com/58995473/71511941-6a2fa680-2894-11ea-9877-7c727b47f639.png">

This naturally made it challenging to debug, test and appreciate the simulation in progress. We then developed a Graphical User Interface using JavaFX. This GUI shows the same scene as above: 

<img width="1279" alt="GUIVersion" src="https://user-images.githubusercontent.com/58995473/71512451-82a0c080-2896-11ea-99b8-67dfd68bf14f.png">

Green tiles represent food tiles with the number on them showing the quantity of food on that hex, light blue tiles represent empty tiles, arrows represent critters, and grey tiles represent rock hexes. We show the end of the world as an endless sea of rock hexes, so that users can never scroll off the edge of the map. 

In this Graphical User Interface, users can: 
+ Load custom worlds and critters they wrote using the File Menu
+ Read through a simple tutorial using the question mark button 
+ Run the simulation step by step or continuously using the buttons at the bottom left
+ Click on a critter and see information about its current state in a resizable sidebar
+ Track a selected critter through a highlight around its hex
+ View the number of turns that this simulation has run and the number of critters that are alive 
+ Distinguish between different critter species based on the critter sprite color

Here is our GUI in action: 

![ActionGUI](https://user-images.githubusercontent.com/58995473/71513716-ce099d80-289b-11ea-92cd-d947f9e8077f.gif)


___
**Dijkstra's Shortest Path Algorithm**

To demonstrate our understanding of Dijskstra’s Shortest Path Algorithm, part of the Data Structures and Algorithms section of our course, we used Dijkstra's Algorithm and a Priority Queue to enable a critter to find the closest food tile within a predetermined distance in-game, including the number of turns that would be required to eat food directly in front of it. This enables critters to be more strategic in looking for food.

Here is an example of Dijsktra’s Algorithm in action: 

![MazeCritterShortened](https://user-images.githubusercontent.com/58995473/71530069-4302c480-28e8-11ea-8a09-dca4618ea17c.gif)
___

**Distributed Application and Thread Safety**

The final core part of our project was separating our Graphical User Interface from the simulation code, allowing the two to be run separately. Because of our implementation of the Model-View-Controller design pattern, where the model has few, if any, references to the view and controller, this was relatively simple to accomplish; all that was required was to make the model code thread-safe using Java's ReentrantReadWriteLocks, so that multiple threads could read the simulation state at the same time, and re-route GUI model requests to the new server. 

As a demonstration, we run two GUIs at the same time, with different permissions. The top GUI has administrator access and can make modifications to the world, whereas the bottom GUI has read only access to the world and therefore has the play and pause buttons greyed out. The two GUIs can display the same underlying simulation state, except perhaps with possible minor timing differences due to the slightly staggered time of HTTP packets: 

![NewConcurrency2](https://user-images.githubusercontent.com/58995473/71531749-67fb3580-28f0-11ea-971b-20f156a80c97.gif)
