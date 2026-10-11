# C# konzolos (CLI) összefoglaló

[← vissza](README.md) · Kész megoldások: LaptopsCLI, FilmCLI, FutoversenyCLI, HasznaltautoCLI, KonyvtarCLI, MenhelyCLI, Smokecli1, DrinkCLI1, AutoKonzol

## Tartalom
1. [Projekt létrehozása, forrásfájlok helye](#1-projekt-létrehozása)
2. [A tipikus feladatsor menete](#2-a-tipikus-feladatsor-menete)
3. [Segédosztály (Category, Manufacturer…)](#3-segédosztály)
4. [Fő osztály: konstruktor, ami egy sort feldolgoz](#4-fő-osztály-konstruktor)
5. [Sorszám (static counter)](#5-sorszám-static-counter)
6. [ToString felülírása (virtuális metódus)](#6-tostring-felülírása)
7. [Saját metódus (számolt érték)](#7-saját-metódus)
8. [Beolvasás fájlból (StreamReader)](#8-beolvasás-fájlból)
9. [Kiírás a képernyőre, formázás](#9-kiírás-formázás)
10. [LINQ – szűrés, rendezés, számolás](#10-linq)
11. [Csoportosítás (GroupBy)](#11-csoportosítás-groupby)
12. [Fájlba írás (StreamWriter)](#12-fájlba-írás)
13. [Bekérés a felhasználótól](#13-bekérés)
14. [Számok, tizedesjegyek, NA értékek](#14-számok-na-értékek)
15. [Stringek](#15-stringek)
16. [Dátum és idő (pl. futási idő "2:37:04")](#16-dátum-és-idő)
17. [Öröklődés, abstract, interface](#17-öröklődés)
18. [Teljes minta Program.cs](#18-teljes-minta)
19. [Tipikus hibák](#19-tipikus-hibák)

---

## 1. Projekt létrehozása

Visual Studio → *Új projekt* → **Console App** (C#) → név pontosan a feladat szerint (pl. `LaptopsCLI`).

Parancssorból:
```
dotnet new console -n LaptopsCLI
cd LaptopsCLI
dotnet run
```

A forrásfájlokat (`laptops.txt`) tedd a projekt mappájába egy `forras` mappába:
```
LaptopsCLI/
├── forras/laptops.txt
├── Program.cs
├── Laptop.cs
└── bin/Debug/net8.0/   ← innen fut a program!
```
Ezért az útvonal **`../../../forras/laptops.txt`** (3 szinttel feljebb a `bin/Debug/net8.0`-ból).

Hogy `dotnet run`-nal is menjen, a `.csproj`-ba a `<PropertyGroup>`-on belülre:
```xml
<RunWorkingDirectory>$(MSBuildProjectDirectory)/bin/$(Configuration)/$(TargetFramework)/</RunWorkingDirectory>
```

Új osztály: Solution Explorer → jobb klikk a projekten → *Add* → *Class* → `Laptop.cs`.

---

## 2. A tipikus feladatsor menete

1. Konzolos projekt a megadott néven.
2. Osztályok az osztálydiagram alapján (`Laptop`, `Category`, `Manufacturer`).
3. Az adatfájl beolvasása egy `List<Laptop>`-ba (az első sor a fejléc → átugrani!).
4. Valamilyen számított érték (átlag, kor, átváltás).
5. **Kiírás** a minta szerint (`ToString` felülírásával).
6. **Szűrés** + kiírás + összesítés (átlag, max, darab).
7. **Fájlba írás** szűrt/rendezett adatokkal.

Mindig írd ki a feladat számát: `Console.WriteLine("5. feladat");`

---

## 3. Segédosztály

Amikor a fájlban `azonosító;név` párok vannak (pl. `3;Gaming`):

```csharp
namespace LaptopsCLI
{
    internal class Category
    {
        public int CategoryCode { get; set; }
        public string CategoryName { get; set; }
    }
}
```

---

## 4. Fő osztály: konstruktor

A konstruktor kap **egy sort** a fájlból, és szétszedi:

```csharp
namespace LaptopsCLI
{
    internal class Laptop
    {
        static int counter = 1;

        public Laptop(string l)
        {
            var r = l.Split(";");               // elválasztó: ";" vagy "," vagy "\t"
            Category = new Category()
            {
                CategoryCode = int.Parse(r[0]),
                CategoryName = r[1]
            };
            CPU = r[2];
            Manufacturer = new Manufacturer()
            {
                ManufacturerCode = int.Parse(r[3]),
                ManufacturerName = r[4]
            };
            Model = r[5];
            OS = r[6];
            Price = double.Parse(r[7]);
            RAM = int.Parse(r[8]);
            Screen = r[9];
            Storage = r[10];
            ID = counter++;
        }

        public Category Category { get; set; }
        public string CPU { get; set; }
        public Manufacturer Manufacturer { get; set; }
        public string Model { get; set; }
        public string OS { get; set; }
        public double Price { get; set; }
        public int RAM { get; set; }
        public string Screen { get; set; }
        public string Storage { get; set; }
        public int ID { get; set; }
    }
}
```

Tipp: nyisd meg a `.txt`-t, és számold meg az oszlopokat: `r[0]`, `r[1]`… sorrendben.

Típusok:
| C# típus | Mikor | Átalakítás |
|---|---|---|
| `int` | egész szám | `int.Parse(r[0])` |
| `double` | tizedes | `double.Parse(r[1])` (lásd 14. pont!) |
| `string` | szöveg | `r[2]` |
| `bool` | igen/nem | `r[3] == "Yes"` vagy `r[3] == "1"` |
| `DateTime` | dátum | `DateTime.Parse(r[4])` |
| `TimeSpan` | időtartam | `TimeSpan.Parse("2:37:04")` |

---

## 5. Sorszám (static counter)

```csharp
static int counter = 1;      // az OSZTÁLYHOZ tartozik, minden példány közösen használja
...
ID = counter++;              // 1, 2, 3, ...
```

Ha a sorszám csak a kiírásnál kell (pl. 6. feladat: „az eredményhalmazon belüli sorszám”):
```csharp
int sorszam = 1;
foreach (var l in f6)
{
    Console.WriteLine($"[{sorszam++}] {l.Category.CategoryName} | {l.Model}");
}
```

---

## 6. ToString felülírása

„Virtuális metódus felhasználásával” = **`override ToString()`**:

```csharp
public override string ToString()
{
    return $"{ID}. {Category.CategoryName} | {Manufacturer.ManufacturerName} {Model} | {OS} | {Math.Round(Price * 4.12, 0)} HUF";
}
```

Többsoros (tabulátorral):
```csharp
public override string ToString()
{
    return $"\tCar ID: {CarId}\n" +
           $"\tBrand: {Brand.BrandName}\n" +
           $"\tYear: {Year}\n" +
           $"\tPrice: {(Price != 0 ? Price.ToString() : "NA")}\n";
}
```

Használat:
```csharp
Console.WriteLine(laptop);              // magától a ToString()-et hívja
Console.WriteLine(laptop.ToString());
```

Saját virtuális metódus (ha öröklődés is van):
```csharp
public virtual string Leiras() => $"{Model} ({OS})";
// leszármazottban:
public override string Leiras() => base.Leiras() + " – gamer";
```

---

## 7. Saját metódus

```csharp
// átlag fogyasztás hétköznap+hétvége alapján (példa)
public double Atlag()
{
    return Math.Round((AmtWeekdays * 5 + AmtWeekends * 2) / 7.0, 2);
}

// éves km (évjárat alapján)
public double KmPerEv()
{
    int kor = DateTime.Now.Year - Year;
    if (kor == 0) return 0;
    return Math.Round((double)Mileage / kor, 2);
}

// ár forintban
public int ArHuf() => (int)Math.Round(Price * 4.12);
```

⚠ Egész osztás: `5 / 2 = 2`! Tizedeshez: `(double)a / b` vagy `a / 2.0`.

---

## 8. Beolvasás fájlból

```csharp
List<Laptop> laptops = new List<Laptop>();

using (StreamReader o = new StreamReader("../../../forras/laptops.txt"))
{
    o.ReadLine();                        // FEJLÉC átugrása!
    while (!o.EndOfStream)
    {
        string line = o.ReadLine();
        laptops.Add(new Laptop(line));
    }
}
```

Ékezetes fájl, ha kérdőjelek jönnek:
```csharp
new StreamReader("../../../forras/a.txt", System.Text.Encoding.UTF8)
// régi, ANSI (Windows-1250) fájlnál:
// System.Text.Encoding.Latin1
```

Rövidebben (egy sorban):
```csharp
var laptops = File.ReadAllLines("../../../forras/laptops.txt")
                  .Skip(1)                       // fejléc
                  .Select(sor => new Laptop(sor))
                  .ToList();
```

Üres sorok kihagyása:
```csharp
if (string.IsNullOrWhiteSpace(line)) continue;
```

---

## 9. Kiírás, formázás

```csharp
Console.WriteLine("5. feladat");
Console.WriteLine($"\tA versenyzők száma: {runners.Count} fő");    // \t = tabulátor
Console.Write("nincs sortörés ");

// első 10 elem
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(laptops[i]);
}

// mind
foreach (var l in laptops)
{
    Console.WriteLine(l);
}

// formázások
$"{ar:N0}"        // 11 900 (ezres tagolás, 0 tizedes)
$"{atlag:F2}"     // 3,14 (2 tizedes)
$"{atlag:0.00}"   // 3,14
$"{szazalek:P1}"  // 45,3 %
$"{nev,-20}|"     // balra igazítva 20 karakteren (táblázatos kiírás)
$"{ar,10}"        // jobbra igazítva 10 karakteren
Math.Round(x, 2)

// feltételes kiírás
$"Ár: {(Price != 0 ? Price.ToString() : "NA")}"
$"{(Visszahozva ? "Igen" : "Nem")}"

// ToString-ben lévő tabulátorok "eltolása" (beljebb kezdés)
Console.WriteLine(gyoztes.ToString().Replace("\t", "\t\t"));

// ékezetek a konzolon
Console.OutputEncoding = System.Text.Encoding.UTF8;   // a Main elejére
```

---

## 10. LINQ

`using System.Linq;` – .NET 6+ alatt magától benne van.

```csharp
// SZŰRÉS
var f6 = laptops.Where(x => x.CPU.Contains("Intel Core i7") && x.Storage.Contains("SSD"));
var britek = residents.Where(x => x.Smoke == "Yes"
                                && x.Nationality.NationalityName == "British"
                                && !x.GrossIncome.Contains("Under"));

// RENDEZÉS
laptops.OrderBy(x => x.Price);                    // növekvő
laptops.OrderByDescending(x => x.Price);          // csökkenő
orszagok.OrderByDescending(x => x.Arany)
        .ThenByDescending(x => x.Ezust)
        .ThenBy(x => x.Nev);                       // több szempont

// ELSŐ / LEGJOBB
var legolcsobb = laptops.OrderBy(x => x.Price).First();
var elso = laptops.FirstOrDefault(x => x.RAM > 32);   // nincs: null
if (elso == null) Console.WriteLine("Nincs ilyen.");
var max = laptops.MaxBy(x => x.Price);                // .NET 6+: maga az objektum

// TOP N
var top20 = laptops.Where(x => x.Category.CategoryName == "Gaming")
                   .OrderBy(x => x.Price)
                   .Take(20);
laptops.Skip(10).Take(5);                          // lapozás

// SZÁMOLÁS
laptops.Count();                                   // összes
laptops.Count(x => x.OS == "Windows 10");          // feltétellel
laptops.Sum(x => x.Price);
laptops.Average(x => x.Price);
laptops.Max(x => x.Price);                         // csak az érték
laptops.Min(x => x.RAM);

// VAN-E / MIND
laptops.Any(x => x.Price > 500000);                // van-e legalább egy
laptops.All(x => x.RAM >= 4);                      // mind ilyen-e

// ÁTALAKÍTÁS
var nevek = laptops.Select(x => x.Model).ToList();
var gyartok = laptops.Select(x => x.Manufacturer.ManufacturerName).Distinct().ToList();
var db = laptops.Select(x => x.Manufacturer.ManufacturerName).Distinct().Count();

// "szöveges szám" maximuma (pl. "2600 to 5200" felső határa)
var maxJovedelem = f6.Select(x => int.Parse(x.GrossIncome.Split(" to ")[1])).Max();

// ÜRES eredmény kezelése (Average/Max üres listán HIBÁT dob!)
if (f6.Any())
{
    Console.WriteLine($"Átlag: {f6.Average(x => x.Price):F2}");
}
else
{
    Console.WriteLine("Nincs ilyen elem.");
}
```

⚠ A `Where` **lusta** – ha többször bejárod, minden alkalommal újra lefut. Ha rögzíteni akarod: `.ToList()`.

LINQ nélkül (ha ezt kérik):
```csharp
int szamlalo = 0;
double maxAr = 0;
Laptop legdragabb = null;
foreach (var l in laptops)
{
    if (l.OS == "Windows 10") szamlalo++;
    if (l.Price > maxAr) { maxAr = l.Price; legdragabb = l; }
}
```

---

## 11. Csoportosítás (GroupBy)

```csharp
// távonként: indulók, célba értek, legjobb idő
var tavok = runners.GroupBy(x => x.Distance.DistanceName);
foreach (var csoport in tavok)
{
    int indulo = csoport.Count();
    int celba = csoport.Count(x => x.Time != "DNF");
    var legjobb = csoport.Where(x => x.Time != "DNF").Min(x => x.Time);
    Console.WriteLine($"{csoport.Key}: {indulo} indulóból {celba} ért célba, legjobb idő: {legjobb}");
}

// kategóriánként darabszám, csökkenő sorrendben
var statisztika = laptops
    .GroupBy(x => x.Category.CategoryName)
    .Select(g => new { Kategoria = g.Key, Db = g.Count(), Atlag = g.Average(x => x.Price) })
    .OrderByDescending(x => x.Db);
foreach (var s in statisztika)
{
    Console.WriteLine($"{s.Kategoria}: {s.Db} db, átlag {s.Atlag:F0}");
}

// Dictionary-be
Dictionary<string, int> darabok = laptops
    .GroupBy(x => x.OS)
    .ToDictionary(g => g.Key, g => g.Count());
```

---

## 12. Fájlba írás

```csharp
Console.WriteLine("7. feladat: Fájlba írás");

var f7 = residents.Where(x => x.Age >= 25 && x.Age <= 30 && x.Region == "Wales" && x.Smoke == "No")
                  .OrderByDescending(x => x.Age);

using (StreamWriter iro = new StreamWriter("../../../forras/wales.txt"))
{
    // iro.WriteLine("Kor;Nem;Végzettség");   // fejléc, ha kéri
    foreach (var f in f7)
    {
        iro.WriteLine($"{f.Age} {f.Gender} {f.Qualification}");
    }
}
```

Több sor egy elemhez (behúzással):
```csharp
iro.WriteLine($"{l.Manufacturer.ManufacturerName} {l.Model}");
iro.WriteLine($"\t\t{l.CPU}");
iro.WriteLine($"\t\t{l.Storage}");
iro.WriteLine($"\t\t{l.Screen}\n");
```

Hozzáfűzés (nem írja felül): `new StreamWriter(utvonal, true)`

Egy sorban:
```csharp
File.WriteAllLines("../../../forras/ki.txt", f7.Select(x => $"{x.Age};{x.Gender}"));
```

⚠ A `using` blokk zárja le a fájlt – nélküle üres maradhat a kimeneti fájl!

---

## 13. Bekérés

```csharp
Console.Write("Kérek egy fajtát: ");
string fajta = Console.ReadLine();

Console.Write("Kérek egy évet: ");
int ev = int.Parse(Console.ReadLine());

// biztonságos (nem dob hibát rossz bemenetre)
int szam;
while (!int.TryParse(Console.ReadLine(), out szam))
{
    Console.Write("Hibás szám, add meg újra: ");
}

// van-e ilyen?
var talalat = kutyak.Where(x => x.Fajta.ToLower() == fajta.ToLower()).ToList();
if (talalat.Count == 0)
{
    Console.WriteLine("Nincs ilyen fajtájú kutya a menhelyen.");
}
```

---

## 14. Számok, NA értékek

**Tizedes vessző/pont probléma!** Magyar Windows-on a `double.Parse("3.5")` HIBÁT dob vagy 35-öt ad.
```csharp
using System.Globalization;
double ar = double.Parse(r[7], CultureInfo.InvariantCulture);   // "3.5" → 3,5
// vagy:
double ar = double.Parse(r[7].Replace(".", ","));
```

Hiányzó érték (NA, üres, "-"):
```csharp
Mileage = r[4] == "NA" ? 0 : int.Parse(r[4]);

// int? = lehet null
public int? Price { get; set; }
Price = r[5] == "NA" ? null : int.Parse(r[5]);
var ismertAruak = cars.Where(x => x.Price != null);

// TryParse – ha nem szám, 0 lesz
int.TryParse(r[4], out int km);
```

Matek:
```csharp
Math.Round(3.456, 2);   // 3,46
Math.Floor(3.9);        // 3
Math.Ceiling(3.1);      // 4
Math.Abs(-5);           // 5
Math.Pow(2, 3);         // 8
Math.Sqrt(16);          // 4
(int)3.9                // 3 (levágja)
10 % 3                  // 1 (maradék)
```

---

## 15. Stringek

```csharp
s.Split(";")                 // tömb
s.Split(" to ")              // több karakteres elválasztó
s.Contains("SSD")            // tartalmaz-e (kis/nagybetű ÉRZÉKENY!)
s.ToLower().Contains(keres.ToLower())   // érzéketlen keresés
s.StartsWith("Intel")
s.EndsWith("GB")
s.Replace("GB", "")
s.Trim()
s.Substring(0, 3)            // első 3 karakter
s.IndexOf(" ")               // első szóköz helye (-1 ha nincs)
s.Length
s.ToUpper()
string.IsNullOrEmpty(s)
string.Join(", ", lista)     // lista → "a, b, c"
s == "Yes"                   // összehasonlítás

// pl. "8GB" → 8
int ram = int.Parse(r[8].Replace("GB", ""));
// pl. "15.6 col - 1366x768" → két rész
var kepernyo = Screen.Split(" - ");   // [0]: "15.6 col", [1]: "1366x768"
```

---

## 16. Dátum és idő

```csharp
DateTime d = DateTime.Parse("2026-03-02 15:27:50");
d.Year; d.Month; d.Day; d.Hour; d.DayOfWeek;
DateTime.Now; DateTime.Today;
d.ToString("yyyy.MM.dd.");
(DateTime.Now - d).TotalDays;          // eltelt napok
int kor = DateTime.Now.Year - SzuletesiEv;

// időtartam: "2:37:04"
TimeSpan ido = TimeSpan.Parse("2:37:04");
ido.TotalSeconds;                       // 9424
ido.TotalMinutes;
TimeSpan.FromSeconds(223).ToString(@"m\:ss");   // "3:43" (tempó/km)

// idő → másodperc kézzel
public int TimeInSeconds()
{
    var t = Time.Split(":");
    return int.Parse(t[0]) * 3600 + int.Parse(t[1]) * 60 + int.Parse(t[2]);
}

// leggyorsabb (a DNF-et kiszűrve!)
var gyoztes = runners.Where(x => x.Time != "DNF").OrderBy(x => x.TimeInSeconds()).First();
```

---

## 17. Öröklődés

```csharp
internal class Jarmu
{
    public string Rendszam { get; set; }
    public Jarmu(string rendszam) { Rendszam = rendszam; }
    public virtual string Leiras() => $"Jármű: {Rendszam}";
}

internal class Auto : Jarmu
{
    public int Ajtok { get; set; }
    public Auto(string rendszam, int ajtok) : base(rendszam)   // ős konstruktora
    {
        Ajtok = ajtok;
    }
    public override string Leiras() => base.Leiras() + $", {Ajtok} ajtós";
}

// abstract: nem példányosítható, a leszármazottnak kötelező megvalósítani
abstract class Alakzat
{
    public abstract double Terulet();
}

// interface
interface IArazhato
{
    int Ar { get; }
}

// enum
enum Valto { Manualis, Automata }
```

---

## 18. Teljes minta

```csharp
namespace LaptopsCLI
{
    internal class Program
    {
        static void Main(string[] args)
        {
            List<Laptop> laptops = new List<Laptop>();

            // 3-4. beolvasás
            using (StreamReader o = new StreamReader("../../../forras/laptops.txt"))
            {
                o.ReadLine();
                while (!o.EndOfStream)
                {
                    string line = o.ReadLine();
                    laptops.Add(new Laptop(line));
                }
            }

            // 5. kiírás
            Console.WriteLine("5. feladat");
            foreach (var l in laptops)
            {
                Console.WriteLine(l);
            }

            // 6. szűrés + átlag
            Console.WriteLine("6. feladat");
            var f6 = laptops.Where(x => x.CPU.Contains("Intel Core i7") && x.Storage.Contains("SSD")).ToList();
            int counter = 1;
            foreach (var l in f6)
            {
                Console.WriteLine($"[{counter++}] {l.Category.CategoryName} | {l.Manufacturer.ManufacturerName} {l.Model} | {l.OS}");
            }
            if (f6.Any())
            {
                Console.WriteLine($"A laptopok átlagára: {f6.Average(x => x.Price):F2} INR");
            }

            // 7. fájlba írás
            Console.WriteLine("7. feladat - Fájlba írás");
            var f7 = laptops.Where(x => x.Category.CategoryName == "Gaming")
                            .OrderBy(x => x.Price)
                            .Take(20);
            using (StreamWriter iro = new StreamWriter("../../../forras/cheap.txt"))
            {
                foreach (var l in f7)
                {
                    iro.WriteLine($"{l.Manufacturer.ManufacturerName} {l.Model}");
                    iro.WriteLine($"\t\t{l.CPU}");
                    iro.WriteLine($"\t\t{l.Storage}");
                    iro.WriteLine($"\t\t{l.Screen}\n");
                }
            }
        }
    }
}
```

---

## 19. Tipikus hibák

| Hiba | Ok | Megoldás |
|---|---|---|
| `DirectoryNotFoundException` / `FileNotFoundException` | rossz útvonal | `../../../forras/...`, a fájl a projektmappában a `forras`-ban van-e? |
| `FormatException` a `Parse`-nál | a fejlécet is feldolgozod, vagy "NA", vagy tizedespont | `o.ReadLine()` a ciklus előtt; `== "NA" ? 0 :`; `CultureInfo.InvariantCulture` |
| `IndexOutOfRangeException` | rossz elválasztó (`;` vs `,` vs `\t`), vagy kevesebb oszlop | nézd meg a txt-t, `Split` paraméter |
| `InvalidOperationException: Sequence contains no elements` | `First()`, `Max()`, `Average()` üres listán | előtte `if (lista.Any())`, vagy `FirstOrDefault()` |
| `NullReferenceException` | `FirstOrDefault` null-t adott | `if (x != null)` |
| üres kimeneti fájl | nincs `using` / nincs lezárva | `using (StreamWriter ...)` |
| `5 / 2 = 2` | egész osztás | `(double)a / b` |
| kérdőjeles ékezetek | kódolás | `Encoding.UTF8` / `Console.OutputEncoding` |
| a `Contains` nem talál | kis/nagybetű | `.ToLower()` mindkét oldalon |
