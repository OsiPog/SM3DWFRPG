# SM3DWFRPG
A Super Minimal 3D Wireframe Renderer in Pygame

## About

I wrote this renderer to visualise my [Pi-day contribution](https://github.com/OsiPog/pi-cube-sphere) 2022. It is capable of drawing the wireframe versions of cubes and spheres such as points. The primitives cube and sphere can be rotated around the y-axis.

## Installation

Download "SM3DWFRPG.py" and add it to your project or add this repo as a dependency. It is recommended to import it using `from SM3DWFRPG import *` as it a very small renderer which will not litter your project with functions and classes as it only consists of two classes. If you insist on importing it as a package I recommend you to use `import SM3DWFRPG as mwfr`


## Documentation

### Classes

#### `Vertex`

`Vertex` is a class which can be used all around the project, it describes a 3-dimensional point such as vectors. 

##### Public Variables and Parameters

| Variable        | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `x` (parameter) | the x-position of the Vertex <br />*default:* `0.0`          |
| `y` (parameter) | the y-position of the Vertex <br />*default:* `0.0`          |
| `z` (parameter) | the z-position of the Vertex<br />*default:* `0.0`           |
| `c` (parameter) | the colour of the Vertex used when it is utilised in a drawing context <br />*default:* `(255, 255, 255)` |

*Initialising:* `Vertex(x, y, z, c)`



##### Methods

###### `Vertex.add(v)`

The x, y and z values of another vertex `v` will be added to a copy of the vertex this method is called from. This copy is returned.

###### `Vertex.become(v)`

The x, y and z values of the vertex this method is called from will *become* the x, y and z values of the vertex `v`.

###### `Vertex.distance_to(v)`

This method returns the distance from the point represented by the vertex this method is called from to the point represented by the vertex `v`.

###### `Vertex.copy()`

This method returns a copy of the vertex this method is called from.

#### SM3DWFRPG

This class should only exist once in the runtime. It is responsible for managing the pygame window and drawing on it.

##### Public Variables and Parameters

|Variable| Description                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| `SCREEN_RES` (parameter: `screen_res`) | This resolution of the pygame window represented as a tuple of 2 integer values. This value is constant and changing it on runtime will result in warping and translation of the image. <br />*default:* `(500, 500)` |
| `FOV` (parameter: `fov`)               | The field of view of the 3D-camera. This value is constant and changing it will not change anything. The parameter (`fov`) is using degrees for usability but this value will be converted to radians on assignment of the constant (`FOV`). <br />*default:* `70.0` |
| `bg_color` (parameter)                 | The background colour of the pygame window represented as a tuple of 3 integer values.<br />*default:* `(255, 255, 255)` |
| `offset` (parameter)                   | The 3-dimensional position of the camera (the *offset* from the point $P(0;0;0)$) represented by an instance of the `Vertex` class.<br />*default:* `Vertex(0.0, 0.0, 0.0)` |
| `render_queue`                         | An array of points represented by instances of the `Vertex` class or lines represented by tuples of 2 instances of the `Vertex` class. It can be interacted with though the basic functions such as adding a point or a line are taken care of by methods of this class.<br />*default:* `[]` |

*Initialising:* `SM3DWFRPG(screen_res, bg_color, fov, offset)`



##### Methods

###### `SM3DWFRPG.draw()`

This method will update and draw all contents of the `render_queue` list to the window and it has to be called continuously for this renderer to function as intended. It iterates through the whole `render_queue` list, if the element is a vertex the colour will be taken from the ` c` variable and if the element is a tuple of two vertices the colour will be taken from the first vertex. Afterwards `render_queue` will be emptied.

###### `SM3DWFRPG.convert_to_2d(v)`

This function takes a 3D vertex and converts it to a point on the screen using the `FOV` and the `offset`. The vertex `v` is multiplied by the projection matrix which is defined as:
$$
M_{projection} = \left(\begin{matrix}\frac{height}{width} *\frac{1}{\tan(\frac{fov}2)} & 0 & 0 \\ 0 & \frac{1}{\tan(\frac{fov}2)} & 0 \\ 0 & 0 &1\end{matrix}\right)
$$
To get the final x and y-value divide the both by the remaining z-value.

###### `SM3DWFRPG.point(point)`

This method adds a given point (`Vertex`) to the current `render_queue`. It will draw this point the next time `draw` is called. The colour will be taken from the from the vertex.

###### `SM3DWFRPG.line(p0, p1)`

This method adds a tuple of two points (`p0` and `p1`) to the current `render_queue`. It will draw these two points as a line the next time `draw` is called. The colour will be taken from the first vertex.

###### `SM3DWFRPG.cube(point, a, rot_y = 0.0, color = (255, 255, 255))`

This method adds a set of lines which represent a cube to the `render_queue`. The cube will have the colour given in the `color` parameter, the origin given the parameter `point` (`Vertex`), the side length `a` and will be rotated around it's origin by `y_rot` radians.

######  `SM3DWFRPG.sphere(self, point, r, rot_y = 0.0, color=(255,255,255))`

 This method adds a set of lines which represent a sphere to the `render_queue`. The sphere will have the colour given in the `color` parameter, the origin given the parameter `point` (`Vertex`), the radius `r` and will be rotated around it's origin by `y_rot` radians.
