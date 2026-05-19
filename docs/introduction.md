# Introduction

Fantas is a cross-platform 2D game framework built around the same scene-graph ideas that make SpriteKit approachable: you create a `Game`, present a `Scene`, compose it from `Node` objects, and bring it to life with animations, input, audio, and physics.

On Apple platforms, Fantas uses SpriteKit as its rendering model. On desktop and other supported targets, Fantas keeps the same high-level API while providing equivalent behavior through its other backends. The goal is not pixel-perfect parity across every platform; the goal is to let you build gameplay, UI, and tools around one consistent programming model.

## Core concepts

### Game and scene lifecycle

`Game` is the application host. It initializes the platform backend, input facades, and audio session, and it presents exactly one active `Scene` at a time. A scene is the root of your game world or screen. Override `Start()` to build content, `Update()` to run frame-by-frame logic, `Finish()` for teardown, and `SizeChanged()` when the visible area changes.

### Scene graph and nodes

Everything drawn in Fantas lives in the scene graph. `Node` is the common base type for transforms, parenting, ordering, hit testing, animations, and physics integration. Higher-level node types specialize that behavior:

- `SpriteNode` draws textures or solid-color quads.
- `ShapeNode` and its derived types draw vector primitives.
- `LabelNode` renders text.
- `CameraNode` controls the visible region of a scene.
- `CropNode`, `TileMapNode`, `ParticleEmitterNode`, and the UI nodes build more specialized behaviors on top of the same hierarchy.

That shared node model is what makes Fantas feel close to SpriteKit: layout, animation, input, and physics all compose around the same graph.

### Actions and motion

`Animation` is Fantas' action system. You can move, rotate, scale, fade, tint, sequence, group, repeat, reverse, or run custom callbacks over time. Animations are attached directly to nodes, so visual behavior stays close to the content it controls.

### Textures, atlases, and tiles

`Texture` loads images from assets, files, resources, or streams. `TextureAtlas` groups named textures for efficient lookup, while the tile types (`TileDefinition`, `TileGroup`, `TileSet`, `TileMapNode`) build reusable tile-based worlds and editors on top of the same rendering model.

### Input and UI

Fantas exposes input through static facades such as `KeyboardInput`, `MouseInput`, `TouchInput`, and `ControllerInput`. You can either poll input state directly or override scene/node callbacks to react to interaction events.

The `Fantas.UI` namespace builds on the scene graph instead of introducing a separate widget tree. Controls such as `ButtonNode`, `ScrollNode`, and the scroll bars are still nodes, so they animate, layer, and compose like the rest of your scene.

### Physics and audio

Each scene can opt into physics through `PhysicsWorld`, `PhysicsBody`, and the joint types. The API mirrors familiar SpriteKit-style concepts: gravity, collision and contact masks, raycasts, queries, and joints.

Audio follows the same pattern. `AudioSession` exposes global sound state, `MusicPlayer` manages longer background tracks, and `SoundEffect` handles short interactive sounds.

## Platform model

Fantas targets the same game code at multiple runtimes. The exact backend differs by platform, but the public API is intentionally stable:

- Apple platforms map closely to SpriteKit concepts and behavior.
- Desktop and other supported platforms keep the same scene, node, input, animation, and physics abstractions through Fantas' platform layers.
- Utility types such as `SystemPlatform`, `CurrentSystemPlatform`, and `SystemPlatformInformation` help you adapt when a platform-specific branch is necessary.

## How to read the API reference

The API reference is easiest to approach in layers:

1. Start with `Game`, `Scene`, `Node`, `SpriteNode`, and `Animation`.
2. Add `Texture`, `LabelNode`, and the shape nodes for rendering.
3. Bring in `TouchInput`, `MouseInput`, `KeyboardInput`, or `ControllerInput` for interaction.
4. Add `PhysicsWorld` and `PhysicsBody` when you need simulation.
5. Explore tiles, particle effects, and UI controls as your project grows.

If you already know SpriteKit, you can think of Fantas as a cross-platform scene-graph API that keeps the same mental model while letting your game code travel beyond Apple's platforms.
