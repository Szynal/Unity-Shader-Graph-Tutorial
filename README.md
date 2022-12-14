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

![Vertices](git-images/Vertices.png?raw=true "Vertices")

 
## Normals

 Let’s imagine that we have a blank sheet of paper, and we ask a friend to draw on the front face of the sheet. How could we determine which is the front face of a blank sheet if both sides are equal? This is why normals exist. A normal corresponds to a perpendicular vector on the surface of a polygon which is used to determine the direction or orientation of a face or vertex.

In Maya, we can visualize the normals of an object by selecting the property vertex normals. It allows us to see where a vertex points in space and determines the hardness level between the different faces of an object.
 
 ![Normals](git-images/Normals.PNG?raw=true "Normals")
 
## Tangents

According to Unity official documentation: 

Tangents are mostly used in bump-mapped Shaders. A tangent is a unit-length vector that follows Mesh surface along horizontal (U) texture direction. Tangents in Unity are represented as Vector4, with x,y,z components defining the vector, and w used to flip the binormal if needed.

Unity calculates the other surface vector (binormal) by taking a cross product between the normal and the tangent, and multiplying the result by tangent.w. Therefore, w should always be 1 or -1.

 
 The letters "U" and "V" denote the axes of the 2D texture because "X", "Y", and "Z" are already used to denote the axes of the 3D object in model space, while "W" (in addition to XYZ) is used in calculating quaternion rotations, a common operation in computer graphics.
 
  ![Tangents](git-images/Tangents.png?raw=true "Tangents")
  
 ## UV coordinates 
 
 
 We have all changed the skin of our favorite character for a better one. UV coordinates are directly related to this concept, since they allow us to position a two-dimensional texture on the surface of a three-dimensional object. These coordinates act as reference points, which control which texels in the texture map correspond to each vertex in the mesh.
 
   ![UV](git-images/UV_01.png?raw=true "UVs")
   

The process of positioning vertices over UV coordinates is called “UV mapping”. It is a process by which UV that appears as a flattened, two-dimensional representation of the object’s mesh is created, edited, and organized. Within our shader, we can access this property, either to position a texture on our 3d model or to save information in it.


### The area of the UV coordinates is equal to a range between 0.0f and 1.0f, where “zero” means the starting point and “one” is the endpoint.

  ![UV](git-images/UV.png?raw=true "Graphic reference to the UV coordinates in a cartesian plane")
  <br />
 Graphic reference to the UV coordinates in a cartesian plane
 

## Vertex color

When we export an object from 3D software, it assigns a color to the object (vertex color) to be affected, either by lighting or replicating another color. 


## Render pipeline architecture

In current versions of Unity, there are three types of rendering pipeline: 

* Built-in RP,
* Universal RP (called Lightweight in previous versions),
* High Definition RP.

A pipeline is a series of stages that perform a more significant task operation. So what does rendering pipeline refer to? Let’s think of this concept as the complete process that a polygon object must take (e.g. object with extension .fbx) to be rendered onto our computer screen; it is like an object travelling through the Super Mario pipes until it reaches its final destination.

So, each rendering pipeline has its characteristics, and depending on the type we are using. Unity divides this architecture into four stages: 
* Application,
* Geometryprocessing,
* Rasterization,
* Pixel processing

Please note that this corresponds to the basic model of a render pipeline for real-time rendering engines. Each of the mentioned stages has threads that we will define next:

   ![UV](git-images/pipeline_architecture.png?raw=true "Render pipeline architecture")


## Application Stage
The application stage starts at the CPU and is responsible for various operations that occur within a scene, e.g., 
* Collision detection,
* Texture animation,
* Keyboard input,
* Mouse input, and more.

Its function is to read the stored data in memory to generate primitives later (e.g. triangles,lines, vertices). At the end of the application stage, all this information is sent to the geometry
processing phase to generate the vertices’ transformation through matrix multiplication.


## Geometry processing phase


The CPU requests the images that we see on our computer screen from the GPU. then  geometry processing phase occurs on the GPU and is responsible for the vertex processing of our object. This phase is divided into four subprocesses which are: vertex shading, projection, clipping and screen mapping.

  ![Geometry processing phase](git-images/Geometry_processing_phase.png?raw=true "Geometry processing phase")


When the primitives have already been assembled in the application stage, the vertex shading, more commonly known as the vertex shader stage, handles two main tasks:

1. It calculates the position of the vertices of the object.
2. Transforms its position to different space coordinates so that they can be projected ionto the computer screen.

Also, within this subprocess, we can select the properties that we want to pass on to the following stages. It means that within the vertex shader stage, we can include normals, tangents, UV coordinates etc.

  <br />
Projection and clipping occur as part of the process, which varies according to the properties of our camera in the scene. It is worth mentioning that the whole rendering process occurs only for those elements that are within the camera frustum, also known as the view-space


   ![Geometry processing phase](git-images/Geometry_processing_phase_02.png?raw=true "Geometry processing phase")

## Pixel processing stage

Using the interpolated values from the previous processes, this last stage starts when all the pixels are ready to be projected onto the screen. At this point, the fragment shader stage, also known as a pixel shader stage, begins and is responsible for the visibility of each pixel.
Basically what it does is compute the final color of a pixel and then send it to the color buffer.


   ![Pixel processing stage](git-images/pixel_processing_stage.png?raw=true "Pixel processing stage")
   
## Types of render pipeline


In Unity, the default rendering path corresponds to   `**forward rendering;** `  this is the initial path for the three types of pipeline render that are included in Unity. This is because it has greater graphics card compatibility and a lighting calculation limit, making it a more optimized process

   ![Types of render pipeline](git-images/rp.png?raw=true "Types of render pipeline")


