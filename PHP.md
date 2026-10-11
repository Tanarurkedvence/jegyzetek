# PHP összefoglaló (XAMPP + PDO + REST API)

[← vissza](README.md) · Működő teljes példa: [babszem_webshop](https://github.com/Tanarurkedvence/babszem_webshop)

## Tartalom
1. [XAMPP indítás, phpMyAdmin, mappák](#1-xampp)
2. [PHP alapok: változók, típusok, kiírás](#2-alapok)
3. [Stringek](#3-stringek)
4. [Tömbök és tömbfüggvények](#4-tömbök)
5. [Feltételek, ciklusok, függvények](#5-feltételek-ciklusok-függvények)
6. [Szuperglobálisok: $_GET, $_POST, $_SERVER](#6-szuperglobálisok)
7. [PDO kapcsolat – config.php](#7-config-php)
8. [Lekérdezések PDO-val (SELECT, INSERT, UPDATE, DELETE)](#8-pdo-lekérdezések)
9. [JSON API alapváz](#9-json-api-alapváz)
10. [Teljes REST végpont (GET lista/egy, POST, PATCH, PUT, DELETE)](#10-teljes-rest-végpont)
11. [JOIN + csoportosítás (kapcsolótábla, many-to-many)](#11-join-csoportosítás)
12. [Validálás, hibaüzenetek](#12-validálás)
13. [Tranzakció](#13-tranzakció)
14. [Klasszikus PHP oldal (HTML űrlap + táblázat)](#14-klasszikus-php-oldal)
15. [Session, bejelentkezés, jelszó](#15-session-jelszó)
16. [SQL összefoglaló](#16-sql-összefoglaló)
17. [Tesztelés és hibakeresés](#17-tesztelés)
18. [HTTP státuszkódok](#18-státuszkódok)

---

## 1. XAMPP

1. **XAMPP Control Panel** → **Apache: Start**, **MySQL: Start**.
2. **phpMyAdmin**: http://localhost/phpmyadmin → *Importálás* → `.sql` fájl kiválasztása → *Indítás*.
3. A projektmappa a `C:\xampp\htdocs\` alá kerül → `C:\xampp\htdocs\babszem_webshop\backend\termekek.php`
   = http://localhost/babszem_webshop/backend/termekek.php
4. Alap felhasználó: `root`, jelszó: **üres** (`""`).

Ha az Apache nem indul: a 80-as portot más foglalja (Skype, IIS) → Config → `httpd.conf` → `Listen 8080`, majd `http://localhost:8080/...`.
Ha a MySQL nem indul: a 3306-os portot egy másik MySQL foglalja.

---

## 2. Alapok

```php
<?php
// egysoros komment
/* többsoros */

$nev = "Anna";           // minden változó $-ral kezdődik
$kor = 20;
$ar = 4.5;
$aktiv = true;
$semmi = null;
const AFA = 0.27;

echo "Szia $nev!";              // dupla idézőjelben a változó behelyettesítődik
echo 'Szia $nev!';              // szimplában NEM
echo "Ár: {$termek['price']} Ft";   // tömbelem stringben → kapcsos zárójel
echo "Név: " . $nev;            // összefűzés PONTTAL (nem +)
print_r($tomb);                 // tömb kiírása (debug)
var_dump($valtozo);             // típussal együtt (debug)

// típus
gettype($kor);       // "integer"
is_int($kor);        // true    ← validálásnál!
is_numeric("42");    // true
is_string($nev);
is_array($t);
isset($x);           // létezik és nem null
empty($x);           // "", 0, "0", null, [], false → true

// átalakítás
(int)"42";           // 42
(float)"3.5";
(string)42;
intval("42abc");     // 42
round(3.456, 2);     // 3.46
number_format(11900, 0, ",", " ");   // "11 900"

$x = $_GET["id"] ?? null;   // null coalescing: ha nincs, null
```

---

## 3. Stringek

```php
strlen("alma");                    // 4 (BÁJTOK! ékezetnél mb_strlen)
mb_strlen("kávé");                 // 4
strtoupper("abc"); strtolower("ABC");
mb_strtolower("KÁVÉ");             // ékezetes kisbetűsítés
trim("  szöveg  ");                // szóközök levágása ← űrlapnál!
str_contains("Babszem", "szem");   // true (PHP 8)
str_starts_with("Babszem", "Bab");
strpos("Babszem", "szem");         // 3 (nincs: false)
stripos(...)                       // kis-nagybetű független
substr("Babszem", 0, 3);           // "Bab"
str_replace("a", "A", "alma");     // "AlmA"
explode(",", "a,b,c");             // ["a","b","c"]  (split)
implode(", ", ["a","b"]);          // "a, b"          (join)
ucfirst("alma");                   // "Alma"
sprintf("%05d", 42);               // "00042"
sprintf("%.2f", 3.14159);          // "3.14"
htmlspecialchars($szoveg);         // HTML-be kiíráskor MINDIG (XSS ellen)
```

---

## 4. Tömbök

```php
$szamok = [3, 1, 4];                 // indexelt
$termek = [                          // asszociatív (= JSON objektum)
    "id" => 5,
    "name" => "Cappuccino",
    "price" => 890
];

$szamok[] = 9;               // végére tesz (push)
$termek["stock"] = 18;       // új kulcs
unset($termek["stock"]);     // törlés
count($szamok);              // darabszám
in_array(4, $szamok);        // true
array_key_exists("id", $termek);
isset($termek["id"]);

foreach ($szamok as $szam) { echo $szam; }
foreach ($termek as $kulcs => $ertek) { echo "$kulcs: $ertek"; }

sort($szamok);               // növekvő (rsort: csökkenő)
usort($termekek, fn($a, $b) => $a["price"] <=> $b["price"]);   // saját rendezés

array_map(fn($t) => $t["name"], $termekek);            // Select
array_filter($termekek, fn($t) => $t["stock"] > 0);    // Where (kulcsokat megtartja!)
array_values($tomb);                                   // újraindexelés 0-tól
array_column($termekek, "name");                       // egy oszlop kigyűjtése
array_column($termekek, "name", "id");                 // id => name
array_sum([1, 2, 3]);       max([1, 5]);   min([1, 5]);
array_unique([1, 1, 2]);    array_merge($a, $b);
array_keys($t);             array_reverse($t);
array_slice($t, 0, 5);      // első 5 (top N)
```

---

## 5. Feltételek, ciklusok, függvények

```php
if ($kor >= 18) {
} elseif ($kor >= 14) {
} else {
}

===  !==  &&  ||  !       // mindig === legyen
$uzenet = $kor >= 18 ? "felnőtt" : "kiskorú";

$eredmeny = match ($metodus) {
    "GET" => "lekérés",
    "POST" => "létrehozás",
    default => "egyéb",
};

for ($i = 0; $i < 10; $i++) { }
while ($feltetel) { }

function osszead(int $a, int $b): int {
    return $a + $b;
}
$negyzet = fn($x) => $x * $x;    // arrow function

// függvényen belül a külső $pdo nem látszik → paraméterként add át:
function termekLetezik(PDO $pdo, int $id): bool { ... }
```

---

## 6. Szuperglobálisok

```php
$_SERVER["REQUEST_METHOD"]   // "GET", "POST", "PATCH", "DELETE", "PUT"
$_GET["id"]                  // URL paraméter: termekek.php?id=5
$_POST["nev"]                // HTML űrlap (form) mezője (name="nev")
$_FILES["kep"]               // feltöltött fájl
$_SESSION["user"]            // session (session_start() után)

// JSON törzs beolvasása (fetch-ből jövő POST/PATCH/PUT):
$adat = json_decode(file_get_contents("php://input"), true);   // true → asszociatív tömb
$nev = $adat["name"] ?? "";
```

⚠ `$_POST` **üres**, ha a kliens JSON-t küld! Ilyenkor `php://input` kell.

---

## 7. config.php

```php
<?php
// PDO kapcsolat (XAMPP: root, jelszó nélkül)
$host = "localhost";
$dbname = "babszem_webshop";
$user = "root";
$password = "";

$pdo = new PDO(
    "mysql:host=$host;dbname=$dbname;charset=utf8mb4",
    $user,
    $password,
    [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,       // hiba esetén kivétel
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC   // asszociatív tömbök
    ]
);
```

A többi fájlban:
```php
require_once "config.php";    // ezután használható a $pdo
```

`require` vs `include`: a `require` hiba esetén megáll, az `include` csak figyelmeztet. A `_once` nem tölti be kétszer.

---

## 8. PDO lekérdezések

### SELECT – több sor
```php
$stmt = $pdo->query("SELECT id, name FROM categories ORDER BY name");
$sorok = $stmt->fetchAll();       // tömbök tömbje
```

### SELECT – paraméterrel (PREPARED STATEMENT – kötelező, ha felhasználói adat van benne!)
```php
$stmt = $pdo->prepare("SELECT * FROM products WHERE id = :id");
$stmt->execute([":id" => $id]);
$termek = $stmt->fetch();          // EGY sor, vagy false ha nincs

if (!$termek) { /* 404 */ }
```

Kérdőjeles változat:
```php
$stmt = $pdo->prepare("SELECT * FROM products WHERE price > ? AND stock > ?");
$stmt->execute([1000, 0]);
```

bindValue típussal (LIMIT-nél kötelező!):
```php
$stmt = $pdo->prepare("SELECT * FROM products ORDER BY price DESC LIMIT :top");
$stmt->bindValue(":top", $top, PDO::PARAM_INT);
$stmt->execute();
```

### Egy érték (COUNT, SUM, MAX)
```php
$stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
$stmt->execute([":id" => $id]);
$db = $stmt->fetchColumn();        // pl. 1
```

### LIKE keresés
```php
$stmt = $pdo->prepare("SELECT * FROM products WHERE name LIKE :s");
$stmt->execute([":s" => "%" . $kereses . "%"]);
```

### INSERT
```php
$stmt = $pdo->prepare("INSERT INTO products (name, description, price, stock)
                       VALUES (:name, :description, :price, :stock)");
$stmt->execute([
    ":name" => $name,
    ":description" => $description,
    ":price" => $price,
    ":stock" => $stock
]);
$ujId = (int)$pdo->lastInsertId();     // az új AUTO_INCREMENT id
```

### UPDATE
```php
$stmt = $pdo->prepare("UPDATE products SET stock = :stock WHERE id = :id");
$stmt->execute([":stock" => $stock, ":id" => $id]);
$stmt->rowCount();     // hány sor változott
```

### DELETE
```php
// ELŐBB a gyerek/kapcsolótábla, UTÁNA a szülő (különben FK hiba)!
$pdo->prepare("DELETE FROM product_categories WHERE product_id = :id")->execute([":id" => $id]);
$pdo->prepare("DELETE FROM products WHERE id = :id")->execute([":id" => $id]);
```

---

## 9. JSON API alapváz

```php
<?php
require_once "config.php";
header("Content-Type: application/json; charset=utf-8");

// minden válasz ezen megy ki
function valasz($statusz, $adat) {
    http_response_code($statusz);
    echo json_encode($adat, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
    exit;
}

if ($_SERVER["REQUEST_METHOD"] !== "GET") {
    valasz(405, ["error" => "Method not allowed"]);
}

$sorok = $pdo->query("SELECT id, name FROM categories ORDER BY name")->fetchAll();

// számok legyenek SZÁMOK a JSON-ban (a PDO stringként adja vissza!)
$eredmeny = [];
foreach ($sorok as $sor) {
    $eredmeny[] = [
        "id" => (int)$sor["id"],
        "name" => $sor["name"]
    ];
}
valasz(200, $eredmeny);
```

- `JSON_UNESCAPED_UNICODE` → az ékezetek olvashatóak maradnak (nem `\u00e1`).
- `JSON_PRETTY_PRINT` → szép, tördelt kimenet.
- Számok: `(int)`, `(float)` cast – különben `"4290"` lesz `4290` helyett!
- CORS (ha a frontend más porton fut, pl. Live Server):
  ```php
  header("Access-Control-Allow-Origin: *");
  header("Access-Control-Allow-Methods: GET, POST, PATCH, PUT, DELETE, OPTIONS");
  header("Access-Control-Allow-Headers: Content-Type");
  if ($_SERVER["REQUEST_METHOD"] === "OPTIONS") { http_response_code(204); exit; }
  ```

---

## 10. Teljes REST végpont

```php
<?php
require_once "config.php";
header("Content-Type: application/json; charset=utf-8");

function valasz($statusz, $adat) {
    http_response_code($statusz);
    echo json_encode($adat, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
    exit;
}
function torzs() {
    $adat = json_decode(file_get_contents("php://input"), true);
    return is_array($adat) ? $adat : [];
}

$metodus = $_SERVER["REQUEST_METHOD"];
$id = isset($_GET["id"]) && $_GET["id"] !== "" ? (int)$_GET["id"] : null;

// ---------- GET ----------
if ($metodus === "GET") {
    if ($id === null) {
        $sorok = $pdo->query("SELECT * FROM products ORDER BY id")->fetchAll();
        valasz(200, $sorok);
    }
    $stmt = $pdo->prepare("SELECT * FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    $termek = $stmt->fetch();
    if (!$termek) valasz(404, ["error" => "A termék nem található."]);
    valasz(200, $termek);
}

// ---------- POST ----------
if ($metodus === "POST") {
    $adat = torzs();
    $name = trim($adat["name"] ?? "");
    $price = $adat["price"] ?? null;
    $stock = $adat["stock"] ?? null;

    if ($name === "") valasz(400, ["error" => "A név megadása kötelező."]);
    if (!is_int($price) || $price <= 0) valasz(400, ["error" => "Az ár csak pozitív egész szám lehet."]);
    if (!is_int($stock) || $stock < 0) valasz(400, ["error" => "A készlet nem lehet negatív."]);

    $stmt = $pdo->prepare("INSERT INTO products (name, price, stock) VALUES (:n, :p, :s)");
    $stmt->execute([":n" => $name, ":p" => $price, ":s" => $stock]);
    valasz(201, ["message" => "Termék sikeresen hozzáadva.", "id" => (int)$pdo->lastInsertId()]);
}

// ---------- PATCH (részleges módosítás) ----------
if ($metodus === "PATCH") {
    if ($id === null) valasz(400, ["error" => "Hiányzó azonosító."]);
    $stock = torzs()["stock"] ?? null;
    if (!is_int($stock) || $stock < 0) valasz(400, ["error" => "A készlet nem lehet negatív."]);

    $stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    if ($stmt->fetchColumn() == 0) valasz(404, ["error" => "A termék nem található."]);

    $pdo->prepare("UPDATE products SET stock = :s WHERE id = :id")->execute([":s" => $stock, ":id" => $id]);
    valasz(200, ["message" => "Készlet frissítve.", "id" => $id, "stock" => $stock]);
}

// ---------- PUT (teljes módosítás) ----------
if ($metodus === "PUT") {
    if ($id === null) valasz(400, ["error" => "Hiányzó azonosító."]);
    $adat = torzs();
    // ... ugyanaz a validálás, mint POST-nál ...
    $stmt = $pdo->prepare("UPDATE products SET name = :n, price = :p, stock = :s WHERE id = :id");
    $stmt->execute([":n" => $adat["name"], ":p" => $adat["price"], ":s" => $adat["stock"], ":id" => $id]);
    if ($stmt->rowCount() === 0) { /* lehet, hogy nincs ilyen, vagy nem változott semmi */ }
    valasz(200, ["message" => "Termék módosítva."]);
}

// ---------- DELETE ----------
if ($metodus === "DELETE") {
    if ($id === null) valasz(400, ["error" => "Hiányzó azonosító."]);
    $stmt = $pdo->prepare("SELECT COUNT(*) FROM products WHERE id = :id");
    $stmt->execute([":id" => $id]);
    if ($stmt->fetchColumn() == 0) valasz(404, ["error" => "A termék nem található."]);

    $pdo->prepare("DELETE FROM product_categories WHERE product_id = :id")->execute([":id" => $id]);
    $pdo->prepare("DELETE FROM products WHERE id = :id")->execute([":id" => $id]);
    valasz(200, ["message" => "Termék törölve."]);
}

// ---------- egyéb ----------
valasz(405, ["error" => "Method not allowed"]);
```

---

## 11. JOIN + csoportosítás

Kapcsolótábla (`product_categories`) → minden terméknél egy `categories` tömb:

```php
$sql = "SELECT p.id, p.name, p.price, c.id AS category_id, c.name AS category_name
        FROM products p
        LEFT JOIN product_categories pc ON pc.product_id = p.id
        LEFT JOIN categories c ON c.id = pc.category_id
        ORDER BY p.id, c.name";
$sorok = $pdo->query($sql)->fetchAll();

$termekek = [];
foreach ($sorok as $sor) {
    $id = (int)$sor["id"];
    if (!isset($termekek[$id])) {           // első sor ehhez a termékhez
        $termekek[$id] = [
            "id" => $id,
            "name" => $sor["name"],
            "price" => (int)$sor["price"],
            "categories" => []
        ];
    }
    if ($sor["category_id"] !== null) {     // LEFT JOIN: kategória nélkül NULL jön
        $termekek[$id]["categories"][] = [
            "id" => (int)$sor["category_id"],
            "name" => $sor["category_name"]
        ];
    }
}
echo json_encode(array_values($termekek), JSON_UNESCAPED_UNICODE);  // array_values → JSON tömb, nem objektum!
```

- `LEFT JOIN`: a kategória nélküli termék is megjelenik (üres tömbbel).
- `INNER JOIN` (`JOIN`): csak ahol van párja.
- `array_values` nélkül a kulcsok (1, 2, 3…) miatt a JSON **objektum** lenne, nem tömb!

Összesítés SQL-ben (pl. statisztika):
```sql
SELECT c.name, COUNT(pc.product_id) AS productCount
FROM categories c
LEFT JOIN product_categories pc ON pc.category_id = c.id
GROUP BY c.id, c.name
ORDER BY c.id;
```

---

## 12. Validálás

```php
$adat = torzs();

// kötelező szöveg
$name = isset($adat["name"]) ? trim($adat["name"]) : "";
if ($name === "") valasz(400, ["error" => "A név megadása kötelező."]);

// pozitív egész szám (JSON-ból szám jön; űrlapból string!)
if (!is_int($adat["price"] ?? null) || $adat["price"] <= 0) { ... }

// űrlapból ($_POST) jövő számnál:
$ar = filter_var($_POST["ar"] ?? "", FILTER_VALIDATE_INT);
if ($ar === false || $ar <= 0) { ... }

// email
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) { ... }

// nem kötelező mező: üres → NULL
$description = isset($adat["description"]) && trim($adat["description"]) !== ""
    ? trim($adat["description"]) : null;

// tömb, és minden elem létező id
$kategoriak = $adat["categories"] ?? [];
if (!is_array($kategoriak)) valasz(400, ["error" => "Nem létező kategória."]);
$ell = $pdo->prepare("SELECT COUNT(*) FROM categories WHERE id = :id");
foreach ($kategoriak as $kid) {
    if (!is_int($kid)) valasz(400, ["error" => "Nem létező kategória."]);
    $ell->execute([":id" => $kid]);
    if ($ell->fetchColumn() == 0) valasz(400, ["error" => "Nem létező kategória."]);
}

// engedélyezett értékek
if (!in_array($fizetes, ["Cash", "Card"])) { ... }
```

⚠ A feladat általában azt kéri, hogy **az első talált hibát** add vissza → a sorrend számít, a leírás sorrendjében ellenőrizz.

---

## 13. Tranzakció

Több összetartozó írás: vagy MIND sikerül, vagy EGYIK SEM.

```php
try {
    $pdo->beginTransaction();

    $pdo->prepare("INSERT INTO products (name, price, stock) VALUES (?, ?, ?)")
        ->execute([$name, $price, $stock]);
    $ujId = (int)$pdo->lastInsertId();

    $kapcs = $pdo->prepare("INSERT INTO product_categories (product_id, category_id) VALUES (?, ?)");
    foreach ($kategoriak as $kid) {
        $kapcs->execute([$ujId, $kid]);
    }

    $pdo->commit();                // véglegesítés
} catch (PDOException $e) {
    $pdo->rollBack();              // visszavonás
    valasz(500, ["error" => "A mentés sikertelen."]);
}
valasz(201, ["message" => "Termék sikeresen hozzáadva.", "id" => $ujId]);
```

---

## 14. Klasszikus PHP oldal

Űrlap + mentés + táblázat egy fájlban (`termekek_lista.php`):

```php
<?php
require_once "config.php";

$hiba = "";
if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $nev = trim($_POST["nev"] ?? "");
    $ar = filter_var($_POST["ar"] ?? "", FILTER_VALIDATE_INT);
    if ($nev === "" || $ar === false || $ar <= 0) {
        $hiba = "Hibás adatok!";
    } else {
        $pdo->prepare("INSERT INTO products (name, price, stock) VALUES (?, ?, 0)")->execute([$nev, $ar]);
        header("Location: termekek_lista.php");   // újratöltéskor ne küldje újra
        exit;
    }
}
$termekek = $pdo->query("SELECT * FROM products ORDER BY name")->fetchAll();
?>
<!DOCTYPE html>
<html lang="hu">
<head><meta charset="UTF-8"><title>Termékek</title></head>
<body>
    <form method="post">
        <input name="nev" placeholder="Név" required>
        <input name="ar" type="number" min="1" required>
        <button>Mentés</button>
    </form>
    <?php if ($hiba): ?>
        <p style="color:red"><?= htmlspecialchars($hiba) ?></p>
    <?php endif; ?>

    <table>
        <tr><th>Név</th><th>Ár</th></tr>
        <?php foreach ($termekek as $t): ?>
            <tr>
                <td><?= htmlspecialchars($t["name"]) ?></td>
                <td><?= number_format($t["price"], 0, ",", " ") ?> Ft</td>
            </tr>
        <?php endforeach; ?>
    </table>
</body>
</html>
```

`<?= $x ?>` = `<?php echo $x; ?>` rövidítés.

---

## 15. Session, jelszó

```php
session_start();                          // MINDEN oldal legelején (kiírás előtt!)
$_SESSION["user_id"] = 5;
if (!isset($_SESSION["user_id"])) { header("Location: login.php"); exit; }
session_destroy();                        // kijelentkezés

// jelszó tárolása: SOHA ne simán!
$hash = password_hash($jelszo, PASSWORD_DEFAULT);
if (password_verify($beirt, $hash)) { /* jó jelszó */ }
```

---

## 16. SQL összefoglaló

```sql
SELECT * FROM products;
SELECT name, price FROM products WHERE price > 3000 AND stock > 0;
SELECT * FROM products WHERE name LIKE '%kávé%';
SELECT * FROM products WHERE id IN (1, 2, 5);
SELECT * FROM products WHERE price BETWEEN 1000 AND 5000;
SELECT * FROM products WHERE description IS NULL;
SELECT * FROM products ORDER BY price DESC, name ASC;
SELECT * FROM products LIMIT 5;                    -- első 5
SELECT * FROM products LIMIT 5 OFFSET 10;          -- lapozás

SELECT COUNT(*), SUM(stock), AVG(price), MIN(price), MAX(price) FROM products;
SELECT category_id, COUNT(*) AS db FROM product_categories
    GROUP BY category_id HAVING COUNT(*) > 2;

SELECT p.name, c.name AS kategoria
FROM products p
INNER JOIN product_categories pc ON pc.product_id = p.id
INNER JOIN categories c ON c.id = pc.category_id;

-- termékek, amik sehol nem szerepelnek
SELECT * FROM products p
WHERE NOT EXISTS (SELECT 1 FROM product_categories pc WHERE pc.product_id = p.id);

INSERT INTO categories (name) VALUES ('Tea');
UPDATE products SET stock = stock - 1 WHERE id = 3;
DELETE FROM products WHERE id = 17;

CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);
CREATE TABLE product_categories (
    product_id INT NOT NULL,
    category_id INT NOT NULL,
    PRIMARY KEY (product_id, category_id),
    FOREIGN KEY (product_id) REFERENCES products(id),
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

---

## 17. Tesztelés

- **Böngésző**: GET-et közvetlenül megnyithatsz: http://localhost/babszem_webshop/backend/termekek.php?id=2
- **Postman / Thunder Client (VS Code)**: metódus kiválasztása → URL → *Body* → *raw* → *JSON* → `{"stock": 15}` → Send.
- **F12 → Network** a frontendnél: kérés, státusz, válasz.
- **Szintaxis ellenőrzés**: `C:\xampp\php\php.exe -l termekek.php`
- **Fehér oldal / „Unexpected token <”**: PHP hiba van. Kapcsold be a hibák kiírását a fájl elején:
  ```php
  ini_set("display_errors", 1);
  error_reporting(E_ALL);
  ```
- `SQLSTATE[HY000] [1049] Unknown database` → nincs importálva az adatbázis / elírt név.
- `SQLSTATE[23000] ... foreign key constraint fails` → előbb a kapcsolótábla sorait töröld.
- `Access denied for user 'root'` → a jelszó XAMPP-ban üres: `""`.
- Ékezetek `???` → `charset=utf8mb4` a DSN-ben és `JSON_UNESCAPED_UNICODE`.
- Az adatbázis visszaállítása: a `.sql` fájl újraimportálása.

---

## 18. Státuszkódok

| Kód | Jelentés | Mikor |
|---|---|---|
| 200 | OK | sikeres GET / PATCH / PUT / DELETE |
| 201 | Created | sikeres POST (létrehozás) |
| 204 | No Content | sikeres törlés, nincs törzs |
| 400 | Bad Request | hibás bemenet, hiányzó id |
| 401 | Unauthorized | nincs bejelentkezve |
| 403 | Forbidden | nincs jogosultsága |
| 404 | Not Found | nincs ilyen erőforrás |
| 405 | Method Not Allowed | nem támogatott metódus |
| 409 | Conflict | ütközés (pl. nem törölhető, mert hivatkoznak rá) |
| 500 | Internal Server Error | szerverhiba |

```php
http_response_code(404);
```
