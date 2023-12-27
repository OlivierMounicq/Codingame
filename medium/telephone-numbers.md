https://www.codingame.com/ide/puzzle/telephone-numbers


![image](https://github.com/OlivierMounicq/Codingame/assets/26850726/78c2bb70-ff9e-40d0-a87e-64819b05347b)

![image](https://github.com/OlivierMounicq/Codingame/assets/26850726/b9511cdf-95f2-443a-ba15-9246b4040b5d)

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
    private static List<Digit> rootList;

    static void Main(string[] args)
    {
        rootList = new List<Digit>();
        
        int N = int.Parse(Console.ReadLine());
        for (int i = 0; i < N; i++)
        {
            string telephone = Console.ReadLine();
            
            if(i == 0)
            {
                var firstTree = SetTree(telephone);
                rootList.Add(firstTree);                
            }
            else
            {
                BuildData(telephone);
            }
        }

        var globalQty = 0;

        foreach(var tree in rootList)
        {
            var treeQty = GetTreeQuantity(tree) + 1;
            globalQty += treeQty;
        }        

        // Write an answer using Console.WriteLine()
        // To debug: Console.Error.WriteLine("Debug messages...");


        // The number of elements (referencing a number) stored in the structure.
        Console.WriteLine(globalQty);
    }

    private static void BuildData(string phoneNumber)
    {
        var firstDigit = int.Parse(phoneNumber[0].ToString());

        var phoneNumberArr = phoneNumber.ToList().Select(t => int.Parse(t.ToString())).ToArray();

        var selectedRoot = rootList.FirstOrDefault(t => t.Value == firstDigit);

        if (selectedRoot == null)
            rootList.Add(SetTree(phoneNumber));
        else
        {
            (var selectToAppend, var deepth) = SearchDigit(selectedRoot, phoneNumberArr, 0);

            var arrToSet = phoneNumberArr.Skip(deepth).ToArray();

            if(selectToAppend != null)
            {
                var currentNode = selectToAppend;

                for (var i = 0; i < arrToSet.Length; i++)
                {
                    var node = new Digit(arrToSet[i]);
                    currentNode.NextDigits.Add(node);
                    currentNode = node;
                }
            }
        }
    }

    private static Digit SetTree(string phoneNumber) 
    { 
        var arr = phoneNumber.ToList().Select(t => int.Parse(t.ToString())).ToArray();

        var digitArr = new Digit[arr.Length];

        digitArr[0] = new Digit(arr[0]);

        for(var idx = 1; idx < arr.Length; idx++) 
        {                
            digitArr[idx] = new Digit(arr[idx]);
            digitArr[idx - 1].NextDigits.Add(digitArr[idx]);
        }

        return digitArr[0];
    }

    private static Digit SetTree(Digit d, int[] arr)
    {
        if (!arr.Any())
            return d;

        var digitArr = new Digit[arr.Length + 1];
        digitArr[0] = d;

        for (var idx = 1; idx < arr.Length + 1; idx++)
        {
            digitArr[idx] = new Digit(arr[idx-0]);
            digitArr[idx - 1].NextDigits.Add(digitArr[idx]);
        }

        return d;
    }

    private static (Digit d, int deepth) SearchDigit(Digit digit, int[] arr, int deepth)
    {
        ++deepth;

        if (deepth >= arr.Length)
            return (d: null, deepth: deepth);

        var valToSearch = arr[deepth];
            
        var node = digit.NextDigits.FirstOrDefault(t => t.Value == valToSearch);

        if(node != null) 
        { 
            return SearchDigit(node, arr, deepth);
        }

        return (d:digit, deepth: deepth);
    }

    private static int GetTreeQuantity(Digit node)
    {
        int qty = node.NextDigits.Count;

        if(node.NextDigits.Any())
        {
            foreach(var digit in node.NextDigits) 
            {
                qty += GetTreeQuantity(digit);
            }
        }
                
        return qty;
    }    
}

public class Digit
{
    public List<Digit> NextDigits { get; }
    public int Value { get; }

    public Digit(int val)
    {
        Value = val;
        NextDigits = new List<Digit>();
    }
} 
```



```cs
namespace TelephoneNumbers
{
    using System.Text;
    internal class Program
    {
        private static List<Digit> rootList;

        static void Main(string[] args)
        {
            rootList = new List<Digit>();

            var cpt = -1;

            foreach(var nb in Tests.Get())
            {
                string telephone = nb;
                cpt++;

                if(cpt == 0)
                {
                    var firstTree = SetTree(nb);
                    rootList.Add(firstTree);

                    var s = System.Text.Json.JsonSerializer.Serialize(firstTree);
                }
                else
                {
                    BuildData(nb);
                }                
            }

            var globalQty = 0;

            foreach(var tree in rootList)
            {
                var treeQty = GetTreeQuantity(tree) + 1;
                globalQty += treeQty;
            }

            Console.WriteLine(globalQty);
            Console.ReadLine();
        }

        private static void BuildData(string phoneNumber)
        {
            var firstDigit = int.Parse(phoneNumber[0].ToString());

            var phoneNumberArr = phoneNumber.ToList().Select(t => int.Parse(t.ToString())).ToArray();

            var selectedRoot = rootList.FirstOrDefault(t => t.Value == firstDigit);

            if (selectedRoot == null)
                rootList.Add(SetTree(phoneNumber));
            else
            {
                (var selectToAppend, var deepth) = SearchDigit(selectedRoot, phoneNumberArr, 0);

                var arrToSet = phoneNumberArr.Skip(deepth).ToArray();

                if(selectToAppend != null)
                {
                    var currentNode = selectToAppend;

                    for (var i = 0; i < arrToSet.Length; i++)
                    {
                        var node = new Digit(arrToSet[i]);
                        currentNode.NextDigits.Add(node);
                        currentNode = node;
                    }
                }
            }
        }

        private static Digit SetTree(string phoneNumber) 
        { 
            var arr = phoneNumber.ToList().Select(t => int.Parse(t.ToString())).ToArray();

            var digitArr = new Digit[arr.Length];

            digitArr[0] = new Digit(arr[0]);

            for(var idx = 1; idx < arr.Length; idx++) 
            {                
                digitArr[idx] = new Digit(arr[idx]);
                digitArr[idx - 1].NextDigits.Add(digitArr[idx]);
            }

            return digitArr[0];
        }

        private static Digit SetTree(Digit d, int[] arr)
        {
            if (!arr.Any())
                return d;

            var digitArr = new Digit[arr.Length + 1];
            digitArr[0] = d;

            for (var idx = 1; idx < arr.Length + 1; idx++)
            {
                digitArr[idx] = new Digit(arr[idx-0]);
                digitArr[idx - 1].NextDigits.Add(digitArr[idx]);
            }

            return d;
        }

        private static (Digit d, int deepth) SearchDigit(Digit digit, int[] arr, int deepth)
        {
            ++deepth;

            if (deepth >= arr.Length)
                return (d: null, deepth: deepth);

            var valToSearch = arr[deepth];
            
            var node = digit.NextDigits.FirstOrDefault(t => t.Value == valToSearch);

            if(node != null) 
            { 
                return SearchDigit(node, arr, deepth);
            }

            return (d:digit, deepth: deepth);
        }

        private static int GetTreeQuantity(Digit node)
        {
            int qty = node.NextDigits.Count;

            if(node.NextDigits.Any())
            {
                foreach(var digit in node.NextDigits) 
                {
                    qty += GetTreeQuantity(digit);
                }
            }
                
            return qty;
        }
    }

    public class Digit
    {
        public List<Digit> NextDigits { get; }
        public int Value { get; }

        public Digit(int val)
        {
            Value = val;
            NextDigits = new List<Digit>();
        }
    }
    public class Tests
    {
        public static IEnumerable<string> Get()
        {
            var list = new List<string>
            {
                "0663429676",
                "0663429678",
                "0663429676",
                "9876543210",
                "0712345688",
            };

            return list;
        }

        public static IEnumerable<string> GetNumbersWithCommonRoot()
        {
            var list = new List<string>
            {
                "0412578440",
                "0412199803",
                "0468892011",
                "112",
                "15",
            };

            return list;
        }

        public static IEnumerable<string> TestLargeNumberList()
        {
            var list = new List<string>
            {
                "2980950684",
                "5047459100",
                "3811658223",
                "6951565505",
                "0200306553",
                "4924502386",
                "59278",
                "633",
                "776524",
                "9967765390",
                "8044781997",
                "1892478193",
                "28214",
                "3855270610",
                "1130326923",
                "9809183034",
                "820100",
                "8885",
                "4619435592",
                "29266",
                "836509",
                "99491441",
                "56296949",
                "1070927140",
                "97698013",
                "7897498543",
                "8061007579",
                "840244",
                "010",
                "76428554",
                "0739",
                "2760084089",
                "0249748838",
                "84684",
                "9030",
                "6296653571",
                "935919",
                "0323041149",
                "8457863538",
                "5887086893",
                "5831",
                "0091759279",
                "0592493370",
                "3952473493",
                "1000453831836",
                "2584942818",
                "546757229115",
                "3193077168",
                "8248210669",
                "7505293",
                "8542929",
                "462606597",
                "0525675869",
                "839896",
                "46166092",
                "9052964578",
                "2176471974",
                "7866215164",
                "8704925708",
                "5655826045",
                "2458",
                "6716286",
                "3470552782",
                "4393722090",
                "2255923569",
                "8433097018",
                "744223",
                "319",
                "821287",
                "6296657916",
                "1410626991",
                "7629841940",
                "7165689252",
                "7340546529",
                "066",
                "8767396542",
                "9280141811",
                "950233",
                "05829141830",
                "5483506538",
                "9913386399",
                "27607",
                "7791",
                "4728286148",
                "973552435531",
                "1560126690",
                "2307194786",
                "0595602871",
                "5879",
                "3079518855",
                "4124346",
                "4247670",
                "7660997091",
                "064241442872",
                "8029512991",
                "762",
                "7572306386",
                "6964871932",
                "7756849173",
                "083466",
                "3151210517",
                "778",
                "5955162172",
                "485929",
                "1857010586",
                "2013858504",
                "8590088501",
                "1531375506",
                "69499",
                "...",
                "35253080",
                "9674472",
                "6801509576",
                "83665",
                "0911929627",
                "59805806",
                "00893",
                "043631",
                "6996382259",
                "499461",
                "9098301791",
                "641",
                "9330698421",
                "9402233307",
                "4372465316",
                "99207",
                "394",
                "5004",
                "8186",
                "8697604240",
                "115803",
                "989588",
                "423027433",
                "8064129063",
                "9005317692",
                "9805613863",
                "7079441961",
                "2245966229",
                "07790650",
                "75302839",
                "963",
                "2361145886",
                "1419970031",
                "2874730894",
                "04213",
                "9021643",
                "8508710631",
                "7414676056",
                "4275073218",
                "4232701920",
                "38505006",
                "021207",
                "981538",
                "4169530018",
                "4509215360",
                "9910225290",
                "35953430",
                "238909987",
                "6093268",
                "0064232986",
                "3108876",
                "1786110627",
                "8274974143",
                "264",
                "521942548",
                "3349166",
                "14828798",
                "8406753875",
                "7776101252",
                "7363760993",
                "39787108",
                "97768857",
                "836053341",
                "195806",
                "222418762",
                "7105766709",
                "470446",
                "4415437",
                "981797689",
                "81646069",
                "6809756060",
                "6396127985",
                "1359299885",
                "7163",
                "5334518079",
                "9887328",
                "3746733735",
                "7093996127",
                "7140745860",
                "3966045019",
                "24908",
                "8150419725",
                "08416861",
                "8742172104",
                "2880651602",
                "0830210059",
                "0396244459",
                "7703",
                "6190",
                "2589602299",
                "2015258842",
                "62687",
                "07904972",
                "2662522461",
                "59580757",
                "9334728",
                "0930906157",
                "1645198803",
                "9447",
                "0944237513",
                "6361064365",
                "6268056435",
                "92431",
                "1274804296",
                "3312543638",
                "538317487667",
                "5175899756",
                "700922380",
                "0660336182",
                "1310108983",
                "5579373",
                "77446610",
                "324945332",
                "0082387819",
                "2504317928",
                "193172",
                "7464964",
                "44930327564",
                "0784530800",
                "0137586583",
                "470",
                "8635",
                "7850258452",
                "0133393081",
                "097033",
                "2547700852",
                "490",
                "091707121",
                "3191977301",
                "63516990806",
                "346529081",
                "5124998143",
                "4762516361",
                "4824374927",
                "6763149",
                "70903",
                "9853010019",
                "66496",
                "6778799018",
                "37847",
                "86976055",
                "1437180287",
                "5096590424",
                "5798043027",
                "7141953020",
                "7450297182",
                "7087462087",
                "8036699838",
                "7421129322",
                "58677823",
                "4345124301",
                "9133397858",
                "0702606",
                "6732720418",
                "7664",
                "9213414696",
                "3011939832",
                "3201077849",
                "9188905761",
                "6234454",
                "459043",
                "0281768546",
                "89028",
                "8344",
                "1894",
                "5299481336",
                "176756",
                "82389541",
                "4721282",
                "5246816390",
                "0938231026",
                "2190584097",
                "5663750070",
                "646345",
                "2387412619",
                "4477730167",
                "52865",
                "9448319213",
                "9481733103",
                "6554009",
                "240",
                "7282820817",
                "97686725",
                "0602521159",
                "890755813",
                "3665892484",
                "8050046089",
                "1570327797",
                "7252",
                "6304545",
                "7862091660",
                "011138",
                "46191622",
                "5229306595",
                "33477",
                "6692223417",
                "2405991561",
                "76240",
                "29293",
                "65949479",
                "5122276",
                "508468",
                "973932526",
                "412837662",
                "26626",
                "1698892",
                "809679776784",
                "90925",
                "7554921",
                "7399047305",
                "490756",
                "3178110689",
                "4199601752",
                "21632",
                "8580449",
                "5399686241",
                "6680209869",
                "130935958",
                "8591058507",
                "473993",
                "1336264536",
                "5415665875",
                "6957457",
                "0692718639",
                "433305",
                "36116",
                "25211",
                "00265930",
                "5162461669",
                "689",
                "1472469241",
                "716",
                "0842523765",
                "4098316351",
                "1350",
                "3195168195",
                "704708629",
                "1195",
                "64837",
                "025893",
                "26338530",
                "7583793302",
                "2062394820",
                "4529237589",
                "1629925530",
                "59647",
                "8119426304",
                "0905943152",
                "6441231847",
                "8554601",
                "431873",
                "7252623",
                "7661673159",
                "2893",
                "5027232875",
                "0886209787",
                "0944883271",
                "19888623",
                "906166",
                "3337186",
                "3121246544",
                "827",
                "3954537570",
                "5460901882",
                "7913258826",
                "110161945",
                "1290",
                "3554",
                "7862629819",
                "3884122221",
                "7180726581",
                "1197271070",
                "860409",
                "02812",
                "193941",
                "1500633161",
                "81194266392",
                "1456935927",
                "6516",
                "6075739500",
                "7795039957",
                "174499079",
                "4028733089",
                "8915581075",
                "2897278195",
                "16607",
                "1093374201",
                "970561",
                "8807958536",
                "1102611859",
                "27413",
                "643",
                "73053",
                "7373339918",
                "0242875641",
                "9735316000",
                "2900326915",
                "7754605784",
                "81366484",
                "0750143107",
                "6328510019",
                "3303585",
                "6016184940",
                "33477345",
                "65042176",
                "210559",
                "32689",
                "7353",
                "4335441",
                "0582272751",
                "6538767157",
                "8031796982",
                "25477",
                "8418129266",
                "1412984368",
                "5003028852",
                "1898",
                "1219977198",
                "966289",
                "9748769441",
                "81797801",
                "0556424256",
                "5477",
                "6962769385",
                "32778049",
                "4104096045",
                "850250489"
            };

            return list;
        }
    }   
}
```



