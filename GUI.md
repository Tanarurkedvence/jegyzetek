# C# grafikus (GUI, WinForms) összefoglaló

[← vissza](README.md) · Kész megoldások: LaptopsGUI, MenhelyGUI, PizzaGUI, EremtablaGUI, FesztivalGUI, ParalimpiaGUI, VarakGUI, AutoGUI

## Tartalom
1. [Projekt létrehozása](#1-projekt-létrehozása)
2. [Más projekt osztályának használata (a CLI osztálya a GUI-ban)](#2-más-projekt-osztálya)
3. [A Form1.cs váza](#3-a-form1cs-váza)
4. [Események bekötése](#4-események-bekötése)
5. [Beolvasás és feltöltés (Read / Write)](#5-beolvasás-és-feltöltés)
6. [Label, TextBox, RichTextBox](#6-label-textbox-richtextbox)
7. [ListBox](#7-listbox)
8. [ComboBox (legördülő)](#8-combobox)
9. [DataGridView (táblázat)](#9-datagridview)
10. [RadioButton, CheckBox, GroupBox](#10-radiobutton-checkbox)
11. [NumericUpDown, DateTimePicker, TrackBar](#11-numericupdown-datetimepicker)
12. [Button, MessageBox (kérdés, kilépés)](#12-button-messagebox)
13. [Keresés gépelés közben (szűrés)](#13-keresés-gépelés-közben)
14. [Fájlba mentés gombra](#14-fájlba-mentés-gombra)
15. [Számlálók (hány elemet nézett meg)](#15-számlálók)
16. [Kép, PictureBox, színek, betűk](#16-kép-színek)
17. [Bemenet ellenőrzése](#17-bemenet-ellenőrzése)
18. [Teljes minta Form1.cs](#18-teljes-minta)
19. [Tipikus hibák](#19-tipikus-hibák)

---

## 1. Projekt létrehozása

Visual Studio → *Új projekt* → **Windows Forms App** (C#, *nem* a .NET Framework-ös!) → név (pl. `LaptopsGUI`).

- **Toolbox** (Ctrl+Alt+X): innen húzod a vezérlőket a formra.
- **Properties** (F4): `(Name)` – ezen a néven éred el kódból; `Text` – a felirata.
- Kódnézet: jobb klikk a formon → *View Code* (F7). Tervező: Shift+F7.
- A `forras` mappa ugyanúgy a projektmappába kerül → `../../../forras/laptops.txt`.

Hasznos Properties:
| Tulajdonság | Mire |
|---|---|
| `(Name)` | változónév a kódban (pl. `nevLabel`, `keresBox`) |
| `Text` | felirat (a form címsora is!) |
| `Font` | betű, méret, félkövér |
| `ForeColor` / `BackColor` | szöveg- / háttérszín |
| `Enabled` | kattintható-e |
| `Visible` | látszik-e |
| `ReadOnly` | TextBox-nál csak olvasható |
| `Dock` / `Anchor` | átméretezéskor hogyan viselkedjen |
| `StartPosition` = CenterScreen | a form középen nyíljon |
| `FormBorderStyle` = FixedSingle, `MaximizeBox` = False | ne lehessen átméretezni |

---

## 2. Más projekt osztálya

Ha a GUI ugyanazt az osztályt használja, mint a CLI (pl. `Kutya`):

**A) legegyszerűbb:** másold át a `Kutya.cs`-t a GUI projektbe, és a fájl tetején írd át a `namespace`-t `MenhelyGUI`-ra.

**B) hivatkozással:** Solution Explorer → GUI projekt → jobb klikk *Dependencies* → *Add Project Reference* → pipáld be a CLI projektet. Az osztály legyen `public class Kutya`, a Form1.cs tetejére:
```csharp
using MenhelyCLI;
```

---

## 3. A Form1.cs váza

```csharp
namespace LaptopsGUI
{
    public partial class Form1 : Form
    {
        List<Laptop> laptops = new List<Laptop>();

        public Form1()
        {
            InitializeComponent();
            // események bekötése ide (lásd 4.)
            this.Load += Form1_Load;
            button1.Click += Button1_Click;
            listBox1.SelectedIndexChanged += ListBox1_SelectedIndexChanged;
        }

        private void Form1_Load(object? sender, EventArgs e)
        {
            Read();
            Write();
        }

        private void Read() { /* fájl → lista */ }
        private void Write() { /* lista → vezérlők */ }
    }
}
```

---

## 4. Események bekötése

**A) Tervezőben:** dupla klikk a vezérlőn → létrehozza a fő eseményét (gomb → Click, ListBox → SelectedIndexChanged). Vagy Properties → villám ikon ⚡ → az esemény mellé dupla klikk.

**B) Kódból** (a konstruktorban, `InitializeComponent()` UTÁN):
```csharp
this.Load += Form1_Load;
button1.Click += Button1_Click;
listBox1.SelectedIndexChanged += ListBox1_SelectedIndexChanged;
comboBox1.SelectedIndexChanged += ComboBox1_SelectedIndexChanged;
textBox1.TextChanged += TextBox1_TextChanged;
dataGridView1.CellClick += DataGridView1_CellClick;
radioButton1.CheckedChanged += Szures;
checkBox1.CheckedChanged += (s, e) => Szures();        // lambda – rövid
numericUpDown1.ValueChanged += NumericUpDown1_ValueChanged;
this.FormClosing += Form1_FormClosing;
```

Írd be, hogy `button1.Click +=`, és nyomj **Tab**-ot → a VS legenerálja a metódust.

Eseménykezelő formája:
```csharp
private void Button1_Click(object? sender, EventArgs e)
{
}
```

Egy kezelő több vezérlőhöz:
```csharp
radioButton1.CheckedChanged += Szures;
radioButton2.CheckedChanged += Szures;
private void Szures(object? sender, EventArgs e) { ... }
```

---

## 5. Beolvasás és feltöltés

```csharp
private void Read()
{
    using (StreamReader o = new StreamReader("../../../forras/laptops.txt"))
    {
        o.ReadLine();                     // fejléc
        while (!o.EndOfStream)
        {
            laptops.Add(new Laptop(o.ReadLine()));
        }
    }
}

private void Write()
{
    foreach (var laptop in laptops)
    {
        listBox1.Items.Add(laptop.ToString());
    }
    listBox1.SelectedIndex = 0;           // az első legyen kijelölve (ha a feladat kéri)
}
```

Fejléc felirat számolt adattal:
```csharp
var gyartokSzama = laptops.Select(x => x.Manufacturer.ManufacturerName).Distinct().Count();
cimLabel.Text = $"VÁLASSZON {gyartokSzama} GYÁRTÓ {laptops.Count} LAPTOPJA KÖZÜL!";
```

---

## 6. Label, TextBox, RichTextBox

```csharp
label1.Text = "Szöveg";
label1.Text = $"{laptops[i].Price} Ft";
label1.ForeColor = Color.Red;

string beirt = textBox1.Text;
textBox1.Text = "";
textBox1.Clear();
int db = int.Parse(textBox1.Text);       // szám beolvasása (ellenőrzés: 17. pont)
textBox1.Focus();
textBox1.ReadOnly = true;

// RichTextBox – többsoros
richTextBox1.Text = $"Kategória\n\t{l.Category.CategoryName}\n" +
                    $"Processzor\n\t{l.CPU}\n" +
                    $"Ár\n\t{Math.Round(l.Price * 4.12)} Ft";
richTextBox1.AppendText("új sor\n");     // hozzáfűzés
richTextBox1.Text += "új sor\n";
```

TextBox többsoros: `Multiline = true` a Properties-ben.

---

## 7. ListBox

```csharp
listBox1.Items.Add("szöveg");
listBox1.Items.Add(laptop);                    // objektum is mehet → a ToString()-jét mutatja
listBox1.Items.Clear();
listBox1.Items.Count;
listBox1.SelectedIndex;                        // kijelölt sor indexe (-1, ha nincs)
listBox1.SelectedItem;                         // kijelölt elem (object)
listBox1.SelectedIndex = 0;                    // kijelölés kódból
listBox1.Items.Contains(gyarto);               // van-e már benne (egyedi lista építéséhez)

// a kiválasztott elem adatai
private void ListBox1_SelectedIndexChanged(object? sender, EventArgs e)
{
    int index = listBox1.SelectedIndex;
    if (index == -1) return;                    // nincs kijelölés → ne szálljon el
    nevLabel.Text = kutyak[index].Nev;
    korLabel.Text = $"{kutyak[index].Kor} év";
    allapotLabel.Text = kutyak[index].Orokbefogadva ? "Gazdára talált" : "Örökbefogadható";
}
```

⚠ Ha a ListBox egy **szűrt** listát mutat, akkor a `SelectedIndex` a szűrt listára vonatkozik, nem az eredetire! Megoldások:
```csharp
// 1) tárold a szűrt listát egy mezőben, és abból indexelj
List<Kutya> latszo = new List<Kutya>();
latszo = kutyak.Where(...).ToList();
var k = latszo[listBox1.SelectedIndex];

// 2) objektumot tegyél a ListBox-ba, és azt kérd vissza
listBox1.Items.Add(kutya);
var k = (Kutya)listBox1.SelectedItem;
```

Egyedi értékek (pl. gyártók) a ListBox-ba:
```csharp
foreach (var nev in autok.Select(x => x.Gyarto.GyartoName).Distinct().OrderBy(x => x))
{
    listBox1.Items.Add(nev);
}
```

---

## 8. ComboBox

```csharp
comboBox1.Items.Add("Budapest");
comboBox1.Items.AddRange(new string[] { "Manuális", "Automata" });
comboBox1.Items.Clear();
comboBox1.SelectedIndex = 0;                  // első kijelölve
comboBox1.SelectedIndex = -1;                 // semmi nincs kijelölve
string valasztott = comboBox1.SelectedItem?.ToString();
string szoveg = comboBox1.Text;

// csak választani lehessen, ne gépelni:
comboBox1.DropDownStyle = ComboBoxStyle.DropDownList;

// fajták a ComboBox-ba, kiválasztásra szűrt lista a ListBox-ba
private void Form1_Load(object? sender, EventArgs e)
{
    Read();
    foreach (var fajta in kutyak.Select(x => x.Fajta).Distinct())
    {
        comboBox1.Items.Add(fajta);
    }
}

private void ComboBox1_SelectedIndexChanged(object? sender, EventArgs e)
{
    string fajta = comboBox1.SelectedItem.ToString();
    listBox1.Items.Clear();
    foreach (var k in kutyak.Where(x => x.Fajta == fajta))
    {
        listBox1.Items.Add(k.ToString());
    }
}
```

Kötés adatforráshoz (rövidebb):
```csharp
comboBox1.DataSource = kutyak.Select(x => x.Fajta).Distinct().ToList();
```

---

## 9. DataGridView

**Oszlopok tervezőben:** a DataGridView jobb felső kis nyila → *Edit Columns* → *Add* (oszlopnév + fejléc szöveg).
Javasolt Properties: `AllowUserToAddRows = False`, `ReadOnly = True`, `SelectionMode = FullRowSelect`, `AutoSizeColumnsMode = Fill`, `RowHeadersVisible = False`.

```csharp
// sorok hozzáadása (az oszlopok SORRENDJÉBEN)
dataGridView1.Rows.Clear();
foreach (var o in orszagok)
{
    dataGridView1.Rows.Add(o.Nev, o.Arany, o.Ezust, o.Bronz, o.Osszeg);
}

// oszlopok kódból (ha nem a tervezőben csináltad)
dataGridView1.ColumnCount = 3;
dataGridView1.Columns[0].Name = "Ország";
dataGridView1.Columns[1].Name = "Arany";
dataGridView1.Columns[2].Name = "Összes";

// kattintás egy sorra
private void DataGridView1_CellClick(object? sender, DataGridViewCellEventArgs e)
{
    if (e.RowIndex < 0) return;             // a FEJLÉCRE kattintásnál -1 → kilépünk
    int sor = e.RowIndex;
    string nev = dataGridView1.Rows[sor].Cells[0].Value.ToString();
    var o = orszagok.First(x => x.Nev == nev);    // név alapján keresd vissza (szűrés miatt biztos!)
    nevLabel.Text = o.Nev;
    numericUpDown1.Value = o.Arany;
}

// kijelölt sor
var kijelolt = dataGridView1.CurrentRow;
dataGridView1.SelectedRows[0].Cells["Nev"].Value;

// cella színezése
dataGridView1.Rows[0].DefaultCellStyle.BackColor = Color.Gold;
```

Adatforrás-kötés (automatikus oszlopok a property-kből):
```csharp
dataGridView1.DataSource = orszagok.OrderByDescending(x => x.Arany).ToList();
```

---

## 10. RadioButton, CheckBox

RadioButton-ok egy **GroupBox**-ba vagy Panel-be kerüljenek – egy csoportban egyszerre csak egy lehet bejelölve.

```csharp
// melyik van bejelölve?
int ajtok = 0;
if (radioButton1.Checked) ajtok = 2;
else if (radioButton2.Checked) ajtok = 3;
else if (radioButton3.Checked) ajtok = 4;
else if (radioButton4.Checked) ajtok = 5;

// induláskor egyik se legyen bejelölve
radioButton1.Checked = false;
this.ActiveControl = listBox1;      // különben a fókuszt kapó rádiógomb bejelölődik!

// a GroupBox-ban bejelölt rádiógomb szövege
var valasztott = groupBox1.Controls.OfType<RadioButton>().FirstOrDefault(r => r.Checked);
string szoveg = valasztott?.Text;

// CheckBox
if (checkBox1.Checked) { }
checkBox1.Checked = true;
checkBox1.CheckedChanged += (s, e) => Szures();

// Szűrés két checkbox alapján (pl. vegetáriánus / csípős pizza)
private void Szures()
{
    listBox1.Items.Clear();
    var lista = pizzak.AsEnumerable();
    if (checkBox1.Checked) lista = lista.Where(x => x.Vegetarianus);
    if (checkBox2.Checked) lista = lista.Where(x => x.Csipos);
    foreach (var p in lista) listBox1.Items.Add(p.Nev);
}
```

---

## 11. NumericUpDown, DateTimePicker

```csharp
numericUpDown1.Minimum = 0;
numericUpDown1.Maximum = 100;
numericUpDown1.Value = 5;                    // decimal típus!
int db = (int)numericUpDown1.Value;
numericUpDown1.ValueChanged += (s, e) => Szamol();

DateTime datum = dateTimePicker1.Value;
dateTimePicker1.Value = DateTime.Today;
dateTimePicker1.Format = DateTimePickerFormat.Short;

trackBar1.Value;  trackBar1.Minimum = 0;  trackBar1.Maximum = 10;
progressBar1.Value = 50;
```

---

## 12. Button, MessageBox

```csharp
MessageBox.Show("Sikeres mentés!");
MessageBox.Show("Szöveg", "Cím");

// kérdés Igen/Nem gombbal
DialogResult valasz = MessageBox.Show(
    $"Biztosan örökbe fogadja {nevLabel.Text} nevű kutyát?",   // szöveg
    "Örökbefogadás",                                            // címsor
    MessageBoxButtons.YesNo,
    MessageBoxIcon.Question);

if (valasz == DialogResult.Yes)
{
    // ...
}

// nevesített paraméterekkel (sorrend mindegy)
MessageBox.Show(
    text: $"Ön {counter} termékünket tekintette meg. Visszavárjuk!",
    caption: "Kilépés",
    buttons: MessageBoxButtons.OK,
    icon: MessageBoxIcon.Information);
```

Gombtípusok: `OK`, `OKCancel`, `YesNo`, `YesNoCancel`, `RetryCancel`.
Ikonok: `Information`, `Question`, `Warning`, `Error`.

**Kilépés gomb:**
```csharp
private void KilepesButton_Click(object? sender, EventArgs e)
{
    MessageBox.Show($"Ön {megtekintett.Count} terméket tekintett meg. Visszavárjuk!");
    Application.Exit();          // vagy: this.Close();
}
```

Bezárás megerősítése (X gombnál is):
```csharp
this.FormClosing += (s, e) =>
{
    if (MessageBox.Show("Biztosan kilép?", "Kilépés", MessageBoxButtons.YesNo) == DialogResult.No)
        e.Cancel = true;
};
```

Gomb tiltása / engedélyezése:
```csharp
button1.Enabled = listBox1.SelectedIndex != -1;
```

---

## 13. Keresés gépelés közben

```csharp
textBox1.TextChanged += TextBox1_TextChanged;

private void TextBox1_TextChanged(object? sender, EventArgs e)
{
    string keres = textBox1.Text.ToLower();
    var talalat = varak.Where(x => x.Nev.ToLower().Contains(keres));

    dataGridView1.Rows.Clear();                 // ListBox-nál: listBox1.Items.Clear();
    foreach (var v in talalat)
    {
        dataGridView1.Rows.Add(v.Nev, v.Varos, v.Epult);
    }
}
```

---

## 14. Fájlba mentés gombra

```csharp
private void MentesButton_Click(object? sender, EventArgs e)
{
    using (StreamWriter iro = new StreamWriter("../../../forras/javaslat.txt"))
    {
        iro.WriteLine($"{nevLabel.Text};{numericUpDown1.Value};{DateTime.Now}");
    }
    MessageBox.Show("Sikeres mentés!");
}

// hozzáfűzés (ne írja felül a régit):
using (StreamWriter iro = new StreamWriter("../../../forras/rendeles.txt", true)) { ... }

// rendezett lista mentése fejléccel
var rendezett = orszagok.OrderByDescending(x => x.Arany).ThenByDescending(x => x.Ezust).ToList();
using (StreamWriter iro = new StreamWriter("../../../forras/eremtabla_uj.txt"))
{
    iro.WriteLine("Orszag;Arany;Ezust;Bronz");
    foreach (var o in rendezett)
        iro.WriteLine($"{o.Nev};{o.Arany};{o.Ezust};{o.Bronz}");
}
```

Mentés-ablak (ha a felhasználó választ helyet):
```csharp
SaveFileDialog sfd = new SaveFileDialog { Filter = "Szövegfájl|*.txt" };
if (sfd.ShowDialog() == DialogResult.OK)
{
    File.WriteAllText(sfd.FileName, richTextBox1.Text);
}
```

---

## 15. Számlálók

„Hány különböző terméket tekintett meg?” – HashSet (ugyanazt kétszer nem számolja):
```csharp
HashSet<int> megtekintett = new HashSet<int>();

private void ListBox1_SelectedIndexChanged(object? sender, EventArgs e)
{
    megtekintett.Add(listBox1.SelectedIndex);
    ...
}
// kilépéskor: megtekintett.Count
```

Kosár / összeg számolása:
```csharp
int reszosszeg = 0;
const int SZALLITAS = 690;

private void KosarButton_Click(object? sender, EventArgs e)
{
    int db = int.Parse(textBox1.Text);
    var p = pizzak[listBox1.SelectedIndex];
    reszosszeg += p.Ar * db;
    richTextBox1.AppendText($"{db} x {p.Nev} - {p.Ar} Ft\n");

    int szallitas = reszosszeg >= 8000 ? 0 : SZALLITAS;
    reszLabel.Text = $"{reszosszeg} Ft";
    szallitasLabel.Text = $"{szallitas} Ft";
    vegLabel.Text = $"{reszosszeg + szallitas} Ft";
}
```

---

## 16. Kép, színek

```csharp
pictureBox1.Image = Image.FromFile("../../../kepek/kutya.jpg");
pictureBox1.SizeMode = PictureBoxSizeMode.Zoom;      // arányosan kitölt

label1.ForeColor = Color.Red;
label1.BackColor = Color.FromArgb(192, 133, 82);
label1.Font = new Font("Arial", 14, FontStyle.Bold);
this.BackColor = Color.WhiteSmoke;
this.Text = "Menhely";                                 // ablak címe
```

---

## 17. Bemenet ellenőrzése

```csharp
// szám-e
if (!int.TryParse(textBox1.Text, out int db) || db <= 0)
{
    MessageBox.Show("Adjon meg pozitív egész számot!", "Hiba", MessageBoxButtons.OK, MessageBoxIcon.Error);
    return;
}

// ki van-e választva valami
if (listBox1.SelectedIndex == -1)
{
    MessageBox.Show("Válasszon egy elemet!");
    return;
}
if (comboBox1.SelectedItem == null) { ... }

// nem üres
if (string.IsNullOrWhiteSpace(textBox1.Text)) { ... }

// év tartomány (pl. kor szerinti keresés)
if (!int.TryParse(minBox.Text, out int min) || !int.TryParse(maxBox.Text, out int max) || min > max)
{
    MessageBox.Show("Hibás évek!");
    return;
}
```

---

## 18. Teljes minta

```csharp
namespace LaptopsGUI
{
    public partial class Form1 : Form
    {
        List<Laptop> laptops = new List<Laptop>();
        HashSet<int> megtekintett = new HashSet<int>();

        public Form1()
        {
            InitializeComponent();
            this.Load += Form1_Load;
            listBox1.SelectedIndexChanged += ListBox1_SelectedIndexChanged;
            kilepesButton.Click += KilepesButton_Click;
        }

        private void Form1_Load(object? sender, EventArgs e)
        {
            Read();
            foreach (var l in laptops)
            {
                listBox1.Items.Add($"{l.Manufacturer.ManufacturerName} {l.Model}");
            }
            int gyartok = laptops.Select(x => x.Manufacturer.ManufacturerName).Distinct().Count();
            cimLabel.Text = $"VÁLASSZON {gyartok} GYÁRTÓ {laptops.Count} LAPTOPJA KÖZÜL!";
            listBox1.SelectedIndex = 0;
        }

        private void ListBox1_SelectedIndexChanged(object? sender, EventArgs e)
        {
            int i = listBox1.SelectedIndex;
            if (i == -1) return;
            megtekintett.Add(i);

            var l = laptops[i];
            richTextBox1.Text =
                $"Kategória\n\t{l.Category.CategoryName}\n" +
                $"Képátló\n\t{l.Screen.Split(" - ")[0]}\n" +
                $"Felbontás\n\t{l.Screen.Split(" - ")[1]}\n" +
                $"Processzor\n\t{l.CPU}\n" +
                $"RAM\n\t{l.RAM} GB\n" +
                $"Háttértár\n\t{l.Storage}\n" +
                $"Ár\n\t{Math.Round(l.Price * 4.12)} Ft";
        }

        private void KilepesButton_Click(object? sender, EventArgs e)
        {
            MessageBox.Show($"Ön {megtekintett.Count} termékünket tekintette meg. Visszavárjuk!");
            Application.Exit();
        }

        private void Read()
        {
            using (StreamReader o = new StreamReader("../../../forras/laptops.txt"))
            {
                o.ReadLine();
                while (!o.EndOfStream)
                {
                    laptops.Add(new Laptop(o.ReadLine()));
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
| `ArgumentOutOfRangeException` a `SelectedIndexChanged`-ben | `Items.Clear()` után az index -1 | `if (index == -1) return;` |
| rossz elem adatai jelennek meg | szűrt ListBox indexével az eredeti listát indexeled | tárold a szűrt listát, vagy objektumot tegyél a ListBox-ba |
| `ArgumentOutOfRangeException` a `CellClick`-ben | fejlécre kattintás (`RowIndex = -1`) | `if (e.RowIndex < 0) return;` |
| nem fut le az eseménykezelő | nincs bekötve | `button1.Click += ...` a konstruktorban, vagy dupla klikk a tervezőben |
| `NullReferenceException` a `SelectedItem.ToString()`-nél | nincs kiválasztva semmi | `?.ToString()` vagy ellenőrzés |
| a rádiógomb magától bejelölődik induláskor | megkapja a fókuszt | `this.ActiveControl = listBox1;` |
| üres DataGridView-sor alul | `AllowUserToAddRows = True` | állítsd `False`-ra |
| a NumericUpDown értéke nem `int` | `decimal` típus | `(int)numericUpDown1.Value` |
| `FileNotFoundException` | rossz útvonal | `../../../forras/...` |
| törölted a tervezőben az eseményt, de a kódban maradt / fordítva | a `Form1.Designer.cs` még hivatkozik rá | Hibalista → dupla klikk → töröld a sort a Designer.cs-ben |