Please note that in Universal RP, we can only use forward as a rendering path, whereas High Definition RP allows illuminated material rendering using either forward or deferred shading.

   ![Types of render pipeline](git-images/types_of_render_pipeline.png?raw=true "Types of render pipeline")


To understand this concept, we are going to suppose that we have an “object” and a “direct
light” in a scene. The interaction between the light and the object is based on two points,
they are.

1. Lighting characteristics.
2. Material characteristics.

The interaction between these two elements is called the lighting model.
The basic lighting model corresponds to the sum of three different properties, which are
ambient color, diffuse reflection and specular reflection.

## Forward rendering

Forward is the default rendering path and supports all typical features of a material (e.g. normal maps, pixel lighting, shadows, etc.). This rendering path has two different code written passes that we can use in our shader, the first, **base pass** and the **second additional pass**. 


As its name says, the additional pass can process “additional lights” per-pixel or also shadows that affect the object, what does this mean? If we have two lights in the scene, our object will be influenced only by one of them, however, if we have defined an additional pass for this configuration, then it will be influenced by both. 


One point that we must take into consideration is that each illuminated pass will generate aseparate **draw call**. A draw call is a call graphic that is made in the GPU every time we want to draw an element on the screen of our computer. 

### Draw call

These calls are processes that requirea large amount of computation, so they need to be kept to a minimum possible, even more so if we are working on projects for mobile devices.

To understand this concept, we are going to suppose that we have four Spheres and one directional light in our scene. Each Sphere, by its nature, generates a call to the GPU, this means that each of them will generate an independent draw call by default.

Likewise, the directional light influences all the Spheres that are found in the scene, therefore, it will generate an additional draw call for each Sphere, this is mainly because a second pass has been included in the shader to calculate the shadow projection, therefore, four Spheres, plus one-directional light will generate eight draw calls in total.


Having determined the base pass, if we add another pass in our shader, then we are going to add a new draw call for each object, and consequently, the graphic load will increase significantly.

There are some ways to optimize this process, which we will talk about later ^^  For now, we will continue the rendering path concept.

   ![Forward Rendering](git-images/forward_rendering.png?raw=true "Forward Rendering")

## Deferred shading

This rendering path ensures that there is only one lighting pass computing each light source in our scene, and only in those pixels that are affected by them, all this through the separation of the geometry and lighting. This figures as an advantage since we could generate a significant amount of light that influences different objects, thereby improving the fidelity of the final render but nominally increasing the per-pixel calculation on the GPU.

While Deferred Shading is superior to Forward when it comes to multi-light source computing, it brings with it some hardware compatibility restrictions and issues.


## Matrices and coordinates systems

In Unity the matrices represent a spatial transformation and among them, we can find: 

* UNITY_MATRIX_MVP.
* UNITY_MATRIX_MV.
* UNITY_MATRIX_V.
* UNITY_MATRIX_P.
* UNITY_MATRIX_VP.
* UNITY_MATRIX_T_MV.
* UNITY_MATRIX_IT_MV.
* unity_ObjectToWorld.
* unity_WorldToObject. 

   ![Matrices](git-images/unity_matrix.png?raw=true "Matrices")

The final value of each vertex in the object space is multiplied by a matrix known as the model matrix (UNITY_MATRIX_M), which allows us to modify the transformation, rotation and scale values of the vertices of an object. Every time that we rotate, change position or
scale our object then the model matrix is updated, but how does this process happen?

To understand this we are going to suppose that we have a Cube in our scene and we want to transform its values using a model matrix. We will start by taking a vertex of our Cube that is at position XYZW [0.5f, -0.5f, -0.5f, 1] relative to its center.


It should be mentioned that channel “W” in the previous example corresponds to a “homogeneous” system of coordinates that allow us to handle vectors and points uniformly. In matrix transformations, the W coordinate can have a value of “zero or one”. When W equals one (e.g. X, Y, Z, 1), it refers to a point in space, whereas, when it equals zero (e.g. X, Y, Z, 0), it refers to a direction in space. 



## What's a Shader?
 
 A shader is a small program with a “.shader” extension (e.g. color.shader), which can be used to generate interesting effects in our projects. Inside, it has mathematical calculations and lists of instructions (commands) that allow color processing for each pixel within the area covering an object on our computer screen.
 
 This program allows us to draw elements (using coordinate systems) based on the properties of a polygonal object. The shaders are executed by the GPU since they have a parallel architecture that consists of thousands of small, efficient cores designed to solve tasks simultaneously, while the CPU has been designed for sequential serial processing. 
 
 
 Note that Unity has three types of files associated with shaders. 
 
* Firstly, we have programs with the “.shader” extension that are capable of compiling in the different types of render
pipelines
* Secondly, we have programs with the “.shadergraph” extension that can only compile in either Universal RP or High Definition RP. In addition, we have files with the “.hlsl” extension that allow us to create customized functions; generally used within a node type called Custom Function, found in Shader Graph.
* There is also another type of program with the extension “.cginc” which we will review in detail later on. For now, we will limit ourselves to making the following association: “.cginc” is linked to “.shader” CGPROGRAM, and “.hlsl” is linked to “.shadergraph” HLSLPROGRAM. Knowing this analogy is fundamental because each extension fulfills a different function and is used in specific contexts.


   ![Shader](git-images/shader.png?raw=true "Shader")




 
# Lighting, shadows, and surfaces

# Compute shader, ray tracing and Sphere tracing

