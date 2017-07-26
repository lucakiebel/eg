# EnumGenerator

### generiert Enumerations aus Excel-Tabellen und speichert diese.

## Inhalt:
 - [EnumGenerator](#enumgenerator-1)
   - [Erklärung der Grundfunktionalität](#erklärung-der-grundfunktionalität)
   - [Erklärung der Funktionalität der Konstanten](#erklärung-der-funktionalität-der-konstanten)
   - [Erklärung der Funktionalität der Methoden](#erklärung-der-funktionalität-der-methoden)
   - [Erklärung der Verwendung am Beispiel](#erklärung-der-verwendung-am-beispiel)
 - [LBKey](#lbkey)
   - [Erklärung der Grundfunktionalität](#erklärung-der-grundfunktionalität-1)
   - [Erklärung der Funktionalität der Methoden](#erklärung-der-funktionalität-der-methoden-1)
   - [Erklärung der Verwendung am Beispiel](#erklärung-der-verwendung-am-beispiel-1)
 - [Properties-Datei](#properties-datei)



----



# EnumGenerator
## Erklärung der Grundfunktionalität:

EnumGenerator generiert Enumerations aus Excel-Tabellen und speichert diese.
Die Datei, die als Parameter übergeben wird, wird zuerst eingelesen. Das Programm benutzt dann [Apaches POI](http://poi.apache.org) um aus der Datei ein `org.apache.poi.hssf.usermodel.HSSFWorkbook` zu erstellen.
Über dieses kann auf die jeweiligen Arbeitsblätter (oder Tabellen) der XLS-Datei zugegriffen werden. Hier wird das Arbeitsblatt `0` und das Arbeitsblatt `2` verwendet. Aus den Arbeitsblättern werden alle relevanten Daten geholt und es werden Objekte erzeugt in denen diese Daten dann zum schreiben von Dateien vorbereitet werden. Die Klasse gibt nichts zurück, alle relevanten Daten werden über die Konsole ausgegeben und Fehler werden in die Log-Dateien geschrieben.

----

## Erklärung der Funktionalität der Konstanten:
 - `ENUM_COLUMN` Nummer der Spalte mit den Namen der Enumerations; aus Arbeitsblatt `0`
 - `CONSTANT_COLUMN` Nummer der Spalte mit den Namen der Konstanten oder Ausprägungen; aus Arbeitsblatt `2`
 - `DIALOG_COLUMN` Nummer der Spalte mit den Dialog-Texten für die Ausprägungen; aus Arbeitsblatt `2`
 - `SLV_ID_COLUMN` Nummer der Spalte mit den Schlüsseln, die Ausprägungen und Dialog-Texte verbindet; aus Arbeitsblatt `2`
 - `SLW_ID_COLUMN` Nummer der Spalte mit den Schlüsseln, die Dialog-Texte innerhalb der Ausprägungen identifizieren; aus Arbeitsblatt `2`

---- 

## Erklärung der Funktionalität der Methoden:
### EnumGenerator(String):EnumGenerator - Konstruktor
**Parameter**: `String filePath` - Der Resource Locator der zu parsenden Datei <br>
**Funktion**: Initialisierung des Parsers [datenParsen(String)](#datenparsenstringvoid---parsen-der-xls-datei)

Der Konstruktor initialisiert den Vorgang zur Generierung von Enum-Klassen aus der Datenbasis, die unter `filePath` zu finden ist.

### datenParsen(String):void - Parsen der XLS-Datei
**Parameter**: `String filePath` - Der Resource Locator der zu parsenden Datei, von [EnumGenerator(String)](#enumgeneratorstringvoid---konstruktor) übergeben <br>
**Rückgabe**: `void` <br>
**Funktion**: Hauptmethode der Klasse, liest die Datei ein und wandelt diese in besser zu verabeitende Datenmodelle um.

### enumKlasseSchreiben(String, String[]):boolean - Schreiben einer Datei
**Parameter**: `String dateiName` - Der Name der zu schreibenden Datei, von [getEnumLines(String, LinkedHashMap)](#getenumlinesstring-linkedhashmaplbkey-stringstring---aufbauen-des-string-arrays-linesf%C3%BCr-enumklasseschreibenstring-string) übergeben <br>
**Parameter**: `String[] lines` - Der Inhalt der Klasse, die geschrieben werden soll, von [getEnumLines(String, LinkedHashMap)](#getenumlinesstring-linkedhashmaplbkey-stringstring---aufbauen-des-string-arrays-linesf%C3%BCr-enumklasseschreibenstring-string) übergeben <br>
**Rückgabe**: `boolean geschrieben`  - ob der Vorgang fehlerfrei durchgeführt wurde <br>  
**Funktion**: Schreibt eine einzelne Datei, die über `lines` übergeben wird und den Namen `dateiName.java` trägt. Wurde das Schreiben fehlerfrei abgeschlossen, so gibt die Methode `true` zurück, traten dabei Fehler auf, `false`. 

### getEnumLines(String, LinkedHashMap<LBKey, String[]>, String, String):String[] - Aufbauen des String Arrays `lines` für [enumKlasseSchreiben(String, String[])](#enumklasseschreibenstring-stringboolean---schreiben-einer-datei)
**Parameter**: `String dateiName` - Name der Klasse, für die hier der Inhalt generiert werden soll <br>
**Parameter**: `LinkedHashMap<LBKey, String[]> daten` - Daten für die Ausprägungen der zu erstellenden Klasse <br>
**Parameter**: `String enumPackage` - Package-Name der zu erstellenden Klasse, in `options.properties` einzustellen <br>
**Parameter**: `String enumPrefix` - Prefix für die Enumeration, in `options.properties` einzustellen <br>
**Rückgabe**: `String[] enumClassLines.toArray()` - Array aller `lines` der zu erstellenden Klasse <br>
**Funktion**: Intern wird eine `LinkedList<String>` aufgebaut und zum Ende in einen `String[]` umgewandelt und ausgegeben.

### getPvBLines(String, LinkedHashMap<LBKey, String[]>, String, String, String, String):String[] - Aufbauen des String Arrays `lines` für [enumKlasseSchreiben(String, String[])](#enumklasseschreibenstring-stringboolean---schreiben-einer-datei)
**Parameter**: `String dateiNamen` - Name der Klasse, für den der Inhalt generiert werden soll <br>
**Parameter**: `LinkedHashMap<LBKey, String[]> daten` - Daten für die Ausprägungen der zu erstellennden Klasse <br>
**Parameter**: ``




### log(String):void - Ausgabe zur Konsole und in das Log
**Parameter**: `String x` - Der String, der über `stdout` ausgeben, und in die Log-Datei geschrieben werden soll <br>
**Rückgabe**: `void` <br>
**Funktion**: Wie von [`System.out.println()`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html#println--)


## Erklärung der Verwendung am Beispiel

Hier wird FondsArt, also `SLV_ID`#36 aus der Tabelle `CodeLeBest`.`SLV Werte Mandant`geholt und in eine Enumeration umgewandelt.

Aus diesem Tabellenausschnitt:
![CodeLeBest Tabelle](https://luca-kiebel.de/images/GvQnDRe.png)


Wird dieser Code:
```java
package de.provinzial.prozesstypes.vertrag.leben;

import provinzial.fw.util.PvXTyp;

public enum PvEFONDS_ART_KZ {
	UNGUELTIG("<leer>", 0),
	ALLE("alle", 1),
	DACH("vermögensverwaltende Fonds", 2),
	AKTIE("Aktienfonds", 3),
	RENTE_IMMOBILIEN("Renten- und Immobilienfonds", 4),
	SONST("sonstige", 5),
	GARANTIE("Lebenszyklusfonds", 6),
	WSF("Wertsicherungsfonds", 7),
	ETF("Indexfonds-ETF", 8),
	PB("Private Banking Fonds", 9),
	MF("Mischfonds", 10);

	private final String bezeichnung;
	private final int leBestCode;

	private PvEAngebotStatus(final String bezeichnung, int leBestCode) {
		this.bezeichnung = bezeichnung;
		this.leBestCode = leBestCode
	}

	public String getBezeichnung() {
		return this.bezeichnung;
	}

	public int getLeBestCode() {
		return this.leBestCode;
	}

	public static PvEAngebotStatus from(final int leBestCode) {
		for (PvEAngebotStatus element : PvEAngebotStatus.values()) {
			if (element.leBestCode == leBestCode) {
				return element;
			}
		}
		throw new PvXTyp("Ungueltiger FONDS_ART_KZ " + leBestCode);
	}
}
```

Der entstandene Code kann dann während der Laufzeit in eine Klasse umgewandelt werden und als Enumeration genutzt werden.

Beim Ausführen der Klasse (zB über CLBTest.class) werden alle gefundenen Enumerations erzeugt. Hier könnte der Code in etwa so ausgesehen haben:

```java 
public class CLBTest {
	public void main(String[] args) {
		String dateiPfad = "C:\\Pfad\\zur\\Tabelle.xls";
		EnumGenerator enumGenerator = new EnumGenerator(dateiPfad);
	}
}
```

Der selbe Effekt kann durch einfaches aufrufen der Klasse (zB über `javaw EnumGenerator "C:\\Pfad\\zur\\Tabelle.xls"`) erreicht werden, hier wird die Methode `main` ausgeführt, die praktisch das selbe macht, was CLBTest.class macht. 

----

# LBKey
## Erklärung der Grundfunktionalität

LBKey ist eine Klasse, deren Objekte dazu genutzt werden können eine Reihe oder Zeile in der CodeLeBest.xls zu identifzieren. Beim Aufbauen des Objektes werden 2 Schlüssel übergeben, die in dieser Verbindung einzigartig sind. So können in `EnumGenerator` `LinkedHashMap`s mit eindeutigen Schlüsseln versehen werden.

## Erklärung der Funktionalität der Konstanten:
 - `slv` SLV_ID für das Schlüsselpaar, Identifier des aktuellen `Enum`s
 - `slw` SLW_ID für das Schlüsselpaar, Identifier des aktuellen Dialogtextes des `Enum`s


## Erklärung der Funktionalität der Methoden:
### LBKey(int, int):LBKey - Konstruktor
**Parameter**: `int slv`  - SLV_ID für das Schlüsselpaar <br>
**Parameter**: `int slw`  - SLW_ID für das Schlüsselpaar <br>
**Funktion**: Initalisierung des Objektes des Schlüssels <br>

### getSLV():int - Getter der SLV_ID
**Rückgabe**: `int this.slv` - Der Schlüssel der Schlüsselverzeichnisliste <br>
 **Funktion**: Gibt den Identifier des `Enum`s aus dem Schlüsselpaar wieder <br>

### getSLW():int  - Getter der SLW_ID
**Rückgabe**: `int this.slw` - Der Schlüssel der Werteliste <br>
**Funktion**: Gibt den Identifier des Dialogtextes innerhalb des `Enum`s wieder <br>

## Erklärung der Verwendung am Beispiel:
Hier wird in [`EnumGenerator.datenParsen`](#datenparsenstringvoid---parsen-der-xls-datei) eine `LinkedHashMap<LBKey, String>` aufgebaut. `lbKey` ist hier der eindeutige Schlüssel, den die `LinkedHashMap` braucht, um Werte nicht zu überschreiben. 
Diese `LinkedHashMap` wird nachher verwendet um die Strings für die Enum-Dateien aufzubauen und den Konstanten, Dialog-Texten und den Identifiern ihr Enum zuzuweisen. 

```java
schluesselVerzListe.rowIterator().forEachRemaining(reihe -> {
	if (reihe.getCell(0) != null && Cell.CELL_TYPE_NUMERIC == reihe.getCell(0).getCellType()) {
		int slv = (int) reihe.getCell(0).getNumericCellValue();
		LBKey lbKey = new LBKey((slv), 0);
		String enumName = reihe.getCell(ENUM_COLUMN).getStringCellValue();
		enumNamen.put(lbKey, enumName);
	}

});
```

# Properties-Datei

Die Properties-Datei wird genutzt, um wichtige Umgebungsvariablen zu setzen, diese beinhalten:

 - `eingabepfad` - DateiPfad für die Eingabedatei der Enumerations
 - `ausgabepfad` - DateiPfad, in den Enumerations geschrieben werden sollen (ohne Slash am Ende)
 - `enumpackage` - Package, dem die zu erstellenden Enumerations angehören
 - `enumprefix` - Enum-Namen-Prefix zur Identifikation, standardmäßig "PvE"
 - `slvs` - SLV_IDs zu denen Enumerations erstellt werden sollen, Komma-getrennt; bei "%" werden alle PvEs geschrieben

Standardmäßig sieht die Properties-Datei wie folgt aus:

```java
#==========   Einstellungen für EnumGenerator   ==========#
#
# DateiPfad für die Eingabedatei der Enumerations
eingabepfad=C:\\Users\\W121442\\workspace\\CLBEnumGenerator\\CodeLeBest.xls
#
# DateiPfad, in den PvE-Enumerations geschrieben werden sollen (ohne Slash am Ende)
pveausgabepfad=C:\\Users\\W121442\\workspace\\CLBEnumGenerator\\PvEs
#
# DateiPfad, in den PvB-Enumerations (Referenzen) geschrieben werden sollen (ohne Slash am Ende)
pvbausgabepfad=C:\\Users\\W121442\\workspace\\CLBEnumGenerator\\PvBs
#
# Enumeration Package
pvepackage=de.provinzial.prozesstypes.vertrag.leben
#
# Enum-Namen-Prefix (PvE)
pveprefix=PvE_
#
# Enum-Namen-Prexif (PvB)
pvbprefix=PvB_
#
# SLV_IDs zu denen Enumerations erstellt werden sollen, Komma-getrennt; bei "%" werden alle PvEs geschrieben
slvs=1
#
# PvB Version
lebestvers=v17_200
#
# DateiPfad für die Logdateien (ohne Slash am Ende)
logpath=C:\\Users\\W121442\\workspace\\CLBEnumGenerator\\logs
#
# Logdateien per Datum (DateiName wird zu "log-$DATUM.txt") Mögliche Werte: 0=ohne Datum,1=mit Datum
logperdat=1
```

Zeilen mit `#` am Anfang sind Kommentare und werden nicht interpretiert. 
