## Hi there 👋
I'm Dawid, and I devote myself full-time to programming.

I'm mainly a Rust and C# developer, but I also enjoy writing some C and python.

I've been working on voxel games for the past two years - voxels are incredible, and there are so many unique ways to optimize voxel rendering that just don't apply to any other rendering techniques. The part of game programming I enjoy the most is optimization and I suppose that's why I love voxels so much.

I also enjoy programming GPU shaders, and parallelizing tasks using the GPU. It's quite rewarding, and the results can be (sometimes inadvertently) mesmerizing.

I'm currently developing many projects, and switching between them, most of them private, but here's the basic overview:

### My voxel game project
For most of the past year, my main focus right now is a voxel game, and a game engine written in Rust to go with it. Most of my other projects are being developed to make this engine.

#### The game
My voxel game uses voxel raymarching to render detailed scened with good performance. The trees are compressed into a 64tree structure. My GPU code is similar to the one from [this article](https://dubiousconst282.github.io/2024/10/03/voxel-ray-tracing/) with some of my own additions. The unshaded rendering of the bistro scene shown below takes ~3.3 ms at 1080p on my RTX 3060 (8GB) and ~16.2 ms at 1080p on my M2 Macbook Air. I'm still working on performant shading with DDGI, meanwhile, this is a screenshot which uses some rudimentary per-voxel-face shading which uses some GPU hashmaps which acculumate lighting information across frames (one shadow ray + one GI ray per voxel frame per face):

<img width="340" height="479" alt="image" src="https://github.com/user-attachments/assets/5895c31e-5788-45e0-8273-aeb6844ba9a3" />

#### The engine
The engine is based on a unique idea - imagine every part of the game as a replaceable mod that just implements some interface. If a player doesn't like some part of the game, he can just replace that part of it with another mod that fulfills the same interface - e.g. let's say the game has an inventory system where the items you can store depend on their weight, and you don't like it - well, you can replace it with an inventory system where the items you can store depend on their volume, or amount.

The mods are either `.dll/.so/.dylib` shared library objects, or `.wasm` webassembly files, and are loaded by one of my other projects - a plugin runtime. The runtime automatically links host functions, automatically cross-links interfaces of plugins and manages execution of the plugins. `WASM` plugins are fully sandboxed, and are fully safe, so loading foreign modifications can be fully safe if the user wishes - the [wasmer](https://github.com/wasmerio/wasmer) library is used to compile the plugins to native code, so that they execute at near-native speeds. Shared library plugins are marginally faster, but unsafe to use.

The engine uses WebGPU for rendering, so it works on any system, and it can compile to work in the browser too, thanks to my networking library.

### My traffic simulation project
I'm also currently creating a traffic simulation game in Unity - imagine just the traffic simulation part of Cities Skylines. It's main objective is to build a road system that would allow people to drive from a few sources into some destinations as efficiently as possible. The road building tools are quite advanced already, you can do quite a lot of cool things, but I still need to improve the traffic behaviour. I'd eventually like to release it on some larger game platform like Steam.

<img width="425" height="233" alt="image" src="https://github.com/user-attachments/assets/308fb9d4-04cb-4d15-87a9-cd9b493781d0" />

### My other projects
#### Operating system
To properly learn C, I've decided to create my own operating system. I've used the [osdev guide](https://wiki.osdev.org/Tutorials) to implement a bare bones operating system and then developed on top of that. My OS currently has working CPU interrupts, keyboard input, multiple TTYs, a built-in tetris game, but really, that's it.

I'd eventually like to add user-space to my OS, and actual ELF file loading, but I'm currently focused on my other projects and uni.

<img width="416" height="270" alt="Screenshot 2025-11-21 at 23 59 59" src="https://github.com/user-attachments/assets/d03167e4-5dc4-48ad-91cb-2e20a3cb7cd4" />

#### Built-in WGSL in Rust
I've developed a crate which allows the user to write [WGSL](https://en.wikipedia.org/wiki/WebGPU_Shading_Language) source code straight in Rust macros. The code can actually be written in Rust too and translated to WGSL at compile-time. I've also implemented a fully integrated namespace extension to the WGSL.

I've also mirrored all the built-in WGSL, and the macro preserves tokens well enough for the IDE to fully associate usages of built-in functions with their definitions, so the user can actually look up documentations of built-in function straight in the editor, with just the `rust-analyzer` extension. This also allows syntax highlighting to work very well, so the syntax coloring is exactly the same as it would be in Rust. Here's an example of such code, from my voxel game project (this actually uses the Rust translation):

<img width="318" height="190" alt="image" src="https://github.com/user-attachments/assets/88b5cf33-fafb-45d7-a6d5-aa7f3b0975f2" />

#### WebRTC-based networking library for game engines
I've also written a netoworking library based on [WebRTC](https://github.com/webrtc-rs/webrtc) for my game engine. It is based on the client-server architecture rather than peer-to-peer like WebRTC usually is (it still does use peer-to-peer connections though, it's WebRTC). Thanks to WebRTC, it can connect to any device on any network without any issues. Unlike the `webrtc` rust crate, this can compile for `wasm32` too, so it works in the browser!
