# Getting Started

The fastest way to understand a Fantas application is to look at the two types used by the templates: `MainGame` and `MainScene`. `MainGame` configures the host window and presents the first scene. `MainScene` builds the scene graph, handles input, and updates game logic.

## 1. Start from a Fantas template

The templates in `Fantas.Template.NuGet` create the standard project shape for each platform. No matter which template you choose, the flow is the same:

- derive a class from `Game`
- derive a class from `Scene`
- present the scene from your startup code
- add nodes in `Start()` and game logic in `Update()`

## 2. Create the game host

The template `MainGame` establishes the preferred logical window size and reacts when the window shape changes:

```csharp
using Fantas;
using System.Drawing;

namespace ProjectName
{
    public class MainGame : Game
    {
        public static readonly Size PreferredSize = new Size(960, 540);

        public MainGame() : base(PreferredSize)
        {
        }

        public override void WindowSizeChanged()
        {
            if (WindowSize.Width < WindowSize.Height)
                CurrentScene.Size = new SizeF(PreferredSize.Height, PreferredSize.Width);
            else
                CurrentScene.Size = PreferredSize;
        }
    }
}
```

A few details are worth keeping in mind:

- the `Game` constructor initializes Fantas' input and audio systems
- the size you pass to `base(...)` defines the preferred logical scene size on desktop targets
- `WindowSizeChanged()` is the right place to adapt your scene when orientation or aspect ratio changes

## 3. Build your first scene

The template `MainScene` shows the normal Fantas workflow: create nodes in `Start()`, keep references to the ones you need later, and override input or update methods for behavior.

```csharp
using System;
using System.Drawing;
using Fantas;
using Fantas.Input;

namespace ProjectName
{
    public class MainScene : Scene
    {
        private CameraNode? camera;
        private RectangleNode? border;

        public MainScene() : base(MainGame.PreferredSize)
        {
            BackgroundColor = Color.CornflowerBlue;
        }

        public override void Start()
        {
            camera = new CameraNode();
            Camera = camera;
            AddChild(camera);

            var square = new SpriteNode(Color.White, new Size(300, 300))
            {
                TopLeftColor = Color.Blue,
                TopRightColor = Color.Red,
                BottomLeftColor = Color.Yellow,
                BottomRightColor = Color.Green
            };

            border = new RectangleNode(Size)
            {
                Alpha = 0.5f,
                FillColor = Color.Transparent,
                StrokeColor = Color.Purple,
                LineWidth = 10
            };

            camera.AddChild(border);

            Texture imageTexture = Texture.FromResource(typeof(MainScene).Assembly, "Image.png", 2f);
            var sprite = new SpriteNode(imageTexture);

            var label = new LabelNode
            {
                Text = "ProjectName",
                TopColor = Color.Red,
                BottomColor = Color.Yellow,
                Position = new PointF(0, -100),
                Outline = true
            };

            var up = Animation.MoveYBy(20, 0.2);
            Animation down = up.ReversedAnimation;
            var bounce = Animation.RepeatForever(Animation.Sequence(up, down));

            sprite.RunAnimation(bounce);
            square.RunAnimation(Animation.RepeatForever(Animation.RotateBy(Math.PI, 2.0)));

            square.AddChild(sprite);
            square.AddChild(label);
            AddChild(square);
        }

        public override void Update()
        {
            if (KeyboardInput.IsKeyDown(Keys.Escape))
                Game.Instance.Exit();
        }

        public override void SizeChanged()
        {
            if (border == null)
                return;

            border.Size = Size;
        }

        public override void OnKeyDown(KeyEventArgs args)
        {
            base.OnKeyDown(args);

            if (args.KeyCode == Keys.F11)
                Game.Instance.IsFullScreen = !Game.Instance.IsFullScreen;
        }
    }
}
```

This one scene already demonstrates most of the framework:

- `CameraNode` controls the viewport
- `SpriteNode`, `RectangleNode`, and `LabelNode` compose the visible content
- `Texture.FromResource(...)` loads an embedded image
- `Animation` objects are attached directly to nodes
- `KeyboardInput` can be polled in `Update()`
- scene callbacks such as `SizeChanged()` and `OnKeyDown(...)` handle lifecycle and interaction

## 4. Present and run the scene

Your startup code only needs to create the game, present a scene, and run the loop:

```csharp
var game = new MainGame();
game.PresentScene(new MainScene());
game.Run();
```

From there, most projects grow in the same direction:

1. add textures, labels, and shape nodes to build the scene
2. respond to input through the scene or the static input facades
3. attach animations for motion and transitions
4. add physics bodies or joints when gameplay needs simulation
5. move reusable UI and gameplay pieces into custom node subclasses

## 5. Where to go next

After your first scene is on screen, the next APIs to learn are usually:

- `Node` and `SpriteNode` for scene composition
- `Animation` for movement and effects
- `Texture` and `TextureAtlas` for asset loading
- `TouchInput`, `MouseInput`, `KeyboardInput`, and `ControllerInput` for interaction
- `PhysicsWorld` and `PhysicsBody` for collisions and motion
- `TileMapNode` if your game uses grid, isometric, or hex maps

That path matches the way Fantas itself is structured: one scene graph, one animation model, and one set of input and rendering concepts across every supported backend.
