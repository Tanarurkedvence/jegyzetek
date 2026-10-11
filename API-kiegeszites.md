# Web API – kiegészítés (saját jegyzet)

> A fő API-leírás a tanári tutorial README-je: [Tanarurkedvence/ASP26032502](https://github.com/Tanarurkedvence/ASP26032502#readme) (1:1 fork az eredetiből: [JuhaszZoltan/ASP26032502](https://github.com/JuhaszZoltan/ASP26032502)). Ez a fájl csak kiegészítés: magyarázatok és **egyedi végpontok** a tutorial modelljeivel.

[← vissza](README.md) · **Minta, amit követünk:** [JuhaszZoltan/ASP26032502](https://github.com/JuhaszZoltan/ASP26032502) – állatkert (zoo) adatbázis, `Animal` / `Department` / `Zookeeper` modellek, scaffoldolt `AnimalsController`.
A tutorial teljes leírása a repó README-jében van (a kereső is abból keres: „ASP tutorial ›” találatok).

## Tartalom
1. [Lépések sorban](#1-lépések-sorban)
2. [Adatbázis: .sql futtatása LocalDB-n](#2-adatbázis)
3. [NuGet csomagok](#3-nuget-csomagok)
4. [appsettings.json – DefaultConnection](#4-appsettingsjson)
5. [Scaffold-DbContext → Models + Data/ApplicationDbContext](#5-scaffold-dbcontext)
6. [JSON-javítás: [JsonIgnore] + [ValidateNever]](#6-jsonignore--validatenever)
7. [API controller scaffoldolása (Controllers/Api)](#7-api-controller-scaffoldolása)
8. [Mit generál a scaffold? (az 5 végpont)](#8-mit-generál-a-scaffold)
9. [{id} → {id:int} javítás](#9-id--idint)
10. [Program.cs és SwaggerUI](#10-programcs-és-swaggerui)
11. [launchSettings.json – induljon a Swagger](#11-launchsettingsjson)
12. [Egyedi végpontok (ha a feladat többet kér)](#12-egyedi-végpontok)
13. [Tesztelés](#13-tesztelés)
14. [Tipikus hibák](#14-tipikus-hibák)

---

## 1. Lépések sorban

- [ ] Új projekt: **ASP.NET Core Web API** (.NET 10), *Use controllers* bepipálva, *Enable OpenAPI support* bepipálva
- [ ] a `.sql` fájl a `Data` mappába → lefuttatni a `(localdb)\MSSQLLocalDB` szerveren
- [ ] 5 NuGet csomag (Package Manager Console)
- [ ] `appsettings.json`: `DefaultConnection`
- [ ] `Scaffold-DbContext` → `Models/*.cs` + `Data/ApplicationDbContext.cs`
- [ ] a modellekben a külső kulcsokra és navigációs tulajdonságokra `[JsonIgnore]` + `[ValidateNever]`
- [ ] `Controllers/Api` mappa → *Add → New Scaffolded Item → API Controller with actions, using Entity Framework*
- [ ] `{id}` → `{id:int}` a generált controllerben
- [ ] `Program.cs`: SwaggerUI
- [ ] `launchSettings.json`: `launchBrowser: true`, `launchUrl: "swagger"`
- [ ] F5 → Swaggerben kipróbálni

---

## 2. Adatbázis

1. Tedd a kapott `.sql` fájlt a projekt **`Data`** mappájába (a tutorialban: `Data/zoo_db.sql`).
2. Futtasd a **`(localdb)\MSSQLLocalDB`** szerveren:
   - **Visual Studio:** View → *SQL Server Object Explorer* → `(localdb)\MSSQLLocalDB` → jobb klikk → *New Query* → másold be → ▶ (Ctrl+Shift+E)
   - vagy nyisd meg a `.sql`-t VS-ben → jobb felül *Connect* → válaszd a LocalDB-t → ▶
   - **SSMS:** Server name: `(localdb)\MSSQLLocalDB` → File → Open → Execute (F5)
3. Ellenőrzés: SQL Server Object Explorer → *Databases* alatt megjelent (pl. `zoo`).

---

## 3. NuGet csomagok

**Tools → NuGet Package Manager → Package Manager Console**:
```shell
Install-Package Microsoft.EntityFrameworkCore
Install-Package Microsoft.EntityFrameworkCore.Design
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Swashbuckle.AspNetCore.SwaggerUI
```

| Csomag | Mire kell |
|---|---|
| `EntityFrameworkCore` | az ORM maga (adatbázis ↔ C# osztályok) |
| `EntityFrameworkCore.Design` | a scaffoldoláshoz kell |
| `EntityFrameworkCore.SqlServer` | SQL Server-hez kapcsolódás (`UseSqlServer`) |
| `EntityFrameworkCore.Tools` | a `Scaffold-DbContext` parancs a PMC-ben |
| `Swashbuckle.AspNetCore.SwaggerUI` | a Swagger tesztfelület |

A controller-scaffoldolás a `Microsoft.VisualStudio.Web.CodeGeneration.Design` csomagot is felteszi magától (első alkalommal).

⚠ A csomagok főverziója egyezzen a projekt .NET verziójával (.NET 10 → 10.x.x).

---

## 4. appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "server=(localdb)\\mssqllocaldb;database=zoo;"
  },
  "Logging": { "LogLevel": { "Default": "Information", "Microsoft.AspNetCore": "Warning" } },
  "AllowedHosts": "*"
}
```
- A `database=` után **az adatbázis neve** jön (amit a `.sql` létrehozott).
- JSON-ben a `\` duplán kell: `\\`.
- Ha a feladat más nevet kér a kapcsolati sztringnek (pl. `BabszemDB`), akkor **mindenhol** azt a nevet használd (appsettings, Scaffold parancs `Name=ConnectionStrings:BabszemDB`, Program.cs `GetConnectionString("BabszemDB")`).

---

## 5. Scaffold-DbContext

Package Manager Console:
```shell
Scaffold-DbContext "Name=ConnectionStrings:DefaultConnection" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -DataAnnotations -Context ApplicationDbContext -ContextDir Data
```

| Kapcsoló | Jelentés |
|---|---|
| `"Name=ConnectionStrings:DefaultConnection"` | a kapcsolati sztringet az appsettings.json-ból veszi |
| `-OutputDir Models` | az entitásosztályok a `Models` mappába |
| `-DataAnnotations` | attribútumokkal (`[Key]`, `[Column("...")]`, `[StringLength]`) írja a modelleket |
| `-Context ApplicationDbContext` | a context osztály neve (ha a feladat mást kér, pl. `BabszemDbContext`, ide azt írd) |
| `-ContextDir Data` | a context a `Data` mappába |

Az eredmény (tutorial):
```
Models/Animal.cs, Models/Department.cs, Models/Zookeeper.cs
Data/ApplicationDbContext.cs
```

Egy generált modell, pl. `Animal`:
```csharp
public partial class Animal
{
    [Key]
    [Column("animal_id")]            // az adatbázisban így hívják az oszlopot
    public int AnimalId { get; set; } // C#-ban így

    [Column("species")]
    [StringLength(50)]
    public string Species { get; set; } = null!;   // kötelező (NOT NULL)

    [Column("name")]
    public string? Name { get; set; }              // ? = lehet NULL

    [Column("department_id")]
    public int? DepartmentId { get; set; }         // KÜLSŐ KULCS

    [ForeignKey("DepartmentId")]
    [InverseProperty("Animals")]
    public virtual Department? Department { get; set; }   // NAVIGÁCIÓS tulajdonság
}
```

---

## 6. [JsonIgnore] + [ValidateNever]

**Probléma:** az `Animal` hivatkozik a `Department`-re, a `Department` pedig az `Animals` listájára… JSON-ba íráskor ez **végtelen kör** lenne / felesleges adat, POST-nál pedig a validáció kötelezőnek venné a navigációs tulajdonságot.

**Megoldás** (a tutorial szerint): a modellben **minden külső kulcsra** és **minden `[InverseProperty(...)]`-os navigációs tulajdonságra** rakd rá ezt a kettőt:
```csharp
[JsonIgnore]
[ValidateNever]
```
A fájl elejére:
```csharp
using System.Text.Json.Serialization;                    // NEM Newtonsoft.Json!
using Microsoft.AspNetCore.Mvc.ModelBinding.Validation;
```

Példa (`Models/Animal.cs`):
```csharp
[Column("department_id")]
[JsonIgnore]
[ValidateNever]
public int? DepartmentId { get; set; }

[ForeignKey("DepartmentId")]
[InverseProperty("Animals")]
[JsonIgnore]
[ValidateNever]
public virtual Department? Department { get; set; }
```
- `[JsonIgnore]` → ez a mező nem kerül bele a JSON válaszba (és a kérésből sem olvassa be).
- `[ValidateNever]` → POST/PUT-nál nem ellenőrzi (nem kell elküldeni).

⚠ A `[JsonIgnore]` a **`System.Text.Json.Serialization`** névtérből jöjjön. Ha a VS a `Newtonsoft.Json`-t ajánlja fel, az rossz – akkor nem fog működni.

---

## 7. API controller scaffoldolása

1. Hozz létre egy **`Controllers/Api`** mappát (jobb klikk a `Controllers`-en → *Add → New Folder*).
2. Jobb klikk az `Api` mappán → **Add → New Scaffolded Item…**
3. Bal oldalt *Common → API* → **API Controller with actions, using Entity Framework** → *Add*
4. Kitöltés:
   - **Model class:** a modell, amihez a végpontok kellenek (pl. `Animal`)
   - **DbContext class:** `ApplicationDbContext`
   - **Controller name:** többes szám + Controller → `AnimalsController` (ezt ajánlja fel magától)
5. *Add* → elkészül a `Controllers/Api/AnimalsController.cs`

Minden modellhez, amihez végpont kell, ismételd meg (pl. `DepartmentsController`, `ZookeepersController`).

---

## 8. Mit generál a scaffold?

```csharp
[Route("api/[controller]")]        // → /api/Animals  (az osztálynév "Controller" nélkül)
[ApiController]
public class AnimalsController : ControllerBase
{
    private readonly ApplicationDbContext _context;
    public AnimalsController(ApplicationDbContext context) { _context = context; }   // DI: megkapja az adatbázist
```

| Metódus | URL | Mit csinál | Válasz |
|---|---|---|---|
| `GetAnimals()` | `GET /api/Animals` | az összes elem | 200 + lista |
| `GetAnimal(int id)` | `GET /api/Animals/5` | egy elem id alapján | 200, vagy **404** ha nincs |
| `PutAnimal(int id, Animal animal)` | `PUT /api/Animals/5` | teljes módosítás | **204**; 400 ha az URL-beli id ≠ a törzsbeli id; 404 ha nincs |
| `PostAnimal(Animal animal)` | `POST /api/Animals` | új elem | **201** + az új elem + Location fejléc |
| `DeleteAnimal(int id)` | `DELETE /api/Animals/5` | törlés | **204**, vagy 404 |

Részletesen:
```csharp
// GET: api/Animals
[HttpGet]
public async Task<ActionResult<IEnumerable<Animal>>> GetAnimals()
{
    return await _context.Animals.ToListAsync();        // SELECT * FROM Animals
}

// GET: api/Animals/5
[HttpGet("{id:int}")]
public async Task<ActionResult<Animal>> GetAnimal(int id)
{
    var animal = await _context.Animals.FindAsync(id);  // keresés elsődleges kulcs alapján
    if (animal == null) return NotFound();              // 404
    return animal;                                      // 200
}

// PUT: api/Animals/5
[HttpPut("{id:int}")]
public async Task<IActionResult> PutAnimal(int id, Animal animal)
{
    if (id != animal.AnimalId) return BadRequest();     // 400
    _context.Entry(animal).State = EntityState.Modified; // "ez módosult"
    try { await _context.SaveChangesAsync(); }          // UPDATE
    catch (DbUpdateConcurrencyException)
    {
        if (!AnimalExists(id)) return NotFound();       // 404
        else throw;
    }
    return NoContent();                                 // 204
}

// POST: api/Animals
[HttpPost]
public async Task<ActionResult<Animal>> PostAnimal(Animal animal)
{
    _context.Animals.Add(animal);                       // INSERT
    await _context.SaveChangesAsync();
    return CreatedAtAction("GetAnimal", new { id = animal.AnimalId }, animal);   // 201
}

// DELETE: api/Animals/5
[HttpDelete("{id:int}")]
public async Task<IActionResult> DeleteAnimal(int id)
{
    var animal = await _context.Animals.FindAsync(id);
    if (animal == null) return NotFound();              // 404
    _context.Animals.Remove(animal);                    // DELETE
    await _context.SaveChangesAsync();
    return NoContent();                                 // 204
}
```

- `async` / `await` / `...Async()` → az adatbázis-művelet alatt nem blokkol. Mindig együtt használd őket.
- `SaveChangesAsync()` nélkül **semmi nem mentődik** az adatbázisba!

---

## 9. {id} → {id:int}

A scaffold `{id}`-t ír, típus nélkül – ez a Swaggeres tesztelésnél gondot okozhat. Kézzel javítsd:

| Eredeti | Javított |
|---|---|
| `[HttpGet("{id}")]` | `[HttpGet("{id:int}")]` |
| `[HttpPut("{id}")]` | `[HttpPut("{id:int}")]` |
| `[HttpDelete("{id}")]` | `[HttpDelete("{id:int}")]` |

Gyorsan: Ctrl+H (csere) → `"{id}"` → `"{id:int}"` → *Replace All* (csak az aktuális fájlban).

---

## 10. Program.cs és SwaggerUI

A controller-scaffold **magától hozzáadja** a `Program.cs`-hez az adatbázis regisztrációját (ellenőrizd, hogy ott van-e):
```csharp
using ASP26032502.Data;                 // a te projekted névtere + .Data
using Microsoft.EntityFrameworkCore;

var connectionString = builder.Configuration.GetConnectionString("DefaultConnection")
    ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found");
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(connectionString));
```

A **SwaggerUI-t kézzel** kell beírni (a VS 2026 alapból már nem teszi bele). A `// Configure the HTTP request pipeline.` résznél:
```csharp
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
    app.UseSwaggerUI(options =>
    {
        options.SwaggerEndpoint("/openapi/v1.json", "API v1");
    });
}
```

A teljes `Program.cs` a tutorialban:
```csharp
using ASP26032502.Data;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

var connectionString = builder.Configuration.GetConnectionString("DefaultConnection")
    ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found");
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(connectionString));

builder.Services.AddControllers();
builder.Services.AddOpenApi();

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
    app.UseSwaggerUI(options =>
    {
        options.SwaggerEndpoint("/openapi/v1.json", "API v1");
    });
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
app.Run();
```

Swagger címe: `https://localhost:####/swagger` (a port a `launchSettings.json`-ban).

---

## 11. launchSettings.json

`Properties/launchSettings.json` → a **`"https"`** profilnál:
```json
"https": {
  "commandName": "Project",
  "dotnetRunMessages": true,
  "launchBrowser": true,
  "launchUrl": "swagger",
  "applicationUrl": "https://localhost:7xxx;http://localhost:5xxx",
  "environmentVariables": {
    "ASPNETCORE_ENVIRONMENT": "Development"
  }
}
```
- `"launchBrowser": true` → F5-re megnyílik a böngésző
- `"launchUrl": "swagger"` → rögtön a Swagger oldalon
- a http és https port legyen különböző

---

## 12. Egyedi végpontok

A scaffold csak az alap 5 műveletet adja. Ha a feladat **mást** kér (szűrés, összesítés, saját hibaüzenet), azt kézzel írod a controllerbe. A minták `ApplicationDbContext`-tel; egy teljes, tesztelt példa ilyen feladatra: [BabszemAPI](https://github.com/Tanarurkedvence/BabszemAPI).

### Szűrés query paraméterrel – `GET /api/Animals/search?species=oroszlán`
```csharp
[HttpGet("search")]
public async Task<IActionResult> Search(string? species, int? minAge)
{
    var query = _context.Animals.AsQueryable();
    if (!string.IsNullOrWhiteSpace(species))
        query = query.Where(a => a.Species.ToLower().Contains(species.ToLower()));
    if (minAge != null)
        query = query.Where(a => a.Age >= minAge);
    return Ok(await query.OrderBy(a => a.Name).ToListAsync());
}
```

### Kapcsolt tábla adataival (névtelen objektum – pontos JSON mezőnevek)
```csharp
[HttpGet("details")]
public async Task<IActionResult> Details()
{
    return Ok(await _context.Animals
        .Select(a => new
        {
            animalId = a.AnimalId,
            name = a.Name,
            species = a.Species,
            departmentName = a.Department != null ? a.Department.Name : null,
            zookeeperName = a.Zookeeper != null ? a.Zookeeper.Name : null
        })
        .ToListAsync());
}
```

### 404 saját üzenettel
```csharp
if (animal == null)
    return NotFound(new { message = "Az állat nem található." });   // {"message": "..."}
```

> Kipróbálva a tutorial `zoo` adatbázisán: pl. `stats?top=3` → Zebra 15, Zsiráf 10, Pingvin 10; az 1-es részleg (Afrikai Szavanna) 35 állat.

### Összesítés – `GET /api/Departments/5/summary`
```csharp
[HttpGet("{id:int}/summary")]
public async Task<IActionResult> Summary(int id)
{
    var d = await _context.Departments
        .Where(d => d.DepartmentId == id)
        .Select(d => new
        {
            name = d.Name,
            animalCount = d.Animals.Count(),
            averageAge = d.Animals.Any() ? d.Animals.Average(a => a.Age) : 0
        })
        .FirstOrDefaultAsync();
    if (d == null) return NotFound(new { message = "A részleg nem található." });
    return Ok(d);
}
```

### Csoportosítás / toplista – `GET /api/Animals/stats?top=3`
```csharp
[HttpGet("stats")]
public async Task<IActionResult> Stats(int top = 5)
{
    if (top < 1) return BadRequest(new { message = "A top értéke legalább 1 legyen." });
    return Ok(await _context.Animals
        .GroupBy(a => a.Species)
        .Select(g => new { species = g.Key, count = g.Count() })
        .OrderByDescending(x => x.count)
        .Take(top)
        .ToListAsync());
}
```

### POST saját ellenőrzéssel (az első hiba)
```csharp
if (string.IsNullOrWhiteSpace(animal.Species))
    return BadRequest(new { message = "A faj megadása kötelező." });
if (animal.Age < 0)
    return BadRequest(new { message = "Az életkor nem lehet negatív." });
```
Ha a hiányzó mezőre is a saját üzenetedet akarod (ne az automatikus 400-at), használj **DTO-t nullable mezőkkel** (`int?`, `string?`) – lásd BabszemAPI `ProductCreateDto`.

### Törlés 409-cel (ha hivatkoznak rá)
```csharp
if (await _context.Animals.AnyAsync(a => a.DepartmentId == id))
    return Conflict(new { message = "A részleg nem törölhető, mert vannak benne állatok." });
```

### Válaszok
| Kód | Metódus |
|---|---|
| 200 | `Ok(adat)` |
| 201 | `CreatedAtAction(...)` |
| 204 | `NoContent()` |
| 400 | `BadRequest(new { message = "..." })` |
| 404 | `NotFound(new { message = "..." })` |
| 409 | `Conflict(new { message = "..." })` |

---

## 13. Tesztelés

- **Swagger**: F5 → `/swagger` → végpont lenyitása → *Try it out* → *Execute*.
- **.http fájl** a projektben (VS-ben *Send request*):
  ```
  @host = https://localhost:7xxx

  GET {{host}}/api/Animals

  ###
  POST {{host}}/api/Animals
  Content-Type: application/json

  { "name": "Mufasa", "species": "Oroszlán", "age": 10 }
  ```
- **Postman**: metódus + URL, POST-nál *Body → raw → JSON*.
- A tutorial szerint alternatíva még: Scalar, (dokumentációhoz) Redoc.

---

## 14. Tipikus hibák

| Hiba | Ok | Megoldás |
|---|---|---|
| `A network-related or instance-specific error` | rossz szervernév | `(localdb)\\mssqllocaldb`; nézd meg az SQL Server Object Explorerben |
| `Cannot open database "zoo"` | nem futott le a `.sql` | futtasd le a LocalDB-n |
| `Connection string 'DefaultConnection' not found` | elírt név az appsettings-ben | a név egyezzen a `GetConnectionString("...")`-gel |
| `A possible object cycle was detected` | navigációs tulajdonság a JSON-ban | `[JsonIgnore]` + `[ValidateNever]` (6. pont) |
| POST-nál 400: „The Department field is required” | nincs `[ValidateNever]` | 6. pont |
| a `[JsonIgnore]` nem hat | `Newtonsoft.Json` névtérből jött | `using System.Text.Json.Serialization;` |
| Swagger 404 / nem nyílik meg | nincs `UseSwaggerUI` vagy rossz URL | 10–11. pont; cím: `/swagger` |
| a Swagger nem tudja tesztelni az id-s végpontot | `{id}` típus nélkül | `{id:int}` (9. pont) |
| a scaffold menüpont hiányzik / hibát dob | hiányzó csomag, vagy nem fordul a projekt | előbb Build (Ctrl+Shift+B), csomagok verziója egyezzen |
| `Unable to resolve service for type 'ApplicationDbContext'` | nincs `AddDbContext` a Program.cs-ben | 10. pont |
| semmi nem mentődik | hiányzik a `SaveChangesAsync()` | add hozzá |
