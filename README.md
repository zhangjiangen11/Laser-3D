# Laser 3D

**Laser 3D** is a self-contained Godot 4.5 node that provides a ready-to-use 3D laser beam with real-time collision detection and automatic visual adjustment.

It is designed to work both **at runtime** and **inside the editor**, offering instant visual feedback without running the game.

---

## Features

- Real-time laser beam with physics-based collision detection
- Automatic beam resizing based on hit distance
- Works in-game and inside the editor (`@tool`)
- Optional collision signal with detailed hit data
- Configurable visuals (color, radius, emission, visibility)
- Full control over collision layers, masks, and filters
- Optional target-based aiming (`look_at` support)
- No external scenes or dependencies (100% code-driven)

---

## Installation

1. Copy the plugin folder to your Godot project's `addons/` directory:

   ```bash
   addons/laser_3d/
   ```

2. Enable it in the **Godot Editor**:  
   **Project → Project Settings → Plugins → Laser3D → Enable**

---

## Basic Usage

1. Add a **Laser3D** node to your scene.
2. Rotate the node to define the laser direction, or assign a target.
3. Configure visual and physics properties in the Inspector.
4. (Optional) Connect to the `collision_detected` signal.

```gdscript
func _ready():
    $Laser3D.collision_detected.connect(_on_laser_hit)

func _on_laser_hit(result: LaserResult) -> void:
    print("Hit:", result.collider)
    print("Point:", result.collision_point)
```

---

## Typical Use Cases

- Weapons (lasers, turrets, sci-fi guns)
- Line-of-sight and visibility checks
- Traps and security systems
- Targeting and aiming helpers
- Sensors and detection beams

---

## Inspector Overview

### Laser / Visual

| Property               | Description                               |
| ---------------------- | ----------------------------------------- |
| `laser_type_visible`   | Controls when the beam is visible         |
| `laser_forward_offset` | Pushes the beam forward to avoid clipping |
| `laser_color`          | Laser color (albedo + emission)           |
| `laser_emission`       | Emission intensity                        |
| `laser_radius`         | Beam thickness                            |
| `laser_layers`         | Render layers used by the mesh            |

---

### Laser / Physics

| Property                                | Description                          |
| --------------------------------------- | ------------------------------------ |
| `laser_active`                          | Enables or disables laser logic      |
| `laser_exclude_parent`                  | Ignores the parent in collisions     |
| `laser_look_at_target`                  | Optional Node3D target               |
| `laser_look_at_position_offset`         | Offset applied to target position    |
| `laser_exceptions`                      | Physics collision exceptions         |
| `laser_exclude_from_the_results_report` | Colliders ignored by the signal      |
| `laser_length`                          | Maximum beam distance                |
| `laser_hit_from_inside`                 | Detect collisions from inside shapes |
| `laser_hit_back_faces`                  | Detect back faces                    |
| `laser_collision_mask`                  | Physics collision mask               |

---

### Laser / Physics / Collide With

| Property                    | Description          |
| --------------------------- | -------------------- |
| `laser_collide_with_bodies` | Detect PhysicsBody3D |
| `laser_collide_with_areas`  | Detect Area3D        |

---

## Signals

| Signal Name                                                    | Description                                                                                                                 |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `collision_detected(collision_result: LaserResult)`            | Emitted when the laser hits a valid object during runtime.                                                                  |
| `collider_changed(old_collider: Object, new_collider: Object)` | Emitted when the collider is changed. Useful when you need to know if the collider has moved out of the laser beam's focus. |
| `laser_visible_change(laser_visible: bool, collider: Object)`  | Emitted every time the laser beam becomes visible or invisible.                                                             |

---

**LaserResult contains:**

- `collider : Object`
- `collision_point : Vector3`
- `collision_normal : Vector3`
- `collision_face_index : int`
- `collider_rid : RID`
- `collider_shape : int`

---

## Public API

### Collision Exceptions

| Method                   | Description                   |
| ------------------------ | ----------------------------- |
| `add_exception(node)`    | Ignore collisions with a node |
| `remove_exception(node)` | Remove ignored node           |
| `clear_exceptions()`     | Clear all exceptions          |

### Signal Filtering

| Method                                               | Description             |
| ---------------------------------------------------- | ----------------------- |
| `add_laser_exclude_from_the_results_report(node)`    | Ignore node in signal   |
| `remove_laser_exclude_from_the_results_report(node)` | Remove from ignore list |

### Collision Query

| Method                            | Description                                                                                      |
| --------------------------------- | ------------------------------------------------------------------------------------------------ |
| `get_collider()`                  | Returns collided object                                                                          |
| `get_collision_point()`           | Returns hit position                                                                             |
| `get_collision_normal()`          | Returns hit normal                                                                               |
| `get_collider_rid()`              | Returns collider RID                                                                             |
| `get_collider_shape()`            | Returns shape index                                                                              |
| `get_collision_face_index()`      | Returns face index                                                                               |
| `get_collision_mask_value(layer)` | Checks collision mask layer                                                                      |
| `is_colliding()`                  | Returns whether any object is intersecting with the ray's vector (considering the vector length) |
| `is_laser_visible()`              | Returns laser is visible                                                                         |

---

## Aiming at a Target

You can optionally assign a `Node3D` target:

```gdscript
laser.laser_look_at_target = enemy
laser.laser_look_at_position_offset = Vector3(0, 1.5, 0)
```

If no target is set, the laser uses its forward direction.

---

## Editor Support

Laser3D provides a **live editor preview**:

- Beam updates instantly when properties change
- Works without running the game
- Ideal for level design and alignment

---

Watch the video: https://youtu.be/8Nyr0VzOIPc

---

## Compatibility

- ✅ Godot **4.5**

---

## 📄 License

MIT License.  
Free to use in personal and commercial projects.
