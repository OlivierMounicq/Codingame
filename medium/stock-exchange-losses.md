https://www.codingame.com/training/medium/stock-exchange-losses

![286063176-1476345b-db17-408f-8636-473061afb38e](https://github.com/OlivierMounicq/Codingame/assets/26850726/3564f6f8-467b-4e7d-abd8-78be19df4601)


![286063260-8d01fe28-e29e-44e5-b1b2-3d51194edb5e](https://github.com/OlivierMounicq/Codingame/assets/26850726/860d68eb-c752-4bfc-90b4-81b2d920b723)


![286063387-8902420a-6288-45ff-8fbe-2f8e7d683d46](https://github.com/OlivierMounicq/Codingame/assets/26850726/9d890f4f-326b-4c0a-9428-39efe16712e9)

![286063868-1d513f0d-ab56-47e7-86a9-6a0550b7283a](https://github.com/OlivierMounicq/Codingame/assets/26850726/9ed05101-7410-45a8-968c-26827d0f3bc9)


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
