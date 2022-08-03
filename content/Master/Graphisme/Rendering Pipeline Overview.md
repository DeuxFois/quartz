---
title: 7 Rendering Pipeline Overview
updated: 2021-12-30 18:17:32Z
created: 2021-12-30 17:32:53Z
source: https://www.khronos.org/opengl/wiki/Rendering_Pipeline_Overview
tags:
  - graphisme
  - s1
---

## Pipeline

[![Rendering Pipeline Flowchart](../../../_resources/RenderingPipeline_209ab5335c7a46e1ae5945f0f505f8c2.png)](https://www.khronos.org/opengl/wiki/File:RenderingPipeline.png)

Diagram of the Rendering Pipeline. The blue boxes are programmable shader stages.

The OpenGL rendering pipeline is initiated when you perform a [rendering operation](https://www.khronos.org/opengl/wiki/Vertex_Rendering "Vertex Rendering"). Rendering operations require the presence of a [properly-defined vertex array object](https://www.khronos.org/opengl/wiki/Vertex_Specification "Vertex Specification") and a linked [Program Object](https://www.khronos.org/opengl/wiki/Program_Object "Program Object") or [Program Pipeline Object](https://www.khronos.org/opengl/wiki/Program_Pipeline_Object "Program Pipeline Object") which provides the [shaders](https://www.khronos.org/opengl/wiki/Shader "Shader") for the programmable pipeline stages.

Once initiated, the pipeline operates in the following order:

1.  [Vertex Processing](https://www.khronos.org/opengl/wiki/Vertex_Processing "Vertex Processing"):
    1.  Each vertex retrieved from the vertex arrays (as defined by the VAO) is acted upon by a [Vertex Shader](https://www.khronos.org/opengl/wiki/Vertex_Shader "Vertex Shader"). Each vertex in the stream is processed in turn into an output vertex.
    2.  Optional primitive [tessellation stages](https://www.khronos.org/opengl/wiki/Tessellation "Tessellation").
    3.  Optional [Geometry Shader](https://www.khronos.org/opengl/wiki/Geometry_Shader "Geometry Shader") primitive processing. The output is a sequence of primitives.
2.  [Vertex Post-Processing](https://www.khronos.org/opengl/wiki/Vertex_Post-Processing "Vertex Post-Processing"), the outputs of the last stage are adjusted or shipped to different locations.
    1.  [Transform Feedback](https://www.khronos.org/opengl/wiki/Transform_Feedback "Transform Feedback") happens here.
    2.  [Primitive Assembly](https://www.khronos.org/opengl/wiki/Primitive_Assembly "Primitive Assembly")
    3.  Primitive [Clipping](https://www.khronos.org/opengl/wiki/Clipping "Clipping"), the [perspective divide](https://www.khronos.org/opengl/wiki/Perspective_Divide "Perspective Divide"), and the [viewport transform](https://www.khronos.org/opengl/wiki/Viewport_Transform "Viewport Transform") to window space.
3.  [Scan conversion and primitive parameter interpolation](https://www.khronos.org/opengl/wiki/Rasterization "Rasterization"), which generates a number of [Fragments](https://www.khronos.org/opengl/wiki/Fragment "Fragment").
4.  A [Fragment Shader](https://www.khronos.org/opengl/wiki/Fragment_Shader "Fragment Shader") processes each fragment. Each fragment generates a number of outputs.
5.  [Per-Sample_Processing](https://www.khronos.org/opengl/wiki/Per-Sample_Processing "Per-Sample Processing"), including but not limited to:
    1.  [Scissor Test](https://www.khronos.org/opengl/wiki/Scissor_Test "Scissor Test")
    2.  [Stencil Test](https://www.khronos.org/opengl/wiki/Stencil_Test "Stencil Test")
    3.  [Depth Test](https://www.khronos.org/opengl/wiki/Depth_Test "Depth Test")
    4.  [Blending](https://www.khronos.org/opengl/wiki/Blending "Blending")
    5.  [Logical Operation](https://www.khronos.org/opengl/wiki/Logical_Operation "Logical Operation")
    6.  [Write Mask](https://www.khronos.org/opengl/wiki/Write_Mask "Write Mask")

## <a id="Vertex_Specification"></a>Vertex Specification

Main article: [Vertex Specification](https://www.khronos.org/opengl/wiki/Vertex_Specification "Vertex Specification")

The process of vertex specification is where the application sets up an ordered list of vertices to send to the pipeline. These vertices define the boundaries of a *primitive*.

Primitives are basic drawing shapes, like triangles, lines, and points. Exactly how the list of vertices is interpreted as primitives is handled via a later stage.

This part of the pipeline deals with a number of objects like [Vertex Array Objects](https://www.khronos.org/opengl/wiki/Vertex_Array_Objects "Vertex Array Objects") and [Vertex Buffer Objects](https://www.khronos.org/opengl/wiki/Vertex_Buffer_Objects "Vertex Buffer Objects"). Vertex Array Objects define what data each vertex has, while Vertex Buffer Objects store the actual vertex data itself.

A vertex's data is a series of [attributes](https://www.khronos.org/opengl/wiki/Vertex_Attributes "Vertex Attributes"). Each attribute is a small set of data that the next stage will do computations on. While a set of attributes do specify a vertex, there is nothing that says that part of a vertex's attribute set needs to be a position or normal. Attribute data is entirely arbitrary; the only meaning assigned to any of it happens in the vertex processing stage.

### <a id="Vertex_Rendering"></a>Vertex Rendering

Main article: [Vertex Rendering](https://www.khronos.org/opengl/wiki/Vertex_Rendering "Vertex Rendering")

Once the vertex data is properly specified, it is then rendered as a [Primitive](https://www.khronos.org/opengl/wiki/Primitive "Primitive") via a drawing command.

## <a id="Vertex_Processing"></a>Vertex Processing

Vertices fetched due to the prior vertex rendering stage begin their processing here. The vertex processing stages are almost all [programmable operations](https://www.khronos.org/opengl/wiki/Shader "Shader"). This allows user code to customize the way vertices are processed. Each stage represents a different kind of shader operation.

Many of these stages are optional.

### <a id="Vertex_shader"></a>Vertex shader

Main article: [Vertex Shader](https://www.khronos.org/opengl/wiki/Vertex_Shader "Vertex Shader")

Vertex shaders perform basic processing of each individual vertex. Vertex shaders receive the attribute inputs from the vertex rendering and converts each incoming vertex into a single outgoing vertex based on an arbitrary, [user-defined program](https://www.khronos.org/opengl/wiki/Shader "Shader").

Vertex shaders can have user-defined outputs, but there is also a special output that represents the final position of the vertex. If there are no subsequent vertex processing stages, vertex shaders are expected to fill in this position with the clip-space position of the vertex, for rendering purposes.

One limitation on vertex processing is that each input vertex *must* map to a specific output vertex. And because [vertex shader invocations](https://www.khronos.org/opengl/wiki/Shader_Invocation "Shader Invocation") cannot share state between them, the input attributes to output vertex data mapping is 1:1. That is, if you feed the exact same attributes to the same vertex shader in the same primitive, you will get the same output vertex data. This gives implementations the right to optimize vertex processing; if they can detect that they're about to process a previously processed vertex, they can use the previously processed data stored in a [post-transform cache](https://www.khronos.org/opengl/wiki/Post_Transform_Cache "Post Transform Cache"). Thus they do not have to run the vertex processing on that data again.

Vertex shaders are not optional.

### <a id="Tessellation"></a>Tessellation

|     |     |     |
| --- | --- | --- |Tessellation
| Core in version |     | 4.6 |
| Core since version |     | 4.0 |
| Core ARB extension | [ARB\_tessellation\_shader](http://www.opengl.org/registry/specs/ARB/tessellation_shader.txt) |     |

Main article: [Tessellation Shader](https://www.khronos.org/opengl/wiki/Tessellation_Shader "Tessellation Shader")

Primitives can be tessellated using two shader stages and a fixed-function tessellator between them. The [Tessellation Control Shader](https://www.khronos.org/opengl/wiki/Tessellation_Control_Shader "Tessellation Control Shader") (TCS) stage comes first, and it determines the amount of tessellation to apply to a primitive, as well as ensuring connectivity between adjacent tessellated primitives. The [Tessellation Evaluation Shader](https://www.khronos.org/opengl/wiki/Tessellation_Evaluation_Shader "Tessellation Evaluation Shader") (TES) stage comes last, and it applies the interpolation or other operations used to compute user-defined data values for primitives generated by the fixed-function tessellation process.

Tessellation as a process is optional. Tessellation is considered active if a TES is active. The TCS is optional, but a TCS can only be used alongside a TES.

### <a id="Geometry_Shader"></a>Geometry Shader

Main article: [Geometry Shader](https://www.khronos.org/opengl/wiki/Geometry_Shader "Geometry Shader")

Geometry shaders are user-defined programs that process each incoming primitive, returning zero or more output primitives.

The input primitives for geometry shaders are the output primitives from a subset of the [Primitive Assembly](https://www.khronos.org/opengl/wiki/Primitive_Assembly "Primitive Assembly") process. So if you send a triangle strip as a single primitive, what the geometry shader will see is a series of triangles.

However, there are a number of input primitive types that are defined specifically for geometry shaders. These adjacency primitives give GS's a larger view of the primitives; they provide access to vertices of primitives adjacent to the current one.

The output of a GS is zero or more simple primitives, much like the output of primitive assembly. The GS is able to remove primitives, or tessellate them by outputting many primitives for a single input. The GS can also tinker with the vertex values themselves, either doing some of the work for the vertex shader, or just to interpolate the values when tessellating them. Geometry shaders can even convert primitives to different types; input point primitives can become triangles, or lines can become points.

Geometry shaders are optional.

## <a id="Vertex_post-processing"></a>Vertex post-processing

Main article: [Vertex Post-Processing](https://www.khronos.org/opengl/wiki/Vertex_Post-Processing "Vertex Post-Processing")

After the shader-based vertex processing, vertices undergo a number of fixed-function processing steps.

### <a id="Transform_Feedback"></a>Transform Feedback

Main article: [Transform Feedback](https://www.khronos.org/opengl/wiki/Transform_Feedback "Transform Feedback")

The outputs of the geometry shader or primitive assembly are written to a series of [buffer objects](https://www.khronos.org/opengl/wiki/Buffer_Objects "Buffer Objects") that have been setup for this purpose. This is called transform feedback mode; it allows the user to transform data via vertex and geometry shaders, then hold on to that data for use later.

The data output into the transform feedback buffer is the data from each primitive emitted by this step.

### <a id="Primitive_assembly"></a>Primitive assembly

Main article: [Primitive Assembly](https://www.khronos.org/opengl/wiki/Primitive_Assembly "Primitive Assembly")

Primitive assembly is the process of collecting a run of vertex data output from the prior stages and composing it into a sequence of primitives. The type of primitive the user rendered with determines how this process works.

The output of this process is an ordered sequence of simple primitives (lines, points, or triangles). If the input is a triangle strip primitive containing 12 vertices, for example, the output of this process will be 10 triangles.

If tessellation or geometry shaders are active, then a limited form of primitive assembly is executed before these [Vertex Processing](https://www.khronos.org/opengl/wiki/Vertex_Processing "Vertex Processing") stages. This is used to feed those particular shader stages with individual primitives, rather than a sequence of vertices.

The rendering pipeline can also be aborted at this stage. This allows the use of [Transform Feedback](https://www.khronos.org/opengl/wiki/Transform_Feedback "Transform Feedback") operations, without having to actually render something.

### <a id="Clipping"></a>Clipping

Main article: [Clipping](https://www.khronos.org/opengl/wiki/Clipping "Clipping")

The primitives are then clipped. Clipping means that primitives that lie on the boundary between the inside of the viewing volume and the outside are split into several primitives, such that the entire primitive lies in the volume. Also, the last [Vertex Processing](https://www.khronos.org/opengl/wiki/Vertex_Processing "Vertex Processing") shader stage can specify user-defined clipping operations, on a per-vertex basis.

The vertex positions are transformed from clip-space to window space via the [Perspective Divide](https://www.khronos.org/opengl/wiki/Perspective_Divide "Perspective Divide") and the [Viewport Transform](https://www.khronos.org/opengl/wiki/Viewport_Transform "Viewport Transform").

### <a id="Face_culling"></a>Face culling

Main article: [Face Culling](https://www.khronos.org/opengl/wiki/Face_Culling "Face Culling")

Triangle primitives can be culled (ie: discarded without rendering) based on the triangle's facing in window space. This allows you to avoid rendering triangles facing away from the viewer. For closed surfaces, such triangles would naturally be covered up by triangles facing the user, so there is never any need to render them. Face culling is a way to avoid rendering such primitives.

## <a id="Rasterization"></a>Rasterization

Main article: [Rasterization](https://www.khronos.org/opengl/wiki/Rasterization "Rasterization")

Primitives that reach this stage are then rasterized in the order in which they were given. The result of rasterizing a primitive is a sequence of *[Fragments](https://www.khronos.org/opengl/wiki/Fragment "Fragment")*.

A fragment is a set of state that is used to compute the final data for a pixel (or sample if [multisampling](https://www.khronos.org/opengl/wiki/Multisampling "Multisampling") is enabled) in the output framebuffer. The state for a fragment includes its position in screen-space, the sample coverage if multisampling is enabled, and a list of arbitrary data that was output from the previous vertex or geometry shader.

This last set of data is computed by interpolating between the data values in the vertices for the fragment. The style of interpolation is defined by the shader that outputed those values.

## <a id="Fragment_Processing"></a>Fragment Processing

Main article: [Fragment Shader](https://www.khronos.org/opengl/wiki/Fragment_Shader "Fragment Shader")

The data for each fragment from the rasterization stage is processed by a fragment shader. The output from a fragment shader is a list of colors for each of the color buffers being written to, a depth value, and a stencil value. Fragment shaders are not able to set the stencil data for a fragment, but they do have control over the color and depth values.

Fragment shaders are optional. If you render without a fragment shader, the depth (and stencil) values of the fragment get their usual values. But the value of all of the colors that a fragment could have are undefined. Rendering without a fragment shader is useful when rendering only a primitive's default depth information to the depth buffer, such as when performing [Occlusion Query](https://www.khronos.org/opengl/wiki/Occlusion_Query "Occlusion Query") tests.

## <a id="Per-Sample_Operations"></a>Per-Sample Operations

Main article: [Per-Sample_Processing](https://www.khronos.org/opengl/wiki/Per-Sample_Processing "Per-Sample Processing")

The fragment data output from the fragment processor is then passed through a sequence of steps.

The first step is a sequence of culling tests; if a test is active and the fragment fails the test, the underlying pixels/samples are not updated (usually). Many of these tests are only active if the user activates them. The tests are:

- Pixel ownership test: Fails if the fragment's pixel is not "owned" by OpenGL (if another window is overlapping with the GL window). Always passes when using a [Framebuffer Object](https://www.khronos.org/opengl/wiki/Framebuffer_Object "Framebuffer Object"). Failure means that the pixel contains undefined values.
- [Scissor Test](https://www.khronos.org/opengl/wiki/Scissor_Test "Scissor Test"): When enabled, the test fails if the fragment's pixel lies outside of a specified rectangle of the screen.
- [Stencil Test](https://www.khronos.org/opengl/wiki/Stencil_Test "Stencil Test"): When enabled, the test fails if the stencil value provided by the test does not compare as the user specifies against the stencil value from the underlying sample in the stencil buffer. Note that the stencil value in the framebuffer can still be modified even if the stencil test fails (and even if the depth test fails).
- [Depth Test](https://www.khronos.org/opengl/wiki/Depth_Test "Depth Test"): When enabled, the test fails if the fragment's depth does not compare as the user specifies against the depth value from the underlying sample in the depth buffer.

**Note:** Though these are specified to happen after the [Fragment Shader](https://www.khronos.org/opengl/wiki/Fragment_Shader "Fragment Shader"), they can be made to happen [before the fragment shader](https://www.khronos.org/opengl/wiki/Early_Fragment_Test "Early Fragment Test") under certain conditions. If they happen before the FS, then any culling of the fragment will also prevent the fragment shader from executing, this saving performance.

After this, [color blending](https://www.khronos.org/opengl/wiki/Blending "Blending") happens. For each fragment color value, there is a specific blending operation between it and the color already in the framebuffer at that location. [Logical Operations](https://www.khronos.org/opengl/wiki/Logical_Operation "Logical Operation") may also take place in lieu of blending, which perform bitwise operations between the fragment colors and framebuffer colors.

Lastly, the fragment data is written to the framebuffer. [Masking operations](https://www.khronos.org/opengl/wiki/Write_Mask "Write Mask") allow the user to prevent writes to certain values. Color, depth, and stencil writes can be masked on and off; individual color channels can be masked as well.