# little-corner
This is a script for Unity that should allow you to replace all nested instances inside a parent object, with new user selected ones. I made this script because as of right now, Unity does not support the ability to update prefabs that are parented to other prefabs, due to their references being broken.
https://feedback.unity3d.com/suggestions/editor-nested-prefabs

To circumvent the issue until they are supported, this script can be used to identify all unique instances inside a parent object by name. If the instances are named as such:

Instance(Clone)(01)

Instance(Clone)(02)

Instance(Clone)(03)

Instance(Clone)(04)

...

Instance(Clone)(112)

then the script should delete the last set of parentheses containing numbers from the instance's name, so they all appear as:

Instance(Clone)
Instance(Clone)
Instance(Clone)
Instance(Clone)

Then it will list an array of all the unique instances inside the inspector, alon with providing you an empty array of matching size. You can use this array to select the prefab you wish to use to replace the instance of the same index. Once the instances are replaced, you can update the parent prefab.

I've already updated this script several times, but I can't guarantee that I've ironed out all the quirks. I mainly used this script to replace all the tiles in a 2D game room object. Every room contains hundreds of nested game objects, comprised of 4-12 prefabs.

This script will be useful to you if: 
-you want to replace all the nested instances inside a parent object
-your parent object has dozens and dozens of instances
-for the most part, your instances come from the same prefab

-sketchygio/inkgio
