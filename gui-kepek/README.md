# WinForms vezérlők – képes áttekintő

[← vissza a jegyzetekhez](../README.md) · Részletes leírás kódpéldákkal: [GUI-Toolbox.md](../GUI-Toolbox.md)

Minden kép egy valódi, futó WinForms vezérlőről készült (.NET 10, Windows 11). ⭐ = gyakran használt.

## Common Controls – közös vezérlők

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="button.png" alt="Button"> | **Button** ⭐ | Gomb. Kattintásra (`Click`) lefut a kódod. |
| <img src="checkbox.png" alt="CheckBox"> | **CheckBox** ⭐ | Pipálható négyzet, igen/nem érték (`Checked`). Több is bepipálható. |
| <img src="checkedlistbox.png" alt="CheckedListBox"> | **CheckedListBox** | Lista, minden sora előtt pipával – több elem kiválasztására. |
| <img src="combobox.png" alt="ComboBox"> | **ComboBox** ⭐ | Legördülő lista, egy elem választható (`SelectedItem`). |
| <img src="datetimepicker.png" alt="DateTimePicker"> | **DateTimePicker** | Dátumválasztó lenyíló naptárral (`Value`). |
| <img src="label.png" alt="Label"> | **Label** ⭐ | Felirat: szöveg kiírása, a felhasználó nem írhat bele (`Text`). |
| <img src="linklabel.png" alt="LinkLabel"> | **LinkLabel** | Kattintható, aláhúzott szöveg, mint egy link. |
| <img src="listbox.png" alt="ListBox"> | **ListBox** ⭐ | Lista, egy sor kijelölhető (`SelectedIndex`). A leggyakoribb elem. |
| <img src="listview.png" alt="ListView"> | **ListView** | Lista nézet; `Details` módban oszlopos táblázat. |
| <img src="maskedtextbox.png" alt="MaskedTextBox"> | **MaskedTextBox** | Szövegmező, ami csak a megadott formátumot engedi (pl. telefonszám). |
| <img src="monthcalendar.png" alt="MonthCalendar"> | **MonthCalendar** | Mindig látható havi naptár. |
| <img src="numericupdown.png" alt="NumericUpDown"> | **NumericUpDown** ⭐ | Számmező fel/le nyilakkal (`Value` – decimal!). |
| <img src="picturebox.png" alt="PictureBox"> | **PictureBox** | Kép megjelenítése (`Image`, `SizeMode = Zoom`). |
| <img src="progressbar.png" alt="ProgressBar"> | **ProgressBar** | Folyamatjelző csík (`Value`, `Maximum`). |
| <img src="radiobutton.png" alt="RadioButton"> | **RadioButton** ⭐ | Választógomb: egy csoportban csak egy lehet bejelölve. |
| <img src="richtextbox.png" alt="RichTextBox"> | **RichTextBox** ⭐ | Többsoros szövegdoboz, lehet benne félkövér, színes rész. |
| <img src="textbox.png" alt="TextBox"> | **TextBox** ⭐ | Szövegmező, ide gépel a felhasználó (`Text`, `TextChanged`). |
| <img src="tooltip.png" alt="ToolTip"> | **ToolTip** | Súgóbuborék, ami az egér alatt megjelenik. |
| <img src="treeview.png" alt="TreeView"> | **TreeView** | Kinyitható fa szerkezet (mint az Intéző bal oldala). |
| <img src="webbrowser.png" alt="WebBrowser"> | **WebBrowser** | Weboldal / HTML megjelenítése a formon. |

## Containers – tárolók

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="flowlayoutpanel.png" alt="FlowLayoutPanel"> | **FlowLayoutPanel** | Tároló, ami a beletett elemeket magától sorba rendezi. |
| <img src="groupbox.png" alt="GroupBox"> | **GroupBox** ⭐ | Keret felirattal; a benne lévő RadioButtonok egy csoportot alkotnak. |
| <img src="panel.png" alt="Panel"> | **Panel** | Felirat nélküli doboz vezérlők csoportosítására. |
| <img src="splitcontainer.png" alt="SplitContainer"> | **SplitContainer** | Két panel húzható elválasztóval. |
| <img src="tabcontrol.png" alt="TabControl"> | **TabControl** | Füles lapok, minden fül külön tartalom. |
| <img src="tablelayoutpanel.png" alt="TableLayoutPanel"> | **TableLayoutPanel** | Rács (sorok × oszlopok), minden cellába egy vezérlő. |

## Menus & Toolbars – menük és eszköztárak

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="contextmenustrip.png" alt="ContextMenuStrip"> | **ContextMenuStrip** | Jobb egérgombra felugró menü. |
| <img src="menustrip.png" alt="MenuStrip"> | **MenuStrip** | Menüsor a form tetején (Fájl, Szerkesztés, Súgó…). |
| <img src="statusstrip.png" alt="StatusStrip"> | **StatusStrip** | Állapotsor a form alján („Kész”, „16 termék”). |
| <img src="toolstrip.png" alt="ToolStrip"> | **ToolStrip** | Eszköztár gombokkal (Új, Megnyitás, Mentés). |
| <img src="toolstripcontainer.png" alt="ToolStripContainer"> | **ToolStripContainer** | Tároló, aminek a széleire eszköztárak tehetők. |

