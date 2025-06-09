---
layout: post
title: "Scripting with blender"
description: "Getting Started with the Scripting Workspace and Blender 2.8 Python Scripting Superpowers for Non-Programmers"
date: 2023-03-21
tags: [blender, scripting, python]
image: https://s3.amazonaws.com/cgcookie-rails/uploads%2F1554405206315-1554405206315.png
author: OBX
---

## All Hail Autocomplete 
Some commands that Blender has can only be done through code and are not found in the interface. In addition, other things that do change in the interface (like scrubbing the timeline) don’t always give you something in the Info Editor that you can copy and paste. 

You can always use Google or the Blender API Docs to help you find the right command to do what you need, but often it’s easier to just find it using autocomplete. If you start typing an address in the Python Console, you can hit Ctrl+Space and Blender will show you all available ways to complete what you’ve written. It’s a great way to navigate the codebase and discover new features. 

![alt text](https://s3.amazonaws.com/cgcookie-rails/uploads%2F1554405205917-1554405205917.png)


The Text Editor also has a Ctrl+Space autocomplete feature, but it doesn’t work the same and is generally not that helpful. If it’s something you’d use often, I’d recommend grabbing  Jacques Lucke’s Code Autocomplete addon which will allow you to work much faster. 


---

1. Open Blender and switch to the **Scripting** workspace at the top.
2. You’ll see:
   - A **Text Editor** to write scripts
   - A **Python Console** to run commands interactively
   - The **Info Editor** (at the top bar), which shows the Python code for each action you perform in the UI

Tip: Use the Info Editor to reverse-engineer scripts by seeing what Python commands are run behind the scenes.

Here's a simple script that clears the scene and adds a cube at the origin.

```python
import bpy

# Delete all objects
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete(use_global=False)

# Add a cube at the origin
bpy.ops.mesh.primitive_cube_add(location=(0, 0, 0))
````

Paste this into the Text Editor and click **Run Script**. You should see a cube appear.

![TIP](https://s3.amazonaws.com/cgcookie-rails/uploads%2F1554405206969-1554405206969.png)


## Understanding `bpy`: Blender’s Python API

The `bpy` module is your interface to everything in Blender. Key parts of it include:

* `bpy.ops`: Access to Blender's operator system (e.g., adding objects, applying modifiers)
* `bpy.data`: Direct access to all data (meshes, materials, cameras, etc.)
* `bpy.context`: Info about the current state (what’s selected, active object, mode)
* `bpy.types`: Definitions for data structures, helpful for creating UI and custom tools

### Example: Move the Selected Object

```python
import bpy

obj = bpy.context.active_object
obj.location.x += 2
```

This moves the selected object 2 units on the X-axis.

---

## Creating a Custom Operator

Operators are functions that can be added to menus or buttons. Here’s how to make a simple one that adds a monkey head:

```python
import bpy

class SimpleOperator(bpy.types.Operator):
    bl_idname = "object.simple_operator"
    bl_label = "Add Monkey Head"

    def execute(self, context):
        bpy.ops.mesh.primitive_monkey_add()
        return {'FINISHED'}

bpy.utils.register_class(SimpleOperator)
bpy.ops.object.simple_operator()
```

This defines and runs a custom operator that adds the “Suzanne” monkey mesh.

---

## Building an Add-on

Add-ons are just Python scripts with a specific structure. Here's the basic boilerplate:

```python
bl_info = {
    "name": "My First Addon",
    "blender": (3, 6, 0),
    "category": "Object"
}

import bpy

class MySimpleOperator(bpy.types.Operator):
    bl_idname = "object.my_simple_operator"
    bl_label = "My Operator"

    def execute(self, context):
        bpy.ops.mesh.primitive_cube_add()
        return {'FINISHED'}

def register():
    bpy.utils.register_class(MySimpleOperator)

def unregister():
    bpy.utils.unregister_class(MySimpleOperator)

if __name__ == "__main__":
    register()
```

You can install this as an add-on from Blender’s preferences under the **Add-ons** tab.

---

