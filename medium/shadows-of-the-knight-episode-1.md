https://www.codingame.com/training/medium/shadows-of-the-knight-episode-1

![285599809-604a3491-8b28-4792-b262-0039e95f4c41](https://github.com/OlivierMounicq/Codingame/assets/26850726/13d2b0da-4864-4289-ac95-c533c7aeb56e)

![285599816-77daceb7-c274-4d32-b41f-84244b168171](https://github.com/OlivierMounicq/Codingame/assets/26850726/61dc1d9e-9be5-453e-a10a-d389929fd06e)

![285599820-74a520fb-0432-4b57-837d-2e3bc8c4baa5](https://github.com/OlivierMounicq/Codingame/assets/26850726/786b9c11-2d05-45df-8dfa-13d9d1edfa6e)

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
class Player
{
    static void Main(string[] args)
    {
        string[] inputs;
        inputs = Console.ReadLine().Split(' ');
        int W = int.Parse(inputs[0]); // width of the building.
        int H = int.Parse(inputs[1]); // height of the building.
        int N = int.Parse(Console.ReadLine()); // maximum number of turns before game over.
        inputs = Console.ReadLine().Split(' ');
        int X0 = int.Parse(inputs[0]);
        int Y0 = int.Parse(inputs[1]);

        int LowerX = 0;
        int LowerY = 0;

        int UpperX = W;
        int UpperY = H;
    
        // game loop
        while (true)
        {
            string bombDir = Console.ReadLine(); // the direction of the bombs from batman's current location (U, UR, R, DR, D, DL, L or UL)

            var tmpX = X0;
            var tmpY = Y0;

            switch(bombDir)
            {
                case "U":
                    X0 = X0;
                    Y0 = (Y0 - LowerY)/2 + LowerY;                    
                    break;
                case "UR":
                    X0 = (UpperX - X0)/2 + X0;                                
                    Y0 = (Y0 - LowerY)/2 + LowerY;
                    break;                

                case "UL":
                    X0 = (X0 - LowerX)/2 + LowerX;
                    Y0 = (Y0 - LowerY)/2 + LowerY;
                    break;

                case "D":
                    X0 = X0;
                    Y0 = (UpperY - Y0)/2 + Y0;
                    break;

                case "DR":
                    X0 = (UpperX - X0)/2 + X0;
                    Y0 = (UpperY - Y0)/2 + Y0;
                    break; 

                case "DL":
                    X0 = (X0 - LowerX)/2 + LowerX;
                    Y0 = (UpperY - Y0)/2 + Y0;
                    break;                         

                case "R":
                    X0 = (UpperX - X0)/2 + X0;
                    Y0 = Y0;
                    break;

                case "L": 
                    X0 = (X0 - LowerX)/2 + LowerX;
                    Y0 = Y0;
                    break;                        
            }

            if(bombDir == "U" || bombDir == "UL" || bombDir == "UR")
            {
                UpperY = tmpY;
            }
            else if(bombDir == "D" || bombDir == "DL" || bombDir == "DR")
            {
                LowerY = tmpY;
            }

            if(bombDir == "R" || bombDir == "UR" || bombDir == "DR")
            {
                LowerX = tmpX;
            }
            else if(bombDir == "L" || bombDir == "UL" || bombDir == "DL")
            {
                UpperX = tmpX;
            }

            // Write an action using Console.WriteLine()
            // To debug: Console.Error.WriteLine("Debug messages...");

            

            // the location of the next window Batman should jump to.
            Console.WriteLine($"{X0} {Y0}");            
        }
    }
}


```


