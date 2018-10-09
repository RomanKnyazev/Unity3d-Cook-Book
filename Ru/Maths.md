## Выбор случайного объекта из списка по шансу
```csharp
public interface IRandom {
    float returnChance { get; }
}

public static partial class FrameworkExtensions{
private static System.Random _r = new System.Random();
public static T GetRandom<T>(this T[] vals) where T : IRandom
        {
            var total = 0f;
            var probs = new float[vals.Length];
            for (int i = 0; i < probs.Length; i++)
            {
                probs[i] = vals[i].returnChance;
                total += probs[i];
            }

            var randomPoint = (float) _r.NextDouble() * total;

            for (int i = 0; i < probs.Length; i++)
            {
                if (randomPoint < probs[i])
                    return vals[i];
                randomPoint -= probs[i];
            }
            return vals[0];
        }
}
```
#### Пример
[![Image from Gyazo](https://i.gyazo.com/cf4ef1ab0f827b0249f04dc04d83db94.png)](https://gyazo.com/cf4ef1ab0f827b0249f04dc04d83db94)
```csharp
    [Serializable]
    public class DataDrop : IData
    {
        public Choice[] choices;

        [Serializable]
        public class Choice : IRandom
        {
            public float chance;
            public SampleObject sample;
            
            public float returnChance => chance;
        }
    }
```

```csharp
    DataDrop lootable;
    var choice = lootable.choices.GetRandom();
```
