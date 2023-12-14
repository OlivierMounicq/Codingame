https://www.codingame.com/ide/puzzle/sudoku-validator

![282645656-71202934-2700-4d3d-af03-f150226dae76](https://github.com/OlivierMounicq/Codingame/assets/26850726/57c57944-c23b-4420-a389-db8c5a2e0490)

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
        var isGood = true;

        var grid = new int[9,9];

        for (int i = 0; i < 9; i++)
        {
            string[] inputs = Console.ReadLine()?.Split(' ');

            Console.Error.WriteLine($"{string.Join(" ", inputs)} | {inputs.Count()}");

            for (int j = 0; j < 9; j++)
            {
                int n = int.Parse(inputs[j]);                
                grid[i,j] = n;
            }
        }

        //On row
        for(var i = 0; i <9; i++)
        {
            var tmp = !Enumerable.Range(0,8).Select(t => grid[i, t])
                        .GroupBy(t => t, (k,v) => new { Value = k, Qty = v.Count()})
                        .Any(t => t.Qty != 1);        

            isGood = isGood && tmp;
        }

        //On column
        for(var i = 0; i <9; i++)
        {
            var tmp = !Enumerable.Range(0,8).Select(t => grid[t, i])
                        .GroupBy(t => t, (k,v) => new { Value = k, Qty = v.Count()})
                        .Any(t => t.Qty != 1);        

            isGood = isGood && tmp;
        } 

        //On sub grid
        isGood = isGood && GetSubMatrix(grid, 0 ,0);
        isGood = isGood && GetSubMatrix(grid, 0 ,3);
        isGood = isGood && GetSubMatrix(grid, 0 ,6);

        isGood = isGood && GetSubMatrix(grid, 3 ,0);
        isGood = isGood && GetSubMatrix(grid, 3 ,3);
        isGood = isGood && GetSubMatrix(grid, 3 ,6);        

        isGood = isGood && GetSubMatrix(grid, 6 ,0);
        isGood = isGood && GetSubMatrix(grid, 6 ,3);
        isGood = isGood && GetSubMatrix(grid, 6 ,6); 



        // Write an answer using Console.WriteLine()
        // To debug: Console.Error.WriteLine("Debug messages...");

        Console.WriteLine($"{isGood.ToString().ToLower()}");
    }

    private static bool GetSubMatrix(int[,] grid, int rowOffset, int colOffset)
    {
        var lst = new List<int>();

        for(var i = 0; i <3; i++)
        {
            for(var j= 0; j <3; j++)
            {
                var r = rowOffset + i;
                var c = colOffset + j;

                lst.Add(grid[r,c]);
            }
        }
        
        var temp = !lst
                    .GroupBy(t => t, (k,v) => new { Value = k, Qty = v.Count()})
                    .Any(t => t.Qty != 1);

        return temp;                    
    }
}
```
