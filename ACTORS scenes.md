### Move to other main scene:
```csharp
// from scene name
ProcessingSceneLoad.To("Scene Game");
```

```csharp
// from id
ProcessingSceneLoad.To(0);
```

### Add/remove new scene to main scene:

```csharp
// from id only
 ProcessingSceneLoad.Add(0);
```

```csharp
// from id only
 ProcessingSceneLoad.Remove(0);
```

### Const workflow

```csharp
public class Scenes
{
    public const int Level1 = 0;
    public const int Level1Room6 = 1;
    public const int Game = 5;
}
```
Keep in mind that ID must be equal to ID from Build Settings

[![Scenes](https://i.gyazo.com/8f2f21337720522fd4bb15e52b3a5721.png)](https://gyazo.com/8f2f21337720522fd4bb15e52b3a5721)
 

```csharp 
// Now you can use something like this:
    ProcessingSceneLoad.Add(Scenes.Game);
    
```
```csharp 
// Now you can use something like this:
    ProcessingSceneLoad.To(Scenes.Game);
    
```
 
 
 
