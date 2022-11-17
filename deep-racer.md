# Deep Racer Guide 
## Parameters 
### Position on Track
* `x` and `y` 
* measured in meters 

### Heading 
* `heading` describes orientation of vehicle in degrees 
* measured counter-clockwise from X-axis
* 0 = $\rightarrow$
* 90 = $\uparrow$
* 180 = $\leftarrow$
* -90 = $\downarrow$

### Waypoints 
* `waypoints` is the ordered list of milestones placed along the track center
* Each waypoint is a `[x, y]` pair of coordinates in meters, measured in the same coordinate system as the car's position

![Waypoints in Amazon Deepracer](images/deepracer-waypoints.png)

### Track Width
* `track_width` is the width of the track

### Distance from center line 
* `distance_from_center` measures displacement of the vehicle from center of track
* `is_left_of_center` is a boolean that tells whether the vehicle is to left of the center line of the track

### All wheels on track
* `all_wheels_on_track` is
  * true if all 4 wheels of the vehicle are inside the track borders
  * false if any wheel is outside of the track 

### Speed 
* `speed` is measured in meters per second

### Steering Angle 
* `steering_angle` measures the steering angle of the vehicle measured in degrees 
* value is *negative* if steering right 
* value is *positive* if steering left 

![](images/deepracer-steering.png)

## Example of Reward Function 
### Stay on Track
Simply rewards the car for staying on the track, which is determined using the parameters `all_wheels_on_track`, `track_width`, and `distance_from_center`
```python 
def reward_function(params):
    '''
    Example of rewarding the agent to stay inside the two borders of the track
    '''

    # Read input parameters
    all_wheels_on_track = params['all_wheels_on_track']
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']

    # Give a very low reward by default
    reward = 1e-3

    # Give a high reward if no wheels go off the track and
    # the agent is somewhere in between the track borders
    if all_wheels_on_track and (0.5*track_width - distance_from_center) >= 0.05:
        reward = 1.0

    # Always return a float value
    return float(reward)
```

### Follow Center Line 