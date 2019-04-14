---
layout: post
title: OpenTK in FSharp examples
subtitle: Colours,Textures,VBOs
banner: OpenTK in F#
subbanner: Quick demo
---

I was trying to use [OpenTK](https://github.com/opentk/opentk) in F#. My knowledge of OpenGL is particularly shallow, but one of the things I have heard is that the 'immediate mode' API (i.e. anything with `GL.Begin()` or `GL.End()` blocks is deprecated.

The next-most accessible API to the layman is the one I'll call the "VBO API", which typically allocates memory (VertexBufferObjects and others) in advance (via `GenBuffer` / `BindBuffer` / `BufferData` like calls) then renders via reference (`BindBuffer` / `VertexPointer` etc.)

I was looking for demos on how to use textures and while I did find some helpful demos (particularly [this](http://deathbyalgorithm.blogspot.com/2013/05/opentk-textures.html) and [this](http://neokabuto.blogspot.com/2014/04/opentk-tutorial-6-part-2-loading.html), they were typically 
1. in C# (which makes sense - OpenTK is a C# library)
2. using the immediate mode API

So, having made something that works in F#, I thought I'd republish a collection of simple F# programs, with the equivalent VBO-like versions.

You can find it [here](https://github.com/wilfredsj/opentk-in-fsharp/tree/master/src). The 5 examples contained cover:
* Colours + Immediate API
* Textures + Immediate API (this is almost directly taken from the [deathbyalgorithm](http://deathbyalgorithm.blogspot.com/2013/05/opentk-textures.html) example)
* Colours + VBO  API
* Textures + VBO API
* Text-writing  

The final example (Text-writing) attempts to be more 'functional' (and more 'DRY') than the others, for example, wrapping state set/unset calls like below. It is not clear immediately if this adds or removes transparency.

~~~~  
let execWithClientState csType fn () = 
  GL.EnableClientState(csType)
  fn ()
  GL.DisableClientState(csType)

let execWithBoundTexture (textureId : int) fn () =
  GL.BindTexture(TextureTarget.Texture2D, textureId)
  fn ()
  GL.BindTexture(TextureTarget.Texture2D, 0)
~~~~

The text-writing also needs a (procedurally generated) png file that has the ASCII characters rendered in a grid, like below. There's such a `texture.png` in the repo. The other texture-using demos need *any* .png file of the same name - may as well use the same file.

[![Courier]({{ site.url }}/img/blog/opentk/texture.png)]({{ site.    url }}/img/blog/opentk/texture.png)
