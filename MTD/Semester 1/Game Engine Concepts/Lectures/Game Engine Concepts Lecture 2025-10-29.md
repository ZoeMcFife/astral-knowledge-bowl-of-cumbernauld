#game_engine_concepts #game_loops #physics

![[MTD_GEC_03_Game_Loop_and_Physics.pdf]]
# Game Loop

- the game loop is the central component of a game engine
- What is the Issue?
	- how to stop the loop?
	- loop???

```python
function main();
	loop forever:
		# do something
```

- loops run as fast as the comptuer can
- TIME 
- Which **speeds, rates, frequencies etc. are important?**

---
## Timelines

- Real-Time
	- measured by cpu timer
	- not always 100% accurate
- Game Time
	- independent of real time
- Global vs Local time
	- process in the engine have their own local timelines *(audio, animation, etc)*
	- those are mapped to the global time

 - we do not speak of times, but of **rates or frequencies**
 - **frames per second** 
 - physics update rate: ~ 120 per second
 - AI behaviors: ~ 1 per second (depends)

- early games did not consider time in their update loop
- typical way: **measure delta time** since last frame update in game loop
- more sophisticated: **running average** or **fixed update rate**

## Implementation

``` python
initialize last_time <- current_system_time_in_milliseconds()
initialize frame_counter <- 0

loop forever:
	current_time <- current_system_time_in_milliseconds()
	delta_time_ms <- current_time - last_time
	delta_time_s <- delta_time_ms / 1000.0
	
	if delta_time_s >= (1 / framerate):
		delta_time_s <- 1 / framerate
		
		# Game loop setps
		
		CheckUserInput()
		ApplyGamePhysics()
		Render()
		
		last_time <- current_time
		frame_counter <- frame_counter + 1
```

## Physics

- **an object in free fall**
	- y(t) = altitude at tie t
	- t = time
	- v0 = initial velocity
	- y0 = initial height
	- g = gravity

$$
y(t + 1) = v_0 t + y_0 - \tfrac{1}{2} g t^2
$$

- Two **major components**
	- **Collision detection** system
	- **rigid body dynamics** simulation
		- **Rigid body:** idealized, infinitely hard solid object (does not exist in reality)
		- calculates how rigid bodies move and interact with physical forces
	
- Combination of **collisions** and **physics**
	- bouncing between objects
	- sliding under friction
	- rolling until stop

- Do we even need physics?
- Physics can result in chaos
- physics does not per se guarantee fun

### Collision Detection

- determines when objects in the game world get into contanct
- therefore, each logical object is represented by one or more simple shapes, called colliders that may or may not intersect
- Colliders
	- data structure independent from an object’s represenation in game
	- simplicity is the key
	- are represented with a shape and a transform
	- the shape describes the general form of the collider
	- the transform positions and orients the shape in 3d space

### Collision and Physics World

- data structure with all colliders
- holding the **“collision world”** independenly increases performance
	- only collidable objects are stored
	- colliders do not need to be indexed
- the same concept applies for the **”physics world”**, which holds rigid bodies

### Convex and Concave Shapes

- **Convex shapes**: no ray starting within the shape will pass the surface more than once
- **Concave shapes:** there exist rays within the shape that pass the surface multiple times
- **Convex hull:** smallest convex shape around a concave object

### Intersections

- intersections drive collisions and physics
- **set-theory**: the subset of members existing in both sets
- **geometrical**: the set of all (infinite) points that lie within both shapes
- points vs sphere
- sphere vs sphere

- **Collision Primitives**
	- spheres
	- capsule
	- box
	- k-polytypes
	- convex volumes
	- polytype
- **Compound**
	- collection of collision primitives

- **Separating axis theorem**
	- calculation of potential overlaps of collider of projections on each 3d axis
- if there is an overlap ….

- **Minkowski difference** 
	- if the subtraction of all points in object A from all points in object B contains the in the diagram origin, they intersect
- can detect concave collisions
- precision is depending on the number of all included points

- collider hierachies
	- replace complex combinations with simple primites and only calculate details when the overarching primitives intersect

- collision queries
	- Ray Cast
	- Shape Cast
	- Phantom: Shape cast with zero distance
- Collision masks / layers
- collision callbacks
	- invoke certain functions when certain collisions are present

### Rigid Body Dynamics

- Classical rigid body dynamics: newtonian Mechanics
	- objects large enough to omit quantum effects
	- objects slow enough to omit relativistic effects
- Constraints collisions
	- in modern engines, the collision world and physics world are identical
- at each time step

- Metric units
- Physics distinguishes between
	- linear dynamics
	- angular dynamics
- center of mass
- force

### Rigid Body Angular Dynamics

- rotation around an axis
- angular speed constant rotation
- angular acceleration 
- moment of intertia: force needed to change the angular speed
	- mass near the axis: small moment of intertia
	- mass away from the axis
	- torque

### Constraints

- point constraints
- stiff spring
- hinge constraint
- prismatic constraint
- constraint chains
- rag doll
### Design Impacts

- **predictability**: animations and clips are more predictable
- **Control**: including physics makes the game less controllable
- **Unexpectedness**

### Engineering Impacts

- **UI**: how to control objects in the physical environment
- **AI**: actions and paths must be dynamic, as objects may block it anytime
- **Graphics**: glitches, objects getting stuck in each other, etc
- **Multiplayer**: where is the physics simulated (client vs server)
- **Debugging**: record / playback may not work properly
