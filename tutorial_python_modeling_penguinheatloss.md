# Incremental Development of Agent Based Model: Penguin Heat Loss

This article will provide a guide to developing a mathematical model to see how penguins as a group will optimally arrange themselves that will minimize the amount of heat lost in a temparte environment like that artic.

This modeling exercise comes from an article in Quanta: Math of the Penguins(https://www.quantamagazine.org/math-of-the-penguins-20200817/) 

Professor David Meyer used the work done by Fancois Blanchette at the University of California, Merced, as an example and exercise to demonstrate in lecture the process of critically looking at the work of another researcher and trying to recreate and validate the results published and to find ways to simplify and extend on their work.

The demonstration done in lecture was made in Mathmatica and so this article will explain how this demonstration can be translated into Python and how to develop in general a agent-based computational model in python for other modeling applications. This exercise is a Monte Carlo simulation where there is a configuration and the changes to this configuration follows probabilistic rules that pushes into a direction that may lead to what we expect to see.

## Simulation Development Increment Outline
1. Outline and create the files and structures of the simulation. Design the sequence of computation and functions in each loop iteration.
2. Design a functioning agent-based model simulation with a single penguin/agent.
3. Create a draw/visualization component of the simulation that will generate visual representations of the system while simulation is running.
4. Implement a function in the simulation that will compute the heatloss of the individual penguin/agent
5. Implement a behavior function for the penguin/agent to determine next position it will move to in its environment
6. Implement a generator for adding additional penguins into the simulation
7. Refine the functions to reflect more accurately (or less arbitrarily) the behavior and computation in the simulation.

## Modules and Resources
This article will rely on the use of several Python modules to help build this model and generate a visual simulation to observe the changes to the penguins position and their movement during the duration of the simulation run. To generate the visual of the simulation we will use Pygame which is usually used to create games that have visual graphics that respond to users inputs and commands.  However, we will leverage Pygame more for its capabiliites for generating simple visual graphics of polygons and design the rules and parameters of the simulation to reflect the conditions identified in the article. 

While it is not entirely necessary to build this simulation as an agent-based model (ABM), this exercise will help to lay the foundation for building other ABMs that could be build to model other phenomena. The penguins in this simulation will be the agents and will be created as objects in the Python code. Thus, this exercise will be developed with an object oriented design focus. 

## Model Goals and Characteristics
This exercise is meant to recreate the conditions seen in the research video of penguins to see if a given set of rules can allow the penguins (agents) in our simulation to generate similar groupings or huddle shapes found in real life/video. Finding similar arangments can explain how penguins survive in the cold artic weather as a result of their ability to arrange themselves to help neighboring penguins minimize the lost of the heat and maintain their body temperature to survive.

(include further descriptions of possible behaviors and rules)

## Designing the Python Program
Choosing to create this simulation as an ABM using Python seems much more complex than the approach used by Professor Meyer who was able to create his model in Mathematica in much simpler code. But hopefully this article will walk through the steps and organization to help understand how to build an ABM in other contexts. 

Before we actually write the code for this simulation, we should spend much of our time thinking about how our simulation will work, what it will do, and the properties it needs to have so it runs the way we need in order to properly inform us of what we would like it to tell us.

### Penguin Agents
The individual penguins in this simulation will be the agents in our model and will be created as objects in our code. To execute this similarly to Professor Meyer and the author of the study, Francois Blanchette, the simulation will visualize each penguin agent as a hexagon. In addition the environment of our simulation will be a hexagonal grid, much like a game board with hexagonal tiles aligned vertically and horiztontally as depicted from the Quanta article:

(insert image from Quanta article)

In object-oriented programming for Python, the objects are instantiations of a certain class written with specific atttributes and behaviors/functions. We will need to think about what instrinsic properties each individual agent/penguin needs to have for it to operate within the simulation and characteristics that will allow it to interact with other agents and the environment, and functions that will dictate how it is meant to respond given parameters/conditions it is faced with. The basic attributes or properties each pegnuin should have is its location on the hexagonal grid represented by a pair of integers (we will go in to further details on how these values are generated and what they represent later). To help with keeping track of all the penguins/agents we might want to give each agent an unique ID to distinguish each agent in the simulation. More specific to our simulation, we would also like for the penguins to have its own body temperature that will change depending on its environment/neighbors. This value will be updated and changed through a calculation done at every time step of the simulation. 

At this time these three attributes (ID, location, temperature) are sufficient to get us started but we are able to and can add more later to better develop our simulation.

### Board/Environment Agent
The environment (or "board") that the agents will occupy and move around in can also be an agent in this simulation as well. This is an option that will add additional work and complicate programming but can be helpful later when we add more features to the model that we want to simulate. However, for now lets turn our attention to how our environment is designed and how we will use it. The unique aspect of this simulation by Francois Blanchette as seen in the image above is treating the penguins are hexagons packed adjacent to one another. Given this restrictive and simplified arrangment we would need to way to properly understand how to describe the location/arrangement of the penguins/agents in this layout of hexagons stacked along one another on any of its sides. Just imaging an entire space of hexagons we can see that there is some kind of vertical and horizontal alignment. This grid like arrangment can we used similarly to a cartesian coordinate system utilizing a pair of integers to identify the speicifc hexagon a penguinn/agent is occupying. while we might not visualize the enviornment as a grid like arrangement of hexagons, our agents will be constrained to moving solely on these hexagonal blocks as it travels from one locaiton to another. The main challenge is having Pygame draw the varous hexagons in the appropriate location to reflect the location nof the agents in the simulation. However, Pygame will require pasing it cartesian coordinate values to identify where to draw the shapes and objects of the simulation. Thus, to simplify our work we need to do we will adopt system based on  "Offset Coordinates" where the top left corner of of the grid represent the coordinates (0,0) and the bottom right corner will represented by the  the coordinates (n,n).

This will be a critical aspect of how our simulation will run so that at each "time step" as it updates the board/environment with the new location of the penguins/agents and their temperatures, there needs to be a proper system by which the functions we will write can easily identify if another agent is adjacent to another agent or if the any of the space around agent is empty and thus exposing the penguin/agent to the cold wind that will dramatically lower its body temperature. We will orient our hexagons horizontally such a pair of its sides that are parallel align horizontally flat (or has its greatest length along the horizontal). We will treat the "pointed" ends as either the "front" or "rear" of the penguin and the horizontal parallel side as the penguin's sides. With this orientation, lets say that we currently are at coordinates (3,4),  the hexagon directly above our current position will be (3,3), the hexagon along its diagonal up to the left will be (2,4), the hexagon along its diagonal down to the left is (2,5), the hexagon along its diagonal down tothe right is (4,5), and the hexagon along its diagonal up to the right is (4,4). Thus, the way we can easily check around every penguin is to use this scheme of (0,-1), (1,0), (1,1), (0,1), (-1,1),(-1,0) to iterate through the spaces in a clockwise order relative to the current location of the current penguin/agent. 

We needed to talk about this in detail jsut so there is no confusion moving forward about the coordinate system we are using and how our environment is laid out. However, we will only need to continue this discussion again once more when we reach the point of using Pygame to draw our penguin/agents.

## Simulation Functions 
We will need to write a few functions to compute aspects of our model we are interested in as well as functions that will help Pygame render the information of our model into a visual output. 

### Computing Heat Loss
One of the critical aspects of our simulation is routinely computing the individual temperatures of our penguins/agents at every "moment"/time step and updating it such that our penguins will respond and move accordingly to our model assumptions. Our heatloss function will look at every penguin/agent in our system  and scan the spaces surrounding it to see if there are any vacant spaces that does not have a penguin occupying it. Any spaces that is vacant will indicate a side of the penguin/agent that is exposed to the wind/cold air that will result in heat loss from its body. In class, Professor Meyer made a simple relationship between the side of the penguin that is exposed and the amount of heat loss. Any penguin with its "front" (pointed edges) exposed will have a heat loss of 4 for each of the 2 front sides/edges. This exposure is referred to as the upnwind heatloss. The "middle" (horizontal parallel sides or edges) exposed will have a heat loss of 2 for each of the middle sides/edges. The "back" (pointed edges) exposed will have a heat loss of 1 for each of the back sides/edges. The total amount of heat loss will be the sum all the sides exposure and their respective temupwind/mid/downwind exposure.

In our simulation we will need to write a function that will routinely iterate through all the penguin/agents and update the temperature of each penguin/agent at each time step. This is necessary since the heat loss will determine the new temperature of each penguin/agent which will then determine which penguin will need to move in order to avoid lossing more heat by finding a new location that has more neighbors to help it accumulate and retain heat. 

### Drawing the Penguins/Agent
The use of Pygame will help us visualize the changes of the simulation at each time step. While this isn't vitally necessary it does add an extra level of understanding the changes happening throughout our simulation and see how our model behaves. However, this extra layer of visualization does add a certain degree of complexity in our code. We will later discuss the various layers of of how our Python code will be organized and interacting to properly execute the simulation, but for now we will ease our way towards that direction by first talking about how we will draw the individual agents with Pygame. There are two ways to approach drawing the hexagons into the screen/window with Pygame: drawing a set of lines forming a hexagon or using the polygon function. Either approach requires passing in the coordinates of the hexagons vertices. AS a reminder, we will need to use cartesian coordinates, not the hexagonal coordinate system we discussed above. In addtion, Pygame uses a coordinate system where the top left corner is (0,0) and the bottom right corner is (n,m) thus the larger we make our window/environment the larger n increases in a positive manner even though the window is increasing its height downwards and its width is increasing postively to the right by m. The polygon function in Pygame needs the center of the shape, the color to fill the shape or color of the edges, and an ordered lists of cartesian coordinate values to draw the vertices and lines connecting vertice to another to draw each edge. 

The task before us is to compute the vertices that will outline the hexagon. This exercise should be done on paper and pen, and will be a simple refresher of high school math/geomtery. Mainly recall that a hexagon is a regular polgon with 6 sides of equal sides/edges. After some geometry you might remember that a hexagon can be partitioned into 6  equilateral triangles. We willuse some trigonometry to compute the (x,y) coordinates as we iterate around the center from 0 degrees to 360 degrees. Now that you have an understanding of the simple math our function will need to engage it, we need to write a function that will take in  the coordinates of the center of the hexagon, use a (for) loop to compute the (x,y) coordinate values using sine and cosine and output a list of 6 x and y values.

Once we create a function to generate our 6 coordinates for the vertices of our hexagon we will use that output and pass that data into the pygame.draw.polgyon() function to draw the edges of a single hexagon.  


### Generating Multiple Penguins/Agents
Our simulation needs more than one penguin/agent to demonstrate what we want to study. So in order to generate additional penguins/agents we can create a generator class to help add additional pegnuins to the simulation and keep track of all of them as they populate the board/environment. This extra class isn't necessary and the generation of additional penguins/agents can be done simply by declaring a set amount of penguins/agents when initializing the simulation but for other kind of agent based models and simulations creating a generator might be helpful. 

To create more than 1 penguin/agent without a generator would simply rely on having a variable that can be changed that indicates how many penguins/agents the simulation should initialize. Each pegnuin agent can be an index value in a list or dictionary containing the agents. The only complication is (randomly) distributing the penguins across the board/environment at the start/initial time (t=0). 

### Penguin Movement
In Professor Meyer's simulation, the penguin that moved was selected by probability weighted by a penguin's temperature. The colder (lower temperature) the penguin the more likely it would move and inversely the warmer (higher temperature) penguin would have a lower probability of moving. 

(insert code to compute the probability based on temperature)

In one complete run of a simulation Professor Meyer choose to have 2000 steps. This was a value that can be changed. He called this variable Nsteps = 2000.

As mentioned in class, this is a simplification of the real world dynamics and observation of the penguins as more than 1 penguin is moving at any given moment. But this simplication can be changed and updated to reflect something closer to the real world observation.

Another observation mentioned in class is that penguins in the middle might get too hot if they stay in the middle and might need to make their way out from the middle of the huddle.

### New Position
At each time step a penguin/agent selected to move (as a result of its temperature) will move in a another position. How this penguin/agent will move will need to be dictated by some rule based on our understanding of the real world observation and translated in some way into our code. In our simple approach to this action, our penguins/agents will take a step to another position in any of the six adjacent spaces around its current position. However, any (or all) of the spaces could be occupied by another penguin. So we would need to have our function check and make sure the new proposed position of our penguin is unoccupied. In addition, this initial version of the new position function will require that the new position have at least two penguins adjacent to our current position after moving to a new position (the idea being that moving toa new position with less than 2 penguins directly adjacent to it will result in seperating from the huddle entirely). The last condition is a probabilisitic. Of the viable choices to move the penguin (based on vacancy and number of adjacent penguins in the new position) new temperatures are computed at each potential position the penguin can move to. The difference between the new temperatures and the current temperature will determine a probability of that position being selected. A position resulting in a warmer temperature than the current will have higher probability and a position resulting in a colder temperature (Monte Carlo).

So to write this function we will select a random position from the six possible positions surrounding the penguin. Recall that the adjacent hexagons in clockwise order relative to the current position (x,y) are: (x+0,y-1), (x+1,y+0), (x+1,y+1), (x+0,y+1), (x-1,y+1),(x-1,y0) 


        def count_adjacent_penguins(position):
          """Function to count the number of adjacent penguins surrounding a position on the board"""
          candidate_position = [(position.x+0, position.y-1), (position.x+1, position.y+0), (position.x+1, position.y+1), (position.x+0, position.y+1), (position.x-1, position.y+1),(position.x-1, position.y+0)]
          count = 0
          for i in candidate_position:
            if (board.check_vacant(i)==1):
              count+=1
          return count

        def new_position(self):
          #Get coordinates of all six spaces surrouding current penguin
          candidates=[] #list of viable positions after screening
          possible_spaces = [(self.x+0,self.y-1), (self.x+1,self.y+0), (self.x+1,self.y+1), (self.x+0,self.y+1), (self.x-1,self.y+1),(self.x-1,self.y+0)]
          #Check if any of the spaces in possible_spaces is occupied, only retain vacant spaces
          for i in possible_spaces:
            if(board.check_vacant(i) == 0):
              #if its vacant and has at least two (>1) penguins adjacent keep it
              if(count_adjacent_penguins(i)>1):
                candidate.append(i)

          tmp=[]  
          #Compute the potential temperature at the remaining positions
          for i,x in enumerate(candidates):
            tmp1 = compute_temp(x)
            #How the probabilites are computed can be up to you to be simple or complex
            tmp.append(compute_temp(x))


      
    
    
  


## Simulation Mechanics
Professor Meyer's simulation 


At this time these three attributes (ID, location, temperature) are sufficient to get us started but we are able to and can add more later to better develop our simulation.
