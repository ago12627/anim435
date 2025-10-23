# Bouncy Ball Creator

This tool creates a ball, Adds "bounciness" attribute based on input field, and Animates rudimentary bounce via expression. 
Each ball will be named bouncyBall#. Bounciness attribute will alter bounce speed, but height will be fixed between 0 & 1 on the y-axis unless otherwise changed.

default bounciness value = 1.0




Python code is shown below for additional reference:
```python
import maya.cmds as cmds
import math
from functools import partial

my_window = cmds.window(title="Bouncy Ball Creator")
my_col_layout = cmds.columnLayout(parent=my_window)

my_row_layout_1 = cmds.rowLayout(numberOfColumns=1, parent=my_col_layout)
my_text_label = cmds.text(parent=my_row_layout_1, label="Ball Bounciness:")

my_row_layout_2 = cmds.rowLayout(numberOfColumns=2, parent=my_col_layout)
bounciness_field = cmds.floatField(parent=my_row_layout_2, value=1.0)
my_button = cmds.button(parent=my_row_layout_2, label="Create and Animate Ball", command=partial(create_bouncy_ball, bounciness_field))


def create_bouncy_ball(bounciness_field, *args):

    bounciness = cmds.floatField(bounciness_field, q=True, value=True)

    ball = cmds.polySphere(radius=0.5, name="bouncyBall")[0]
    cmds.move(0, 1, 0, ball)
    
    cmds.addAttr(ball, longName="bounciness", attributeType="float", defaultValue=bounciness, keyable=True)
    cmds.setAttr(f"{ball}.bounciness", bounciness)
    
    expr = f"{ball}.translateY = abs(sin(time * {ball}.bounciness)) * 3;"
    cmds.expression(name=f"{ball}_bounceExpr", string=expr)
    
    print(f"Created {ball} with bounciness = {bounciness}")

cmds.showWindow(my_window)
```
