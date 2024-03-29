Работая с Unity нам часто приходится использовать рейкасты чтобы взаимодействовать с объектами вокруг. Работа с рейкастами хороша тем, что мы не завязываемся на "волшебных" методах Unity в монобехейверах, таких как OnTriggerEnter и тп и можем работать из любых скриптов. Это идеально попадает под концепцию обработчиков в Акторах, где мы работаем с массивом сущностей.

Однако бездумное использование Raycast методов приводит к аллокациям памяти. Чтобы этого избежать используются методы типа RaycastNonAlloc которые сохраняют список столкновений в предустановленный массив. 

Ниже представлены примеры работы с такими методами и как можно максимально удобно их использовать через свою обертку. Здесь описаны не все методы работы с юнитивскими рейкастами но их легко добавлять.
```csharp
public static class Phys
    {
        public static readonly RaycastHit2D[] hits = new RaycastHit2D[20];
        public static readonly Collider2D[] colliders = new Collider2D[20];


        public static int CircleOverlap2D(Vector2 pos, float radius, int mask = 1 << 0, float min = float.NegativeInfinity,
            float max = float.PositiveInfinity)
        {
            return Physics2D.OverlapCircleNonAlloc(pos, radius, colliders, mask, min, max);
        }

        public static int Raycast2D(Vector2 position, Vector2 direction)
        {
            return Physics2D.RaycastNonAlloc(position, direction, hits);
        }

        public static int Raycast2D(Vector2 position, Vector2 direction, float distance)
        {
            return Physics2D.RaycastNonAlloc(position, direction, hits, distance);
        }

        public static int Raycast2D(Vector2 position, Vector2 direction, float distance, int mask)
        {
            return Physics2D.RaycastNonAlloc(position, direction, hits, distance, mask);
        }

        public static int Raycast2D(Vector2 position, Vector2 direction, float distance, int mask, float min = float.NegativeInfinity,
            float max = float.PositiveInfinity)
        {
            return Physics2D.RaycastNonAlloc(position, direction, hits, distance, mask, min, max);
        }
    }
```

Примеры использования :
```csharp
   void Update()
        {
             // считаем кадры  
            var frames = UnityEngine.Time.frameCount;
             // каждые 5 кадров делаем что-то, в нашем случае проверяем столкновения.
            if (frames % 5 == 0)
            {
                // мы отпарвляем луч из позиции объекта вправо и хотим получить все столкновения на дистанции в 1 единицу измерения.
                // эти столкновения будут храниться в Phys.hits а метод вернет нам кол-во этих столкновений. Оно никогда не превысит размер массива Phys.hits.
                var amount = Phys.Raycast2D(transform.position, Vector2.Right, 1.0f);
                for (int i = 0; i < amount; i++)
                {
                    // перебираем все RaycastHit2d попавшие в область рейкаста.
                    var hit = Phis.hits[i];
                }
            }
        }
 
```
