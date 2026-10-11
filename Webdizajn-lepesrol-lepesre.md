# Webdizájn feladat lépésről lépésre – teljesen kezdőknek

[← vissza](README.md) · Kész, kipróbált megoldás: [webmegoldas](https://github.com/Tanarurkedvence/webmegoldas) · Összefoglalók: [HTML.md](HTML.md) · [CSS.md](CSS.md) · [JavaScript.md](JavaScript.md)

> **Mit kell csinálni egy mondatban?**
> Kapsz **képeket** és egy **szövegfájlt**, meg **képernyőképeket** (mintákat), hogy hogyan nézzen ki az oldal gépen, tableten és mobilon. Neked ebből kell megcsinálnod **3 fájlt**, hogy az oldal pont úgy nézzen ki.

**A 3 fájl és a szerepük – egy ház hasonlattal:**
| Fájl | Mi ez? | Ház hasonlat |
|---|---|---|
| `index.html` | **MI** van az oldalon (címek, szövegek, képek, gombok) | a **falak, szobák, bútorok** |
| `style.css` | **HOGYAN** néz ki (színek, méretek, elrendezés, mobilnézet) | a **festés, tapéta, berendezés** |
| `script.js` | **MIT CSINÁL** (itt csak: a ☰ gombra lenyílik a menü) | a **villanykapcsoló** |

A példa a **Babszem Kávézó** feladat. Ha nálad más a téma, a [12. pontban](#12-ha-más-a-feladat-mit-cserélj) leírom, mit kell átírni – a szerkezet ugyanaz szokott lenni.

**Pontok (10):**
| Lépés | Mit csinálsz | Pont |
|---|---|---|
| [2](#2-a-html-váz--1-pont) | HTML alapváz, szemantikus elemek | 1 |
| [3](#3-fejléc-és-menüsor--2-pont) | fejléc + menüsor | 2 |
| [4](#4-hero--1-pont) | hero (nagy kép felül) | 1 |
| [5](#5-rólunk--2-pont) | Rólunk szakasz | 2 |
| [6](#6-kedvenceink-kártyák--1-pont) | kártyák | 1 |
| [7](#7-lábléc--1-pont) | lábléc | 1 |
| [8–9](#8-tablet-1000-px-alatt--1-pont) | tablet + mobil nézet, hamburger menü | 2 |

## Tartalom
0. [Aranyszabályok](#0-aranyszabályok)
1. [Előkészítés: mappa, fájlok, megnyitás](#1-előkészítés)
2. [A HTML váz – 1 pont](#2-a-html-váz--1-pont)
3. [Fejléc és menüsor – 2 pont](#3-fejléc-és-menüsor--2-pont)
4. [Hero – 1 pont](#4-hero--1-pont)
5. [Rólunk – 2 pont](#5-rólunk--2-pont)
6. [Kedvenceink (kártyák) – 1 pont](#6-kedvenceink-kártyák--1-pont)
7. [Lábléc – 1 pont](#7-lábléc--1-pont)
8. [Tablet (1000 px alatt) – 1 pont](#8-tablet-1000-px-alatt--1-pont)
9. [Mobil (600 px alatt) + hamburger menü – 1 pont](#9-mobil-600-px-alatt--hamburger--1-pont)
10. [Hogyan próbáld ki a mobilnézetet?](#10-mobilnézet-kipróbálása)
11. [A KÉSZ fájlok egyben](#11-a-kész-fájlok-egyben)
12. [Ha más a feladat: mit cserélj?](#12-ha-más-a-feladat-mit-cserélj)
13. [Végső ellenőrzés leadás előtt (pontlista)](#13-végső-ellenőrzés)
14. [Ha valami nem úgy néz ki](#14-ha-valami-nem-úgy-néz-ki)

---

## 0. Aranyszabályok

1. **A szövegeket MÁSOLD a `szoveg.txt`-ből** (Ctrl+C / Ctrl+V), ne gépeld! A feladat kifejezetten kéri, és így nem lesz elírás.
2. **Lépésenként haladj, és minden lépés után nézd meg a böngészőben** (mentés: Ctrl+S, böngészőben frissítés: F5).
3. **A képek útvonala: `kepek/hero.jpg`** – perjel NÉLKÜL az elején! (`/kepek/...`-kal nem jelenik meg.)
4. A feladatlapon lévő **számokat** (600px, 30px, 2fr 1fr, 1000px, 600px…) **pontosan** használd – ezeket nézik a javítók.
5. Külső keretrendszert (Bootstrap) **nem** szabad használni.

**Mi a különbség a `class` és az `id` között?**
- `class="valami"` → a **CSS**-nek szól („ez így nézzen ki”). Több elemen is lehet ugyanaz.
- `id="valami"` → **egyedi** név, főleg a **JavaScript**-nek („ezt az egy elemet keresd meg”). Csak egyszer szerepelhet!

---

## 1. Előkészítés

1. Csinálj egy mappát (pl. az Asztalon `webmegoldas`).
2. Másold bele a kapott **`kepek`** mappát (benne: `hero.jpg`, `rolunk1.jpg`, `rolunk2.jpg`, `kave1.jpg`, `kave2.jpg`, `kave3.jpg`).
3. Nyisd meg a mappát **VS Code**-ban: *File → Open Folder*.
4. Hozd létre a 3 üres fájlt (bal oldalt, a mappa neve melletti „új fájl” ikon):
   - `index.html`
   - `style.css`
   - `script.js`

Így néz ki a mappa:
```
webmegoldas/
├── index.html
├── style.css
├── script.js
└── kepek/
    ├── hero.jpg
    ├── rolunk1.jpg  rolunk2.jpg
    └── kave1.jpg  kave2.jpg  kave3.jpg
```
⚠ A feladat azt kéri, hogy **ez a 3 fájl legyen, és a `kepek` mappa mellettük**. Ne tedd a CSS-t külön `css` mappába!

**Hogyan nézd meg az oldalt?** Az `index.html`-en jobb klikk → *Reveal in File Explorer* → dupla klikk rá → megnyílik a böngészőben. (Webdizájnnál ez jó, itt nincs PHP.) Ha VS Code-ban van **Live Server** bővítmény: jobb klikk → *Open with Live Server* – ez mentéskor magától frissít.

---

## 2. A HTML váz – 1 pont

**Mi ez?** Az oldal „csontváza”. Minden HTML oldal így kezdődik.

Írd az `index.html`-be (VS Code-ban: írd be, hogy `!`, nyomj **Tab**-ot → legenerálja a felét, utána javítsd ki így):

```html
<!DOCTYPE html>
<html lang="hu">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Babszem Kávézó</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <!-- 3. lépés: ide jön a <header> -->

    <main>
        <!-- 4. lépés: ide jön a hero -->
        <!-- 5. lépés: ide jön a Rólunk -->
        <!-- 6. lépés: ide jön a Kedvenceink -->
    </main>

    <!-- 7. lépés: ide jön a <footer> -->

    <script src="script.js"></script>
</body>

</html>
```

**Mit jelent? (mindegyikre pont jár!)**
| Sor | Jelentés | Ha kihagyod |
|---|---|---|
| `lang="hu"` | az oldal nyelve magyar | ❌ pontlevonás (a VS Code `en`-t ír – **javítsd!**) |
| `charset="UTF-8"` | ékezetek helyesen | ❌ `Ã¡` jelenik meg `á` helyett |
| `viewport` | mobilon ne legyen kicsinyítve | ❌ a mobilnézet NEM fog működni |
| `<title>` | a böngészőfül felirata | ❌ ha „Document” marad, pontlevonás |
| `<link ... style.css>` | bekapcsolja a CSS-t | ❌ nincs semmi stílus |
| `<script src="script.js">` | bekapcsolja a JS-t (a `</body>` elé!) | ❌ nem nyílik a hamburger menü |
| `<!-- ... -->` | megjegyzés, nem látszik | – (a végén kitörölheted) |

**Szemantikus elemek** (a feladat kéri, pont jár érte): `header` (fejléc), `nav` (menü), `main` (fő tartalom), `section` (szakasz), `article` (önálló doboz/kártya), `footer` (lábléc). Ezeket fogjuk használni `div` helyett, ahol értelmes.

Most írd a `style.css` elejére az **alapokat**:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root {
    --sotet: #2b1d16;
    --krem: #f5ebe0;
    --karamell: #c08552;
    --tejes: #e6d5c3;
}

body {
    font-family: Arial, sans-serif;
    background-color: var(--krem);
    color: var(--sotet);
    line-height: 1.6;
}

h1,
h2,
h3 {
    font-family: Georgia, serif;
}
```

**Mit jelent?**
- `* { margin: 0; padding: 0; box-sizing: border-box; }` → „mindent nullázz le” – a böngésző saját margóit kiszedjük, és a méretek kiszámíthatóak lesznek. **Mindig ezzel kezdj.**
- `:root { --sotet: #2b1d16; ... }` → **színváltozók** (a feladat kéri: „CSS változóként (:root) adja meg”). Később így használod: `var(--sotet)`. A neveket és színkódokat a feladatlap táblázatából vedd!
- `body` → az egész oldal: betűtípus Arial, háttér krém, szövegszín sötét.
- `h1, h2, h3` → a címsorok betűtípusa Georgia (a feladat kéri).

✅ **Teszt:** nyisd meg az oldalt → üres krémszínű oldal. (Még nincs tartalom.) Ha fehér: a CSS nincs bekötve (nézd meg a `<link>` sort és a fájlnevet).

---

## 3. Fejléc és menüsor – 2 pont

**Mit kér a feladat?** Sötét háttéren középen a főcím. Alatta karamell színű menüsor **CSS griddel két részre (2fr 1fr)**: bal oldalt a 4 menüpont **egyenletesen elosztva (flex)**, jobb oldalt 2 lekerekített gomb (**border-radius: 30px**), a Regisztráció sötét hátterű, fehér betűs.

### HTML – a `<!-- 3. lépés -->` megjegyzés HELYÉRE:
```html
    <header>
        <div class="fejlec">
            <h1>Babszem Kávézó</h1>
            <button id="hamburger" class="hamburger" aria-label="Menü">☰</button>
        </div>
        <nav class="navbar" id="navbar">
            <div class="nav_list">
                <a href="#">Kezdőlap</a>
                <a href="#">Kínálat</a>
                <a href="#">Üzleteink</a>
                <a href="#">Kapcsolat</a>
            </div>
            <div class="log">
                <button id="login">Bejelentkezés</button>
                <button id="signup">Regisztráció</button>
            </div>
        </nav>
    </header>
```

**Mit jelent?**
- `<header>` → a fejléc (szemantikus elem).
- `<div class="fejlec">` → egy doboz a címnek és a ☰ gombnak (a gomb csak mobilon fog látszani – 9. lépés).
- `<nav class="navbar" id="navbar">` → a menüsor. A `class` a CSS-nek, az `id` a JavaScriptnek kell.
- `<div class="nav_list">` → a 4 menüpont (`<a href="#">` = link, ami sehova nem visz).
- `<div class="log">` → a 2 gomb. Mindkettőnek saját `id`-ja van, mert máshogy néznek ki.

### CSS – a `style.css` végére:
```css
/* ===== Fejléc ===== */
header {
    background-color: var(--sotet);
}

.fejlec {
    position: relative;
    text-align: center;
    padding: 25px 20px;
}

.fejlec h1 {
    color: white;
    font-size: 1.8rem;
}

.hamburger {
    display: none;
    position: absolute;
    top: 50%;
    right: 20px;
    transform: translateY(-50%);
    background: none;
    color: white;
    border: 2px solid white;
    border-radius: 8px;
    font-size: 1.4rem;
    padding: 0 10px;
    cursor: pointer;
}

/* ===== Menüsor ===== */
.navbar {
    background-color: var(--karamell);
    display: grid;
    grid-template-columns: 2fr 1fr;
    align-items: center;
    padding: 12px 20px;
}

.nav_list {
    display: flex;
    justify-content: space-evenly;
}

.nav_list a {
    text-decoration: none;
    color: var(--sotet);
    font-weight: bold;
    font-size: 0.9rem;
}

.log {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
}

.log button {
    padding: 8px 18px;
    border: 2px solid var(--sotet);
    border-radius: 30px;
    font-weight: bold;
    cursor: pointer;
}

#login {
    background-color: var(--krem);
    color: var(--sotet);
}

#signup {
    background-color: var(--sotet);
    color: white;
}
```

**Mit jelent? (a lényeg)**
| CSS | Mit csinál | Feladatbeli kérés |
|---|---|---|
| `header { background-color: var(--sotet); }` | sötét háttér | „sötét háttéren” |
| `.fejlec { text-align: center; }` | a cím középre | „középre igazított főcím” |
| `.navbar { display: grid; grid-template-columns: 2fr 1fr; }` | **két oszlop**, a bal kétszer olyan széles | „CSS griddel két részre (2fr 1fr)” |
| `.nav_list { display: flex; justify-content: space-evenly; }` | a menüpontok egymás mellett, **egyenletesen** | „egyenletesen elosztva (flex)” |
| `.log { display: flex; justify-content: flex-end; }` | a gombok jobbra | „jobb oldalon a két gomb” |
| `.log button { border-radius: 30px; }` | lekerekített gombok | „border-radius: 30px” |
| `#signup { background-color: var(--sotet); color: white; }` | Regisztráció sötét, fehér betű | „sötét hátterű, fehér feliratú” |
| `text-decoration: none` | a linkek ne legyenek aláhúzva | (a minta szerint) |
| `.hamburger { display: none; }` | a ☰ gomb **el van rejtve** (mobilon fog megjelenni) | – |

✅ **Teszt:** felül sötét sáv, középen fehér „Babszem Kávézó”; alatta karamell sáv: bal oldalt 4 menüpont szépen elosztva, jobb oldalt 2 kerek gomb, a Regisztráció sötét.

---

## 4. Hero – 1 pont

**Mit kér a feladat?** 600 px magas sáv a `hero.jpg` háttérképpel (cover, középre igazítva), rajta **balról jobbra halványuló sötét átmenet** (linear-gradient). Bal oldalt cím, alcím és egy karamell színű, lekerekített gomb.

### HTML – a `<main>`-en belül, a `<!-- 4. lépés -->` helyére:
```html
        <section class="hero">
            <div class="herocon">
                <h2>Minden nap egy jó kávéval kezdődik</h2>
                <p>Frissen pörkölt kávé, házi sütemények és nyugodt sarkok négy városban.</p>
                <button>Nézd meg a kínálatot</button>
            </div>
        </section>
```

### CSS – a `style.css` végére:
```css
/* ===== Hero ===== */
.hero {
    height: 600px;
    background-image: linear-gradient(to right, rgba(43, 29, 22, 0.9), rgba(43, 29, 22, 0)), url(kepek/hero.jpg);
    background-size: cover;
    background-position: center;
    display: flex;
    align-items: center;
}

.herocon {
    margin-left: 8%;
    max-width: 500px;
    color: white;
}

.herocon h2 {
    font-size: 2.6rem;
    line-height: 1.2;
}

.herocon p {
    margin-top: 15px;
}

.herocon button {
    margin-top: 25px;
    padding: 12px 24px;
    border: none;
    border-radius: 30px;
    background-color: var(--karamell);
    color: white;
    font-weight: bold;
    cursor: pointer;
}
```

**Mit jelent?**
| CSS | Mit csinál |
|---|---|
| `height: 600px` | 600 px magas (a feladat kéri) |
| `background-image: linear-gradient(...), url(kepek/hero.jpg)` | **két réteg**: felül az átmenet, alatta a kép. **Az átmenet van elöl**, vesszővel elválasztva! |
| `linear-gradient(to right, rgba(43,29,22,0.9), rgba(43,29,22,0))` | balról (90% sötét) jobbra (teljesen átlátszó) halványul. A `43, 29, 22` a `--sotet` szín (#2b1d16) számokkal |
| `background-size: cover` | a kép kitölti a sávot (a feladat kéri) |
| `background-position: center` | középre igazítva (a feladat kéri) |
| `display: flex; align-items: center;` | a szöveg **függőlegesen középre** kerül |
| `.herocon { margin-left: 8%; max-width: 500px; }` | a szöveg balra, nem túl széles |
| `.herocon button { border-radius: 30px; background-color: var(--karamell); }` | karamell, lekerekített gomb |

✅ **Teszt:** nagy kávés kép, bal oldalt sötétebb (ott jól olvasható a fehér szöveg), jobbra világosodik. Bal oldalt cím, alcím, karamell gomb.

❌ Ha nincs kép: rossz az útvonal. A CSS a `style.css`-hez képest keres → `url(kepek/hero.jpg)` (ha a CSS a gyökérben van, ahogy kell).

---

## 5. Rólunk – 2 pont

**Mit kér a feladat?** A tartalom **az oldal 80%-a**, középre igazítva. Első sor: **kép + szövegdoboz 1fr 2fr**. Második sor: **szövegdoboz + kép 2fr 1fr** (grid). A képek kitöltik a helyüket (**object-fit: cover**), a dobozok **20 px-esen lekerekítettek, karamell keretesek, tejes hátterűek**.

### HTML – a hero `</section>` UTÁN:
```html
        <section class="about_us">
            <h2>Rólunk</h2>
            <div class="one_two">
                <img src="kepek/rolunk1.jpg" alt="Kávébabok">
                <article class="textbox">
                    <p>A Babszem 2016-ban egy aprócska budapesti pultként indult, ahol a reggeli kávé mellé mindig jutott egy jó szó is. Ma négy városban várunk, de a filozófiánk nem változott: minden csésze számít. A kávébabot közvetlenül kis ültetvényekről szerezzük be, és saját pörkölőnkben, kis adagokban pörköljük, hogy minden fajta a legjobb arcát mutassa. Nálunk nincs futószalag – a baristáink ismerik a törzsvendégek kedvencét, és szívesen mesélnek arról, honnan érkezett a csészédben lévő kávé.</p>
                </article>
            </div>
            <div class="two_one">
                <article class="textbox">
                    <p>Hiszünk abban, hogy egy kávézó több, mint egy hely, ahol kávét iszunk. Nálunk lehet tanulni, dolgozni, beszélgetni vagy csak csendben nézni az esőt az ablakon át. A süteményeinket minden reggel helyben sütjük, a tejet környékbeli gazdaságoktól vásároljuk, a zaccot pedig a közeli közösségi kerteknek adjuk tovább komposztnak. Ha szeretnéd megtanulni, hogyan készül a tökéletes cappuccino, nézz be egy szombati barista-kurzusunkra!</p>
                </article>
                <img src="kepek/rolunk2.jpg" alt="Két csésze kávé latte arttal">
            </div>
        </section>
```

- Két sor: `one_two` (kép, aztán szöveg) és `two_one` (szöveg, aztán kép).
- A szövegdobozok `<article>`-ek (szemantikus elem: önálló tartalom).
- **Minden `<img>`-nek van `alt` szövege** – a feladat kéri, pont jár érte! (Rövid leírás a képről.)
- A szövegeket **másold** a `szoveg.txt`-ből.

### CSS – a `style.css` végére:
```css
/* ===== Rólunk ===== */
.about_us,
.fav {
    width: 80%;
    margin: 0 auto;
}

.about_us h2,
.fav h2 {
    text-align: center;
    font-size: 2rem;
    padding: 40px 0 25px;
}

.one_two,
.two_one {
    display: grid;
    gap: 20px;
    margin-bottom: 20px;
}

.one_two {
    grid-template-columns: 1fr 2fr;
}

.two_one {
    grid-template-columns: 2fr 1fr;
}

.one_two img,
.two_one img {
    width: 100%;
    height: 100%;
    min-height: 250px;
    object-fit: cover;
    border-radius: 20px;
}

.textbox {
    display: flex;
    align-items: center;
    padding: 25px;
    background-color: var(--tejes);
    border: 2px solid var(--karamell);
    border-radius: 20px;
}
```

**Mit jelent?**
| CSS | Mit csinál |
|---|---|
| `width: 80%; margin: 0 auto;` | 80% széles, **középen** (az `auto` margó középre tolja) |
| `.one_two { grid-template-columns: 1fr 2fr; }` | 1. sor: kép keskeny, szöveg kétszer olyan széles |
| `.two_one { grid-template-columns: 2fr 1fr; }` | 2. sor: fordítva |
| `gap: 20px` | hézag a kép és a doboz között |
| `object-fit: cover` + `height: 100%` | a kép kitölti a helyét, nem torzul (levágja a széleit) |
| `.textbox { border: 2px solid var(--karamell); border-radius: 20px; background-color: var(--tejes); }` | karamell keret, 20px kerekítés, tejes háttér |
| `.about_us h2, .fav h2 { text-align: center; }` | a szakaszcímek középen (a Kedvenceink címét is már most beállítjuk) |

✅ **Teszt:** középen „Rólunk” cím, alatta 2 sor: kép–szöveg, aztán szöveg–kép; lekerekített, karamell keretes, bézs dobozok.

---

## 6. Kedvenceink (kártyák) – 1 pont

**Mit kér a feladat?** **Három egyforma szélességű kártya (grid)**. Fehérek, lekerekítettek, **árnyékosak**, a kép **200 px magas**. Ha az egeret föléviszed, **kissé megemelkedik (transform)**.

### HTML – a Rólunk `</section>` UTÁN (még a `</main>` előtt):
```html
        <section class="fav">
            <h2>Kedvenceink</h2>
            <div class="boxgrid">
                <article class="box">
                    <img src="kepek/kave1.jpg" alt="Cappuccino">
                    <div class="boxtext">
                        <h3>Cappuccino</h3>
                        <p>Selymes tejhab, dupla espresso, latte art szívvel.</p>
                        <p class="ar">890 Ft</p>
                    </div>
                </article>
                <article class="box">
                    <img src="kepek/kave2.jpg" alt="Espresso">
                    <div class="boxtext">
                        <h3>Espresso</h3>
                        <p>Házi blendünk sűrű, mogyorós krémával.</p>
                        <p class="ar">590 Ft</p>
                    </div>
                </article>
                <article class="box">
                    <img src="kepek/kave3.jpg" alt="Flat white">
                    <div class="boxtext">
                        <h3>Flat white</h3>
                        <p>Kevesebb hab, több kávé – a baristák kedvence.</p>
                        <p class="ar">990 Ft</p>
                    </div>
                </article>
            </div>
        </section>
```
- 3 egyforma `<article class="box">` – mindegyikben kép, cím (`h3`), leírás, ár.
- Az árnak **`class="ar"`** van (nem `id`!), mert 3-szor szerepel – `id` csak egyszer lehetne.

### CSS – a `style.css` végére:
```css
/* ===== Kedvenceink ===== */
.fav {
    padding-bottom: 50px;
}

.boxgrid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 25px;
}

.box {
    background-color: white;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
    transition: transform 0.3s;
}

.box:hover {
    transform: translateY(-8px);
}

.box img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    display: block;
}

.boxtext {
    padding: 20px;
}

.boxtext p {
    margin-top: 8px;
}

.ar {
    color: var(--karamell);
    font-weight: bold;
}
```

**Mit jelent?**
| CSS | Mit csinál |
|---|---|
| `grid-template-columns: repeat(3, 1fr)` | **3 egyforma oszlop** (= `1fr 1fr 1fr`) |
| `background-color: white; border-radius: 20px;` | fehér, lekerekített |
| `box-shadow: 0 5px 15px rgba(0,0,0,0.15)` | **árnyék** (lefelé 5px, elmosva 15px, halvány fekete) |
| `overflow: hidden` | a kép sarkai se lógjanak ki a lekerekítésből |
| `transition: transform 0.3s` | a mozgás **finom** legyen (0,3 mp alatt) |
| `.box:hover { transform: translateY(-8px); }` | egér fölötte → **8px-szel feljebb** (megemelkedik) |
| `.box img { height: 200px; object-fit: cover; }` | a kép 200px magas, kitölti |
| `.ar { color: var(--karamell); }` | az ár karamell színű |

✅ **Teszt:** 3 egyforma fehér kártya árnyékkal; vidd fölé az egeret → kicsit felemelkedik.

---

## 7. Lábléc – 1 pont

**Mit kér a feladat?** Sötét háttéren **három egyforma oszlop** (Címünk, Nyitvatartás, Elérhetőség), **karamell színű címekkel**; alatta **vékony karamell vonallal** elválasztva, **középre igazítva** a szerzői jogi sor.

### HTML – a `</main>` UTÁN, a `<!-- 7. lépés -->` helyére:
```html
    <footer>
        <div class="cont">
            <div>
                <h3>Címünk</h3>
                <p>1052 Budapest, Váci utca 18.</p>
            </div>
            <div>
                <h3>Nyitvatartás</h3>
                <p>Hétfő–Péntek: 7:00–19:00</p>
                <p>Szombat–Vasárnap: 8:00–18:00</p>
            </div>
            <div>
                <h3>Elérhetőség</h3>
                <p>+36 1 234 5678</p>
                <p>hello@babszem.hu</p>
            </div>
        </div>
        <p class="copy">© 2026 Babszem Kávézó – Minden csésze számít.</p>
    </footer>
```
- **Minden oszlop egy saját `<div>`** (cím + sorai együtt). Így a 3 oszlopos grid magától jó lesz, és mobilon szépen egymás alá kerülnek.

### CSS – a `style.css` végére:
```css
/* ===== Lábléc ===== */
footer {
    background-color: var(--sotet);
    color: white;
    padding: 40px 0 20px;
}

.cont {
    width: 80%;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    padding-bottom: 25px;
    border-bottom: 1px solid var(--karamell);
}

.cont h3 {
    color: var(--karamell);
    margin-bottom: 10px;
}

.copy {
    text-align: center;
    padding-top: 20px;
    font-size: 0.85rem;
}
```

**Mit jelent?**
| CSS | Mit csinál |
|---|---|
| `footer { background-color: var(--sotet); }` | sötét háttér |
| `.cont { display: grid; grid-template-columns: repeat(3, 1fr); }` | 3 egyforma oszlop |
| `border-bottom: 1px solid var(--karamell)` | **vékony karamell vonal** az oszlopok alatt |
| `.cont h3 { color: var(--karamell); }` | karamell címek |
| `.copy { text-align: center; }` | a © sor középen |

✅ **Teszt:** asztali nézetben **az oldal kész**, és úgy néz ki, mint a feladatlap első mintája! 🎉 Most jön a mobil.

---

## 8. Tablet (1000 px alatt) – 1 pont

**Mit kér a feladat?** 1000 px alatt: a menüsor **egy oszlopos** (gombok középen), a Rólunk sorai **egymás alá** kerülnek úgy, hogy **mindkét sorban a kép legyen felül**, a kártyák **két oszlopba** rendeződnek.

**Mi az a `@media`?** Egy „ha” a CSS-ben: **„HA a képernyő legfeljebb 1000 px széles, AKKOR ezek a szabályok is érvényesek”**. A benne lévő szabályok felülírják a fentieket.

### CSS – a `style.css` végére:
```css
/* ===== Tablet: 1000 px alatt ===== */
@media (max-width: 1000px) {
    .navbar {
        grid-template-columns: 1fr;
        gap: 12px;
    }

    .log {
        justify-content: center;
    }

    .one_two,
    .two_one {
        grid-template-columns: 1fr;
    }

    /* a második sorban is a kép legyen felül */
    .two_one img {
        order: -1;
    }

    .one_two img,
    .two_one img {
        height: 300px;
    }

    .boxgrid {
        grid-template-columns: repeat(2, 1fr);
    }
}
```

**Mit jelent?**
| CSS | Mit csinál |
|---|---|
| `.navbar { grid-template-columns: 1fr; }` | a menüsor **1 oszlop** (a gombok a menüpontok alá kerülnek) |
| `.log { justify-content: center; }` | a gombok **középre** |
| `.one_two, .two_one { grid-template-columns: 1fr; }` | a Rólunk sorai: kép és doboz **egymás alá** |
| `.two_one img { order: -1; }` | ⭐ a 2. sorban a HTML-ben a szöveg van elöl – az `order: -1` **előre hozza a képet** → **mindkét sorban a kép van felül** (erre külön figyelnek!) |
| `height: 300px` | egymás alatt a kép ne legyen óriási |
| `.boxgrid { grid-template-columns: repeat(2, 1fr); }` | kártyák **2 oszlopban** |

✅ **Teszt:** lásd a [10. pontot](#10-mobilnézet-kipróbálása), állíts **820 px**-et → a menü egy oszlopban, gombok középen, Rólunk egymás alatt (a képek mindig felül), kártyák 2 oszlopban.

---

## 9. Mobil (600 px alatt) + hamburger – 1 pont

**Mit kér a feladat?** 600 px alatt a menüsor **rejtett**, helyette a fejléc **jobb felső sarkában ☰ gomb**. Rákattintva a menü **egymás alatti** elemekkel **lenyílik**, újabb kattintásra **eltűnik** – a `script.js`-ben egy **`active` osztály ki-be kapcsolásával**. A hero **380 px** magas, a kártyák és a lábléc oszlopai **egymás alá** kerülnek.

### CSS – a `style.css` VÉGÉRE (a tablet rész UTÁN!):
```css
/* ===== Mobil: 600 px alatt ===== */
@media (max-width: 600px) {
    .fejlec {
        text-align: left;
    }

    .fejlec h1 {
        font-size: 1.4rem;
    }

    .hamburger {
        display: block;
    }

    .navbar {
        display: none;
    }

    .navbar.active {
        display: grid;
    }

    .nav_list {
        flex-direction: column;
        align-items: center;
        gap: 8px;
    }

    .log {
        flex-direction: column;
        align-items: center;
    }

    .hero {
        height: 380px;
    }

    .herocon {
        margin: 0 20px;
    }

    .herocon h2 {
        font-size: 1.8rem;
    }

    .about_us,
    .fav {
        width: 90%;
    }

    .boxgrid,
    .cont {
        grid-template-columns: 1fr;
    }
}
```

⚠ **A 600-as rész a 1000-es UTÁN legyen!** A CSS-ben a később írt szabály nyer. Ha fordítva írod, a mobilon a tablet-szabályok felülírják a mobilosakat.

**Mit jelent?**
| CSS | Mit csinál |
|---|---|
| `.hamburger { display: block; }` | a ☰ gomb **megjelenik** (eddig `none` volt) |
| `.fejlec { text-align: left; }` | a cím balra, hogy a gomb jobbra elférjen |
| `.navbar { display: none; }` | a menüsor **el van rejtve** |
| `.navbar.active { display: grid; }` | ⭐ HA a menüsoron rajta van az `active` osztály, **megjelenik** – ezt a JS kapcsolja |
| `.nav_list { flex-direction: column; }` + `.log { flex-direction: column; }` | lenyitva az elemek **egymás alatt** |
| `.hero { height: 380px; }` | a hero 380 px (a feladat kéri) |
| `.boxgrid, .cont { grid-template-columns: 1fr; }` | kártyák és lábléc **egy oszlopban** (egymás alatt) |

A ☰ gomb helyét a fejléc CSS-ében már beállítottuk (3. lépés): `position: absolute; right: 20px; top: 50%;` → a fejléc jobb szélén, függőlegesen középen.

### JavaScript – a `script.js`-be:
```js
// Hamburger menü: kattintásra az "active" osztály ki-be kapcsolása a menüsoron
const hamburger = document.getElementById("hamburger");
const navbar = document.getElementById("navbar");

hamburger.addEventListener("click", function () {
    navbar.classList.toggle("active");
});
```

**Mit jelent soronként?**
1. `document.getElementById("hamburger")` → megkeresi a ☰ gombot az `id`-ja alapján.
2. `document.getElementById("navbar")` → megkeresi a menüsort.
3. `addEventListener("click", ...)` → „ha rákattintanak a gombra, csináld ezt”.
4. `navbar.classList.toggle("active")` → **ki-be kapcsolja** az `active` osztályt: ha nincs rajta → rárakja (menü megjelenik), ha rajta van → leveszi (menü eltűnik). **Pontosan ezt kéri a feladat.**

✅ **Teszt:** állíts **400 px**-et → felül a cím balra, jobbra a ☰. A menü nem látszik. Kattints a ☰-ra → lenyílik egymás alatti elemekkel, a gombok középen. Újra kattintás → eltűnik. A hero alacsonyabb, a kártyák és a lábléc oszlopai egymás alatt.

❌ Ha nem nyílik le: F12 → Console → piros hiba? Valószínűleg elírt `id` (a HTML-ben `id="hamburger"` és `id="navbar"` legyen), vagy hiányzik a `<script src="script.js">` sor.

---

## 10. Mobilnézet kipróbálása

A böngészőben (Chrome / Edge):
1. **F12** (fejlesztői eszközök).
2. **Ctrl+Shift+M** (vagy a bal felső „telefon-tablet” ikon) → felül megjelenik egy sáv.
3. A sávon a *Dimensions* mellé írd be a szélességet: **1400**, **820**, **400** → hasonlítsd össze a feladatlap mintáival.
4. A ☰ gombra a kattintást itt is ki tudod próbálni.

Vagy egyszerűen: húzd keskenyre a böngészőablakot.

---

## 11. A KÉSZ fájlok egyben

Ha elakadtál, vagy egyben akarod bemásolni (a 2–9. lépés együtt):

### index.html
```html
<!DOCTYPE html>
<html lang="hu">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Babszem Kávézó</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <header>
        <div class="fejlec">
            <h1>Babszem Kávézó</h1>
            <button id="hamburger" class="hamburger" aria-label="Menü">☰</button>
        </div>
        <nav class="navbar" id="navbar">
            <div class="nav_list">
                <a href="#">Kezdőlap</a>
                <a href="#">Kínálat</a>
                <a href="#">Üzleteink</a>
                <a href="#">Kapcsolat</a>
            </div>
            <div class="log">
                <button id="login">Bejelentkezés</button>
                <button id="signup">Regisztráció</button>
            </div>
        </nav>
    </header>

    <main>
        <section class="hero">
            <div class="herocon">
                <h2>Minden nap egy jó kávéval kezdődik</h2>
                <p>Frissen pörkölt kávé, házi sütemények és nyugodt sarkok négy városban.</p>
                <button>Nézd meg a kínálatot</button>
            </div>
        </section>

        <section class="about_us">
            <h2>Rólunk</h2>
            <div class="one_two">
                <img src="kepek/rolunk1.jpg" alt="Kávébabok">
                <article class="textbox">
                    <p>A Babszem 2016-ban egy aprócska budapesti pultként indult, ahol a reggeli kávé mellé mindig jutott egy jó szó is. Ma négy városban várunk, de a filozófiánk nem változott: minden csésze számít. A kávébabot közvetlenül kis ültetvényekről szerezzük be, és saját pörkölőnkben, kis adagokban pörköljük, hogy minden fajta a legjobb arcát mutassa. Nálunk nincs futószalag – a baristáink ismerik a törzsvendégek kedvencét, és szívesen mesélnek arról, honnan érkezett a csészédben lévő kávé.</p>
                </article>
            </div>
            <div class="two_one">
                <article class="textbox">
                    <p>Hiszünk abban, hogy egy kávézó több, mint egy hely, ahol kávét iszunk. Nálunk lehet tanulni, dolgozni, beszélgetni vagy csak csendben nézni az esőt az ablakon át. A süteményeinket minden reggel helyben sütjük, a tejet környékbeli gazdaságoktól vásároljuk, a zaccot pedig a közeli közösségi kerteknek adjuk tovább komposztnak. Ha szeretnéd megtanulni, hogyan készül a tökéletes cappuccino, nézz be egy szombati barista-kurzusunkra!</p>
                </article>
                <img src="kepek/rolunk2.jpg" alt="Két csésze kávé latte arttal">
            </div>
        </section>

        <section class="fav">
            <h2>Kedvenceink</h2>
            <div class="boxgrid">
                <article class="box">
                    <img src="kepek/kave1.jpg" alt="Cappuccino">
                    <div class="boxtext">
                        <h3>Cappuccino</h3>
                        <p>Selymes tejhab, dupla espresso, latte art szívvel.</p>
                        <p class="ar">890 Ft</p>
                    </div>
                </article>
                <article class="box">
                    <img src="kepek/kave2.jpg" alt="Espresso">
                    <div class="boxtext">
                        <h3>Espresso</h3>
                        <p>Házi blendünk sűrű, mogyorós krémával.</p>
                        <p class="ar">590 Ft</p>
                    </div>
                </article>
                <article class="box">
                    <img src="kepek/kave3.jpg" alt="Flat white">
                    <div class="boxtext">
                        <h3>Flat white</h3>
                        <p>Kevesebb hab, több kávé – a baristák kedvence.</p>
                        <p class="ar">990 Ft</p>
                    </div>
                </article>
            </div>
        </section>
    </main>

    <footer>
        <div class="cont">
            <div>
                <h3>Címünk</h3>
                <p>1052 Budapest, Váci utca 18.</p>
            </div>
            <div>
                <h3>Nyitvatartás</h3>
                <p>Hétfő–Péntek: 7:00–19:00</p>
                <p>Szombat–Vasárnap: 8:00–18:00</p>
            </div>
            <div>
                <h3>Elérhetőség</h3>
                <p>+36 1 234 5678</p>
                <p>hello@babszem.hu</p>
            </div>
        </div>
        <p class="copy">© 2026 Babszem Kávézó – Minden csésze számít.</p>
    </footer>

    <script src="script.js"></script>
</body>

</html>
```

### style.css
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root {
    --sotet: #2b1d16;
    --krem: #f5ebe0;
    --karamell: #c08552;
    --tejes: #e6d5c3;
}

body {
    font-family: Arial, sans-serif;
    background-color: var(--krem);
    color: var(--sotet);
    line-height: 1.6;
}

h1,
h2,
h3 {
    font-family: Georgia, serif;
}

/* ===== Fejléc ===== */
header {
    background-color: var(--sotet);
}

.fejlec {
    position: relative;
    text-align: center;
    padding: 25px 20px;
}

.fejlec h1 {
    color: white;
    font-size: 1.8rem;
}

.hamburger {
    display: none;
    position: absolute;
    top: 50%;
    right: 20px;
    transform: translateY(-50%);
    background: none;
    color: white;
    border: 2px solid white;
    border-radius: 8px;
    font-size: 1.4rem;
    padding: 0 10px;
    cursor: pointer;
}

/* ===== Menüsor ===== */
.navbar {
    background-color: var(--karamell);
    display: grid;
    grid-template-columns: 2fr 1fr;
    align-items: center;
    padding: 12px 20px;
}

.nav_list {
    display: flex;
    justify-content: space-evenly;
}

.nav_list a {
    text-decoration: none;
    color: var(--sotet);
    font-weight: bold;
    font-size: 0.9rem;
}

.log {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
}

.log button {
    padding: 8px 18px;
    border: 2px solid var(--sotet);
    border-radius: 30px;
    font-weight: bold;
    cursor: pointer;
}

#login {
    background-color: var(--krem);
    color: var(--sotet);
}

#signup {
    background-color: var(--sotet);
    color: white;
}

/* ===== Hero ===== */
.hero {
    height: 600px;
    background-image: linear-gradient(to right, rgba(43, 29, 22, 0.9), rgba(43, 29, 22, 0)), url(kepek/hero.jpg);
    background-size: cover;
    background-position: center;
    display: flex;
    align-items: center;
}

.herocon {
    margin-left: 8%;
    max-width: 500px;
    color: white;
}

.herocon h2 {
    font-size: 2.6rem;
    line-height: 1.2;
}

.herocon p {
    margin-top: 15px;
}

.herocon button {
    margin-top: 25px;
    padding: 12px 24px;
    border: none;
    border-radius: 30px;
    background-color: var(--karamell);
    color: white;
    font-weight: bold;
    cursor: pointer;
}

/* ===== Rólunk ===== */
.about_us,
.fav {
    width: 80%;
    margin: 0 auto;
}

.about_us h2,
.fav h2 {
    text-align: center;
    font-size: 2rem;
    padding: 40px 0 25px;
}

.one_two,
.two_one {
    display: grid;
    gap: 20px;
    margin-bottom: 20px;
}

.one_two {
    grid-template-columns: 1fr 2fr;
}

.two_one {
    grid-template-columns: 2fr 1fr;
}

.one_two img,
.two_one img {
    width: 100%;
    height: 100%;
    min-height: 250px;
    object-fit: cover;
    border-radius: 20px;
}

.textbox {
    display: flex;
    align-items: center;
    padding: 25px;
    background-color: var(--tejes);
    border: 2px solid var(--karamell);
    border-radius: 20px;
}

/* ===== Kedvenceink ===== */
.fav {
    padding-bottom: 50px;
}

.boxgrid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 25px;
}

.box {
    background-color: white;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
    transition: transform 0.3s;
}

.box:hover {
    transform: translateY(-8px);
}

.box img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    display: block;
}

.boxtext {
    padding: 20px;
}

.boxtext p {
    margin-top: 8px;
}

.ar {
    color: var(--karamell);
    font-weight: bold;
}

/* ===== Lábléc ===== */
footer {
    background-color: var(--sotet);
    color: white;
    padding: 40px 0 20px;
}

.cont {
    width: 80%;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    padding-bottom: 25px;
    border-bottom: 1px solid var(--karamell);
}

.cont h3 {
    color: var(--karamell);
    margin-bottom: 10px;
}

.copy {
    text-align: center;
    padding-top: 20px;
    font-size: 0.85rem;
}

/* ===== Tablet: 1000 px alatt ===== */
@media (max-width: 1000px) {
    .navbar {
        grid-template-columns: 1fr;
        gap: 12px;
    }

    .log {
        justify-content: center;
    }

    .one_two,
    .two_one {
        grid-template-columns: 1fr;
    }

    /* a második sorban is a kép legyen felül */
    .two_one img {
        order: -1;
    }

    .one_two img,
    .two_one img {
        height: 300px;
    }

    .boxgrid {
        grid-template-columns: repeat(2, 1fr);
    }
}

/* ===== Mobil: 600 px alatt ===== */
@media (max-width: 600px) {
    .fejlec {
        text-align: left;
    }

    .fejlec h1 {
        font-size: 1.4rem;
    }

    .hamburger {
        display: block;
    }

    .navbar {
        display: none;
    }

    .navbar.active {
        display: grid;
    }

    .nav_list {
        flex-direction: column;
        align-items: center;
        gap: 8px;
    }

    .log {
        flex-direction: column;
        align-items: center;
    }

    .hero {
        height: 380px;
    }

    .herocon {
        margin: 0 20px;
    }

    .herocon h2 {
        font-size: 1.8rem;
    }

    .about_us,
    .fav {
        width: 90%;
    }

    .boxgrid,
    .cont {
        grid-template-columns: 1fr;
    }
}
```

### script.js
```js
// Hamburger menü: kattintásra az "active" osztály ki-be kapcsolása a menüsoron
const hamburger = document.getElementById("hamburger");
const navbar = document.getElementById("navbar");

hamburger.addEventListener("click", function () {
    navbar.classList.toggle("active");
});
```

---

## 12. Ha más a feladat: mit cserélj?

**Más téma** is lehet (pl. virágbolt, edzőterem), de a szerkezet általában ugyanez: **fejléc + menü, hero, bemutatkozó rész, kártyák, lábléc, + reszponzivitás + hamburger**.

| Mit | Hol | Mire |
|---|---|---|
| színek | `:root` | a feladatlap táblázatából (név + kód). Ha más a változó neve (pl. `--fo-szin`), **mindenhol** cseréld (Ctrl+H) |
| betűtípusok | `body`, `h1, h2, h3` | a feladatlap szerint |
| szövegek | `index.html` | a kapott `.txt`-ből **másold** |
| képek | `src="kepek/..."`, `url(kepek/...)` | a kapott fájlnevek (kis-nagybetű is számít!) |
| `alt` szövegek | minden `<img>` | rövid leírás a képről |
| számok | 600px, 30px, 20px, 200px, 80%, 2fr 1fr, 1fr 2fr | **pontosan** a feladatlap szerint |
| töréspontok | `@media (max-width: 1000px)`, `(max-width: 600px)` | a feladatlap szerinti szélességek |
| hány kártya / oszlop | `repeat(3, 1fr)` | ha 4 kártya kell: `repeat(4, 1fr)` |
| menüpontok száma | `<a href="#">` sorok | annyi, amennyi a mintán |

**Tipp:** a feladatlap minden mondatát olvasd végig, és **húzd alá a számokat és a CSS-szavakat** (grid, flex, border-radius, object-fit, transform, linear-gradient…). Mindegyikhez kell egy sor a CSS-ben – ezekért jár a pont.

---

## 13. Végső ellenőrzés

Pipáld ki leadás előtt. Zárójelben a pont.

**Szerkezet (1)**
- [ ] `header`, `nav`, `main`, `section`, `article`, `footer` használva
- [ ] `lang="hu"`, `charset="UTF-8"`, `viewport`, `<title>` = az oldal neve
- [ ] **minden** `<img>`-nek van `alt`-ja
- [ ] 3 fájl: `index.html`, `style.css`, `script.js` + mellettük a `kepek` mappa
- [ ] színek `:root`-ban változóként; címek Georgia, szöveg Arial

**Fejléc (2)**
- [ ] sötét háttér, középre igazított főcím
- [ ] karamell menüsor, `grid-template-columns: 2fr 1fr`
- [ ] menüpontok `flex`-szel egyenletesen
- [ ] 2 gomb `border-radius: 30px`, a Regisztráció sötét + fehér betű

**Hero (1)**
- [ ] 600px, `hero.jpg`, `cover`, `center`, `linear-gradient` balról jobbra halványuló
- [ ] bal oldalt cím, alcím, karamell lekerekített gomb

**Rólunk (2)**
- [ ] 80% széles, középen
- [ ] 1. sor `1fr 2fr`, 2. sor `2fr 1fr`
- [ ] képek `object-fit: cover`
- [ ] dobozok 20px kerekítés, karamell keret, tejes háttér

**Kedvenceink (1)**
- [ ] 3 egyforma oszlop (grid), fehér, lekerekített, árnyékos kártyák
- [ ] kép 200px magas; hover → `transform` emelkedés

**Lábléc (1)**
- [ ] sötét háttér, 3 egyforma oszlop, karamell címek
- [ ] vékony karamell vonal, középre igazított © sor

**Reszponzivitás (2)**
- [ ] 1000px alatt: menüsor 1 oszlop, gombok középen; Rólunk egymás alatt, **mindkét sorban kép felül**; kártyák 2 oszlop
- [ ] 600px alatt: menüsor rejtett, ☰ jobb felül; kattintásra lenyílik/eltűnik (`active` osztály a `script.js`-ben); hero 380px; kártyák és lábléc egymás alatt

**És végül:**
- [ ] kipróbáltad 1400 / 820 / 400 px-en, és hasonlít a mintákra
- [ ] F12 → Console: nincs piros hiba

---

## 14. Ha valami nem úgy néz ki

**Első lépés:** F12 → **Elements** fül → kattints a bal felső nyílra, majd az oldalon arra az elemre, ami rossz → jobb oldalt látod, milyen CSS szabályok hatnak rá (az áthúzott = felülírták).

| Amit látsz | Ok | Megoldás |
|---|---|---|
| semmi stílus, fehér oldal, Times betű | a CSS nincs bekötve | `<link rel="stylesheet" href="style.css">` a `<head>`-ben; a fájlnév pontosan `style.css` |
| nem jelennek meg a képek | rossz útvonal | `src="kepek/kave1.jpg"` – nincs `/` az elején, a fájlnév egyezik (kis-nagybetű!) |
| a hero háttérképe nem látszik | rossz útvonal a CSS-ben | `url(kepek/hero.jpg)`, és a gradient van **elöl**, vesszővel |
| `Ã¡` az `á` helyett | hiányzik a charset | `<meta charset="UTF-8">` |
| mobilon pici az egész, nem vált át | hiányzik a viewport | `<meta name="viewport" ...>` |
| a mobilnézet nem működik | rossz sorrend | a `600px`-es `@media` a `1000px`-es **után** legyen |
| a 2. Rólunk sorban tableten a szöveg van felül | hiányzik az `order: -1` | `.two_one img { order: -1; }` a 1000px-es részben |
| a ☰ gomb asztali nézetben is látszik | hiányzik a `display: none` | `.hamburger { display: none; }` az alap CSS-ben |
| a ☰-ra kattintva nem nyílik le | JS hiba / elírt id / nincs `.navbar.active` szabály | F12 Console; `id="hamburger"`, `id="navbar"`; a 600px-es részben `.navbar.active { display: grid; }` |
| a menüpontok aláhúzottak, kékek | alap link-stílus | `text-decoration: none; color: var(--sotet);` |
| a kártyák nem egyforma szélesek | flex-et használtál grid helyett | `display: grid; grid-template-columns: repeat(3, 1fr);` |
| a hover ugrál, nem finom | a `transition` rossz helyen | a `transition` az alap `.box`-ra kell, nem a `:hover`-re |
| vízszintes görgetősáv mobilon | valami fix szélességű | `box-sizing: border-box` a `*`-on; `max-width` a `width` helyett |
| a lábléc oszlopai összekeveredtek | a címek és szövegek nincsenek oszloponként `<div>`-be csomagolva | minden oszlop: `<div><h3>…</h3><p>…</p></div>` |
