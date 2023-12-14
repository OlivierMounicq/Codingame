https://www.codingame.com/training/medium/stock-exchange-losses

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
        int n = int.Parse(Console.ReadLine());
        string[] inputs = Console.ReadLine().Split(' ');

        var values = new List<int>();

        for (int i = 0; i < n; i++)
        {
            int v = int.Parse(inputs[i]);
            Console.Error.WriteLine($"value : {v}");
            values.Add(v);
        }

        bool toContinue = true;
        var lossList = new List<int>();
        var skipQty = 0;

        while(toContinue)
        {
            Console.Error.WriteLine($"skipQuantity : {skipQty}");

            var tmp = values.Skip(skipQty).Select((t, i) => new { Index = i, Value = t }).ToList();

            var min = tmp.Min(t => t.Value);
            var minData = tmp.Where(t => t.Value == min).OrderByDescending(t => t.Index).First();

            //max
            var subRange = tmp.Take(minData.Index + 1).ToList();
            var max = subRange.Max(t => t.Value);

            var loss = min -  max < 0 ? min - max : 0;
            lossList.Add(loss);

            Console.Error.WriteLine($"{minData.Index} : {minData.Value}");

            skipQty += minData.Index + 1;
            toContinue = skipQty < values.Count;
        }

        //min
        var worstLoss = lossList.Min(t => t);

        // Write an answer using Console.WriteLine()
        // To debug: Console.Error.WriteLine("Debug messages...");

        Console.WriteLine($"{worstLoss}");
    }
}
```
