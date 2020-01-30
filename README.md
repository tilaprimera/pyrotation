# pyrotation
This repository is a Python package to help teach and learn the math of 3D rotation. It has two modules:

* [pyrotation_demo.py](https://github.com/duolu/pyrotation/blob/master/pyrotation/pyrotation_demo.py) - contains GUI based interactive demo of 3D rotation of reference frame.
* [pyrotation.py](https://github.com/duolu/pyrotation/blob/master/pyrotation/pyrotation.py) - contains the core class and routines for representation and operation of 3D rotation.

Both modules are tested on Python 3.5+.

See the [slides](https://docs.google.com/presentation/d/1Xv72RjoL_4scx613WTR3YDfh70VPxslJ42-XyDsqbac/edit?usp=sharing) to learn the representations of 3D rotation.

## pyrotation_demo.py

The **pyrotation_demo** module provides four interactive GUI visualizers based on the matplotlib, corresponding to the four representations of the 3D rotation implemented in the **pyrotation** module. Users can drag sliders to directly change parameters of the representation and the corresponding rotation of reference frame is shown in real-time. This demo is mainly made for learning how 3D rotation work.

This module requires numpy, matplotlib, and the **pyrotation** module. 


## pyrotation.py

The **pyrotation** module provides four different representations of a 3D rotation as well as corresponding operations on these representations:

* **Angle-axis** - represented as a numpy 3-dimensional vector **u**, where its direction is the axis and its length is the angle (the angle could be negative, which means the rotation uses the opposite direction of the vector). The rotation follows the right-hand rule.
* **Euler angles** (in z-y'-x" intrinsic convention) - represented as a tuple of three angles (z, y, x). These angles are also called Tait-Bryan angles, or (yaw, pitch, roll) angles. There are other conventions of Euler angles and only this specific notation is implemented and used in the demo.
* **Rotation matrix** - represented as a numpy 3-by-3 matrix **R**.
* **Unit quaternion** - represented as an object **q** of a custom quaternion class defined in the pyrotation module. The quaternion is in Hamiltonian convetion, i.e., (qw, qx, qy, qz).

This module requires numpy. In all cases, coordinate of a 3D point is represented as a numpy 3-dimensional vector. Array of 3D points is represented as numpy 3-by-n matrix, where n is the number of points. The coordinate reference frame is right-handed.


# Getting Started

## Installation

Just download pyrotation.py and pyrotation_demo.py and run.

## Basic Usage of pyrotation_demo

Put both **pyrotation.py** and **pyrotation_demo.py**

	$ python3 ./pyrotation_demo.py [mode]

The "mode" can be one of the following:

* "u" or "angle_axis" - angle-axis (default if mode is not given)
* "e" or "euler" - Euler angles (in z-y'-x" intrinsic convention)
* "r" or "R" or "rotation_matrix" - rotation matrix
* "q" or "quaternion" - unit quaternion

For example, if the following command is used,

	$ python3 ./pyrotation_demo.py q

Then the demo with quaternion is shown. 

Some common explanations of the demo GUI are shown as follows.

1. In all demoes, the axes uses this color settings.

	* the **x-axis** is red.
	* the **y-axis** is green. 
	* the **z-axis** is blue. 

1. The three axes of the original reference frame are represented in dashed lines with black and color alternation, and the rotated reference frame are represented in solid lines with colors. 

1. A solid disk on the XOY plane in the original reference frame is shown to represent the "ground"

### Demo of Angle-Axis Representation of a 3D Rotation

![angle-axis annotation.](figures/angle_axis_annotated.png) <img src="https://github.com/duolu/pyrotation/blob/master/figures/angle_axis.gif" width="350">

Explanations:

1. In the demo GUI, the rotation axis is represented in a dotted and dashed line through the origin. The rotation vector, i.e., the vector **u**, is shown as a solid black arrow on the axis. The projection of the axis on the ground is shown as a dotted line. 

1. Besides, a dashed circle is shown, where the rotation axis is through the center of the circle and perpendicular to the plane of the circle. Two dashed arrows are on this plane pointing from the center of the circle to the original and rotated x-axis. On the circle, a red arc with arrow shows the rotation angle, pointing from the original x-axis to the rotated x-axis. Together they show the conic shape of rotating a vector or point along an axis. 

1. Note that this axis is always throught the origin. An arbitrary rotation with an axis not going through the origin can be decomposed into a translation from a point on the axis to the origin, a rotation with an axis through the origin, and another translation back to the point.

1. The rotation angle and the rotation axis can be directly controlled by the three sliders. Note that to control the axis, alt-azimuth angles of the axis are used. Thus, even in the degenerated case, where the rotation angle is zero, the axis can still be defined using the alt-azimuth angles (the dotted line representing the projection of the axis on the ground is calculated by the azimuth angle, so even the axis is pointing to perpendicular to the ground, this dotted line is still defined and shown). The rotation angle is in `[-180, +180]` degrees, the alt angle is in in `[-90, +90]` degrees, and the azimuth angle is in `[-180, +180]` degrees.


### Demo of Euler Angles Representation of a 3D Rotation

![euler annotation.](figures/euler_annotated.png) <img src="https://github.com/duolu/pyrotation/blob/master/figures/euler.gif" width="350">

Explanations:

1. Yaw angle is shown as a blue arc on the ground, pointing from the x-axis of the original frame to the projection of the rotated x-axis on the ground.

1. Pitch angle is shown as a green arc, pointing from where the yaw angle arc ends to the rotated x-axis, i.e., from the tip of the projection of the rotated x-axis on the ground, which is shown as a dotted red line, to the tip of the rotated x-axis.

1. Roll angle is shown as a red arc in the rotated YOZ plane on a dashed circle, pointing from the tip of the projection of the rotated y-axis on the ground, which is shown as a dotted green line, to the tip of the rotated y-axis.

1. The projection of the z-axis of the original reference frame on the YOZ plane of the rotated reference frame is shown as a dotted blue line.

1. The three Euler angles can be directly controlled by the three sliders. All three angles are in `[-180, +180]` degrees. Even in the gimbal lock case, where the pitch angle is -90 degree or +90 degree, the yaw angle and the roll angle can be independently controlled, though their effects would be indistinguishable in this case.

### Demo of Rotation Matrix Representation of a 3D Rotation

![rotation matrix annotation.](figures/rotation_matrix_annotated.png) <img src="https://github.com/duolu/pyrotation/blob/master/figures/rotation_matrix.gif" width="350">

The three basis vectors of the rotated reference frame can be directly controlled (i.e., the three columns of the rotation matrix). Note that they are three orthonormal vectors and hence, two of them are enough to uniquely determine the rotation. This demo program provides three modes: (1) "ux-uy", i.e., manipulating the new x-axis and y-axis, (2) "uy-uz", i.e., manipulating the new y-axis and z-axis, (3) "uz-ux", i.e., manipulating the new z-axis and x-axis. It is generally difficult to directly manipulate 3D vectors through the UI, instead, this demo program uses alt-azimuth angles to control the direction of the basis vectors (they are unit vectors so direction would be enough and the length does not matter).


### Demo of Quaternion Representation of a 3D Rotation

![quaternion annotation.](figures/quaternion_annotated.png) <img src="https://github.com/duolu/pyrotation/blob/master/figures/quaternion.gif" width="350">

The four components of the unit quaternion can be directly controlled by the four sliders. Note that since it is a the four components are coupled and the user can not change one component without influence to others. This demo program provides two manipulations in general: (1) Changing the rotation angle while maintaining the axis, by manipulating qw, or (2) Changing the axis direction while maintaining the angle, by manipulating qx, qy, qz and setting qw to 0.


## Basic Usage of pyrotation

// TODO