## Data – adatok

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="datagridview.png" alt="DataGridView"> | **DataGridView** ⭐ | Táblázat sorokkal és oszlopokkal (`Rows.Add`, `CellClick`). |
| <img src="bindingnavigator.png" alt="BindingNavigator"> | **BindingNavigator** | Léptető sáv rekordok között (első, előző, következő, utolsó). |

## Components – komponensek

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="errorprovider.png" alt="ErrorProvider"> | **ErrorProvider** | Piros hibajel egy vezérlő mellett, ha rossz az adat. |

## Printing – nyomtatás

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="pagesetupdialog.png" alt="PageSetupDialog" width="260"> | **PageSetupDialog** | Oldalbeállítás: papírméret, tájolás, margók. |
| <img src="printdialog.png" alt="PrintDialog" width="320"> | **PrintDialog** | A „Nyomtatás” ablak: nyomtató, példányszám. |
| <img src="printpreviewcontrol.png" alt="PrintPreviewControl" width="200"> | **PrintPreviewControl** | Nyomtatási kép a formon belül. |
| <img src="printpreviewdialog.png" alt="PrintPreviewDialog" width="320"> | **PrintPreviewDialog** | Nyomtatási kép külön ablakban. |

## Dialogs – párbeszédablakok

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="colordialog.png" alt="ColorDialog" width="320"> | **ColorDialog** | Színválasztó ablak (`Color`). |
| <img src="folderbrowserdialog.png" alt="FolderBrowserDialog" width="320"> | **FolderBrowserDialog** | Mappaválasztó ablak (`SelectedPath`). |
| <img src="fontdialog.png" alt="FontDialog" width="320"> | **FontDialog** | Betűtípus-választó ablak (`Font`). |
| <img src="openfiledialog.png" alt="OpenFileDialog" width="320"> | **OpenFileDialog** | „Megnyitás” ablak fájl kiválasztására (`FileName`). |
| <img src="savefiledialog.png" alt="SaveFileDialog" width="320"> | **SaveFileDialog** | „Mentés másként” ablak (`FileName`). |

## All Windows Forms – további elemek

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="domainupdown.png" alt="DomainUpDown"> | **DomainUpDown** | Léptető szöveges elemek között (Kicsi / Közepes / Nagy). |
| <img src="hscrollbar.png" alt="HScrollBar"> | **HScrollBar** | Vízszintes görgetősáv, értéket lehet vele állítani. |
| <img src="vscrollbar.png" alt="VScrollBar"> | **VScrollBar** | Függőleges görgetősáv. |
| <img src="trackbar.png" alt="TrackBar"> | **TrackBar** | Csúszka egy érték beállítására. |
| <img src="propertygrid.png" alt="PropertyGrid" width="260"> | **PropertyGrid** | Tulajdonságrács, mint a VS Properties ablaka. |
| <img src="splitter.png" alt="Splitter"> | **Splitter** | Régi, húzható elválasztó (helyette: SplitContainer). |

## MessageBox (kódból hívod, nincs a Toolboxban)

| Kép | Név | Mi ez? |
|---|---|---|
| <img src="messagebox_info.png" alt="MessageBox"> | **MessageBox.Show("…")** ⭐ | Egyszerű üzenetablak OK gombbal. |
| <img src="messagebox.png" alt="MessageBox Igen/Nem"> | **MessageBox – YesNo** ⭐ | Kérdés Igen/Nem gombbal; a válasz `DialogResult.Yes` / `No`. |

## Kép nélkül (nem látszanak a formon)

Ezek a tervező alján lévő szürke sávba kerülnek, a futó programban nincs saját kinézetük.

| Név | Mi ez? |
|---|---|
| **NotifyIcon** | Ikon a Windows tálcán (az óra mellett), buborék-üzenettel. |
| **BindingSource** | Közvetítő egy lista és a vezérlők között (adatkötés). |
| **DataSet** | Memóriában lévő „mini adatbázis” táblákkal. |
| **BackgroundWorker** | Hosszú művelet futtatása a háttérben, hogy ne fagyjon le az ablak. |
| **EventLog** | Írás a Windows eseménynaplójába. |
| **FileSystemWatcher** | Mappa figyelése: szól, ha fájl jön létre / változik / törlődik. |
| **HelpProvider** | F1-re súgószöveg egy vezérlőhöz. |
| **ImageList** | Képgyűjtemény más vezérlők (ListView, TreeView) ikonjaihoz. |
| **Process** | Másik program vagy fájl megnyitása. |
| **SerialPort** | Kommunikáció soros (COM) porton, pl. Arduino. |
| **ServiceController** | Windows-szolgáltatás indítása / leállítása. |
| **Timer** ⭐ | Időzítő: adott időközönként lefuttat egy eseményt (`Tick`). |
| **PrintDocument** | A nyomtatás „motorja”: a `PrintPage` eseményben rajzolod az oldalt. |
| **ElementHost** | WPF-es vezérlő megjelenítése WinForms ablakban. |
