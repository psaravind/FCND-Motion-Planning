## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`

`motion_planning.py` implementes a state machine as shown below ![Finite State machine for mition planning](./misc/motion_planning.png).
Each state is represented by a node, edges show the transistions from one state to another. The states defined in lines 15 to 22 is the status of the drone that is waiting to execute a transition. 

States | Description
------- | -----------
MANUAL | This is the initial state of the drone, where the drone could be manually moved
ARMING | In this state the drone is armed and taken control by the module for path planning to reach a goal
PLANNING | Drone is in planning state and finds the waypoints to reach the goal
TAKEOFF | Drone takes off to go to next way point
WAYPOINT | Drone is at a way point
LANDING | Drone lands at a waypoint or goal
DISARMING | Drone is disarmed to release control

The transitions which are set of actions from one state to another are done in lines 61 to 72 and their corresponding implementations are in lines 74 to 107.

Transition | Description
------- | -----------
Manual | Drone is transitioned from DISARMING state to MANUAL state
Arming | Drone is transitioned from MANUAL state to ARMING state, the drone is armed and taken control by the module
Take off | Drone is transitioned from PLANNING state to TAKEOFF state with drone going to target altitude
Waypoint | Pops the next waypoint from the stack and sets the drone target to go to next waypoint
Landing | Drone reached its goal and is prepared to land
Disarming | Drone is disarmed and released control

`planning_utils.py` implements  
create_grid
valid_actions
a_star
heuristic
prune_path


### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here students should read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. Explain briefly how you accomplished this in your code.


And here is a lovely picture of our downtown San Francisco environment from above!
![Map of SF](./misc/map.png)

#### 2. Set your current local position
Here as long as you successfully determine your local position relative to global home you'll be all set. Explain briefly how you accomplished this in your code.


Meanwhile, here's a picture of me flying through the trees!
![Forest Flying](./misc/in_the_trees.png)

#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Minimal requirement here is to modify the code in planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2), but more creative solutions are welcome. Explain the code you used to accomplish this step.

#### 6. Cull waypoints 
For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.



### Execute the flight
#### 1. Does it work?
It works!

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  
# Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.


