#game_engine_concepts #hrt_anniversary 

![[dog-rainy-dog.mp4]]

![[MTD_GEC_05_Graphics_Rendering.pdf]]

**Solution 1**  → Experiment

→ statistical analysis

**Solution 2** → Model

→ We could simulate “artificial users”

# Human Input

- for both solutions → Strategies
- Input Tasks
	- Text
	- Position
	- Selection
	- Quantify
	- etc

## Text

- Keyboard
- Gamepad 
- Knob
- Speech

### Typing Performance Measures

- Notation
	- Presented Text **P**
	- Transcribed Text **T**
	- Correct **C**

- Entry Rates
	- Completion Time
	- Keys per Seconds
	- Chars per seconds
	- WPM
- Error Rates
	- Error Rate


- Average Word per Minute
- Average Error Rate

## Position

- Mouse
- Trackball
- Joystick
- Touch
- Gestures

### Fitt’s Law

- Which targets are easy to reach and why?

```handdrawn-ink
{
	"versionAtEmbed": "0.3.4",
	"filepath": "Semester 1/Game Engine Concepts/Lectures/attachments/Ink/Drawing/2025.11.11 - 16.33pm.drawing",
	"width": 500,
	"aspectRatio": 1
}
```
- Near and Big Targets are easier to Reach

- Hard Fact in HCI
- Allows to predict pointing tasks
- allies only to liner movements
- training can improve performance

- Original Experiemtnt 1954
- only left right pointing tasks

- Undershooting
- Overshooting


### Implications for Game Interface Designs

- 5 positions
	- Hot Corners
	- current cursor position
- Position, arrangement, and size of context menu items
- growing targets
- pie menus
	- just 1px distance to the target
	- large menus

## Selection

- Choose object from a s et of alternatives
	- cognitively demanding
	- menus, lists
	- no input device
	- selection has impace on performace
	- Hick’s Law models selection task
		- time needed is proportional to log number of alternatives
		- does not apple to linear search → random list
		- alternatives must be related → must be obvious
		- **Consequence** → menus should be ordered


### Selection Performance

- Hick’s Law
	- used to decide how manus are arranged
	- lists should be ordered
	- icons make search easier → but not related to hicks law
	- if large list has sub lists, does not apple

- Simple Tasks
	- Time needed to select a colored button depending on the color of a light inceases with numbrer of options
- Martials arts
	- Time needed to block an attack with the number of blocking techniques
- Trafic signs
- Braking


## Hierarchical Task Analysis

- What is the analysis for
- define goals
- data
- draft hierarchical diagram
- evaluate the decomposition
- identify important operations
- design based on “Selecting / supporting touchpoints in space and time”

### Shooting a ball

- Find / Grab a ball
	- Finding a ball
		- Perceiving ball location
			- Look in environment
			- Look on map
		- Move to ball location
	- Grabbing the Ball
		- Bend Down
		- Pick ball
			- stretch arms
			- move fingers
			- reposition
- Throwing the ball
	- Selecting direction
		- look towards target
		- lunge with arm
		- orient arm
	- Throwing
		- Select Force
		- Throw
			- Move arm forwards
			- open hand

 > What are the users task?
 
 In Vr → all of these
 Keyboard / Mouse Games → less atomic tasks
 
# Game Engine Graphics & Rendering

## Relavance

- Foundation for graphical digital media and print media
- Foundation for special effects and computer-generated movies
- foundation for computer games

## Visual Perception

- Color → light waves
- #color 
## Graphics Pipeline

- Application → Geometry → Rasterization → Screen

- Application
	- Math
- Geometry
	- Geometry
	- Lighting
	- Transformations
	- etc
- Rasterization
	- generating 2D image from geometry
- Screen
	- final display

## Geometric Primitives

- Triangle Meshes
- vertices, edges, faces, polygons, surfaces


- Vertex is a point in 3D space
- Triangles consist of
	- 3 non collinear vertices interconned
	- 1 mormal vector
- optimization
- etc

- Right hand rule
- relationship between vertex order and normal vector is convention
- polygons can be stored in different ways

## Coordinate Systems

- 2D
	- Screen Coords
	- Viewport
- 3D
	- World
	- Object
	- hierarchical


- Left / right handed
- Geometrical transformation

## Simple 2D Transformation

- Matrix for scaling

## Projections

- same mechanisms and rules like cameras 
- Virtual Camera is in World Space

## Frustrum

- Frustrum describes a volume, which content is projected for rendering
- perspective projection
- parallel projection

## Perspective Projection

- Aspect Ratio
- Field of View

## Level of Detail

- Increase rendering performance by reducing the amounts of drawn triangles
- same 3d object with different resolutions
- depending on distance

## Scene Graph

- Abstraction layer for graphics programming
- 3 key areays
	- object representation
	- interactivity
	- architecture
- Directed Acyclic Graph
- for the concepts of nested transformations
- Structure
	- internal nodes are used for hierarchi building
	- leaf nodes contain geometry

- Hierarchy
- Culling
	- removing everything from a scene that does not contribute to the final iamge
- Bounding volume hierarcy
- file i/o


- Traversal of the scene graph
	- 3 phases
		- Update or application
		- Cull
		- Draw or Render

## Culling Techniques

- Frustrum Culling
- Backface Culling
- Occlusion Culling
- Contribution Culling

## Light and Light Sources

- Ambiant Light
- Directional Light
- Point Light
- Spot Light

## Illumination and Reflectance

- Illumination
	- Light Sources
	- Surface Properties

- Reflectance
	- ambient
	- diffuse
	- specular
	- phong #phong 

## Shading

- Flat or constant shading 
	- most simple and fast shading
	- triangle is assigned is a single color value
	- etc

- Gouraud Shading
	- calcuates the individual colros of vertices
- Phong Shading
	- normal vectors are interpolated
	- more realistic than gouraoud
	- requires more computation

## Textures

- adds surface details to a 3d model
- 2d images are mapped onto a 3d geometrical object

## Z-Buffer or Depth Bugger

- Algorithm
	- for eahc polygon in the frustrum
		- determin the pixels to be drawn
		- for each pixel to be drawn
			- compute the depth value z at the x, y position
			- if z(x,y) < current value (x,y) then current value (x,y) := z(x,y)
- Render image