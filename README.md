# Unity-Shader-Graph-Tutorial
 
 
# Introduction to the shader programming language
 
 
I have been able to identify at least four fundamental areas that facilitatethe understanding of shaders and their structure, such as:

* properties of a polygonal object
* the structure of a render pipeline
* matrices
* coordinate systems
 
 ### Primitive
 A primitive is a three-dimensional geometric object formed by polygons and is used as a predefined object in different development software. Within Unity, Maya or Blender, we can find other primitives. The most common are: Spheres, Boxes, Quads, Cylinders and Cpsules.

### Mesh:
 Have vertices, tangents, normals, UV coordinates and color, which are stored within a data type called “mesh” 
We can access all these properties independently within a shader and keep them in vectors (e.g. float4 pos: POSITION [n]).

## Vertices

The vertices of an object, corresponding to the set of points that define the area of a surface in either a two-dimensional or three-dimensional space. In Maya and Blender, the vertices are represented as the intersection points of the mesh and an object. 

Two main things characterize these points:
*  They are children of the transform component.
*  They have a defined position according to the center of the total volume of the object.


![Vertices](git-images/Vertices.png?raw=true "Preview Image")

 
 
 
 
## What's a Shader?
 
 A Shader is a user-defined program designed to run on some stage of a graphics processor
 
# Lighting, shadows, and surfaces

# Compute shader, ray tracing and Sphere tracing

