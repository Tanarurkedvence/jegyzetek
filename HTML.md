# HTML összefoglaló

[← vissza](README.md)

## Tartalom
1. [Alapváz (mindig ezzel kezdj!)](#1-alapváz)
2. [Szemantikus elemek](#2-szemantikus-elemek)
3. [Szöveg elemek](#3-szöveg-elemek)
4. [Link, kép, média](#4-link-kép-média)
5. [Listák](#5-listák)
6. [Táblázat](#6-táblázat)
7. [Űrlapok](#7-űrlapok)
8. [Attribútumok: id, class, data-](#8-attribútumok)
9. [Speciális karakterek](#9-speciális-karakterek)
10. [Teljes oldalsablon (fejléc, hero, rólunk, kártyák, lábléc)](#10-teljes-oldalsablon)
11. [Ellenőrzőlista leadás előtt](#11-ellenőrzőlista)

---

## 1. Alapváz

VS Code-ban: üres `.html` fájlba írd be, hogy `!` majd **Tab** → kész az alapváz.

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

    <script src="script.js"></script>
</body>
</html>
```

⚠ Amit a feladat pontoz:
- `lang="hu"` (NE `en`!)
- `charset="UTF-8"`
- `viewport` meta (e nélkül nem működik a mobilnézet)
- `title` a **valódi oldalnév** (NE „Document”)
- a `<script>` a `</body>` elé, VAGY a head-be `defer`-rel: `<script src="script.js" defer></script>`

---

## 2. Szemantikus elemek

| Elem | Mire |
|---|---|
| `<header>` | fejléc (logó, főcím, menü) |
| `<nav>` | navigáció / menü |
| `<main>` | a fő tartalom (oldalanként **egy**) |
| `<section>` | tematikus szakasz (Rólunk, Kedvenceink…), általában van címe |
| `<article>` | önálló tartalom (kártya, hír, blogposzt, szövegdoboz) |
| `<aside>` | mellékes tartalom, oldalsáv |
| `<footer>` | lábléc |
| `<figure>` + `<figcaption>` | kép képaláírással |
| `<address>` | elérhetőség |
| `<time datetime="2026-10-12">` | dátum |
| `<div>` | általános doboz (ha egyik sem illik) |
| `<span>` | általános soron belüli elem |

```html
<header>
    <h1>Babszem Kávézó</h1>
    <nav> ... </nav>
</header>
<main>
    <section class="hero"> ... </section>
    <section class="rolunk">
        <h2>Rólunk</h2>
        <article> ... </article>
    </section>
</main>
<footer> ... </footer>
```

---

## 3. Szöveg elemek

```html
<h1>Főcím</h1>       <!-- oldalanként lehetőleg egy -->
<h2>Szakaszcím</h2>
<h3>Alcím</h3>        <!-- h4, h5, h6 -->

<p>Bekezdés.</p>
<br>                  <!-- sortörés (ne térközre használd, arra a CSS margin!) -->
<hr>                  <!-- vízszintes vonal -->

<strong>fontos (félkövér)</strong>
<em>hangsúlyos (dőlt)</em>
<b>félkövér</b> <i>dőlt</i> <u>aláhúzott</u>
<mark>kiemelt</mark>
<small>apró betű</small>
<sub>alsó index</sub> <sup>felső index</sup>  <!-- H<sub>2</sub>O, m<sup>2</sup> -->
<blockquote>Idézet</blockquote>
<code>kód</code>
<pre>előformázott   szöveg (megtartja a szóközöket)</pre>
<abbr title="HyperText Markup Language">HTML</abbr>
```

---

## 4. Link, kép, média

```html
<a href="https://example.com">Külső link</a>
<a href="https://example.com" target="_blank">Új lapon</a>
<a href="#rolunk">Ugrás a Rólunk szakaszhoz</a>   <!-- <section id="rolunk"> -->
<a href="#">Üres link (menühöz)</a>
<a href="mailto:hello@babszem.hu">Email</a>
<a href="tel:+3612345678">Telefon</a>
<a href="aloldal.html">Másik oldal</a>

<img src="kepek/kave1.jpg" alt="Cappuccino">
<!-- az alt KÖTELEZŐ (pontot ér)! Útvonal relatív, NEM /kepek/... -->

<figure>
    <img src="kepek/hero.jpg" alt="Kávéscsésze">
    <figcaption>Frissen főzött kávé</figcaption>
</figure>

<video src="video.mp4" controls width="400"></video>
<audio src="hang.mp3" controls></audio>

<iframe src="https://www.google.com/maps/embed?..." width="600" height="450"
        style="border:0;" loading="lazy"></iframe>
```

Útvonalak:
- `kepek/a.jpg` – az aktuális mappában lévő `kepek` mappában
- `../kepek/a.jpg` – egy mappával feljebb
- `/kepek/a.jpg` – a szerver gyökeréből → **fájlként megnyitva NEM megy, kerüld!**

---

## 5. Listák

```html
<ul>                <!-- pöttyös -->
    <li>Espresso</li>
    <li>Cappuccino</li>
</ul>

<ol>                <!-- számozott; type="a", start="5" -->
    <li>Első</li>
    <li>Második</li>
</ol>

<dl>                <!-- leírás lista -->
    <dt>Espresso</dt>
    <dd>Erős feketekávé</dd>
</dl>
```

Menü listával (gyakori minta):
```html
<nav>
    <ul>
        <li><a href="#">Kezdőlap</a></li>
        <li><a href="#">Kínálat</a></li>
    </ul>
</nav>
```

---

## 6. Táblázat

```html
<table>
    <caption>Árlista</caption>
    <thead>
        <tr>
            <th>Termék</th>
            <th>Ár</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Espresso</td>
            <td>590 Ft</td>
        </tr>
        <tr>
            <td colspan="2">Két oszlopot foglal</td>
        </tr>
        <tr>
            <td rowspan="2">Két sort foglal</td>
            <td>...</td>
        </tr>
    </tbody>
    <tfoot>
        <tr><td>Összesen</td><td>1480 Ft</td></tr>
    </tfoot>
</table>
```

---

## 7. Űrlapok

```html
<form id="urlap" action="mentes.php" method="post">
    <label for="nev">Név</label>
    <input type="text" id="nev" name="nev" placeholder="Pl. Kiss Anna" required>

    <!-- vagy label-be csomagolva, akkor nem kell for/id: -->
    <label>Email <input type="email" name="email" required></label>

    <input type="password" name="jelszo" minlength="8">
    <input type="number" name="ar" min="0" step="1" value="0">
    <input type="date" name="datum">
    <input type="time" name="ido">
    <input type="tel" name="telefon" pattern="[0-9]{9,11}">
    <input type="url" name="weboldal">
    <input type="color" name="szin">
    <input type="range" name="mennyiseg" min="0" max="10">
    <input type="file" name="kep" accept="image/*">
    <input type="search" name="kereses">
    <input type="hidden" name="id" value="5">

    <!-- egy választható -->
    <label><input type="radio" name="fizetes" value="Cash" checked> Készpénz</label>
    <label><input type="radio" name="fizetes" value="Card"> Kártya</label>

    <!-- több választható -->
    <label><input type="checkbox" name="kategoria[]" value="1"> Szemes kávé</label>
    <label><input type="checkbox" name="kategoria[]" value="5"> Bio</label>

    <select name="varos">
        <option value="">-- Válassz --</option>
        <option value="bp" selected>Budapest</option>
        <option value="db">Debrecen</option>
    </select>

    <textarea name="uzenet" rows="4" cols="40"></textarea>

    <fieldset>
        <legend>Szállítás</legend>
        ...
    </fieldset>

    <button type="submit">Küldés</button>
    <button type="reset">Törlés</button>
    <button type="button">Sima gomb (nem küld)</button>
</form>
```

Hasznos attribútumok: `required`, `placeholder`, `min`, `max`, `minlength`, `maxlength`, `pattern`, `readonly`, `disabled`, `checked`, `selected`, `autofocus`, `value`.

⚠ PHP-nak csak a `name` attribútummal rendelkező mezők mennek át!

---

## 8. Attribútumok

```html
<div id="fejlec">               <!-- egyedi, egyszer az oldalon (JS getElementById) -->
<div class="doboz kiemelt">     <!-- több osztály szóközzel; ismételhető -->
<div style="color: red;">       <!-- inline stílus (kerüld, inkább CSS) -->
<button data-id="5">            <!-- saját adat → JS: gomb.dataset.id -->
<div hidden>                    <!-- rejtett -->
<button aria-label="Menü">☰</button>  <!-- akadálymentesség -->
<img loading="lazy">            <!-- lusta betöltés -->
<a title="Tooltip szöveg">
```

---

## 9. Speciális karakterek

| Kód | Jel |
|---|---|
| `&lt;` `&gt;` | < > |
| `&amp;` | & |
| `&nbsp;` | nem törő szóköz |
| `&copy;` | © |
| `&ndash;` `&mdash;` | – — |
| `&quot;` | " |
| `&#9776;` | ☰ (hamburger) |
| `&times;` | × (bezárás) |
| `&rarr;` | → |
| `&euro;` | € |

---

## 10. Teljes oldalsablon

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
                <p>Alcím szöveg.</p>
                <button>Nézd meg a kínálatot</button>
            </div>
        </section>

        <section class="rolunk">
            <h2>Rólunk</h2>
            <div class="sor1">
                <img src="kepek/rolunk1.jpg" alt="Kávébabok">
                <article class="szovegdoboz"><p>Szöveg...</p></article>
            </div>
            <div class="sor2">
                <article class="szovegdoboz"><p>Szöveg...</p></article>
                <img src="kepek/rolunk2.jpg" alt="Két csésze kávé">
            </div>
        </section>

        <section class="kedvencek">
            <h2>Kedvenceink</h2>
            <div class="kartyak">
                <article class="kartya">
                    <img src="kepek/kave1.jpg" alt="Cappuccino">
                    <div class="kartya-szoveg">
                        <h3>Cappuccino</h3>
                        <p>Leírás.</p>
                        <p class="ar">890 Ft</p>
                    </div>
                </article>
                <!-- további kártyák ... -->
            </div>
        </section>
    </main>

    <footer>
        <div class="lablec">
            <div>
                <h3>Címünk</h3>
                <p>1052 Budapest, Váci utca 18.</p>
            </div>
            <div>
                <h3>Nyitvatartás</h3>
                <p>Hétfő–Péntek: 7:00–19:00</p>
            </div>
            <div>
                <h3>Elérhetőség</h3>
                <p>+36 1 234 5678</p>
            </div>
        </div>
        <p class="copy">© 2026 Babszem Kávézó – Minden csésze számít.</p>
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

> Lábléc tipp: minden oszlop legyen egy saját `<div>`-ben (cím + sorai), így a grid 3 oszlopa magától jó lesz, mobilon pedig szépen egymás alá kerül.

---

## 11. Ellenőrzőlista

- [ ] `lang="hu"`, `charset`, `viewport`, rendes `title`
- [ ] szemantikus elemek: `header`, `nav`, `main`, `section`, `article`, `footer`
- [ ] **minden** `<img>`-nek van `alt`
- [ ] relatív útvonalak (`kepek/...`), a fájlnevek kis/nagybetűre is egyeznek
- [ ] minden `id` csak egyszer szerepel
- [ ] a szövegeket a megadott `.txt`-ből másoltad (ne gépeld újra!)
- [ ] a fájlnevek pontosan a kért nevek (`index.html`, `style.css`, `script.js`)
- [ ] F12 → Console: nincs piros hiba
- [ ] kipróbáltad a megadott szélességeken (Ctrl+Shift+M)
