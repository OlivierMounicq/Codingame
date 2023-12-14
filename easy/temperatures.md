https://www.codingame.com/training/easy/temperatures

![282558521-efca9895-b616-42cc-9846-301838cc913c](https://github.com/OlivierMounicq/Codingame/assets/26850726/a34afe75-d73f-4a27-89f7-efc9c859ead1)

```cs
using System;
using System.Linq;
using System.IO;
using System.Text;
using System.Collections;
using System.Collections.Generic;

/**
 * Auto-generated code below aims at helping you parse
 * the standard input according to the problem statement.
 **/
class Solution
{
    static void Main(string[] args)
    {
        int n = int.Parse(Console.ReadLine()); // the number of temperatures to analyse
        string[] inputs = Console.ReadLine()?.Split(' ');

        var temp = 0;
        var tempList = new List<int>();
        
        for (int i = 0; i < n; i++)
        {
            int t = int.Parse(inputs[i]);// a temperature expressed as an integer ranging from -273 to 5526
            tempList.Add(t);
        }

        if(tempList.Any())
        {
            temp = tempList
                    .Select(t => new { Temp = t, AbsTemp = Math.Abs(t)} )
                    .GroupBy(t => t.AbsTemp, (k, v) => new { AbsTemp = k, TempLst = v.OrderByDescending(t => t.Temp).ToList()} )
                    .OrderBy(t => t.AbsTemp)
                    .First()
                    .TempLst.First().Temp;                    
        }

        // Write an answer using Console.WriteLine()
        // To debug: Console.Error.WriteLine("Debug messages...");

        Console.WriteLine($"{temp}");
    }
}
```

