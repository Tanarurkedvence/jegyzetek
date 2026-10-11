# LINQ összefoglaló – listán belüli szűrés C#-ban

[← vissza](README.md) · **Futtatható példaprojekt:** [LinqPeldak](https://github.com/Tanarurkedvence/LinqPeldak) (mind a 44 példa a laptops.txt valódi adatain, az eredmények alább)

A példák a `List<Laptop> laptops` listán futnak (a Laptops feladat adatai, 325 laptop).
A `Laptop` mezői: `Category.CategoryName`, `CPU`, `Manufacturer.ManufacturerName`, `Model`, `OS`, `Price` (double, INR), `RAM` (int), `Screen`, `Storage`, `ID`.

> `x => ...` = „minden x elemre” (lambda). Az `x` helyett bármilyen nevet írhatsz (`l => l.Price`).

## Tartalom
- [Gyors táblázat: „a feladat ezt kéri” → ezt írd](#gyors-táblázat)
- [Szűrés (Where)](#szűrés-where) – 01–10
- [Rendezés (OrderBy)](#rendezés-orderby) – 11–14
- [Kiválasztás (First, Take, Max-os elem)](#kiválasztás) – 15–19
- [Összesítés (Count, Sum, Average, Min, Max)](#összesítés) – 20–24
- [Átalakítás (Select, Distinct)](#átalakítás-select) – 25–30
- [Csoportosítás (GroupBy)](#csoportosítás-groupby) – 31–37
- [Haladó: átlag feletti, duplikátum, halmazok](#haladó) – 38–40
- [Query szintaxis és LINQ nélkül (foreach)](#query-szintaxis-és-foreach) – 41–42
- [Fájlba írás LINQ eredménnyel](#fájlba-írás) – 43–44
- [ToList: mikor kell?](#tolist-mikor-kell)
- [Tipikus hibák](#tipikus-hibák)

---

## Gyors táblázat

| A feladat szövege | LINQ |
|---|---|
| „Hány darab … van?” | `lista.Count(x => feltétel)` |
| „Írja ki azokat, amelyek …” | `lista.Where(x => feltétel)` + `foreach` |
| „… és …” / „… vagy …” | `&&` / `\|\|` a `Where`-ben |
| „… tartalmazza a … szót” | `x.Nev.Contains("szó")` |
| „kis- és nagybetűtől függetlenül” | `x.Nev.ToLower().Contains(keres.ToLower())` |
| „Átlagosan mennyi …” | `lista.Average(x => x.Ar)` |
| „Összesen mennyi …” | `lista.Sum(x => x.Ar)` |
| „Mennyi a legnagyobb/legkisebb …” (érték) | `lista.Max(x => x.Ar)` / `Min` |
| „Melyik a legdrágább …” (elem) | `lista.OrderByDescending(x => x.Ar).First()` vagy `lista.MaxBy(x => x.Ar)` |
| „Az első 10 / 20 legolcsóbb …” | `.OrderBy(x => x.Ar).Take(20)` |
| „… szerint növekvő/csökkenő sorrendben” | `.OrderBy(...)` / `.OrderByDescending(...)` |
| „azonos … esetén … szerint” | `.ThenBy(...)` / `.ThenByDescending(...)` |
| „Van-e …?” | `lista.Any(x => feltétel)` |
| „Mindegyik …?” | `lista.All(x => feltétel)` |
| „Hány különböző …?” | `lista.Select(x => x.Gyarto).Distinct().Count()` |
| „… -nként / kategóriánként / városonként” | `lista.GroupBy(x => x.Kategoria)` |
| „Melyik a leggyakoribb …?” | `.GroupBy(...).OrderByDescending(g => g.Count()).First().Key` |
| „Hány százaléka …?” | `(double)lista.Count(feltétel) / lista.Count * 100` |
| „Ha nincs ilyen, írja ki: …” | `var x = lista.FirstOrDefault(...); if (x == null) ...` vagy `if (!lista.Any(...))` |

---

## Szűrés (Where)

### 01. Egy feltétel
```csharp
var gaming = laptops.Where(x => x.Category.CategoryName == "Gaming").ToList();
Console.WriteLine($"Gaming laptopok: {gaming.Count} db");
```
> Gaming laptopok: 55 db

### 02. Több feltétel – ÉS (`&&`)
```csharp
var i7ssd = laptops.Where(x => x.CPU.Contains("Intel Core i7") && x.Storage.Contains("SSD")).ToList();
```
> i7 + SSD: 111 db

Hosszú feltétel több sorba tördelve (olvashatóbb):
```csharp
var f6 = residents.Where(x => x.Smoke == "Yes"
                           && x.Nationality.NationalityName == "British"
                           && x.MaritalStatus == "Married"
                           && !x.GrossIncome.Contains("Under"));
```

### 03. VAGY (`||`)
```csharp
var appleVagyMsi = laptops.Where(x => x.Manufacturer.ManufacturerName == "Apple"
                                   || x.Manufacturer.ManufacturerName == "MSI").ToList();
```
> Apple vagy MSI: 24 db

⚠ ÉS + VAGY együtt → zárójelezz: `x => x.RAM >= 8 && (x.OS == "Windows 10" || x.OS == "Linux")`

### 04. Tartomány
```csharp
var kozepes = laptops.Where(x => x.RAM >= 8 && x.RAM <= 16).ToList();
```
> 8–16 GB RAM: 190 db

Évszám tartomány: `x => x.Year >= 2018 && x.Year <= 2021`

### 05. Tagadás (`!`) és nem egyenlő (`!=`)
```csharp
var nemWindows = laptops.Where(x => !x.OS.Contains("Windows")).ToList();
var celbaErtek = runners.Where(x => x.Time != "DNF");
```
> Nem Windows: 38 db

### 06. Kis/nagybetű független keresés
```csharp
string keres = "zenbook";
var talalat = laptops.Where(x => x.Model.ToLower().Contains(keres.ToLower())).ToList();
```
> 'zenbook' a modellnévben: 5 db

Alternatíva: `x.Model.Contains(keres, StringComparison.OrdinalIgnoreCase)`

### 07. Kezdődik / végződik
```csharp
laptops.Count(x => x.CPU.StartsWith("Intel"));     // 310
laptops.Count(x => x.Storage.EndsWith("SSD"));     // 141
```

### 08. Egy listában szereplő értékek közül
```csharp
var kertGyartok = new List<string> { "Dell", "Lenovo" };
var dellLenovo = laptops.Count(x => kertGyartok.Contains(x.Manufacturer.ManufacturerName));
```
> Dell vagy Lenovo: 136 db

### 09. Számolt értékre szűrés
```csharp
var dragaHuf = laptops.Where(x => x.Price * 4.12 > 400000).ToList();
// metódussal: laptops.Where(x => x.ArHuf() > 400000)
```
> 400 000 Ft felett: 16 db

### 10. Szövegből kivágott számra szűrés
```csharp
// Screen: "15.6 col - 1366x768" → első szó "15.6" → 15,6
var nagyKepernyo = laptops.Where(x => double.Parse(x.Screen.Split(" ")[0].Replace(".", ",")) >= 17).ToList();
```
> Legalább 17 colos: 33 db

Más példák:
```csharp
// "2600 to 5200" → felső határ 5200
x => int.Parse(x.GrossIncome.Split(" to ")[1]) > 5000
// "8GB" → 8
x => int.Parse(x.Ram.Replace("GB", "")) >= 8
// "NA" kiszűrése előbb!
x => x.Price != "NA" && int.Parse(x.Price) > 1000000
```

---

## Rendezés (OrderBy)

### 11. Növekvő
```csharp
foreach (var l in laptops.OrderBy(x => x.Price).Take(3))
    Console.WriteLine($"{l.Manufacturer.ManufacturerName} {l.Model}: {l.Price:F0} INR");
```
> Acer C740-C9QX (3205U/2GB/32GB/Chrome: 7285 INR
> Vero V131 (X5-Z8350/4GB/32GB/FHD/W10): 8206 INR
> Acer Chromebook 15: 8750 INR

### 12. Csökkenő
```csharp
laptops.OrderByDescending(x => x.Price).Take(3)
```
> Asus ROG G701VO: 166419 INR · HP ZBook 17: 165348 INR · Razer Blade Pro: 146491 INR

### 13. Több szempont (ThenBy)
```csharp
laptops.OrderByDescending(x => x.RAM).ThenBy(x => x.Price)
```
> 64 GB – 166419 INR – Asus ROG G701VO
> 32 GB – 135647 INR – Lenovo IdeaPad Y900-17ISK
> 16 GB – 41447 INR – Dell Inspiron 5567

Éremtábla-sorrend (arany, ezüst, bronz, majd név):
```csharp
orszagok.OrderByDescending(x => x.Arany).ThenByDescending(x => x.Ezust).ThenByDescending(x => x.Bronz).ThenBy(x => x.Nev)
```

⚠ Két `OrderBy` egymás után = a második **felülírja** az elsőt. Második szempont mindig `ThenBy`!

### 14. Név szerint (ABC)
```csharp
laptops.Select(x => x.Manufacturer.ManufacturerName).Distinct().OrderBy(x => x)
```
> Acer, Apple, Asus, Dell, Fujitsu …

---

## Kiválasztás

### 15. First / FirstOrDefault
```csharp
var elsoApple = laptops.First(x => x.Manufacturer.ManufacturerName == "Apple");   // MacBook 12

var nincs = laptops.FirstOrDefault(x => x.RAM > 1000);     // nincs ilyen → null
if (nincs == null) Console.WriteLine("Nincs ilyen.");
```
⚠ A `First()` **hibát dob**, ha nincs találat; a `FirstOrDefault()` `null`-t ad.

### 16. Last, ElementAt
```csharp
laptops.Last();          // utolsó
laptops.ElementAt(4);    // 5. elem (= laptops[4])
```

### 17. A legdrágább elem (az egész objektum) – 3 módszer
```csharp
var a = laptops.OrderByDescending(x => x.Price).First();
var b = laptops.MaxBy(x => x.Price);                          // .NET 6+
var c = laptops.First(x => x.Price == laptops.Max(y => y.Price));
```
> ROG G701VO | ROG G701VO | ROG G701VO

⚠ `laptops.Max(x => x.Price)` csak a **számot** adja vissza (166419,…), nem a laptopot!

### 18. Take / Skip
```csharp
laptops.OrderBy(x => x.Price).Take(5);             // 5 legolcsóbb
laptops.OrderBy(x => x.Price).Skip(5).Take(5);     // 6–10. hely
```

### 19. TakeWhile
```csharp
laptops.OrderBy(x => x.Price).TakeWhile(x => x.Price < 15000).Count();   // 26
```

---

## Összesítés

### 20. Count
```csharp
laptops.Count();                              // 325
laptops.Count(x => x.OS == "Windows 10");     // 252
```

### 21. Sum, Average, Min, Max
```csharp
laptops.Sum(x => x.RAM);          // 2574 GB
laptops.Average(x => x.Price);    // 46573,29 INR
laptops.Min(x => x.RAM);          // 2
laptops.Max(x => x.RAM);          // 64
```
Kerekítve kiírva: `$"{laptops.Average(x => x.Price):F2}"` vagy `Math.Round(..., 2)`.

### 22. Szűrt listán – és az ÜRES lista!
```csharp
var apple = laptops.Where(x => x.Manufacturer.ManufacturerName == "Apple").ToList();
if (apple.Any())
    Console.WriteLine($"Apple átlagár: {apple.Average(x => x.Price):F2} INR");   // 49203,60
else
    Console.WriteLine("Nincs ilyen laptop.");
```
⚠ Üres listán az `Average`, `Max`, `Min`, `First` **kivételt dob** (`Sequence contains no elements`). A `Count` és a `Sum` üres listán 0.

### 23. Any / All
```csharp
laptops.Any(x => x.RAM == 32);    // True – van-e legalább egy
laptops.All(x => x.RAM >= 2);     // True – mind ilyen-e
```

### 24. Százalék
```csharp
double arany = (double)laptops.Count(x => x.Storage.Contains("SSD")) / laptops.Count * 100;
Console.WriteLine($"{arany:F2}%");     // 58,15%
```
⚠ A `(double)` kell, különben egész osztás → 0!

---

## Átalakítás (Select)

### 25. Egy mező kigyűjtése
```csharp
List<string> modellek = laptops.Select(x => x.Model).ToList();
Console.WriteLine(string.Join(" | ", modellek.Take(3)));
```
> 15-bs053od (i7-7500U/6GB/1TB/W10) | Rog GL753VE-DS74 | Inspiron 7579

### 26. Számolt érték
```csharp
laptops.Select(x => Math.Round(x.Price * 4.12))      // 99872, 301685, 224064 …
```

### 27. Névtelen objektum (több mező)
```csharp
var lista = laptops.Select(x => new
{
    Nev = x.Manufacturer.ManufacturerName + " " + x.Model,
    Huf = Math.Round(x.Price * 4.12)
});
foreach (var x in lista) Console.WriteLine($"{x.Nev}: {x.Huf} Ft");
```
> HP 15-bs053od (i7-7500U/6GB/1TB/W10): 99872 Ft

### 28. Sorszám az eredményben (Select indexszel)
```csharp
foreach (var s in i7ssd.Select((l, i) => $"[{i + 1}] {l.Category.CategoryName} | {l.Model}"))
    Console.WriteLine(s);
```
> [1] Gaming | Rog GL753VE-DS74
> [2] 2 in 1 Convertible | Inspiron 7579

(Egyszerűbb: `int sorszam = 1;` és a `foreach`-ben `sorszam++`.)

### 29. Distinct – egyedi értékek
```csharp
var gyartok = laptops.Select(x => x.Manufacturer.ManufacturerName).Distinct().ToList();
Console.WriteLine($"{gyartok.Count} gyártó");
```
> 12 gyártó: Acer, Apple, Asus, Dell, Fujitsu, HP, Lenovo, MSI, Razer, Samsung, Toshiba, Vero

### 30. Szövegből kivágott szám maximuma
```csharp
// CPU: "Intel Core i7 7500U 2.7GHz" → utolsó szó → "2.7" → 2,7
var maxGhz = laptops.Where(x => x.CPU.EndsWith("GHz"))
                    .Select(x => double.Parse(x.CPU.Split(" ").Last().Replace("GHz", "").Replace(".", ",")))
                    .Max();
```
> Legnagyobb órajel: 3,6 GHz

---

## Csoportosítás (GroupBy)

A `GroupBy` csoportokat ad; minden csoportnak van `Key`-e (ami szerint csoportosítottál), és maga a csoport egy lista (lehet rá `Count()`, `Average()`, `OrderBy()`…).

### 31. Darabszám csoportonként
```csharp
foreach (var g in laptops.GroupBy(x => x.Category.CategoryName).OrderByDescending(g => g.Count()))
    Console.WriteLine($"{g.Key}: {g.Count()} db");
```
> Notebook: 178 db · Gaming: 55 db · Ultrabook: 44 db · 2 in 1 Convertible: 35 db · Netbook: 8 db · Workstation: 5 db

### 32. Statisztika csoportonként
```csharp
var stat = laptops.GroupBy(x => x.Manufacturer.ManufacturerName)
                  .Select(g => new { Gyarto = g.Key, Db = g.Count(), Atlag = g.Average(x => x.Price), Max = g.Max(x => x.Price) })
                  .OrderByDescending(x => x.Atlag);
foreach (var s in stat)
    Console.WriteLine($"{s.Gyarto}: {s.Db} db, átlag {s.Atlag:F0}, max {s.Max:F0} INR");
```
> Razer: 2 db, átlag 94786, max 146491 INR
> MSI: 20 db, átlag 68053, max 104624 INR

### 33. Minden csoport legjobb eleme
```csharp
foreach (var g in laptops.GroupBy(x => x.Category.CategoryName).OrderBy(g => g.Key))
{
    var legolcsobb = g.OrderBy(x => x.Price).First();
    Console.WriteLine($"{g.Key}: {legolcsobb.Manufacturer.ManufacturerName} {legolcsobb.Model}");
}
```
> Gaming: Lenovo Legion Y520-15IKBN (34289 INR) · Ultrabook: Asus ZenBook UX305CA-UBM1 (30521 INR) …

Futóverseny-minta (távonként indulók, célba értek, legjobb idő):
```csharp
foreach (var g in runners.GroupBy(x => x.Distance.DistanceName))
{
    int celba = g.Count(x => x.Time != "DNF");
    var legjobb = g.Where(x => x.Time != "DNF").Min(x => x.Time);
    Console.WriteLine($"{g.Key}: {g.Count()} indulóból {celba} ért célba, legjobb idő: {legjobb}");
}
```

### 34. Csak bizonyos csoportok (Where a csoportokra – SQL HAVING)
```csharp
laptops.GroupBy(x => x.Manufacturer.ManufacturerName).Where(g => g.Count() >= 40).Select(g => g.Key);
```
> HP, Dell, Lenovo

### 35. Két mező szerint
```csharp
laptops.GroupBy(x => new { x.Manufacturer.ManufacturerName, x.Category.CategoryName })
       .OrderByDescending(g => g.Count())
// g.Key.ManufacturerName, g.Key.CategoryName
```
> HP / Notebook: 65 db · Lenovo / Notebook: 38 db · Dell / Notebook: 36 db

### 36. ToDictionary
```csharp
Dictionary<string, int> osDb = laptops.GroupBy(x => x.OS).ToDictionary(g => g.Key, g => g.Count());
Console.WriteLine(osDb["Windows 10"]);                                    // 252
int mac = osDb.ContainsKey("macOS") ? osDb["macOS"] : 0;                 // 0 – nincs ilyen kulcs
```

### 37. Leggyakoribb érték
```csharp
var leggyakoribb = laptops.GroupBy(x => x.RAM).OrderByDescending(g => g.Count()).First();
Console.WriteLine($"{leggyakoribb.Key} GB ({leggyakoribb.Count()} db)");
```
> 8 GB (136 db)

---

## Haladó

### 38. Átlag feletti elemek
```csharp
double atlag = laptops.Average(x => x.Price);          // ELŐRE kiszámolni (gyorsabb)!
laptops.Count(x => x.Price > atlag);                   // 143
```

### 39. Duplikátumok
```csharp
laptops.GroupBy(x => x.Model).Where(g => g.Count() > 1).OrderByDescending(g => g.Count())
```
> Inspiron 3567: 9x · XPS 13: 8x · EliteBook 840: 7x

### 40. Halmazműveletek
```csharp
var gamingGy = laptops.Where(x => x.Category.CategoryName == "Gaming").Select(x => x.Manufacturer.ManufacturerName).Distinct();
var ultraGy  = laptops.Where(x => x.Category.CategoryName == "Ultrabook").Select(x => x.Manufacturer.ManufacturerName).Distinct();
gamingGy.Intersect(ultraGy);   // mindkettőben: Asus, Dell, HP, Lenovo, Razer
gamingGy.Except(ultraGy);      // csak az elsőben: Acer, MSI
gamingGy.Union(ultraGy);       // bármelyikben (egyediként)
```

Két lista összekapcsolása (Join):
```csharp
var eredmeny = kolcsonzesek.Join(olvasok,
                                 k => k.OlvasoId,
                                 o => o.Id,
                                 (k, o) => new { o.Nev, k.KonyvCim });
```

---

## Query szintaxis és foreach

### 41. Query szintaxis (SQL-szerű) – ugyanazt tudja
```csharp
var query = from l in laptops
            where l.CPU.Contains("Intel Core i7") && l.Storage.Contains("SSD")
            orderby l.Price descending
            select l;
```
> 111 db, legdrágább: ROG G701VO

```csharp
// csoportosítás query szintaxissal
var csop = from l in laptops
           group l by l.Category.CategoryName into g
           select new { Kategoria = g.Key, Db = g.Count() };
```

### 42. LINQ NÉLKÜL (ha a feladat tiltja, vagy „programozási tételekkel” kéri)
```csharp
int db = 0;
double osszeg = 0;
Laptop max = null;
foreach (var l in laptops)
{
    if (l.CPU.Contains("Intel Core i7") && l.Storage.Contains("SSD"))   // szűrés
    {
        db++;                                                           // megszámlálás
        osszeg += l.Price;                                              // összegzés
        if (max == null || l.Price > max.Price) max = l;                // maximumkiválasztás
    }
}
Console.WriteLine($"{db} db, átlag {osszeg / db:F2} INR, legdrágább: {max.Model}");
```
> 111 db, átlag 70978,71 INR, legdrágább: ROG G701VO

Eldöntés (van-e) és kiválasztás (az első) LINQ nélkül:
```csharp
bool van = false;
foreach (var l in laptops) { if (l.RAM == 32) { van = true; break; } }

Laptop elso = null;
foreach (var l in laptops) { if (l.Manufacturer.ManufacturerName == "Apple") { elso = l; break; } }
```

Csoportosítás LINQ nélkül (Dictionary):
```csharp
Dictionary<string, int> darab = new Dictionary<string, int>();
foreach (var l in laptops)
{
    string k = l.Category.CategoryName;
    if (darab.ContainsKey(k)) darab[k]++;
    else darab[k] = 1;
}
foreach (var par in darab) Console.WriteLine($"{par.Key}: {par.Value} db");
```

---

## Fájlba írás

### 43. Szűrt, rendezett lista
```csharp
var cheap = laptops.Where(x => x.Category.CategoryName == "Gaming").OrderBy(x => x.Price).Take(20);
using (StreamWriter iro = new StreamWriter("../../../forras/cheap.txt"))
{
    foreach (var l in cheap)
    {
        iro.WriteLine($"{l.Manufacturer.ManufacturerName} {l.Model}");
        iro.WriteLine($"\t\t{l.CPU}");
        iro.WriteLine($"\t\t{l.Storage}");
        iro.WriteLine($"\t\t{l.Screen}\n");
    }
}
```

### 44. Csoportos statisztika pontosvesszővel
```csharp
using (StreamWriter iro = new StreamWriter("../../../forras/statisztika.txt"))
{
    iro.WriteLine("Kategoria;Darab;Atlagar");
    foreach (var g in laptops.GroupBy(x => x.Category.CategoryName).OrderBy(g => g.Key))
        iro.WriteLine($"{g.Key};{g.Count()};{g.Average(x => x.Price):F2}");
}
```

---

## ToList: mikor kell?

- A `Where`, `OrderBy`, `Select` **nem fut le azonnal** – csak amikor bejárod (foreach, Count, First…). Minden bejárásnál **újra lefut**.
- `.ToList()` → egyszer lefut, és az eredmény egy rendes lista lesz (van `.Count` property-je és `[i]` indexelése).
- Kell a `ToList()`, ha:
  - indexelni akarsz: `lista[i]` (pl. ListBox `SelectedIndex` alapján!),
  - többször használod az eredményt (gyorsabb),
  - az eredeti listát módosítanád közben.

```csharp
var szurt = laptops.Where(x => x.RAM >= 16).ToList();
Console.WriteLine(szurt.Count);     // property, nem metódus
Console.WriteLine(szurt[0].Model);  // indexelhető
```

---

## Tipikus hibák

| Hiba | Ok | Megoldás |
|---|---|---|
| `Sequence contains no elements` | `First`, `Max`, `Average` üres listán | `if (lista.Any())`, `FirstOrDefault` |
| `NullReferenceException` | a `FirstOrDefault` null-t adott | `if (x != null)` |
| `Cannot apply indexing with [] to IEnumerable` | `Where` eredményét indexeled | `.ToList()` |
| a rendezés nem jó | két `OrderBy` egymás után | második helyett `ThenBy` |
| a `Max` számot ad, nem objektumot | így működik | `MaxBy` vagy `OrderByDescending(...).First()` |
| 0% / rossz átlag | egész osztás | `(double)a / b` |
| a `Contains` nem talál | kis/nagybetű | `.ToLower()` mindkét oldalra |
| `FormatException` a lambdában | "NA" vagy tizedespont a `Parse`-ban | előbb szűrd ki a "NA"-t; `.Replace(".", ",")` |
