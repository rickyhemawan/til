# Vector 2D Basic Movement

[Vector2D Documentation](https://docs.godotengine.org/en/stable/classes/class_vector2.html?#method-descriptions)

For **Basic movement** use `move_toward(param1, param2)`. 
`param1` is `Vector2D` which is the target location
`param2` is `float`	which is the speed or `delta`

**Then use** `move_and_slide(param1)`.
`param1` is `Vector2D`
This method also return `Vector2D`
What this function does is basically register location changes towards the character

Example:
```
extends KinematicBody2D

var velocity = Vector2D.ZERO
const ACCELERATION = 500
const FRICTION = 500
const MAX_SPEED = 80

func _physics_process(delta):
	# Listen for key press @ 60 fps
	var input_vector = Vector2.ZERO
	input_vector.x = Input.get_action_strength("ui_right") - Input.get_action_strength("ui_left")
	input_vector.y = Input.get_action_strength("ui_down") - Input.get_action_strength("ui_up")
	
	# Assigning Velocity
	velocity = velocity.move_toward(Vector(1.2, 2.1) * MAX_SPEED, ACCELERATION * delta)
	velocity = move_and_slide(velocity)
```
This code is fine if the character can only move horizontal or vertical such as `(x,0)` or `(0,y)`. The Problem occurs when moving diagonally such as `(x,y)` position. For instance:
Move `(2,0)` means going to the right for 2 pts. (Left Negative, Right Positive)
Move `(0,2)` means going bottom 2 pts. (Top Negative, Bottom positive)
Move `(2,2)` means going bottom right **2$\sqrt{2}$ pts**. Because of **Pyhtagoras Theorem** z<sup>2</sup> = x<sup>2</sup> + y<sup>2</sup>

To make it going bottom right for 2 pts its need a built in function called `normalized()`
Example:
```
input_vector = input_vector.normalized()
```