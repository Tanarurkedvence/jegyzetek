# CSS összefoglaló

[← vissza](README.md)

## Tartalom
1. [Alapok, bekötés, szelektorok](#1-alapok-bekötés-szelektorok)
2. [Változók (`:root`)](#2-változók-root)
3. [Box model: margin, padding, border](#3-box-model)
4. [Szöveg és betűtípus](#4-szöveg-és-betűtípus)
5. [Színek, háttér, átmenet (gradient)](#5-színek-háttér-gradient)
6. [Méretek és mértékegységek](#6-méretek-és-mértékegységek)
7. [Display](#7-display)
8. [Flexbox](#8-flexbox)
9. [Grid](#9-grid)
10. [Pozicionálás](#10-pozicionálás)
11. [Képek: object-fit, háttérkép](#11-képek)
12. [Lekerekítés, árnyék](#12-lekerekítés-árnyék)
13. [Hover, transition, transform, animáció](#13-hover-transition-transform-animáció)
14. [Pszeudo-osztályok és -elemek](#14-pszeudo-osztályok-és--elemek)
15. [Reszponzivitás: media query](#15-reszponzivitás-media-query)
16. [Hamburger menü (CSS része)](#16-hamburger-menü)
17. [Táblázat, lista, űrlap stílus](#17-táblázat-lista-űrlap)
18. [Gyakori minták (kész receptek)](#18-gyakori-minták)
19. [Hibakeresés, tipikus hibák](#19-tipikus-hibák)

---

## 1. Alapok, bekötés, szelektorok

```html
<!-- a <head>-be -->
<link rel="stylesheet" href="style.css">
```

Ha a CSS a `css/` mappában van, a benne lévő képútvonal **a CSS fájlhoz képest** relatív:
`url(../kepek/hero.jpg)`. Ha a gyökérben van: `url(kepek/hero.jpg)`.
**Soha ne** írj `/kepek/...`-t (perjellel kezdve) – fájlként megnyitva nem tölt be!

| Szelektor | Jelentés |
|---|---|
| `*` | minden elem |
| `p` | minden `<p>` |
| `.doboz` | `class="doboz"` |
| `#fejlec` | `id="fejlec"` (id csak **egyszer** szerepelhet az oldalon!) |
| `header h1` | `h1` a `header`-en **belül** (bármilyen mélyen) |
| `nav > a` | `a`, ami a `nav` **közvetlen** gyereke |
| `h2 + p` | `p`, ami **közvetlenül** egy `h2` után jön |
| `h1, h2, h3` | mindhárom (vesszővel felsorolás) |
| `.box.kiemelt` | olyan elem, aminek **mindkét** osztálya megvan |
| `a[href]` | `a`, aminek van `href` attribútuma |
| `input[type="text"]` | szöveges input |
| `.navbar.active` | navbar, amikor az `active` osztály is rajta van (JS teszi rá) |

**Erősorrend (specificitás):** `!important` > inline `style=""` > `#id` > `.osztály` / `:hover` > `elem`.
Azonos erősségnél a **később** írt szabály nyer.

```css
/* reset – minden feladat elejére jó */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box; /* a padding és border benne van a width-ben */
}
```

---

## 2. Változók (`:root`)

```css
:root {
    --sotet: #2b1d16;
    --krem: #f5ebe0;
    --karamell: #c08552;
    --tejes: #e6d5c3;
    --radius: 20px;
}

body {
    background-color: var(--krem);
    color: var(--sotet);
}

.doboz {
    border: 2px solid var(--karamell);
    border-radius: var(--radius);
}

/* tartalék érték, ha nincs ilyen változó: */
color: var(--nincs-ilyen, black);
```

---

## 3. Box model

```
 margin  (kívül, átlátszó)
 ┌ border ───────────────┐
 │ padding (belül)        │
 │   ┌ tartalom ───────┐  │
 │   └─────────────────┘  │
 └────────────────────────┘
```

```css
.doboz {
    width: 300px;
    height: 200px;
    padding: 20px;               /* mind a 4 oldal */
    padding: 10px 20px;          /* fent-lent | jobb-bal */
    padding: 10px 20px 30px 40px;/* fent | jobb | lent | bal (óramutató) */
    padding-top: 10px;           /* egy oldal */
    margin: 0 auto;              /* VÍZSZINTES KÖZÉPRE IGAZÍTÁS (kell hozzá width!) */
    border: 2px solid black;     /* vastagság | stílus | szín */
    border-bottom: 1px solid var(--karamell); /* csak alul vonal */
    border-style: dashed;        /* solid, dashed, dotted, double, none */
}
```

Középre igazított, a lap 80%-át kitöltő tartalom:
```css
.tartalom {
    width: 80%;
    margin: 0 auto;
}
```

Max szélesség (nagy képernyőn se legyen túl széles):
```css
.tartalom { max-width: 1200px; margin: 0 auto; padding: 0 20px; }
```

---

## 4. Szöveg és betűtípus

```css
body {
    font-family: Arial, sans-serif;     /* szövegtörzs */
}
h1, h2, h3 {
    font-family: Georgia, serif;        /* címsorok */
}

p {
    font-size: 16px;          /* px, rem, em, %, small, large, x-large */
    font-weight: bold;        /* normal, bold, 100..900 */
    font-style: italic;
    line-height: 1.6;         /* sortávolság */
    text-align: center;       /* left, right, center, justify */
    text-decoration: none;    /* linkek aláhúzásának eltüntetése! */
    text-decoration: underline;
    text-transform: uppercase;/* lowercase, capitalize */
    letter-spacing: 2px;
    word-spacing: 4px;
    color: white;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
    white-space: nowrap;      /* ne törjön sort */
    text-indent: 30px;        /* első sor behúzása */
}

/* túl hosszú szöveg "..." */
.vagott {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
```

Google Font (ha engedik):
```html
<link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
```
```css
body { font-family: 'Roboto', sans-serif; }
```

---

## 5. Színek, háttér, gradient

```css
color: red;
color: #c08552;                    /* hex */
color: rgb(192, 133, 82);
color: rgba(0, 0, 0, 0.5);         /* 0.5 = 50% átlátszó */
opacity: 0.8;                       /* az egész elem átlátszósága */

background-color: var(--krem);
background-image: url(kepek/hero.jpg);
background-size: cover;            /* kitölti, levágja ami kilóg */
background-size: contain;          /* teljes kép látszik */
background-position: center;
background-repeat: no-repeat;
background-attachment: fixed;      /* parallax hatás */
```

### Átmenet (gradient)
```css
/* balról jobbra halványuló sötét */
background: linear-gradient(to right, rgba(43,29,22,0.9), rgba(43,29,22,0));

/* felülről lefelé, két szín */
background: linear-gradient(to bottom, #2b1d16, #c08552);

/* szöggel */
background: linear-gradient(45deg, red, blue);

/* kör alakú */
background: radial-gradient(circle, white, gray);
```

### Gradient + háttérkép egyszerre (HERO!)
A **gradient kerül előre**, vesszővel elválasztva, utána a kép:
```css
.hero {
    height: 600px;
    background-image:
        linear-gradient(to right, rgba(43,29,22,0.9), rgba(43,29,22,0)),
        url(kepek/hero.jpg);
    background-size: cover;
    background-position: center;
}
```

---

## 6. Méretek és mértékegységek

| Egység | Jelentés |
|---|---|
| `px` | pixel (fix) |
| `%` | a szülő méretéhez képest |
| `em` | a saját/szülő betűméretéhez képest |
| `rem` | a `html` betűméretéhez képest (alap 16px → `2rem` = 32px) |
| `vw` / `vh` | a képernyő szélességének / magasságának 1%-a (`100vh` = teljes magasság) |
| `fr` | grid: a maradék hely egy része |

```css
width: 100%;
max-width: 600px;
min-height: 100vh;
height: fit-content;
width: calc(100% - 40px);          /* számolás */
font-size: clamp(1.5rem, 4vw, 3rem); /* min, ajánlott, max – folyékony méret */
aspect-ratio: 16 / 9;              /* arány tartása */
```

---

## 7. Display

```css
display: block;        /* új sorban, teljes szélesség (div, p, h1, section) */
display: inline;       /* soron belül, width/height NEM hat (span, a) */
display: inline-block; /* soron belül, de width/height hat */
display: none;         /* eltűnik (hely sem marad) */
display: flex;
display: grid;

visibility: hidden;    /* láthatatlan, de a helye megmarad */
```

---

## 8. Flexbox

**Egy irányú** elrendezés (sor VAGY oszlop). A szülőre kell írni!

```css
.menu {
    display: flex;
    flex-direction: row;              /* row (alap), column, row-reverse */
    justify-content: space-evenly;    /* FŐTENGELY mentén */
    align-items: center;              /* KERESZTTENGELY mentén */
    gap: 15px;                        /* távolság az elemek között */
    flex-wrap: wrap;                  /* ha nem fér ki, új sorba tör */
}
```

`justify-content` értékei (vízszintes, ha `row`):
| érték | hatás |
|---|---|
| `flex-start` | balra |
| `flex-end` | jobbra |
| `center` | középre |
| `space-between` | szélső elemek a széleken, közte egyenlő hely |
| `space-around` | mindenki körül egyenlő hely |
| `space-evenly` | **teljesen egyenletes** elosztás |

`align-items`: `stretch` (alap), `flex-start`, `flex-end`, `center`, `baseline`.

Gyerekre írható:
```css
.elem {
    flex: 1;            /* egyenlően osztoznak a helyen */
    flex: 2;            /* kétszer annyi helyet kap */
    flex-grow: 1;
    flex-shrink: 0;     /* ne zsugorodjon */
    flex-basis: 200px;  /* kiinduló méret */
    order: -1;          /* előrébb kerül (alap 0) */
    align-self: center; /* csak ez az egy elem */
}
```

**Tökéletes középre igazítás (vízszintesen ÉS függőlegesen):**
```css
.kozep {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

---

## 9. Grid

**Két irányú** elrendezés (sorok ÉS oszlopok).

```css
.racs {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;        /* 3 egyforma oszlop */
    grid-template-columns: repeat(3, 1fr);     /* ugyanaz rövidebben */
    grid-template-columns: 2fr 1fr;            /* 2:1 arány (pl. menü + gombok) */
    grid-template-columns: 1fr 2fr;            /* kép + szöveg 1:2 */
    grid-template-columns: 200px 1fr;          /* fix + maradék */
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* MAGÁTÓL reszponzív! */
    grid-template-rows: 100px auto;
    gap: 20px;                                  /* sor- és oszloptáv */
    row-gap: 10px;
    column-gap: 30px;
    align-items: center;                        /* cellán belül függőlegesen */
    justify-items: center;                      /* cellán belül vízszintesen */
}
```

Gyerek elem elhelyezése:
```css
.szeles {
    grid-column: 1 / 3;      /* 1. vonaltól a 3.-ig = 2 oszlopot foglal */
    grid-column: span 2;     /* 2 oszlopot foglal */
    grid-column: 1 / -1;     /* teljes sort foglal */
    grid-row: 1 / 3;
}
```

Elnevezett területek:
```css
.oldal {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-template-areas:
        "fejlec fejlec"
        "oldalsav tartalom"
        "lablec lablec";
}
header { grid-area: fejlec; }
aside  { grid-area: oldalsav; }
main   { grid-area: tartalom; }
footer { grid-area: lablec; }
```

### Flex vagy Grid?
- **Flex**: egy sor elem (menüpontok, gombok egymás mellett, kártya belseje).
- **Grid**: rács, oszlopok (kártyák 3 oszlopban, kép + szöveg arányban, lábléc 3 oszlop).

---

## 10. Pozicionálás

```css
position: static;    /* alap */
position: relative;  /* a saját helyéhez képest tolható; "horgony" az absolute gyerekeknek */
position: absolute;  /* a legközelebbi relative szülőhöz képest */
position: fixed;     /* a képernyőhöz rögzítve (görgetéskor is ott marad) */
position: sticky;    /* görgetéskor odaragad (top: 0 kell hozzá) */

top: 0; right: 20px; bottom: 0; left: 0;
z-index: 10;         /* ki van felül (csak position-nel működik) */
```

Jobb felső sarokba tett gomb (pl. hamburger):
```css
.fejlec { position: relative; }
.hamburger {
    position: absolute;
    top: 50%;
    right: 20px;
    transform: translateY(-50%);  /* függőlegesen pontosan középre */
}
```

Ragadós fejléc:
```css
header { position: sticky; top: 0; z-index: 100; }
```

---

## 11. Képek

```css
img {
    width: 100%;          /* kitölti a szülőt */
    height: 200px;
    object-fit: cover;    /* kitölti, nem torzul, levágja a fölösleget */
    object-fit: contain;  /* teljes kép látszik, maradhat üres hely */
    object-position: center;
    display: block;       /* eltünteti a kép alatti kis rést */
}
```

Kép, ami kitölti a grid cellát:
```css
.sor img { width: 100%; height: 100%; object-fit: cover; border-radius: 20px; }
```

---

## 12. Lekerekítés, árnyék

```css
border-radius: 20px;          /* lekerekített sarkok */
border-radius: 30px;          /* "pirula" gomb */
border-radius: 50%;           /* KÖR (négyzetes elemnél) */
border-top-left-radius: 20px;

box-shadow: 0 5px 15px rgba(0,0,0,0.15);  /* x | y | elmosás | szín */
box-shadow: 0 0 10px black inset;          /* belső árnyék */

overflow: hidden; /* ha a kép kilóg a lekerekített kártya sarkán */
```

---

## 13. Hover, transition, transform, animáció

```css
.kartya {
    transition: transform 0.3s, box-shadow 0.3s;   /* milyen tulajdonság, mennyi idő */
    transition: all 0.3s ease;
}
.kartya:hover {
    transform: translateY(-8px);   /* KISSÉ MEGEMELKEDIK */
    box-shadow: 0 10px 25px rgba(0,0,0,0.25);
}

button:hover { background-color: var(--sotet); color: white; cursor: pointer; }
```

`transform` lehetőségek:
```css
transform: translateY(-10px);   /* mozgatás fel */
transform: translateX(20px);
transform: scale(1.05);         /* nagyítás 5%-kal */
transform: rotate(45deg);
transform: translate(-50%, -50%); /* középre igazítás absolute-tal */
transform: scale(1.1) rotate(5deg); /* több egyszerre */
```

Animáció:
```css
@keyframes beuszas {
    from { opacity: 0; transform: translateY(30px); }
    to   { opacity: 1; transform: translateY(0); }
}
.hero h2 { animation: beuszas 1s ease-out; }

@keyframes forgas { to { transform: rotate(360deg); } }
.toltes { animation: forgas 1s linear infinite; }
```

---

## 14. Pszeudo-osztályok és -elemek

```css
a:hover        { }   /* egér felette */
a:active       { }   /* kattintás közben */
input:focus    { outline: 2px solid var(--karamell); }
li:first-child { }
li:last-child  { border-bottom: none; }
li:nth-child(2){ }   /* a 2. */
tr:nth-child(even) { background: #eee; }  /* páros sorok (zebracsík) */
tr:nth-child(odd)  { }
p:not(.kiemelt){ }
input:checked  { }
button:disabled{ opacity: 0.5; }

/* tartalom beszúrása előtte/utána */
.ar::after   { content: " Ft"; }
.uj::before  { content: "★ "; color: gold; }
::placeholder { color: gray; }
::selection   { background: var(--karamell); }
```

---

## 15. Reszponzivitás: media query

A `<head>`-ből **NEM HIÁNYOZHAT**:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

```css
/* asztali stílus felül (alap) ... */

/* 1000 px ALATT (tablet) */
@media (max-width: 1000px) {
    .navbar { grid-template-columns: 1fr; }     /* menüsor egy oszlop */
    .log { justify-content: center; }           /* gombok középen */
    .one_two, .two_one { grid-template-columns: 1fr; } /* egymás alá */
    .two_one img { order: -1; }                 /* a kép legyen felül! */
    .kartyak { grid-template-columns: repeat(2, 1fr); } /* 2 oszlop */
}

/* 600 px ALATT (mobil) */
@media (max-width: 600px) {
    .hamburger { display: block; }
    .navbar { display: none; }
    .navbar.active { display: grid; }
    .hero { height: 380px; }
    .kartyak, .lablec { grid-template-columns: 1fr; } /* egymás alá */
}
```

⚠ **A kisebb méretű media query kerüljön LEJJEBB**, különben a nagyobb felülírja!

Mobile-first változat (alul kicsi, felfelé bővül):
```css
.kartyak { grid-template-columns: 1fr; }
@media (min-width: 600px)  { .kartyak { grid-template-columns: repeat(2, 1fr); } }
@media (min-width: 1000px) { .kartyak { grid-template-columns: repeat(3, 1fr); } }
```

Egyéb:
```css
@media (orientation: landscape) { }
@media print { nav, footer { display: none; } }
@media (prefers-color-scheme: dark) { :root { --krem: #222; } }
```

**Elemek sorrendjének cseréje mobilon:** `order: -1;` (flex/grid gyereken)
vagy `flex-direction: column-reverse;`.

---

## 16. Hamburger menü

HTML:
```html
<header>
    <div class="fejlec">
        <h1>Oldal neve</h1>
        <button id="hamburger" class="hamburger">☰</button>
    </div>
    <nav class="navbar" id="navbar"> ... </nav>
</header>
```

CSS:
```css
.fejlec { position: relative; text-align: center; padding: 25px; }
.hamburger { display: none; position: absolute; right: 20px; top: 50%;
             transform: translateY(-50%); background: none; color: white;
             border: 2px solid white; border-radius: 8px; font-size: 1.4rem; }

@media (max-width: 600px) {
    .fejlec { text-align: left; }
    .hamburger { display: block; }
    .navbar { display: none; }
    .navbar.active { display: grid; }          /* JS kapcsolja */
    .nav_list { flex-direction: column; align-items: center; }
}
```

JS: lásd [JavaScript.md](JavaScript.md#hamburger-menü).

---

## 17. Táblázat, lista, űrlap

```css
table { width: 100%; border-collapse: collapse; }  /* összevont keretek */
th, td { border: 1px solid #ccc; padding: 8px; text-align: left; }
th { background: var(--sotet); color: white; }
tr:nth-child(even) { background: #f2f2f2; }
tr:hover { background: #ddd; }

ul { list-style: none; }          /* pöttyök eltüntetése */
ul { list-style-type: square; }   /* disc, circle, decimal */

input, select, textarea {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-family: inherit;         /* örökli a body betűtípusát */
}
textarea { resize: vertical; }
button { cursor: pointer; border: none; padding: 10px 20px; border-radius: 30px; }
```

---

## 18. Gyakori minták

### Fejléc + menüsor (grid 2fr 1fr, bal oldalt flex)
```css
.navbar { display: grid; grid-template-columns: 2fr 1fr; align-items: center;
          background: var(--karamell); padding: 12px 20px; }
.nav_list { display: flex; justify-content: space-evenly; }
.nav_list a { text-decoration: none; color: var(--sotet); font-weight: bold; }
.log { display: flex; justify-content: flex-end; gap: 10px; }
.log button { border-radius: 30px; padding: 8px 18px; }
#signup { background: var(--sotet); color: white; }
```

### Kártyák (3 oszlop, fehér, árnyék, emelkedés)
```css
.kartyak { display: grid; grid-template-columns: repeat(3, 1fr); gap: 25px; }
.kartya { background: white; border-radius: 20px; overflow: hidden;
          box-shadow: 0 5px 15px rgba(0,0,0,.15); transition: transform .3s; }
.kartya:hover { transform: translateY(-8px); }
.kartya img { width: 100%; height: 200px; object-fit: cover; display: block; }
.kartya-szoveg { padding: 20px; }
```

### Kép + szövegdoboz arányos sorokban
```css
.sor1 { display: grid; grid-template-columns: 1fr 2fr; gap: 20px; }
.sor2 { display: grid; grid-template-columns: 2fr 1fr; gap: 20px; }
.szovegdoboz { background: var(--tejes); border: 2px solid var(--karamell);
               border-radius: 20px; padding: 25px; display: flex; align-items: center; }
```

### Lábléc 3 oszlop + elválasztó vonal + copyright
```css
footer { background: var(--sotet); color: white; padding: 40px 0 20px; }
.lablec { width: 80%; margin: 0 auto; display: grid; grid-template-columns: repeat(3, 1fr);
          padding-bottom: 25px; border-bottom: 1px solid var(--karamell); }
.lablec h3 { color: var(--karamell); }
.copy { text-align: center; padding-top: 20px; }
```

### Lábléc mindig alul (rövid oldalnál is)
```css
body { min-height: 100vh; display: flex; flex-direction: column; }
main { flex: 1; }
```

### Kör alakú profilkép
```css
.avatar { width: 120px; height: 120px; border-radius: 50%; object-fit: cover; }
```

### Gomb
```css
.gomb { display: inline-block; padding: 12px 24px; background: var(--karamell);
        color: white; border: none; border-radius: 30px; font-weight: bold;
        text-decoration: none; cursor: pointer; transition: background .3s; }
.gomb:hover { background: var(--sotet); }
```

---

## 19. Tipikus hibák

- **Nem töltődik be a CSS** → rossz útvonal a `<link href>`-ben, vagy elírt fájlnév (kis/nagybetű!).
- **Nem látszik a háttérkép** → az útvonal a **CSS fájlhoz** képest relatív; `/`-rel ne kezdd.
- **Nem működik a media query** → hiányzik a `viewport` meta; vagy rossz a sorrend (kisebb méret legyen lejjebb).
- **Kilóg az elem / vízszintes görgetősáv** → `box-sizing: border-box;` hiányzik, vagy fix `width` px-ben.
- **`margin: auto` nem igazít középre** → nincs megadva `width`, vagy `inline` elem.
- **`z-index` nem hat** → nincs `position` az elemen.
- **A hover átmenet ugrik** → a `transition` az alap elemre kell, nem a `:hover`-re.
- **Duplikált `id`** → többször használt stílushoz `class` kell.
- F12 → Elements fül: látod, melyik szabály hat és melyik van áthúzva. Az eszköztár ikonjával (Ctrl+Shift+M) kipróbálhatod a mobilnézetet.
