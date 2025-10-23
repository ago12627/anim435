# Bouncy Ball Creator
usage: assignment3.py [-h] [-b BOUNCINESS]

options:

    -h, --help      show this help message and exit
    -b BOUNCINESS, --bounciness BOUNCINESS
                    Define the bounciness of the ball (float).

This tool creates a ball, adds "bounciness" attribute based on input field, animates rudimentary bounce via expression, and saves out a file under the name 'bouncyball.mb' in the current directory.
In the maya scene, ball will be named 'bouncyBall'. Bounciness attribute will alter bounce speed, but height will be fixed between 0 & 1 on the y-axis unless otherwise changed.



Python code is shown below for additional reference:
```python
import os
import argparse
import maya.standalone
maya.standalone.initialize()
import maya.cmds as cmds

def create_bouncy_ball(bounciness):

    ball = cmds.polySphere(radius=0.5, name="bouncyBall")[0]
    cmds.move(0, 1, 0, ball)

    if not cmds.attributeQuery('bounciness', node=ball, exists=True):
        cmds.addAttr(ball, longName="bounciness", attributeType="float", defaultValue=bounciness, keyable=True)
    cmds.setAttr(f"{ball}.bounciness", bounciness)
    
    expr = f"{ball}.translateY = abs(sin(time * {ball}.bounciness)) * 3;"
    cmds.expression(name=f"{ball}_bounceExpr", string=expr)

    print(f"Created a ball with bouncing animation ( bounciness = {bounciness} ).")


parser = argparse.ArgumentParser()
parser.add_argument('-b', '--bounciness', help="Define the bounciness of the ball (float).")
args = parser.parse_args()

print("Creating a bouncing ball.")

if not args.bounciness:
    bounciness = input("How bouncy should the ball be? (Standard value is 1.0)\n")
else:
    bounciness = args.bounciness

args.bounciness = float(bounciness)
create_bouncy_ball(bounciness=args.bounciness)

output_path = os.path.join(os.getcwd(), "bouncyball.mb")
maya.cmds.file(rename=output_path)
maya.cmds.file(save=True)
```
