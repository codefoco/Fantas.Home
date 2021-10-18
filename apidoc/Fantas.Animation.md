---
uid: Fantas.Animation
---
The class [`Animation`](xref:Fantas.Animation) is used to animamate nodes. You can create a sequences of animations or a group of animations.

For instance, to make one node move from 100 points up and 100 points right you can use [`Animation.MoveBy`](xref:Fantas.Animation.MoveBy(System.Numerics.Vector2,System.Double))

```cs
    // Create animation to move a node
    Animation moveBy = Animation.MoveBy(100, 100);

    // Run animation on node
    node.RunAnimation(moveBy);

```

To combine multiple animations you can use [`Animation.Group`](xref:Fantas.Animation.Group(Fantas.Animation[])) to execute animation in parallel or [`Animation.Sequence`](xref:Fantas.Animation.Sequence(Fantas.Animation[])) to execute in sequence.