# CarND-Path-Planning-Project

### Goal
In this project the goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 50 m/s^3.

### Steps Followed:

#### Get Car State and Analyze Sensor Fusion data
The simulator provides the localization data of the main car which includes the cartesian coordiante (x,y), the frenet coordinate (s,d), the car velocity in cartesian (vx, vy).
This sensor fusion data along with the current state of car is used for path planning. The sensor fusion data contains the id, and state of the vehicle similar to the main car state. This data is anaysed to avoid collliding with vehicles in lane as well as decide when to change lane. Main importance is given to vehicle within 30 meters of the main vehilce in current as well as adjacent lanes.

#### Waypoint 
Widely spaced waypoints at distance of 30 , 60 and 90 meters from the vehicle are selected. These waypoints are shifted from global to local coordiante system with the main vehicle being at ceneter. Spline is fit to these waypoints.

The final path consistes of the unreached path from last iteration along with the path in front of the vehicle. Then based on the desired velocity, the waypoint x coordinates are spaced and the y coordiantes are calculated based on the spline fit. This generates the waypoint and trajectory to follow.

#### Avoid Collision and Lane Change
In order to avoid collision and decide when to change lane, the sensor fusion data is analyzed. The sensor fusion data is iterated to find vehicles in the same lane as the main vehicle and if found, the reference speed of the vehicle was gradually reduced. The change in reference speed affect how the waypoints are spaced out.

When the main vehicle found any car in the same lane and within 30 meters in front, it looks for vehicles in lanes adjacent to it. Based on the vehicle speed and location, their future location is predicted. If there is no vehicle in the adjacent lane within 30 meters in front as well as 15 meters behind it, then the main car assumes that its safe to change lanes to that lane.

Based on the lane change decision, the future points are added to the trajectory that take into consideration the lane change, as the selection of waypoints ahead of the car is selected based on the lane selected.
