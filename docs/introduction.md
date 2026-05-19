# Introduction

Fantas is a cross-platform 2D game framework built around the same scene-graph ideas that make SpriteKit approachable: you create a [`Game`](xref:Fantas.Game), present a [`Scene`](xref:Fantas.Scene), compose it from [`Node`](xref:Fantas.Node) objects, and bring it to life with animations, input, audio, and physics.

On Apple platforms, Fantas uses SpriteKit as its rendering model. On desktop and other supported targets, Fantas keeps the same high-level API while providing equivalent behavior through its other backends. That compatibility-first approach also extends to older .NET and Windows versions, and to Linux handheld or low-end devices that rely on OpenGL ES. The goal is not pixel-perfect parity across every platform; the goal is to let you build gameplay, UI, and tools around one consistent programming model.

If you want to get a project running first, start with the platform quick starts for [Windows](../windows.md), [macOS](../mac.md), or [Linux](../linux.md).

## Core concepts

### Game and scene lifecycle

[`Game`](xref:Fantas.Game) is the application host. It initializes the platform backend, input facades, and audio session, and it presents exactly one active [`Scene`](xref:Fantas.Scene) at a time. A scene is the root of your game world or screen. Override `Start()` to build content, `Update()` to run frame-by-frame logic, `Finish()` for teardown, and `SizeChanged()` when the visible area changes.

### Scene graph and nodes

Everything drawn in Fantas lives in the scene graph. [`Node`](xref:Fantas.Node) is the common base type for transforms, parenting, ordering, hit testing, animations, and physics integration. Higher-level node types specialize that behavior:

- [`SpriteNode`](xref:Fantas.SpriteNode) draws textures or solid-color quads.
- [`ShapeNode`](xref:Fantas.ShapeNode) and its derived types draw vector primitives.
- [`LabelNode`](xref:Fantas.LabelNode) renders text.
- [`CameraNode`](xref:Fantas.CameraNode) controls the visible region of a scene.
- [`CropNode`](xref:Fantas.CropNode), [`TileMapNode`](xref:Fantas.TileMapNode), [`ParticleEmitterNode`](xref:Fantas.ParticleEmitterNode), and the UI nodes build more specialized behaviors on top of the same hierarchy.

That shared node model is what makes Fantas feel close to SpriteKit: layout, animation, input, and physics all compose around the same graph.

### Actions and motion

[`Animation`](xref:Fantas.Animation) is Fantas' action system. You can move, rotate, scale, fade, tint, sequence, group, repeat, reverse, or run custom callbacks over time. Animations are attached directly to nodes, so visual behavior stays close to the content it controls.

### Textures, atlases, and tiles

[`Texture`](xref:Fantas.Texture) loads images from assets, files, resources, or streams. [`TextureAtlas`](xref:Fantas.TextureAtlas) groups named textures for efficient lookup, while the tile types ([`TileDefinition`](xref:Fantas.TileDefinition), [`TileGroup`](xref:Fantas.TileGroup), [`TileSet`](xref:Fantas.TileSet), [`TileMapNode`](xref:Fantas.TileMapNode)) build reusable tile-based worlds and editors on top of the same rendering model.

### Input and UI

Fantas exposes input through static facades such as [`KeyboardInput`](xref:Fantas.Input.KeyboardInput), [`MouseInput`](xref:Fantas.Input.MouseInput), [`TouchInput`](xref:Fantas.Input.TouchInput), and [`ControllerInput`](xref:Fantas.Input.ControllerInput). You can either poll input state directly or override scene/node callbacks to react to interaction events.

The `Fantas.UI` namespace builds on the scene graph instead of introducing a separate widget tree. Controls such as [`ButtonNode`](xref:Fantas.UI.ButtonNode), [`ScrollNode`](xref:Fantas.UI.ScrollNode), and the scroll bars are still nodes, so they animate, layer, and compose like the rest of your scene.

### Physics and audio

Each scene can opt into physics through [`PhysicsWorld`](xref:Fantas.Physics.PhysicsWorld), [`PhysicsBody`](xref:Fantas.Physics.PhysicsBody), and the joint types. The API mirrors familiar SpriteKit-style concepts: gravity, collision and contact masks, raycasts, queries, and joints.

Audio follows the same pattern. [`AudioSession`](xref:Fantas.Audio.AudioSession) exposes global sound state, [`MusicPlayer`](xref:Fantas.Audio.MusicPlayer) manages longer background tracks, and [`SoundEffect`](xref:Fantas.Audio.SoundEffect) handles short interactive sounds.

## Platform model

Fantas targets the same game code at multiple runtimes. The exact backend differs by platform, but the public API is intentionally stable:

- Apple platforms map closely to SpriteKit concepts and behavior.
- Desktop and other supported platforms keep the same scene, node, input, animation, and physics abstractions through Fantas' platform layers, including older .NET and Windows targets.
- Linux support is practical for handheld and lower-end devices that depend on OpenGL ES.
- Utility types such as [`SystemPlatform`](xref:Fantas.SystemPlatform), [`CurrentSystemPlatform`](xref:Fantas.CurrentSystemPlatform), and [`SystemPlatformInformation`](xref:Fantas.SystemPlatformInformation) help you adapt when a platform-specific branch is necessary.

## How to read the API reference

The API reference is easiest to approach in layers:

1. Start with [`Game`](xref:Fantas.Game), [`Scene`](xref:Fantas.Scene), [`Node`](xref:Fantas.Node), [`SpriteNode`](xref:Fantas.SpriteNode), and [`Animation`](xref:Fantas.Animation).
2. Add [`Texture`](xref:Fantas.Texture), [`LabelNode`](xref:Fantas.LabelNode), and the shape nodes for rendering.
3. Bring in [`TouchInput`](xref:Fantas.Input.TouchInput), [`MouseInput`](xref:Fantas.Input.MouseInput), [`KeyboardInput`](xref:Fantas.Input.KeyboardInput), or [`ControllerInput`](xref:Fantas.Input.ControllerInput) for interaction.
4. Add [`PhysicsWorld`](xref:Fantas.Physics.PhysicsWorld) and [`PhysicsBody`](xref:Fantas.Physics.PhysicsBody) when you need simulation.
5. Explore tiles, particle effects, and UI controls as your project grows.

If you already know SpriteKit, you can think of Fantas as a cross-platform scene-graph API that keeps the same mental model while letting your game code travel beyond Apple's platforms.
