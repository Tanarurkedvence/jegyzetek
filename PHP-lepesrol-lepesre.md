# PHP feladat lépésről lépésre – teljesen kezdőknek

[← vissza](README.md) · Kész, kipróbált megoldás: [babszem_webshop](https://github.com/Tanarurkedvence/babszem_webshop) · Részletes PHP összefoglaló: [PHP.md](PHP.md)

> **Mit kell csinálni egy mondatban?**
> Kapsz egy **kész weboldalt** (frontend) és egy **adatbázist** (.sql). Neked a kettő közé kell megírnod **3 PHP fájlt** (backend), ami az adatbázisból kiszedi az adatokat, és odaadja a weboldalnak.

```
 adatbázis (MySQL)  ◀──▶  TE ÍROD: backend/*.php  ◀──▶  kész weboldal (frontend)
```

**Ezt a 3 fájlt kell megírnod:**
| Fájl | Mit csinál | Pont |
|---|---|---|
| `config.php` | kapcsolódik az adatbázishoz | 1 |
| `kategoriak.php` | visszaadja a kategóriákat | 1 |
| `termekek.php` | termékek: listázás, egy termék, új, készlet módosítás, törlés | 13 |

A példa a **Babszem webshop** feladat (kávék). Ha nálad más a téma (pl. könyvek, autók), a [13. pontban](#13-ha-más-a-feladat-mit-cserélj) leírom, mit kell átírni.

## Tartalom
0. [Mielőtt bármit csinálsz – 3 aranyszabály](#0-aranyszabályok)
1. [XAMPP elindítása](#1-xampp-elindítása)
2. [Adatbázis betöltése (phpMyAdmin)](#2-adatbázis-betöltése)
3. [A mappa a helyére (htdocs)](#3-a-mappa-a-helyére)
4. [config.php – 1 pont](#4-configphp--1-pont)
5. [kategoriak.php – 1 pont](#5-kategoriakphp--1-pont)
6. [termekek.php 1. rész: alap + összes termék – 4 pont](#6-termekekphp-1-rész-összes-termék--4-pont)
7. [termekek.php 2. rész: egy termék (?id=) – 2 pont](#7-egy-termék--2-pont)
8. [termekek.php 3. rész: új termék (POST) – 3 pont](#8-új-termék-post--3-pont)
9. [termekek.php 4. rész: készlet módosítás (PATCH) – 2 pont](#9-készlet-módosítás-patch--2-pont)
10. [termekek.php 5. rész: törlés (DELETE) – 1 pont](#10-törlés-delete--1-pont)
11. [termekek.php 6. rész: minden más metódus (405) – 1 pont](#11-minden-más-405--1-pont)
12. [A KÉSZ termekek.php egyben](#12-a-kész-termekekphp-egyben)
13. [Ha más a feladat: mit cserélj?](#13-ha-más-a-feladat-mit-cserélj)
14. [Végső ellenőrzés leadás előtt (pontlista)](#14-végső-ellenőrzés)
15. [Ha valami nem megy](#15-ha-valami-nem-megy)

---

## 0. Aranyszabályok

1. **Mindig `http://localhost/...` címen nyisd meg** a dolgokat a böngészőben. **Soha ne** dupla kattintással! (Akkor a PHP nem fut le.)
2. **Minden fájl `<?php`-vel kezdődik**, és minden utasítás végén **`;`** van. A legtöbb hiba egy hiányzó `;`.
3. **Lépésenként haladj, és minden lépés után teszteld!** Ha a 4. lépés működik, csak utána menj az 5.-re. Így ha elromlik valami, tudod, hol.

A hibaüzenetek, szövegek **betűre** egyezzenek a feladatlappal (ékezet, pont a végén, kis-nagybetű)! Ezeken múlnak a pontok. Másold ki őket a feladatlapból, ne gépeld.

---

## 1. XAMPP elindítása

1. Start menü → **XAMPP Control Panel**.
2. Az **Apache** sorában: **Start** → zöld lesz.
3. A **MySQL** sorában: **Start** → zöld lesz.

✅ **Teszt:** böngészőben nyisd meg: http://localhost → megjelenik a XAMPP oldala.

❌ Ha az Apache nem indul (piros, „port 80 in use”): valami más foglalja a 80-as portot (pl. Skype). Zárd be, vagy szólj a felügyelőnek.

---

## 2. Adatbázis betöltése

1. Böngésző: **http://localhost/phpmyadmin**
2. Felül a menüben: **Importálás** (Import).
3. **Fájl kiválasztása** → válaszd ki a kapott `.sql` fájlt (pl. `babszem_webshop.sql`).
4. Lent: **Importálás** / **Indítás** gomb.
5. Zöld üzenet = siker.

✅ **Teszt:** bal oldalt megjelenik az adatbázis (pl. `babszem_webshop`). Kattints rá → látod a táblákat (`categories`, `products`, `product_categories`). Kattints a `products`-ra → látod a termékeket.

📝 **Írd fel magadnak** (ezek kellenek majd):
- az **adatbázis nevét**: `babszem_webshop`
- a **táblák nevét** és az **oszlopok nevét** (a feladatlapon is benne van)

---

## 3. A mappa a helyére

1. A kapott `babszem_webshop` mappát (amiben a `frontend` és a `backend` van) **másold** ide:
   **`C:\xampp\htdocs\`**
   → így lesz: `C:\xampp\htdocs\babszem_webshop\frontend\...` és `...\backend\`
2. A `backend` mappa **üres** – ide jön a 3 fájlod.
3. Nyisd meg a mappát **VS Code**-ban: *File → Open Folder* → `C:\xampp\htdocs\babszem_webshop`.

✅ **Teszt:** http://localhost/babszem_webshop/frontend/index.html → megjelenik az oldal, de azt írja: **„A termékeket most nem sikerült betölteni.”** → **Ez most normális!** (Még nincs backend.)

**Új fájl létrehozása VS Code-ban:** bal oldalt a `backend` mappán jobb klikk → *New File* → beírod a nevet (pl. `config.php`) → Enter.
**Mentés:** Ctrl+S (minden módosítás után!).

---

## 4. config.php – 1 pont

**Mi ez?** Ez a fájl kapcsolódik az adatbázishoz. A többi fájl ezt „behúzza”, hogy ne kelljen mindegyikbe beírni.

Hozd létre: `backend/config.php`, és írd bele **pontosan** ezt:

```php
<?php
// Kapcsolódás az adatbázishoz (XAMPP: felhasználó root, jelszó nincs)
$pdo = new PDO(
    "mysql:host=localhost;dbname=babszem_webshop;charset=utf8mb4",
    "root",
    "",
    [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC
    ]
);
```

**Mit jelent?**
| Rész | Jelentés |
|---|---|
| `$pdo` | ebben a változóban lesz a kapcsolat (PHP-ban minden változó `$`-ral kezdődik) |
| `dbname=babszem_webshop` | **az adatbázis neve** – ha nálad más, ezt írd át! |
| `charset=utf8mb4` | ékezetek miatt kell – **a feladat kéri, pont jár érte** |
| `"root"`, `""` | felhasználó és (üres) jelszó |
| `PDO::ERRMODE_EXCEPTION` | hiba esetén kivétel – **a feladat kéri, pont jár érte** |
| `PDO::FETCH_ASSOC` | a sorokat név szerint kapjuk (`$sor["name"]`) – kényelmi |

✅ **Teszt:** http://localhost/babszem_webshop/backend/config.php
- **Üres fehér oldal = JÓ!** (Nem ír ki semmit, csak kapcsolódik.)
- Ha hibaszöveg jön (pl. `Unknown database`): rossz az adatbázis neve, vagy nincs importálva.

---

## 5. kategoriak.php – 1 pont

**Mi ez?** Ha a weboldal elkéri, visszaadja az összes kategóriát **név szerint ABC-ben**.

Hozd létre: `backend/kategoriak.php`:

```php
<?php
require_once "config.php";

header("Content-Type: application/json; charset=utf-8");

// csak GET kérésre válaszolunk, minden másra 405
if ($_SERVER["REQUEST_METHOD"] !== "GET") {
    http_response_code(405);
    echo json_encode(["error" => "Method not allowed"]);
    exit;
}

$stmt = $pdo->query("SELECT id, name FROM categories ORDER BY name");
$kategoriak = [];
foreach ($stmt->fetchAll() as $sor) {
    $kategoriak[] = [
        "id" => (int)$sor["id"],
        "name" => $sor["name"]
    ];
}

echo json_encode($kategoriak, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
```

**Mit jelent soronként?**
1. `require_once "config.php";` → „töltsd be a config.php-t” → innentől van `$pdo`.
2. `header(...)` → megmondjuk a böngészőnek, hogy JSON-t küldünk.
3. `$_SERVER["REQUEST_METHOD"]` → milyen kérés jött (GET, POST…). Ha **nem** GET → `405` kód + hibaüzenet → `exit` (vége).
4. `SELECT id, name FROM categories ORDER BY name` → SQL: kérd le a kategóriákat név szerint rendezve.
5. `foreach` → végigmegy a sorokon, és mindegyikből csinál egy `{"id": .., "name": ..}` párt.
   `(int)` → az id **számként** menjen ki, ne szövegként (`6`, nem `"6"`).
6. `echo json_encode(...)` → kiírja JSON-ként. A `JSON_UNESCAPED_UNICODE` miatt az ékezetek olvashatóak maradnak.

✅ **Teszt:** http://localhost/babszem_webshop/backend/kategoriak.php → ezt kell látnod:
```json
[
    { "id": 6, "name": "Ajándékötlet" },
    { "id": 5, "name": "Bio" },
    ...
    { "id": 1, "name": "Szemes kávé" }
]
```
✅ Frissítsd a weboldalt (F5) → a **kategóriagombok** már megjelennek! (A termékek még nem.)

---

## 6. termekek.php 1. rész: összes termék – 4 pont

Ez a legnagyobb fájl. **Darabonként** rakjuk össze. Minden darab után mentés + teszt.

### 6.1 Az eleje (minden kérésnél lefut)

Hozd létre: `backend/termekek.php`, és írd bele:

```php
<?php
require_once "config.php";

// MINDEN válasz JSON (a feladat kéri – 1 pont része)
header("Content-Type: application/json; charset=utf-8");

// Segédfüggvény: kiír egy választ a megadott kóddal, és befejezi a programot
function valasz($kod, $adat)
{
    http_response_code($kod);
    echo json_encode($adat, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
    exit;
}

// Segédfüggvény: a kérés törzséből (body) érkező JSON beolvasása tömbbe
function torzs()
{
    $adat = json_decode(file_get_contents("php://input"), true);
    return is_array($adat) ? $adat : [];
}

$metodus = $_SERVER["REQUEST_METHOD"];                                   // GET, POST, PATCH, DELETE...
$id = isset($_GET["id"]) && $_GET["id"] !== "" ? (int)$_GET["id"] : null; // ?id=5 → 5, ha nincs → null
```

**Mit jelent?**
- `valasz(200, $adat)` → így fogunk mindig válaszolni. Egy sor, és kész (kód + JSON + vége). Ezt **sokszor** használjuk, ezért érdemes függvény.
- `torzs()` → amikor a weboldal új terméket küld (POST), az adatok a kérés „törzsében” jönnek JSON-ként. Ez kiolvassa.
- `$id` → ha a cím végén van `?id=5`, akkor `$id = 5`, különben `null` (nincs).

### 6.2 A termékek lekérdezése (függvény)

Írd az előző rész **alá**:

```php
// Termékek lekérdezése a kategóriáikkal együtt.
// Ha $egyId meg van adva, csak azt az egy terméket kéri le.
function termekekLekerdezese($pdo, $egyId = null)
{
    $sql = "SELECT p.id, p.name, p.description, p.price, p.stock,
                   c.id AS category_id, c.name AS category_name
            FROM products p
            LEFT JOIN product_categories pc ON pc.product_id = p.id
            LEFT JOIN categories c ON c.id = pc.category_id";
    if ($egyId !== null) {
        $sql .= " WHERE p.id = :id";
    }
    $sql .= " ORDER BY p.id, c.name";

    $stmt = $pdo->prepare($sql);
    if ($egyId !== null) {
        $stmt->bindValue(":id", $egyId, PDO::PARAM_INT);
    }
    $stmt->execute();

    // Egy termék több sorban is jöhet (ahány kategóriája van) → összerakjuk termékenként
    $termekek = [];
    foreach ($stmt->fetchAll() as $sor) {
        $tid = (int)$sor["id"];
        if (!isset($termekek[$tid])) {
            $termekek[$tid] = [
                "id" => $tid,
                "name" => $sor["name"],
                "description" => $sor["description"],
                "price" => (int)$sor["price"],
                "stock" => (int)$sor["stock"],
                "categories" => []
            ];
        }
        if ($sor["category_id"] !== null) {
            $termekek[$tid]["categories"][] = [
                "id" => (int)$sor["category_id"],
                "name" => $sor["category_name"]
            ];
        }
    }
    return array_values($termekek);
}
```

**Mit jelent? (Ez éri a legtöbb pontot!)**

Az adatbázisban 3 tábla van:
- `products` – a termékek
- `categories` – a kategóriák
- `product_categories` – **melyik termék melyik kategóriában van** (egy terméknek több is lehet!)

A `LEFT JOIN` a hármat összefésüli. Ha egy terméknek **2 kategóriája** van, **2 sort** kapunk róla:

| id | name | category_id | category_name |
|---|---|---|---|
| 2 | Etióp Yirgacheffe | 6 | Ajándékötlet |
| 2 | Etióp Yirgacheffe | 1 | Szemes kávé |

A `foreach` ezt **összerakja egy termékké**:
- Ha ezt a terméket még nem láttuk (`!isset`) → létrehozza, **üres** `categories` listával.
- Ha a sorban van kategória (`!== null`) → hozzáadja a listájához.
- Így lesz: `{"id": 2, ..., "categories": [{"id":6,...}, {"id":1,...}]}`

| A feladat ezt kéri | Ezért van ez a kód |
|---|---|
| „kapcsolja össze a három táblát (LEFT JOIN)” | a két `LEFT JOIN` sor |
| „a sorokat termékenként csoportosítsa” | a `foreach` + `isset` |
| „kategória nélküli termék üres tömböt kapjon” | `"categories" => []` + az `if (... !== null)` |
| „id, price, stock számként” | a `(int)` mindegyik előtt |
| „azonosító szerint rendezve” | `ORDER BY p.id` |
| „prepared statement” (a 7. lépésnél) | `prepare` + `:id` + `bindValue` |

- `array_values` → hogy a JSON-ben **lista** (`[...]`) legyen, ne objektum.

### 6.3 GET kérés kezelése (összes termék)

Írd az előzőek **alá**:

```php
// ===== GET =====
if ($metodus === "GET") {
    if ($id === null) {
        valasz(200, termekekLekerdezese($pdo));      // nincs ?id → az összes termék
    }
}
```

✅ **Teszt:** http://localhost/babszem_webshop/backend/termekek.php → egy hosszú lista, minden terméknél `categories` tömb. Nézd meg:
- a számok **idézőjel nélkül** vannak (`"price": 3490`, nem `"price": "3490"`) ✔
- az ékezetek rendben ✔

✅ Frissítsd a weboldalt (F5) → **megjelennek a termékkártyák!** 🎉 A kereső és a kategóriagombok is működnek.

---

## 7. Egy termék – 2 pont

A GET részt **bővítsd** így (a `valasz(200, termekekLekerdezese($pdo));` sor **után**, még a GET-es `if`-en **belül**):

```php
// ===== GET =====
if ($metodus === "GET") {
    if ($id === null) {
        valasz(200, termekekLekerdezese($pdo));      // nincs ?id → az összes termék
    }
    $termek = termekekLekerdezese($pdo, $id);       // van ?id → csak az az egy
    if (count($termek) === 0) {
        valasz(404, ["error" => "A termék nem található."]);
    }
    valasz(200, $termek[0]);                        // [0] → objektumként, NEM tömbben!
}
```

**Miért `[0]`?** A függvény mindig listát ad vissza. Egy terméknél ez egy 1 elemű lista → a `[0]` kiveszi belőle az egyetlen elemet. A feladat azt kéri: **„objektumként, nem tömbben”** – erre jár a pont.

✅ **Teszt:**
- http://localhost/babszem_webshop/backend/termekek.php?id=2 → egy objektum `{ "id": 2, "name": "Etióp Yirgacheffe 250 g", ... }` (nem `[ ]`-ben!)
- http://localhost/babszem_webshop/backend/termekek.php?id=99 → `{ "error": "A termék nem található." }`

---

## 8. Új termék (POST) – 3 pont

Írd a GET rész **alá**:

```php
// ===== POST: új termék =====
if ($metodus === "POST") {
    $adat = torzs();
    $name = isset($adat["name"]) ? trim($adat["name"]) : "";
    $description = isset($adat["description"]) && trim($adat["description"]) !== "" ? trim($adat["description"]) : null;
    $price = $adat["price"] ?? null;
    $stock = $adat["stock"] ?? null;
    $kategoriak = $adat["categories"] ?? [];

    // Ellenőrzések – a feladatlap sorrendjében, az ELSŐ hibánál megállunk
    if ($name === "") {
        valasz(400, ["error" => "A név megadása kötelező."]);
    }
    if (!is_int($price) || $price <= 0) {
        valasz(400, ["error" => "Az ár csak pozitív egész szám lehet."]);
    }
    if (!is_int($stock) || $stock < 0) {
        valasz(400, ["error" => "A készlet nem lehet negatív."]);
    }
    if (!is_array($kategoriak)) {
        valasz(400, ["error" => "Nem létező kategória."]);
    }
    $ellenorzes = $pdo->prepare("SELECT COUNT(*) FROM categories WHERE id = :id");
    foreach ($kategoriak as $kategoriaId) {
        if (!is_int($kategoriaId)) {
            valasz(400, ["error" => "Nem létező kategória."]);
        }
        $ellenorzes->execute([":id" => $kategoriaId]);
        if ($ellenorzes->fetchColumn() == 0) {
            valasz(400, ["error" => "Nem létező kategória."]);
        }
    }

    // Mentés TRANZAKCIÓBAN: vagy minden mentődik, vagy semmi
    try {
        $pdo->beginTransaction();

        $stmt = $pdo->prepare("INSERT INTO products (name, description, price, stock)
                               VALUES (:name, :description, :price, :stock)");
        $stmt->execute([
            ":name" => $name,
            ":description" => $description,
            ":price" => $price,
            ":stock" => $stock
        ]);
        $ujId = (int)$pdo->lastInsertId();          // az új termék azonosítója

        $kapcsolat = $pdo->prepare("INSERT INTO product_categories (product_id, category_id)
                                    VALUES (:product_id, :category_id)");
        foreach ($kategoriak as $kategoriaId) {
            $kapcsolat->execute([":product_id" => $ujId, ":category_id" => $kategoriaId]);
        }

        $pdo->commit();                             // véglegesítés
    } catch (PDOException $e) {
        $pdo->rollBack();                           // hiba esetén visszavonás
        valasz(500, ["error" => "A termék mentése sikertelen."]);
    }

    valasz(201, ["message" => "Termék sikeresen hozzáadva.", "id" => $ujId]);
}
```

**Mit jelent?**
1. `torzs()` → kiolvassa, amit a weboldal küldött (név, leírás, ár, készlet, kategóriák).
2. **Ellenőrzések** egymás után. Az első hibánál `valasz(400, ...)` → kiírja a hibát, és **megáll** (a `valasz` függvényben ott az `exit`).
   - `trim` → levágja a szóközöket (csak szóköz = üres név).
   - `is_int` → tényleg egész szám-e (a `12.5` nem az).
   - kategóriák: mindegyikre megnézi az adatbázisban, hogy létezik-e (`COUNT(*)` → 0 = nincs ilyen).
3. **Tranzakció** (`beginTransaction` … `commit`): két dolgot mentünk (a terméket ÉS a kategóriakapcsolatokat). Ha a második elszállna, a `rollBack` az elsőt is visszavonja → nem marad félkész adat. **A feladat kéri, pont jár érte.**
4. `lastInsertId()` → az adatbázis által adott új azonosító.
5. Végül `201` + üzenet + id.

✅ **Teszt a weboldalon:** az oldal alján lévő űrlapon:
- hagyd üresen a nevet → **Mentés** → alatta: „A név megadása kötelező.” ✔
- írj nevet, árnak `0` → „Az ár csak pozitív egész szám lehet.” ✔
- töltsd ki rendesen (pl. Kenyai AA 250 g, 4590, 20, pipáld be: Szemes kávé, Bio) → „Termék sikeresen hozzáadva.” és megjelenik a kártya ✔

---

## 9. Készlet módosítás (PATCH) – 2 pont

Írd a POST rész **alá**:

```php
// ===== PATCH: készlet módosítása =====
if ($metodus === "PATCH") {
    if ($id === null) {
        valasz(400, ["error" => "Hiányzó azonosító."]);
    }
    $stock = torzs()["stock"] ?? null;
    if (!is_int($stock) || $stock < 0) {
        valasz(400, ["error" => "A készlet nem lehet negatív."]);
    }

    $stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    if ($stmt->fetchColumn() == 0) {
        valasz(404, ["error" => "A termék nem található."]);
    }

    $stmt = $pdo->prepare("UPDATE products SET stock = :stock WHERE id = :id");
    $stmt->execute([":stock" => $stock, ":id" => $id]);

    valasz(200, ["message" => "Készlet frissítve.", "id" => $id, "stock" => $stock]);
}
```

**Mit jelent?** Sorban: van-e `?id=` (ha nem → 400) → jó-e a készlet (negatív vagy hiányzik → 400) → létezik-e a termék (ha nem → 404) → `UPDATE` → `200`.

✅ **Teszt a weboldalon:** egy kártya alján írj be új készletet → **Készlet mentése** → „Készlet frissítve.”, a kártyán is átíródik ✔. Írj be `-3`-at → „A készlet nem lehet negatív.” ✔

---

## 10. Törlés (DELETE) – 1 pont

Írd a PATCH rész **alá**:

```php
// ===== DELETE: termék törlése =====
if ($metodus === "DELETE") {
    if ($id === null) {
        valasz(400, ["error" => "Hiányzó azonosító."]);
    }
    $stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    if ($stmt->fetchColumn() == 0) {
        valasz(404, ["error" => "A termék nem található."]);
    }

    // ELŐBB a kapcsolótábla sorai, UTÁNA a termék (különben az adatbázis nem engedi)
    $stmt = $pdo->prepare("DELETE FROM product_categories WHERE product_id = :id");
    $stmt->execute([":id" => $id]);
    $stmt = $pdo->prepare("DELETE FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);

    valasz(200, ["message" => "Termék törölve."]);
}
```

**Miért előbb a kapcsolótábla?** A `product_categories` „hivatkozik” a termékre. Ha a terméket törölnéd előbb, az adatbázis hibát adna (idegen kulcs). **A feladat kifejezetten kéri ezt a sorrendet.**

✅ **Teszt a weboldalon:** egy kártyán **Törlés** → megerősítés → „Termék törölve.”, eltűnik a kártya ✔

---

## 11. Minden más (405) – 1 pont

A fájl **legaljára** (minden `if` után):

```php
// ===== bármilyen más metódus (pl. PUT) =====
valasz(405, ["error" => "Method not allowed"]);
```

**Miért működik?** Ha GET/POST/PATCH/DELETE jött, azok a részek már válaszoltak és kiléptek (`exit`). Ide **csak** akkor jut el a program, ha valami más jött (pl. PUT) → 405.

Ehhez a ponthoz tartozik még (már megcsináltuk):
- ✔ a fájl elején a `header("Content-Type: application/json; charset=utf-8");` → minden válasz JSON
- ✔ PATCH és DELETE: hiányzó `id` → 400 „Hiányzó azonosító.”

✅ **Teszt a böngésző konzolában** (F12 → *Console* fül → beírod → Enter):
```js
fetch("../backend/termekek.php", { method: "PUT" }).then(r => r.json().then(d => console.log(r.status, d)))
```
→ `405 {error: 'Method not allowed'}` ✔

```js
fetch("../backend/termekek.php", { method: "DELETE" }).then(r => r.json().then(d => console.log(r.status, d)))
```
→ `400 {error: 'Hiányzó azonosító.'}` ✔

(A konzolban a `frontend/index.html` oldalon állj, hogy a `../backend/` útvonal jó legyen.)

⚠ Ezek után a konzolban megjelenik két piros sor: *Failed to load resource: … 405* és *… 400*. **Ez NEM hiba** – a böngésző minden 4xx választ pirossal jelez, és mi pont ezt teszteltük.

---

## 12. A KÉSZ termekek.php egyben

Ha elakadtál, vagy egyben akarod bemásolni – ez a teljes fájl (a 6–11. lépések együtt):

```php
<?php
require_once "config.php";

header("Content-Type: application/json; charset=utf-8");

function valasz($kod, $adat)
{
    http_response_code($kod);
    echo json_encode($adat, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
    exit;
}

function torzs()
{
    $adat = json_decode(file_get_contents("php://input"), true);
    return is_array($adat) ? $adat : [];
}

function termekekLekerdezese($pdo, $egyId = null)
{
    $sql = "SELECT p.id, p.name, p.description, p.price, p.stock,
                   c.id AS category_id, c.name AS category_name
            FROM products p
            LEFT JOIN product_categories pc ON pc.product_id = p.id
            LEFT JOIN categories c ON c.id = pc.category_id";
    if ($egyId !== null) {
        $sql .= " WHERE p.id = :id";
    }
    $sql .= " ORDER BY p.id, c.name";

    $stmt = $pdo->prepare($sql);
    if ($egyId !== null) {
        $stmt->bindValue(":id", $egyId, PDO::PARAM_INT);
    }
    $stmt->execute();

    $termekek = [];
    foreach ($stmt->fetchAll() as $sor) {
        $tid = (int)$sor["id"];
        if (!isset($termekek[$tid])) {
            $termekek[$tid] = [
                "id" => $tid,
                "name" => $sor["name"],
                "description" => $sor["description"],
                "price" => (int)$sor["price"],
                "stock" => (int)$sor["stock"],
                "categories" => []
            ];
        }
        if ($sor["category_id"] !== null) {
            $termekek[$tid]["categories"][] = [
                "id" => (int)$sor["category_id"],
                "name" => $sor["category_name"]
            ];
        }
    }
    return array_values($termekek);
}

$metodus = $_SERVER["REQUEST_METHOD"];
$id = isset($_GET["id"]) && $_GET["id"] !== "" ? (int)$_GET["id"] : null;

// ===== GET =====
if ($metodus === "GET") {
    if ($id === null) {
        valasz(200, termekekLekerdezese($pdo));
    }
    $termek = termekekLekerdezese($pdo, $id);
    if (count($termek) === 0) {
        valasz(404, ["error" => "A termék nem található."]);
    }
    valasz(200, $termek[0]);
}

// ===== POST: új termék =====
if ($metodus === "POST") {
    $adat = torzs();
    $name = isset($adat["name"]) ? trim($adat["name"]) : "";
    $description = isset($adat["description"]) && trim($adat["description"]) !== "" ? trim($adat["description"]) : null;
    $price = $adat["price"] ?? null;
    $stock = $adat["stock"] ?? null;
    $kategoriak = $adat["categories"] ?? [];

    if ($name === "") {
        valasz(400, ["error" => "A név megadása kötelező."]);
    }
    if (!is_int($price) || $price <= 0) {
        valasz(400, ["error" => "Az ár csak pozitív egész szám lehet."]);
    }
    if (!is_int($stock) || $stock < 0) {
        valasz(400, ["error" => "A készlet nem lehet negatív."]);
    }
    if (!is_array($kategoriak)) {
        valasz(400, ["error" => "Nem létező kategória."]);
    }
    $ellenorzes = $pdo->prepare("SELECT COUNT(*) FROM categories WHERE id = :id");
    foreach ($kategoriak as $kategoriaId) {
        if (!is_int($kategoriaId)) {
            valasz(400, ["error" => "Nem létező kategória."]);
        }
        $ellenorzes->execute([":id" => $kategoriaId]);
        if ($ellenorzes->fetchColumn() == 0) {
            valasz(400, ["error" => "Nem létező kategória."]);
        }
    }

    try {
        $pdo->beginTransaction();

        $stmt = $pdo->prepare("INSERT INTO products (name, description, price, stock)
                               VALUES (:name, :description, :price, :stock)");
        $stmt->execute([
            ":name" => $name,
            ":description" => $description,
            ":price" => $price,
            ":stock" => $stock
        ]);
        $ujId = (int)$pdo->lastInsertId();

        $kapcsolat = $pdo->prepare("INSERT INTO product_categories (product_id, category_id)
                                    VALUES (:product_id, :category_id)");
        foreach ($kategoriak as $kategoriaId) {
            $kapcsolat->execute([":product_id" => $ujId, ":category_id" => $kategoriaId]);
        }

        $pdo->commit();
    } catch (PDOException $e) {
        $pdo->rollBack();
        valasz(500, ["error" => "A termék mentése sikertelen."]);
    }

    valasz(201, ["message" => "Termék sikeresen hozzáadva.", "id" => $ujId]);
}

// ===== PATCH: készlet módosítása =====
if ($metodus === "PATCH") {
    if ($id === null) {
        valasz(400, ["error" => "Hiányzó azonosító."]);
    }
    $stock = torzs()["stock"] ?? null;
    if (!is_int($stock) || $stock < 0) {
        valasz(400, ["error" => "A készlet nem lehet negatív."]);
    }

    $stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    if ($stmt->fetchColumn() == 0) {
        valasz(404, ["error" => "A termék nem található."]);
    }

    $stmt = $pdo->prepare("UPDATE products SET stock = :stock WHERE id = :id");
    $stmt->execute([":stock" => $stock, ":id" => $id]);

    valasz(200, ["message" => "Készlet frissítve.", "id" => $id, "stock" => $stock]);
}

// ===== DELETE: termék törlése =====
if ($metodus === "DELETE") {
    if ($id === null) {
        valasz(400, ["error" => "Hiányzó azonosító."]);
    }
    $stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    if ($stmt->fetchColumn() == 0) {
        valasz(404, ["error" => "A termék nem található."]);
    }

    $stmt = $pdo->prepare("DELETE FROM product_categories WHERE product_id = :id");
    $stmt->execute([":id" => $id]);
    $stmt = $pdo->prepare("DELETE FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);

    valasz(200, ["message" => "Termék törölve."]);
}

// ===== bármilyen más metódus =====
valasz(405, ["error" => "Method not allowed"]);
```

---

## 13. Ha más a feladat: mit cserélj?

A **szerkezet ugyanaz marad** (config + egy lista-fájl + egy fő fájl GET/POST/PATCH/DELETE ágakkal). Csak a **neveket** cseréld a feladatlap szerint. VS Code-ban: **Ctrl+H** (keresés és csere).

| Itt | Ezt cseréld | Mire (példa: könyvesbolt) |
|---|---|---|
| config.php | `dbname=babszem_webshop` | `dbname=konyvesbolt` |
| mindenhol | `products` (tábla) | `books` |
| mindenhol | `categories` (tábla) | `genres` |
| mindenhol | `product_categories` (kapcsolótábla) | `book_genres` |
| SQL + kapcsolótábla | `product_id`, `category_id` | `book_id`, `genre_id` |
| SELECT / INSERT / tömbök | `name, description, price, stock` (oszlopok) | `title, author, price, stock` |
| hibaüzenetek | „A termék nem található.” stb. | **a feladatlapról kimásolva** |
| fájlnevek | `kategoriak.php`, `termekek.php` | **amit a feladat ír** (a frontend ezeket hívja!) |

**Hogyan tudod meg, mit hív a frontend?** Nyisd meg a `frontend/js/script.js`-t, és keresd meg (Ctrl+F) a `.php` szót → ott a pontos fájlnév és útvonal. Ugyanitt látod, milyen mezőneveket vár (`product.name`, `product.price` …) – **ugyanilyen nevekkel** add vissza a JSON-t!

**Ha nincs kapcsolótábla** (egy terméknek csak 1 kategóriája van, pl. `products.category_id` oszlop): egyszerűbb, nem kell csoportosítani:
```php
$sorok = $pdo->query("SELECT p.id, p.name, p.price, c.name AS category_name
                      FROM products p LEFT JOIN categories c ON c.id = p.category_id
                      ORDER BY p.id")->fetchAll();
```

---

## 14. Végső ellenőrzés

Pipáld ki leadás előtt. Zárójelben a pont.

**config.php (1)**
- [ ] PDO, `charset=utf8mb4`, `PDO::ERRMODE_EXCEPTION`, a többi fájl `require_once`-szal tölti be

**kategoriak.php (1)**
- [ ] GET → minden kategória (`id`, `name`), név szerint ABC-ben
- [ ] más metódus → 405 `{"error": "Method not allowed"}`

**termekek.php – GET lista (4)**
- [ ] minden termék, azonosító szerint rendezve
- [ ] mindegyikben `categories` tömb (`id`, `name`), kategória nélkül `[]`
- [ ] LEFT JOIN + termékenkénti csoportosítás
- [ ] `id`, `price`, `stock` **számként** (nincs idézőjel)

**termekek.php?id=N (2)**
- [ ] egy termék **objektumként** (nem `[ ]`-ben)
- [ ] nincs ilyen → 404 `{"error": "A termék nem található."}`
- [ ] prepared statement (`prepare` + `:id`)

**POST (3)**
- [ ] 4 hibaüzenet, 400-zal, a feladat sorrendjében
- [ ] **tranzakció** (`beginTransaction` / `commit` / `rollBack`)
- [ ] siker → 201 `{"message": "Termék sikeresen hozzáadva.", "id": 17}`

**PATCH (2)**
- [ ] negatív/hiányzó készlet → 400; nincs termék → 404
- [ ] siker → 200 `{"message": "Készlet frissítve.", "id": 6, "stock": 15}`

**DELETE (1)**
- [ ] előbb a kapcsolótábla, aztán a termék; nincs → 404; siker → 200 `{"message": "Termék törölve."}`

**Általános (1)**
- [ ] minden válasz `Content-Type: application/json; charset=utf-8`
- [ ] PATCH/DELETE `id` nélkül → 400 „Hiányzó azonosító.”
- [ ] más metódus (PUT) → 405 „Method not allowed”

**És végül:**
- [ ] a weboldalon minden működik: kártyák, kereső, kategóriagombok, készlet mentése, törlés, új termék
- [ ] F12 → Console: oldalbetöltés után nincs piros hiba (a saját 405/400-as tesztjeid pirosa nem számít)
- [ ] ha teszteltél (töröltél/felvettél), **importáld újra a .sql-t**, hogy az eredeti adatok legyenek (és az új termék id-ja megint 17 legyen)

---

## 15. Ha valami nem megy

**Első lépés mindig:** nyisd meg a PHP fájlt **közvetlenül** a böngészőben (pl. `.../backend/termekek.php`) – ott látod a PHP hibaüzenetét.
A weboldalon: **F12 → Network** fül → F5 → kattints a `termekek.php` sorra → **Response**.

| Amit látsz | Ok | Megoldás |
|---|---|---|
| „A termékeket most nem sikerült betölteni.” | a PHP hibát ad vagy nem JSON-t | nyisd meg közvetlenül a `termekek.php`-t, olvasd el a hibát |
| `Parse error: syntax error, unexpected ...` `on line 25` | elírás: hiányzó `;`, `)`, `}` vagy `"` | nézd meg a **25. sort és az előtte lévőt** |
| `Unknown database 'babszem_webshop'` | nincs importálva / elírt név | 2. lépés; `dbname=` a config.php-ban |
| `Access denied for user 'root'` | jelszó | XAMPP-ban a jelszó `""` (üres) |
| `No connection could be made` / `[2002]` | nem fut a MySQL | XAMPP → MySQL Start |
| `Table '...' doesn't exist` / `Unknown column` | elírt tábla- vagy oszlopnév | phpMyAdmin-ban nézd meg a pontos nevet |
| `Call to undefined function valasz()` | a függvény nincs a fájlban / elírtad | a 6.1 rész legyen a fájl tetején |
| `Undefined variable $pdo` | nincs `require_once "config.php";` | tedd a fájl elejére |
| `Cannot add or update a child row: a foreign key constraint fails` | törlésnél a sorrend, vagy nem létező kategória | DELETE: előbb `product_categories`; POST: kategória-ellenőrzés |
| a számok `"3490"` (idézőjelben) | nincs `(int)` | `(int)$sor["price"]` |
| ékezetek helyett `\u00e1` | hiányzik a `JSON_UNESCAPED_UNICODE` | `json_encode($adat, JSON_UNESCAPED_UNICODE)` |
| ékezetek helyett `???` | hiányzik a `charset=utf8mb4` | config.php |
| `Not Found` a böngészőben | rossz cím / nem a htdocs-ban van | `C:\xampp\htdocs\babszem_webshop\backend\...` |
| egy termék `[ { ... } ]`-ben jön | hiányzik a `[0]` | `valasz(200, $termek[0]);` |
| a POST mindig „A név megadása kötelező.” | a törzs nem jön át | `file_get_contents("php://input")` + `json_decode(..., true)` |
