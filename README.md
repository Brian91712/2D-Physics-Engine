# 2D Physics Engine
This is a simple and easy to use 2D Physics Engine programmed using the Skript interpreted language created by the SkriptLang team.
## Dependencies
- Skript-2.15+
- oopsk-1.0-beta2
- skript-reflect v2.6.3+
- [2D-Scene-Renderer](https://github.com/Brian91712/2D-Scene-Renderer)
## Install
With [pacskage](https://github.com/miberss/pacskage):
```
/package install https://github.com/Brian91712/2D-Physics-Engine
```
## Usage
First, create a `Scene` object using the `2D-Scene-Renderer` pacskage. Then, create a `PhysicalScene` object and attach the `Scene` to it. You can additionally define `substeps`, `time`, and `gravity`.
```applescript
set {_scene} to setup_scene(Scene struct instance):
  position: location(0.5, 4.5, 1.9999, world "world", 180, 0)
  scene_scale: vector(1, 1, 0)
  background_colour: rgb(0, 0, 0, 255)
set {_physical-scene} to PhysicalScene struct instance:
  scene: {_scene}
  substeps: 25 # The amount of iterations per tick. The higher it is, the more accurate the simulation will be. Optional value.
  time: 1 # The time scale. Optional value.
  gravity: vector(0, -200, 0) # The gravity vector. Optional value.
```
After you create a physical scene, you can start spawning bodies inside it using the `create_rigidbody(PhysicalScene struct, Rigidbody struct, position, scale, colour, rotation)` function. All the parameters after `Rigidbody struct` are optional. The `Rigidbody struct` section provides `kinematic`, `e`, `mass`, `velocity`, and `angular_velocity`.
```applescript
set {_platform} to create_rigidbody({_physical-scene}, Rigidbody struct instance, position: vector(0, -100, 0), scale: vector(300, 20, 0), colour: rgb(240, 183, 60)):
  kinematic: true # Makes it static.
# By the way, rotation and angular velocity are both in radians.
set {_box} to create_rigidbody({_physical-scene}, Rigidbody struct instance, position: vector(0, 100, 0), scale: vector(20, 20, 0), colour: rgb(19, 127, 146)):
  angular_velocity: pi/16 # Radians per second
  velocity: vector(0, 100, 0) # Pixels per second
  mass: 10
  e: 0.5 # Coefficient of restitution. As in, how bouncy this object is.
```
Finally, you can update the scene using the `update_physical_scene(PhysicalScene struct)` function. It returns how many milliseconds the step took to finish. The simulation also takes care of its own rendering to increase efficiency, so you don't need to call any functions from the `2D-Scene-Renderer`, and it is preferable not to do that, as it might cause issues.
```applescript
loop 100 times:
  set {_speed} to update_physical_scene({_physical-scene})
  send action bar "%{_speed}%ms" to all players
  wait 1 tick
```
