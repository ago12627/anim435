# ANIM435 Scene Saver
usage:  
        export ONEDRIVE_PATH=[your_path_name]  
        maya  

        Then in Maya script editor, run assignment4.py, enter scene name, and press save button.

        (personal-use shell example in assignment4.sh)


This tool takes environment variable 'ONEDRIVE_PATH', displays a window to ask for a scene name, and saves current maya scene under the given scene name in the 'scenes' folder of 'ONEDRIVE_PATH'. 'ONEDRIVE_PATH' is assumed to be a working Maya directory with a standard 'scenes' folder. 

Hence the header, this tool works alongside 'assignment4.sh' to save scenes to my ANIM435 OneDrive Maya directory. However, this and other labels in the tool window are arbitrary and can be used with any Maya directory path.

Please note that this tool does not set project to given directory automatically.



Python code is shown below for additional reference:
```python
import os
import maya.cmds as cmds
from functools import partial

"""
Saves current scene in a directory (dir) given by the environment variable 'ONEDRIVE_PATH'

args: 
    name_field : Scene name from window text field
"""

def save_scene_in_dir(name_field, *args):
    dir_path = os.getenv('ONEDRIVE_PATH')
    if not dir_path:
        cmds.warning("Environment variable 'ONEDRIVE_PATH' not found. Please export.")
        return
        
    scene_name = cmds.textField(name_field, q=True, text=True).strip()
    
    scene_dir = os.path.join(dir_path, "scenes")
    scene_path = os.path.join(scene_dir, f"{scene_name}.mb")
    
    if not os.path.exists(scene_dir):
        cmds.warning(f"'scenes' folder not found in: {dir_path}")
        return
        
    cmds.file(rename=scene_path)
    cmds.file(save=True, type="mayaBinary")
    
    print(f"Save '{scene_name}.mb' in {scene_dir}")

# Window Set-up: displays simple window where user can input scene name and run save_scene_in_dir()

# for personal use, this script specifically is meant to take my ANIM435 OneDrive directory as 'ONEDRIVE_PATH' and save scenes there.
# However, the title and labels are arbitrary and can be used with any given Maya directory.

my_window = cmds.window(title="ANIM435 Scene Setup")
my_col_layout = cmds.columnLayout(parent=my_window)

my_row_layout_1 = cmds.rowLayout(numberOfColumns=1, parent=my_col_layout)
my_text_label = cmds.text(parent=my_row_layout_1, label="Scene Name: ")

my_row_layout_2 = cmds.rowLayout(numberOfColumns=2, parent=my_col_layout)
name_field = cmds.textField(parent=my_row_layout_2, text="temp")
my_button = cmds.button(parent=my_row_layout_2, label="Save Scene in ANIM435 OneDrive Directory", command=partial(save_scene_in_dir, name_field))

cmds.showWindow(my_window)
print(os.getenv('ONEDRIVE_PATH'))
```