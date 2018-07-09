# FCND - 3D Motion Planning
![Quad Image](./misc/enroute.png)

# Content
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test and Bresenham method to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates.
7. Write it up.

# Setup
1. Download the Motion-Planning [simulator](https://github.com/udacity/FCND-Simulator-Releases/releases).
2. Set up [Python environment](https://github.com/udacity/FCND-Term1-Starter-Kit)
3. Clone this [Repository](https://github.com/udacity/FCND-Motion-Planning)
4. Execute ```Motion_planning.py ```

You're reading it! Below I describe the details of procedures in my project.

### Starter Code

The project start in `Motion_planning.py` and `planning_utils.py` is a package containing important algorithms.  `Motion_planning.py` includes
map normalization method，states transition method, waypoints production methond and communication method. `planning_utils.py` includes the A star path searching method,collinearity check method, points pruning method.

And here's a lovely image of my results
![Top Down View](./misc/mp1.png)

### My Path Planning Algorithm

#### 1. Set your global home position

Here I read the first line of the csv file. In Line 133 to Line 136 I use the split() method to divide the first line into fragments, then
extract the numeric string `37.792480` of lat0 and `-122.397450` of lon0 as floating point values and use the self.set_home_position() method to set global home.

#### 2. Set your current local position

The drone need to be able to takeoff from anywhere. I obtain the drone's current position in geodetic coordinates from self.global_position, and the global home position have being setted in last step. then I use global_to_local() to convert the current global position to local position.

#### 3. Set grid start position from local position

To convert start position to current position rather than map center，I use my_local_position minus north/east_offset to get the coordinate in grid map,and set it to value grid_start.

#### 4. Set grid goal position from geodetic coords

To adapt to set goal as latitude / longitude position, I use function global_to_local to convert global_goal to local_goal. Just like the start position,I use local_goal minus north/east_offset to get the coordinate in grid map,and set it to value grid_goal.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)

Modifying A* to include diagonal motion include 2 step,
step 1: In the class Action I add the  NORTHWEST, NORTHEAST, SOUTHWEST and SOUTHEAST which has a cost of sqrt(2) .
step 2: In the function valid_actions I add the rules of removing diagonal motion, those are the motion stepping out of the map or crash the obstacles in map.

#### 6. Cull waypoints

For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.
I have two method to cull different kinds of waypoints. The first is collinearity test in function collinearity_check() which test every three closest points. If the three points are collinear, then the middle point is removed. The second is Bresenham cross test in function cross() and pick_uncrossed(). I use random dichotomy to test whether the straight line of two points crash any obstacle, if not the points between the two will be all removed.

### Execute the flight
#### 1. Does it work?
It works!
