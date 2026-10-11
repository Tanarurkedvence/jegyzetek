# JavaScript összefoglaló

[← vissza](README.md)

## Tartalom
1. [Bekötés](#1-bekötés)
2. [Változók, típusok](#2-változók-típusok)
3. [Operátorok, feltételek](#3-operátorok-feltételek)
4. [Ciklusok](#4-ciklusok)
5. [Függvények](#5-függvények)
6. [Stringek](#6-stringek)
7. [Tömbök és tömbfüggvények](#7-tömbök)
8. [Objektumok, JSON](#8-objektumok-json)
9. [DOM: elemek kiválasztása](#9-dom-kiválasztás)
10. [DOM: módosítás, létrehozás](#10-dom-módosítás)
11. [Események](#11-események)
12. [Hamburger menü](#hamburger-menü)
13. [Űrlap kezelése, ellenőrzés](#13-űrlap)
14. [fetch – API hívás (GET, POST, PATCH, DELETE)](#14-fetch)
15. [Lista megjelenítése API-ból (kártyák)](#15-lista-megjelenítése)
16. [Keresés, szűrés, rendezés](#16-keresés-szűrés-rendezés)
17. [Időzítők, localStorage, egyebek](#17-egyebek)
18. [Hibakeresés](#18-hibakeresés)

---

## 1. Bekötés

```html
<!-- a </body> elé: -->
<script src="script.js"></script>

<!-- vagy a <head>-be defer-rel (megvárja, míg betölt a HTML): -->
<script src="js/script.js" defer></script>
```

Ha a scripted a head-ben van `defer` nélkül → `null`-t kapsz a `getElementById`-ből!

---

## 2. Változók, típusok

```js
const nev = "Anna";      // nem írható felül (alapértelmezettként ezt használd)
let kor = 20;            // felülírható
// var – régi, ne használd

typeof 5          // "number"
typeof "a"        // "string"
typeof true       // "boolean"
typeof []         // "object"
typeof undefined  // "undefined"
Array.isArray([]) // true

// átalakítás
Number("42")        // 42
parseInt("42px")    // 42
parseFloat("3.5")   // 3.5
String(42)          // "42"
(3.14159).toFixed(2)        // "3.14"
(11900).toLocaleString("hu-HU") // "11 900"
isNaN(Number("abc"))        // true

// sablon string (backtick!)
const szoveg = `${nev} ${kor} éves`;
```

---

## 3. Operátorok, feltételek

```js
===   !==     // MINDIG ezt használd (típust is néz)
==    !=      // kerüld
&&  ||  !     // és, vagy, nem
a ?? "alap"   // ha a null/undefined, akkor "alap"
obj?.mezo     // nem dob hibát, ha obj null

if (kor >= 18) {
    console.log("nagykorú");
} else if (kor >= 14) {
    console.log("kamasz");
} else {
    console.log("gyerek");
}

const uzenet = kor >= 18 ? "felnőtt" : "kiskorú";   // ternary

switch (nap) {
    case "hétfő":
        console.log("Hétfő");
        break;
    default:
        console.log("Más nap");
}
```

---

## 4. Ciklusok

```js
for (let i = 0; i < 10; i++) { }

for (const elem of tomb) { }        // tömb ELEMEIN
for (const kulcs in objektum) { }   // objektum KULCSAIN

tomb.forEach((elem, index) => { });

while (feltetel) { }
do { } while (feltetel);

break;     // kilép
continue;  // következő kör
```

---

## 5. Függvények

```js
function osszead(a, b) {
    return a + b;
}

const szoroz = (a, b) => a * b;          // nyíl függvény, egy sor → automatikus return
const negyzet = x => x * x;
const udvozol = (nev = "vendég") => `Szia ${nev}!`;   // alapérték

// aszinkron (fetch-hez)
async function betolt() {
    const valasz = await fetch("api.php");
    return await valasz.json();
}
```

---

## 6. Stringek

```js
const s = "Babszem Kávézó";
s.length                 // 14
s.toUpperCase()          // "BABSZEM KÁVÉZÓ"
s.toLowerCase()
s.includes("Kávé")       // true   ← keresésnél!
s.startsWith("Bab")      // true
s.endsWith("zó")
s.indexOf("Kávé")        // 8 (nincs: -1)
s.slice(0, 7)            // "Babszem"
s.substring(8)           // "Kávézó"
s.replace("Kávézó", "Bolt")
s.replaceAll("a", "A")
s.split(" ")             // ["Babszem", "Kávézó"]
"  szöveg  ".trim()      // "szöveg"
"5".padStart(3, "0")     // "005"
"ab".repeat(3)           // "ababab"

// kis-nagybetű független keresés:
nev.toLowerCase().includes(keresett.toLowerCase())
```

---

## 7. Tömbök

```js
const t = [3, 1, 4, 1, 5];
t.length
t[0]                    // első
t[t.length - 1]         // utolsó
t.push(9)               // végére tesz
t.pop()                 // végéről levesz
t.unshift(0)            // elejére
t.shift()               // elejéről levesz
t.splice(1, 2)          // 1. indextől 2 elemet töröl
t.includes(4)           // true
t.indexOf(4)            // 2
t.join(", ")            // "3, 1, 4, 1, 5"
[...t]                  // másolat
[...t1, ...t2]          // összefűzés
```

**A legfontosabbak (LINQ-szerűek):**
```js
const termekek = [
    { id: 1, nev: "Espresso", ar: 590, keszlet: 10 },
    { id: 2, nev: "Latte", ar: 950, keszlet: 0 },
    { id: 3, nev: "Cappuccino", ar: 890, keszlet: 5 }
];

// szűrés (C#: Where)
const keszleten = termekek.filter(t => t.keszlet > 0);

// átalakítás (C#: Select)
const nevek = termekek.map(t => t.nev);              // ["Espresso", "Latte", "Cappuccino"]

// első találat (C#: FirstOrDefault) – nincs: undefined
const latte = termekek.find(t => t.nev === "Latte");
const index = termekek.findIndex(t => t.id === 2);

// van-e ilyen? (C#: Any) / mind ilyen? (C#: All)
termekek.some(t => t.ar > 900);     // true
termekek.every(t => t.ar > 0);      // true

// összegzés (C#: Sum)
const osszErtek = termekek.reduce((osszeg, t) => osszeg + t.ar * t.keszlet, 0);

// átlag
const atlag = termekek.reduce((s, t) => s + t.ar, 0) / termekek.length;

// max / min
const max = Math.max(...termekek.map(t => t.ar));
const legdragabb = termekek.reduce((a, b) => a.ar > b.ar ? a : b);

// rendezés (MÓDOSÍTJA a tömböt! másolathoz: [...termekek].sort(...))
termekek.sort((a, b) => a.ar - b.ar);              // ár szerint növekvő
termekek.sort((a, b) => b.ar - a.ar);              // csökkenő
termekek.sort((a, b) => a.nev.localeCompare(b.nev, "hu"));  // név szerint (ékezetes is jó)

// darabszám feltétellel (C#: Count)
const db = termekek.filter(t => t.keszlet === 0).length;

// csoportosítás
const csoport = {};
termekek.forEach(t => {
    const kulcs = t.keszlet > 0 ? "van" : "nincs";
    if (!csoport[kulcs]) csoport[kulcs] = [];
    csoport[kulcs].push(t);
});

// egyedi értékek (C#: Distinct)
const egyedi = [...new Set([1, 1, 2, 3])];   // [1, 2, 3]
```

---

## 8. Objektumok, JSON

```js
const termek = { id: 5, nev: "Cappuccino", ar: 890 };
termek.nev               // "Cappuccino"
termek["ar"]             // 890
termek.keszlet = 18;     // új mező
delete termek.keszlet;
Object.keys(termek)      // ["id", "nev", "ar"]
Object.values(termek)
Object.entries(termek)   // [["id",5], ["nev","Cappuccino"], ...]

const { nev, ar } = termek;       // destrukturálás
const uj = { ...termek, ar: 990 }; // másolat módosított árral

// JSON
const szoveg = JSON.stringify(termek);       // objektum → JSON szöveg
const obj = JSON.parse(szoveg);              // JSON szöveg → objektum
JSON.stringify(termek, null, 2);             // szépen formázva
```

Osztály:
```js
class Termek {
    constructor(nev, ar) {
        this.nev = nev;
        this.ar = ar;
    }
    bruttoAr() {
        return Math.round(this.ar * 1.27);
    }
}
const t = new Termek("Espresso", 590);
```

---

## 9. DOM kiválasztás

```js
document.getElementById("hamburger")           // id alapján (# NÉLKÜL!)
document.querySelector(".navbar")              // első találat CSS szelektorral
document.querySelector("#login")
document.querySelectorAll(".kartya")           // MINDEN találat (NodeList, forEach-elhető)
document.getElementsByClassName("kartya")      // élő lista (forEach NINCS rajta)
document.getElementsByTagName("p")

elem.querySelector("h3")                       // elemen BELÜL keres
elem.parentElement
elem.children
elem.closest(".kartya")                        // legközelebbi ilyen ős
```

---

## 10. DOM módosítás

```js
const cim = document.querySelector("h1");

cim.textContent = "Új cím";                 // csak szöveg (biztonságos)
cim.innerHTML = "<em>Új</em> cím";          // HTML-ként értelmezi
input.value                                 // input értéke (mindig STRING!)
input.value = "";
checkbox.checked                            // true/false
select.value                                // kiválasztott option value-ja

// stílus
cim.style.color = "red";
cim.style.backgroundColor = "black";        // kötőjel helyett camelCase
cim.style.display = "none";

// osztályok (EZ a jobb módszer)
elem.classList.add("active");
elem.classList.remove("active");
elem.classList.toggle("active");            // ki-be kapcsol
elem.classList.contains("active");          // true/false

// attribútumok
img.src = "kepek/kave2.jpg";
img.setAttribute("alt", "Espresso");
img.getAttribute("alt");
gomb.dataset.id                             // data-id="5" → "5"
gomb.disabled = true;

// új elem létrehozása
const li = document.createElement("li");
li.textContent = "Espresso";
li.className = "termek";
lista.appendChild(li);          // a végére
lista.prepend(li);              // az elejére
li.remove();                    // törlés
lista.innerHTML = "";           // kiürítés
```

---

## 11. Események

```js
gomb.addEventListener("click", function () { });
gomb.addEventListener("click", () => { });

input.addEventListener("input", () => { });     // minden betűnél (keresőmező!)
input.addEventListener("change", () => { });    // amikor elhagyja / select változik
form.addEventListener("submit", (e) => {
    e.preventDefault();                          // NE töltse újra az oldalt!
});
window.addEventListener("load", () => { });
document.addEventListener("DOMContentLoaded", () => { });
elem.addEventListener("mouseover", () => { });
elem.addEventListener("mouseout", () => { });
document.addEventListener("keydown", (e) => {
    if (e.key === "Escape") { }
});
window.addEventListener("scroll", () => { console.log(window.scrollY); });
window.addEventListener("resize", () => { console.log(window.innerWidth); });

// esemény objektum
e.target          // amire kattintottak
e.target.value

// esemény delegálás (sok gombhoz egy figyelő, pl. dinamikusan létrehozott elemeknél)
lista.addEventListener("click", (e) => {
    const gomb = e.target.closest("button");
    if (!gomb) return;
    console.log(gomb.dataset.id);
});
```

---

## Hamburger menü

```js
// Hamburger menü: kattintásra az "active" osztály ki-be kapcsolása
const hamburger = document.getElementById("hamburger");
const navbar = document.getElementById("navbar");

hamburger.addEventListener("click", function () {
    navbar.classList.toggle("active");
});
```

CSS-ben hozzá: `.navbar { display: none; }` és `.navbar.active { display: grid; }` a mobilos media query-ben.

Bővítés: menüpontra kattintva csukódjon be:
```js
document.querySelectorAll("#navbar a").forEach(link => {
    link.addEventListener("click", () => navbar.classList.remove("active"));
});
```

---

## 13. Űrlap

```js
const form = document.getElementById("urlap");
const hiba = document.getElementById("hiba");

form.addEventListener("submit", (e) => {
    e.preventDefault();

    const nev = document.getElementById("nev").value.trim();
    const ar = Number(document.getElementById("ar").value);
    const kategoriak = [...document.querySelectorAll("#kategoriak input:checked")]
                        .map(cb => Number(cb.value));
    const fizetes = document.querySelector('input[name="fizetes"]:checked')?.value;

    if (nev === "") {
        hiba.textContent = "A név megadása kötelező.";
        return;
    }
    if (!Number.isInteger(ar) || ar <= 0) {
        hiba.textContent = "Az ár csak pozitív egész szám lehet.";
        return;
    }

    hiba.textContent = "";
    console.log({ nev, ar, kategoriak, fizetes });
    form.reset();       // mezők kiürítése
});
```

Email ellenőrzés regexszel:
```js
const jo = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
```

---

## 14. fetch

### GET
```js
async function betoltes() {
    try {
        const valasz = await fetch("../backend/termekek.php");
        if (!valasz.ok) throw new Error("HTTP " + valasz.status);
        const adatok = await valasz.json();
        console.log(adatok);
    } catch (hiba) {
        console.log("Hiba:", hiba);
    }
}
betoltes();
```

Régebbi `.then()` forma (ugyanaz):
```js
fetch("../backend/kategoriak.php")
    .then(r => r.json())
    .then(adatok => console.log(adatok))
    .catch(h => console.log(h));
```

### POST / PATCH / DELETE (JSON törzzsel)
```js
async function kuldes(url, metodus, torzs) {
    const opciok = {
        method: metodus,
        headers: { "Content-Type": "application/json" }
    };
    if (torzs !== undefined) opciok.body = JSON.stringify(torzs);

    const valasz = await fetch(url, opciok);
    const adat = await valasz.json();
    return { ok: valasz.ok, status: valasz.status, adat };
}

// új termék
await kuldes("../backend/termekek.php", "POST",
    { name: "Kenyai AA", description: "...", price: 4590, stock: 20, categories: [1, 5] });

// készlet módosítás
await kuldes("../backend/termekek.php?id=6", "PATCH", { stock: 15 });

// törlés
const e = await kuldes("../backend/termekek.php?id=17", "DELETE");
uzenet.textContent = e.adat.message || e.adat.error;
```

Query paraméter:
```js
const url = `api/products?categoryId=${id}&search=${encodeURIComponent(kereses)}`;
```

Form adat küldése (nem JSON, hanem klasszikus POST – PHP-ban `$_POST`):
```js
await fetch("mentes.php", { method: "POST", body: new FormData(form) });
```

---

## 15. Lista megjelenítése

```js
const tarolo = document.getElementById("termekek");

function megjelenit(termekek) {
    tarolo.innerHTML = "";
    if (termekek.length === 0) {
        tarolo.innerHTML = "<p>Nincs ilyen termék.</p>";
        return;
    }
    termekek.forEach(t => {
        const kartya = document.createElement("div");
        kartya.className = "kartya";
        kartya.innerHTML = `
            <h3>${t.name}</h3>
            <p>${t.description ?? "Nincs leírás."}</p>
            <p class="ar">${t.price.toLocaleString("hu-HU")} Ft</p>
            <button class="torles" data-id="${t.id}">Törlés</button>
        `;
        kartya.querySelector(".torles").addEventListener("click", () => torol(t.id));
        tarolo.appendChild(kartya);
    });
}
```

Táblázatba:
```js
tbody.innerHTML = adatok.map(a => `
    <tr><td>${a.id}</td><td>${a.name}</td><td>${a.price} Ft</td></tr>
`).join("");
```

Select feltöltése:
```js
kategoriak.forEach(k => {
    const opt = document.createElement("option");
    opt.value = k.id;
    opt.textContent = k.name;
    select.appendChild(opt);
});
```

---

## 16. Keresés, szűrés, rendezés

```js
let osszesTermek = [];

kereso.addEventListener("input", frissit);
kategoriaSelect.addEventListener("change", frissit);
rendezes.addEventListener("change", frissit);

function frissit() {
    const szo = kereso.value.toLowerCase();
    const kat = kategoriaSelect.value;

    let lista = osszesTermek.filter(t =>
        t.name.toLowerCase().includes(szo) &&
        (kat === "" || t.categories.some(c => String(c.id) === kat))
    );

    if (rendezes.value === "ar") lista.sort((a, b) => a.price - b.price);
    if (rendezes.value === "nev") lista.sort((a, b) => a.name.localeCompare(b.name, "hu"));

    megjelenit(lista);
    darab.textContent = `${lista.length} termék`;
}
```

---

## 17. Egyebek

```js
setTimeout(() => uzenet.textContent = "", 3000);   // 3 mp múlva egyszer
const id = setInterval(() => { }, 1000);            // másodpercenként
clearInterval(id);

// localStorage (böngészőben megmarad)
localStorage.setItem("kosar", JSON.stringify(kosar));
const kosar = JSON.parse(localStorage.getItem("kosar")) ?? [];
localStorage.removeItem("kosar");

// dátum
const most = new Date();
most.getFullYear(); most.getMonth() + 1; most.getDate();
most.toLocaleDateString("hu-HU");     // "2026. 10. 12."

// matek
Math.round(2.5)  // 3
Math.floor(2.9)  // 2
Math.ceil(2.1)   // 3
Math.random()    // 0..1
Math.floor(Math.random() * 10) + 1   // 1..10

// párbeszéd
alert("Üzenet");
const igen = confirm("Biztosan törlöd?");
const nev = prompt("Neved?");

// görgetés
document.getElementById("rolunk").scrollIntoView({ behavior: "smooth" });
window.scrollTo({ top: 0, behavior: "smooth" });

// URL paraméter olvasása (oldal.html?id=5)
const id = new URLSearchParams(location.search).get("id");
```

---

## 18. Hibakeresés

- **F12 → Console**: itt látod a hibákat. `console.log(valtozo)` mindenhova!
- **F12 → Network**: látod a fetch kéréseket, a státuszkódot és a választ (Response fül).
- `Cannot read properties of null` → a `getElementById` nem találta az elemet (elírt id, vagy a script hamarabb fut, mint a HTML → tedd a `</body>` elé vagy `defer`).
- `Unexpected token < in JSON` → a szerver HTML-t (PHP hibaüzenetet) adott vissza JSON helyett → nézd meg a Network fülön a választ.
- `fetch` CORS hiba fájlként megnyitva → szerverről nyisd meg (`http://localhost/...` vagy Live Server).
- Az `input.value` mindig **string** → számhoz `Number(...)`.
