https://www.codingame.com/ide/puzzle/chemical-equation-balancing


![289324946-dbe09c5d-2344-425c-957c-8133cf63f1b2](https://github.com/OlivierMounicq/Codingame/assets/26850726/96e5382f-320d-4478-a55b-d6f17f84a934)

```cs
using System;
using System.Linq;
using System.IO;
using System.Text;
using System.Collections;
using System.Collections.Generic;
using System.Text.RegularExpressions;

/**
 * Auto-generated code below aims at helping you parse
 * the standard input according to the problem statement.
 **/
class Solution
{
    private static int Max { get; set; }        
    private static List<int[]> Equations { get; set; }

    static void Main(string[] args)
    {
        string unbalanced = Console.ReadLine();
        var input = unbalanced;
        
        var arr = input.Split("->");

        var left = arr[0];
        var right = arr[1];

        var leftInput = arr[0].Split("+").Select(t => (Str: t.Trim(), Sign: 1)).ToArray();
        var rightInput = arr[1].Split("+").Select(t => (Str: t.Trim(), Sign: -1)).ToArray();

        var all = leftInput.Concat(rightInput).ToArray();
        var data = GetArray(all);

        Equations = data.Select(t => t.Value).ToList();

        Max = all.Length;
            

        var solution =  PrepareCoefficient(0, new int[Max]);

        var balancedEquation = BuildSolution(solution, input);        


        // Write an answer using Console.WriteLine()
        // To debug: Console.Error.WriteLine("Debug messages...");

        Console.WriteLine(balancedEquation);
    }

    public static IDictionary<string, int[]> GetArray((string Str, int Sign)[] arr)
    {
        var length = arr.Length;
        var dico = new Dictionary<string, int[]>();


        for (var i = 0; i < length; i++)
        {
            var multiplierStr = Regex.Match(arr[i].Str, @"^\d+").Value;
            var mulplitier = int.Parse(multiplierStr == string.Empty ? "1" : multiplierStr);

            foreach (Match m in Regex.Matches(arr[i].Str, @"([A-Z][a-z]{0,}\d{0,}){1,}?"))
            {
                var tmp = m.Value;
                var atom = Regex.Match(tmp, @"[A-Z][a-z]{0,}").Value;
                var quantityStr = Regex.Match(tmp, @"\d+").Value;

                var quantity = int.Parse(quantityStr == string.Empty ? "1" : quantityStr);

                var multiplicateur = arr[i].Sign;

                if (dico.ContainsKey(atom))
                    dico[atom][i] = quantity * mulplitier * multiplicateur;
                else
                {
                    var array = new int[length];
                    array[i] = quantity * multiplicateur;
                    dico.Add(atom, array);
                }
            }
        }

        return dico;
    }

    public static int[] PrepareCoefficient(int idx, int[] array)
    {
        if (idx == Max)
        {
            var target = new int[Max];
            array.CopyTo(target, 0);
                
            if (Verify(target))
                return target;
        }
        else if (idx < Max)
        {
            for (var i = 1; i < 21; i++)
            {
                array[idx] = i;                    
                var resultArr = PrepareCoefficient(idx + 1, array);

                if (resultArr != null)
                    return resultArr;
            }
        }
        else
            return null;

        return null;
    }

    public static bool Verify(int[] coefficients)
    {
        foreach(var equation in Equations)
        {
            var res = 0;
            for(var i = 0; i < equation.Length; i++)
            {
                res += equation[i] * coefficients[i];
            }

            if(res != 0)
                return false;
        }

        return true;
    }    

    public static string BuildSolution(int[] coefficients, string initialEquation)
    {
        var arr = initialEquation.Split("->");

        var left = arr[0].Split("+").Select((t, idx) => (Str: t.Trim(), Idx: idx, LeftHand : true)).ToArray();

        var offset = arr[0].Split("+").Length;
        var right = arr[1].Split("+").Select((t, idx) => (Str: t.Trim(), Idx: idx + offset, LeftHand: false)).ToArray();

        var data = left.Concat(right).ToArray();

        var parts = new List<(string Str, bool IsLeftHand)>();

        foreach(var p in data)
        {
            var c = coefficients[p.Idx] == 1 ? string.Empty : coefficients[p.Idx].ToString();
            var r = $"{c}{p.Str}";
            parts.Add((Str: r, IsLeftHand: p.LeftHand));
        }

        var leftHand = parts.Where(t => t.IsLeftHand).Select(t => t.Str).Aggregate((f, acc) => f + " + " + acc);
        var rightHand = parts.Where(t => !t.IsLeftHand).Select(t => t.Str).Aggregate((f, acc) => f + " + " + acc);

        var balancedEquation = $"{leftHand} -> {rightHand}";
        return balancedEquation;
    }
}
```

In VS 2022:

```cs
using System;
using System.ComponentModel.DataAnnotations;
using System.Reflection.Metadata;
using System.Text.RegularExpressions;

namespace ChemicalEquation
{
    internal class Program
    {
        private static int Max { get; set; }        
        private static List<int[]> Equations { get; set; }

        static void Main(string[] args)
        {
            //var input = "Pt + HNO3 + HCl -> H2PtCl6 + NO2 + H2O";

            //var input = "H2 + O2 -> H2O";
            var input = "CO2 + H2O -> C6H12O6 + O2";
            //var input = "NaOH + H2CO3 -> Na2CO3 + H2O";
            //var input = "C2H2 + O2 -> CO2 + H2O";
            //var input = "Rb + HNO3 -> RbNO3 + H2";
            //var input = "Pt + HNO3 + HCl -> H2PtCl6 + NO2 + H2O";
            //var input = "S + HNO3 -> H2SO4 + NO2 + H2O";
            //var input = "ABCD -> A2B2 + C2D2";
            //var input = "FeCl3 + HF -> FeF3 + HCl";

            var arr = input.Split("->");

            var left = arr[0];
            var right = arr[1];

            var leftInput = arr[0].Split("+").Select(t => (Str: t.Trim(), Sign: 1)).ToArray();
            var rightInput = arr[1].Split("+").Select(t => (Str: t.Trim(), Sign: -1)).ToArray();

            var all = leftInput.Concat(rightInput).ToArray();
            var data = GetArray(all);

            Equations = data.Select(t => t.Value).ToList();

            Max = all.Length;
            

            var solution =  PrepareCoefficient(0, new int[Max]);

            var balancedEquation = BuildSolution(solution, input);

            Console.WriteLine(balancedEquation);



            Console.WriteLine("Hello, World!");
        }


        public static IDictionary<string, int[]> GetArray((string Str, int Sign)[] arr)
        {
            var length = arr.Length;
            var dico = new Dictionary<string, int[]>();


            for (var i = 0; i < length; i++)
            {
                var multiplierStr = Regex.Match(arr[i].Str, @"^\d+").Value;
                var mulplitier = int.Parse(multiplierStr == string.Empty ? "1" : multiplierStr);

                foreach (Match m in Regex.Matches(arr[i].Str, @"([A-Z][a-z]{0,}\d{0,}){1,}?"))
                {
                    var tmp = m.Value;
                    var atom = Regex.Match(tmp, @"[A-Z][a-z]{0,}").Value;
                    var quantityStr = Regex.Match(tmp, @"\d+").Value;

                    var quantity = int.Parse(quantityStr == string.Empty ? "1" : quantityStr);

                    var multiplicateur = arr[i].Sign;

                    if (dico.ContainsKey(atom))
                        dico[atom][i] = quantity * mulplitier * multiplicateur;
                    else
                    {
                        var array = new int[length];
                        array[i] = quantity * multiplicateur;
                        dico.Add(atom, array);
                    }
                }
            }

            return dico;
        }

        public static int[] PrepareCoefficient(int idx, int[] array)
        {
            if (idx == Max)
            {
                var target = new int[Max];
                array.CopyTo(target, 0);
                
                if (Verify(target))
                    return target;
            }
            else if (idx < Max)
            {
                for (var i = 1; i < 21; i++)
                {
                    array[idx] = i;                    
                    var resultArr = PrepareCoefficient(idx + 1, array);

                    if (resultArr != null)
                        return resultArr;
                }
            }
            else
                return null;

            return null;
        }

        public static bool Verify(int[] coefficients)
        {
            foreach(var equation in Equations)
            {
                var res = 0;
                for(var i = 0; i < equation.Length; i++)
                {
                    res += equation[i] * coefficients[i];
                }

                if(res != 0)
                    return false;
            }

            return true;
        }

        public static string BuildSolution(int[] coefficients, string initialEquation)
        {
            var arr = initialEquation.Split("->");

            var left = arr[0].Split("+").Select((t, idx) => (Str: t.Trim(), Idx: idx, LeftHand : true)).ToArray();

            var offset = arr[0].Split("+").Length;
            var right = arr[1].Split("+").Select((t, idx) => (Str: t.Trim(), Idx: idx + offset, LeftHand: false)).ToArray();

            var data = left.Concat(right).ToArray();

            var parts = new List<(string Str, bool IsLeftHand)>();

            foreach(var p in data)
            {
                var c = coefficients[p.Idx] == 1 ? string.Empty : coefficients[p.Idx].ToString();
                var r = $"{c}{p.Str}";
                parts.Add((Str: r, IsLeftHand: p.LeftHand));
            }

            var leftHand = parts.Where(t => t.IsLeftHand).Select(t => t.Str).Aggregate((f, acc) => f + " + " + acc);
            var rightHand = parts.Where(t => !t.IsLeftHand).Select(t => t.Str).Aggregate((f, acc) => f + " + " + acc);

            var balancedEquation = $"{leftHand} -> {rightHand}";
            return balancedEquation;
        }
    }
}
```


The goal is to solve an equation like 

```
αS + βHNO3 -> γH2SO4 + δNO2 + ηH2O
```

Where the unknows are α, β, γ, δ and η.
We have to determine for each atom a vector. To equilibrate the equation, the term on the ight hand side will be negative.

| Atom | Vector |
|-------|----------|
| S     | (1, 0, 1, 0, 0) |
| H     | (0, 1, -2, 0, 2) |
| N     | (0, 1, 0, -1, 0) |
| O     | (0, 3, -4, -2, -1) |

And for each atom : 

                   (α)
                   (β)
                   (γ)
                   (δ)
(1, 0, 1, 0, 0) *  (η) = 0







