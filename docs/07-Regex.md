# Exkurs: Reguläre Ausdrücke 

## Was sind reguläre Ausdrücke?

Reguläre Ausdrücke (oder engl. Regular Expression, kurz: RegEx, RegExp) sind verallgemeinerte Suchmuster (patterns) für Zeichenketten. Mithilfe von regulären Ausdrücken können syntaktische Konstrukte so beschrieben werden, dass sie ein Computer versteht. Ein syntaktisches Konstrukt ist zum Beispiel eine Zahl zwischen 1900 und 2000, eine Telefonnummer, eine Adresse, eine URL oder auch ein bestimmtes Wort in verschiedenen Flexionsformen. Mithilfe von regulären Ausdrücken können also Texte nach bestimmten Mustern durchsucht werden, und die gefundenen Konstrukte können anschließend z.B. entfernt oder bearbeitet werden. Die meisten Programmiersprachen, darunter auch R, stellen Funktionen bereit, welche die Verwendung von regulären Ausdrücken erlauben. In den Beispielen in diesem Kapitel werden wir nach Mustern in zwei deutschsprachigen Märchentexten suchen. Zur Formulierung der Suchmuster verwenden wir deswegen das deutsche Alphabet. Reguläre Ausdrücke können aber genauso für andere Alphabete, Schriftsysteme und  

## Reguläre Ausdrücke in R

Als Beispieltexte nutzen wir die beiden Märchen Rapunzel und Aschenputtel. <br />
Zur Vorbereitung installieren wir die folgenden Pakete...





```r
install.packages(c("readtext", "stringr", "quanteda"))
```

...und laden die installierten Pakete: 


```r
library(readtext)
library(stringr)
library(quanteda)
```

### R Base Funktionen

In R können Texte nach regulären Ausdrücken entweder mithilfe von R-Basisfunktionen durchsucht werden, oder mithilfe von Funktionen aus spezialisierten Paketen. Zunächst betrachten wir die Basisfunktionen, und lernen später reguläre Ausdrücke selbst kennen:  

  | Funktion         |  Bedeutung                             |      
  |------------------|----------------------------------------|
  | grep(), grepl()  |  Search for matches of a regular expression/pattern in a character vector; either return the indices into the character vector that match, the strings that happen to match, or a TRUE/FALSE vector indicating which elements match. |                   
  | regexpr(), gregexpr() |  Search a character vector for regular expression matches and return the indices of the string where the match begins and the length of the match.      |                   
  | sub(), gsub()    |  Search a character vector for regular expression matches and replace that match with another string.       |                   
  
  Quelle: https://www.youtube.com/watch?v=q8SzNKib5-4  <br />
  Offizielle Dokumentation: https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/grep


Wir verwenden die `readtext()`-Funktion zum Einlesen unserer Märchen. 


```r
maerchen_df <- readtext(c("data/Rapunzel_(1850).txt", "data/Aschenputtel_(1850).txt"))
length(maerchen_df$text)
```

```
## [1] 2
```

```r
maerchen_df
```

```
## readtext object consisting of 2 documents and 0 docvars.
## $text
## [1] "# A data frame: 2 × 2"                                
## [2] "  doc_id                  text                "       
## [3] "  <chr>                   <chr>               "       
## [4] "1 Aschenputtel_(1850).txt \"\\\"\\nEinem rei\\\"...\""
## [5] "2 Rapunzel_(1850).txt     \"\\\"\\nEs war ei\\\"...\""
## 
## $summary
## $summary[[1]]
## NULL
## 
## 
## attr(,"class")
## [1] "trunc_mat"
```

Durch Einlesen des Märchens mithilfe der `readtext()`-Funktion haben wir einen Dataframe mit zwei Spalten und zwei Zeilen erhalten. Wir wollen nicht den gesamten Datafrmae, sondern nur die Märchentexte nach Matches zu durchsuchen und wählen deswegen nur die Spalte `text` als Eingabe: 



```r
grep("Rapunzel", maerchen_df$text, value=F) 
```

```
## [1] 2
```

```r
grepl("Rapunzel", maerchen_df$text)
```

```
## [1] FALSE  TRUE
```

```r
regexpr("Rapunzel", maerchen_df$text)
```

```
## [1]  -1 625
## attr(,"match.length")
## [1] -1  8
```

```r
gregexpr("Rapunzel", maerchen_df$text)
```

```
## [[1]]
## [1] -1
## attr(,"match.length")
## [1] -1
## 
## [[2]]
##  [1]  625  751 1005 1176 1334 1903 2077 2334 2672 2709 3006 3016 3056 3527 3972
## [16] 3982 4026 4287 4297 4415 4729 5380 5782 6006 6117 6264 6274 6406 6697 7162
## [31] 7376
## attr(,"match.length")
##  [1] 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
```

Die Funktionen können aber auch auf jeden Text einzeln angewandt werden, indem separat auf beide Texte zugegriffen wird: 


```r
grep("Rapunzel", maerchen_df$text[2], value=F) 
```

```
## [1] 1
```

```r
grepl("Rapunzel", maerchen_df$text[2])
```

```
## [1] TRUE
```

```r
regexpr("Rapunzel", maerchen_df$text[2])
```

```
## [1] 625
## attr(,"match.length")
## [1] 8
```

```r
gregexpr("Rapunzel", maerchen_df$text[2])
```

```
## [[1]]
##  [1]  625  751 1005 1176 1334 1903 2077 2334 2672 2709 3006 3016 3056 3527 3972
## [16] 3982 4026 4287 4297 4415 4729 5380 5782 6006 6117 6264 6274 6406 6697 7162
## [31] 7376
## attr(,"match.length")
##  [1] 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
```

```r
grep("Rapunzel", maerchen_df$text[1], value=F) 
```

```
## integer(0)
```

```r
grepl("Rapunzel", maerchen_df$text[1])
```

```
## [1] FALSE
```

```r
regexpr("Rapunzel", maerchen_df$text[1])
```

```
## [1] -1
## attr(,"match.length")
## [1] -1
```

```r
gregexpr("Rapunzel", maerchen_df$text[1])
```

```
## [[1]]
## [1] -1
## attr(,"match.length")
## [1] -1
```

:::task
Verständnisfragen:

- Interpretiert die Ausgabe der vier Funktionen. Welche Informationen liefern die verschiedenen Funktionen? Wonach wird gesucht? 
- Was wird ausgegeben, wenn das Muster nicht gefunden wird? 

:::


Die Funktionen ``regexpr()`` und ``gregexpr()`` können um die Funktion ``regmatches`` ergänzt werden, um statt Funstellen die tatsächlich gefundenen Zeichenketten anzuzeigen: 


```r
regmatches(maerchen_df$text[2], regexpr("Rapunzel", maerchen_df$text[2]))
```

```{style="max-height: 100px;"}
## [1] "Rapunzel"
```

```r
regmatches(maerchen_df$text[2], gregexpr("Rapunzel", maerchen_df$text[2]))
```

```{style="max-height: 100px;"}
## [[1]]
##  [1] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
##  [7] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [13] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [19] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [25] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [31] "Rapunzel"
```

Da wir nur nach genau der Zeichenkette "Rapunzel" gesucht haben, wird natürlich auch nur "Rapunzel" gefunden. 

Mithilfe der Funktionen ``sub()`` und ``gsub()`` können gefundene Zeichenketten gelöscht oder ersetzt werden. Mit der bereits bekannten Funktion ``grep()`` können wir daraufhin überprüfen, ob die Zeichenketten erfolgreich ersetzt wurden. 


```r
# Zeichenkette ersetzen mit gsub()
peterpan_1 <- gsub("Rapunzel", "Peter Pan", maerchen_df$text[2])
grepl("Rapunzel", peterpan_1)
```

```{style="max-height: 100px;"}
## [1] FALSE
```

```r
grepl("Peter Pan", peterpan_1)
```

```{style="max-height: 100px;"}
## [1] TRUE
```

```r
# Zeichenkette ersetzen mit sub()
peterpan_2 <- sub("Rapunzel", "Peter Pan", maerchen_df$text[2])
grepl("Rapunzel", peterpan_2)
```

```{style="max-height: 100px;"}
## [1] TRUE
```

```r
# Zeichenkette löschen mit gsub() - geht auch mit sub()
rapunzel_cleaned <- gsub("Rapunzel", "", maerchen_df$text[2])
grepl("Rapunzel", rapunzel_cleaned)
```

```{style="max-height: 100px;"}
## [1] FALSE
```


:::task
Verständnisfragen:

- Überprüfen wir die Ergebnisse, so fällt auf, dass bei Anwendung der Funktion ``sub()`` nicht alle Instanzen der Zeichenkette "Rapunzel" ersetzt wurden. Woran liegt das? 
- Wie unterscheidet sich folglich das Verhalten der Funktionen ``gsub()`` und ``sub()``?

:::


Im Beispiel oben haben wir die Funktionen auf das readtext-Objekt angewandt. Es kann aber auch ein Quanteda corpus-Objekt mithilfe von regulären Ausdrücken durchsucht werden: 


```r
maerchen_korpus <- corpus(maerchen_df)
grepl("Rapunzel", maerchen_korpus[1])
```

```
## [1] FALSE
```

```r
grepl("Rapunzel", maerchen_korpus[2])
```

```
## [1] TRUE
```

```r
grepl("Rapunzel", maerchen_korpus)
```

```
## [1] FALSE  TRUE
```


### Spezielle Pakete: stringr 

Die Suche mithilfe von R-base-Funktionen kann etwas kompliziert werden, insbesondere, wenn die gefundenen Zeichenketten mithilfe von `regmatches()` und `gregexpr()` ausgegeben werden sollen. Außerdem ist es nicht immer möglich, die R-base-Funktionen mithilfe des Pipe-Operators zu verketten. Deswegen gibt es `stringr`, ein Paket aus dem Tidyverse (s. Kapitel R Basics IV), welches spezialisierte Funktionen zur Arbeit mit regulären Ausdrücken bietet. Die unübersichtliche Kombination von `regmatches()` und `gregexpr()` kann beispielsweise durch die stringr-Funktion `str_extract_all()` ersetzt werden: 


```r
str_extract_all(maerchen_df$text[2], "Rapunzel")
```

```
## [[1]]
##  [1] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
##  [7] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [13] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [19] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [25] "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel" "Rapunzel"
## [31] "Rapunzel"
```

Für eine Übersicht über stringr-Funktionen und einen Vergleich mit den R-base-Funktionen siehe: https://stringr.tidyverse.org/articles/from-base.html <br />

:::task
Verständnisfragen:

- Welche stringr Funktionen können anstelle der bereits kennengelernten R base Funktionen verwendet werden? 
- Googelt das Paket stringi. Was ist der Unterschied zu stringr?

:::



## Regex Syntax

### Basics Syntax

Bisher haben wir nur nach einzelnen Worten gesucht. Oft interessieren wir
uns aber für komplexere Ausdrücke, Zeichen, oder verschiedene Varianten desselben Worts. Um solche syntaktischen Strukturen zu beschreiben, gibt es verschiedene Regex-Symbole, die im Folgenden vorgestellt werden. 


  | Zeichen          |  Bedeutung                             |  Beispiel         |
  |------------------|----------------------------------------|-------------------|
  | ``\``            |  Escaped character: s.o.               |                   |
  | ``⎮``            |  Veroderung (ohne Leerzeichen!)        |                   |
  |                  |  Achtung: Auf Eindeutigkeit achten!    |                   |
  |                  |  Klammern nutzen!                      |                   |
  | ``()``           |  Gruppen: Einheiten können mit runden  | Baum(krone⎮stamm) |           
  |                  |  Klammern begrenzt werden              | 199(4⎮5)          |
  | ``[]``           |  Zeichenklasse                         | [a-zA-ZäöüÄÖÜß]   |
  |                  |  - Bindestrich für Zahlen- oder        | [0-9]             |
  |                  |    Buchstabenreihen                    |                   |
  |                  |  - Reihenfolge innerhalb von Klassen   |                   | 
  |                  |    nicht festgelegt                    |                   |
  |                  |  - Sucht nach allen Kombinationen der  |                   |
  |                  |    Zeichen, wenn kein "oder" spezifiz. |                   |
  | ``[^]``          |  Verneinung                            | [^aeo]            |
  | ``{}``           |  Quantifizierer                        |                   |
  |                  |  - zwischen 1 und 4                    | [a-z]{1,4}        |
  |                  |  - mindestens 1                        | [a-z]{1,}         |
  |                  |  - genau 4                             | [a-z]{4}          |
  | ``+``            |  "mindestens einmal", bis unendlich    | [1986]+           |
  |                  |  (Frage: Was ist der Unterschied?)     | (1986)+           |
  | ``*``            |  "beliebig oft", 0 bis unendlich       | [1986]*           |
  |                  |                                        | (1986)*           |
  | ``.*``           |  Wildcard: matcht Zeichen beliebig oft |                   |
  |                  |  (Aber: außer new line characters)     |                   |
  | ``?``            |  Mögliches, aber optionales Match      |                   |
  | ``^``            |  Positionsmarker: ^ außerhalb einer    |                   |
  |                  |  Zeichenklasse markiert Zeilenanfang   |                   |
  | ``$``            |  Positionsmarker: Markiert Zeilenende  |                   |
  
Merkt euch: Reguläre Ausdrücke stehen immer in doppelten Anführungszeichen! 
Soll innerhalb des Ausdrucks nach Anführungszeichen gesucht werden, müssen diese "escaped" werden. Dazu nutzt man den Backslash `\`. Das gilt auch für alle anderen Zeichen, die eine "Regex-interne" Bedeutung haben, wie Punkte, Bindestriche, Klammern, u.ä.

:::task
Verständnisfragen:

- Wie hätte sich die Ausgabe der "Rapunzel"-Beispiele oben verändert, wenn wir statt nach "Rapunzel" nach "Rapunzeln?" gesucht hätten?
- Wie hätten wir nach allen Varianten des Namens "Rapunzel" suchen können?

:::


### Zeichenklassen

Nach manchen Zeichen wird so oft gesucht, dass sich bereits nützliche Abkürzungen dafür entwickelt haben. Sowohl die R-base-Funktionen als auch die stringr-Funktionen unterstützen per Default die Suche nach den gängigsten Regex-Zeichenklassen.  

  | Klasse           |  Bedeutung                                   |
  |------------------|----------------------------------------------|
  | ``\w``           | Wortzeichen = [a-zA-Z]                       |
  | ``\W``           | kein Wortzeichen                             |
  | ``\d``           | Zahlzeichen = [0-9]                          |
  | ``\D``           | kein Zahlzeichen                             |              
  | ``\s``           | Leerzeichen                                  |
  | ``\S``           | kein Leerzeichen                             |
  | ``\r``           | carriage return (andere Art von Leerzeichen) |
  | ``\n``           | new line character (neue Zeile)              |
  | ``\t``           | Tabulatorzeichen                             |




```r
# Mit R-Basisfunktionen
regmatches(maerchen_df$text[2], gregexpr("\\s\\w{8}\\s", maerchen_df$text[2]))
```

```
## [[1]]
##  [1] " Hoffnung "  " Zauberin "  " bekommen "  " erschrak "  " sogleich " 
##  [6] " Begierde "  " erschrak "  " Zauberin "  " zornigem "  " schlecht " 
## [11] " Zauberin "  " erschien "  " Rapunzel "  "\nRapunzel " " schönste " 
## [16] " Zauberin "  " Zauberin "  "\nRapunzel " " Zauberin "  " wickelte " 
## [21] " Zauberin "  " Zauberin "  " Rapunzel "  " Zauberin "  " erschrak " 
## [26] " gewaltig "  " erblickt "  " erzählte "  " Rapunzel "  " jedesmal " 
## [31] " flechten "  " herunter "  " Zauberin "  " Rapunzel "  " schwerer " 
## [36] " Flechten "  " Rapunzel "  " Wüstenei "  " Rapunzel "  " Zauberin " 
## [41] " Flechten "  " giftigen "  " Rapunzel "  " liebsten "  " wanderte " 
## [46] " Rapunzel "  " erkannte "  " Rapunzel "
```

```r
# Mit stringr-Funktion
str_extract_all(maerchen_df$text[2], "\\s\\w{8}\\s")
```

```
## [[1]]
##  [1] " Hoffnung "  " Zauberin "  " bekommen "  " erschrak "  " sogleich " 
##  [6] " Begierde "  " erschrak "  " Zauberin "  " zornigem "  " schlecht " 
## [11] " Zauberin "  " erschien "  " Rapunzel "  "\nRapunzel " " schönste " 
## [16] " Zauberin "  " Zauberin "  "\nRapunzel " " Zauberin "  " wickelte " 
## [21] " Zauberin "  " Zauberin "  " Rapunzel "  " Zauberin "  " erschrak " 
## [26] " gewaltig "  " erblickt "  " erzählte "  " Rapunzel "  " jedesmal " 
## [31] " flechten "  " herunter "  " Zauberin "  " Rapunzel "  " schwerer " 
## [36] " Flechten "  " Rapunzel "  " Wüstenei "  " Rapunzel "  " Zauberin " 
## [41] " Flechten "  " giftigen "  " Rapunzel "  " liebsten "  " wanderte " 
## [46] " Rapunzel "  " erkannte "  " Rapunzel "
```

:::task
Verständnisfrage:

- Was bedeutet das Muster `\\s\\w{8}\\s`? Wonach wird gesucht? 

:::

### Lookarounds

Sogenannte Lookarounds kommen zur Anwendung, wenn nach einem Muster gesucht wird, das den Anfang oder das Ende von einem gesuchten String markiert, aber selbst nicht gematcht werden soll. 

  | Sequenz               |  Bedeutung                                   |
  |-----------------------|----------------------------------------------|
  | (?=foo)	Lookahead     | Asserts that what immediately follows the current position in the string is foo   |
  | (?<=foo) Lookbehind   | Asserts that what immediately precedes the current position in the string is foo  |
  | (?!foo)	Negative Lookahead  | Asserts that what immediately follows the current position in the string is not foo   |
  | (?<!foo)	Negative Lookbehind   | Asserts that what immediately precedes the current position in the string is not foo  |         


Quelle: https://www.rexegg.com/regex-lookarounds.html

Die stringr-Funktionen unterstützen per Default die Suche mithilfe von Lookarounds. Um Lookarounds im Zusammenhang mit R-base-Funktionen zu verwenden, muss beim Funktionsaufruf das zusätzliche Argument `perl = TRUE` übergeben werden.


```r
# Mit R-Basisfunktionen
regmatches(maerchen_df$text[2], gregexpr("(?<=\\s)\\w{8}(?=\\s)", maerchen_df$text[2], perl = TRUE))
```

```
## [[1]]
##  [1] "Hoffnung" "Zauberin" "bekommen" "erschrak" "sogleich" "Begierde"
##  [7] "erschrak" "Zauberin" "zornigem" "schlecht" "Zauberin" "erschien"
## [13] "sogleich" "Rapunzel" "Rapunzel" "Zauberin" "Zauberin" "Rapunzel"
## [19] "Zauberin" "wickelte" "Zauberin" "Zauberin" "Rapunzel" "Zauberin"
## [25] "erschrak" "Rapunzel" "gewaltig" "erblickt" "Rapunzel" "jedesmal"
## [31] "flechten" "herunter" "Zauberin" "Rapunzel" "schwerer" "Flechten"
## [37] "Rapunzel" "Rapunzel" "Zauberin" "Flechten" "giftigen" "Rapunzel"
## [43] "liebsten" "wanderte" "Rapunzel" "erkannte" "Rapunzel"
```

```r
# Mit stringr-Funktion
str_extract_all(maerchen_df$text[2], "(?<=\\s)\\w{8}(?=\\s)")
```

```
## [[1]]
##  [1] "Hoffnung" "Zauberin" "bekommen" "erschrak" "sogleich" "Begierde"
##  [7] "erschrak" "Zauberin" "zornigem" "schlecht" "Zauberin" "erschien"
## [13] "sogleich" "Rapunzel" "Rapunzel" "schönste" "Zauberin" "Zauberin"
## [19] "Rapunzel" "Zauberin" "wickelte" "Zauberin" "Zauberin" "Rapunzel"
## [25] "Zauberin" "erschrak" "Rapunzel" "gewaltig" "erblickt" "erzählte"
## [31] "Rapunzel" "jedesmal" "flechten" "herunter" "Zauberin" "Rapunzel"
## [37] "schwerer" "Flechten" "Rapunzel" "Wüstenei" "Rapunzel" "Zauberin"
## [43] "Flechten" "giftigen" "Rapunzel" "liebsten" "wanderte" "Rapunzel"
## [49] "erkannte" "Rapunzel"
```

:::task
Verständnisfrage:

- Wie unterscheiden sich die Ergebnisse der Suche mithilfe von Lookarounds von den Ergebnissen der Suche nach `\\s\\w{8}\\s` im vorigen Abschnitt? Warum ist das so? 

:::

## Regex für nicht-lateinische Schriften

Reguläre Ausdrücke können natürlich auch zur Suche in Texten mit nicht-lateinischen Schriften oder in multilingualen Texten verwendet werden: Entweder explizit, indem einfach das Muster in der entsprechenden Sprache formuliert wird, oder aber mithilfe der Unicode-Zeichenklasse `\p` (s.u.). 



```r
rapunzel_rus <- readtext("data/Колокольчик_(Гримм;_Полевой).txt")
regmatches(rapunzel_rus$text[1], gregexpr("Колокольчик", rapunzel_rus$text[1]))
```

```
## [[1]]
## [1] "Колокольчик" "Колокольчик" "Колокольчик" "Колокольчик" "Колокольчик"
## [6] "Колокольчик" "Колокольчик"
```

Zur Suche nach Mustern in verschiedenen Schriften kann auch eine spezielle Zeichenklasse für Unicode-Zeichen verwendet werden, `\p`. Diese Zeichenklasse erlaubt es, ein bestimmtes Alphabet zu spezifizieren, oder alphabetübergreifend nach einem Zeichen in allen in Unicode abgebildeten Schriften zu suchen. Die stringr-Funktionen unterstützen per Default die Suche mithilfe der Unicode-Zeichenklasse `\p`. Um diese Zeichenklasse im Zusammenhang mit R-base-Funktionen zu verwenden, muss beim Funktionsaufruf das zusätzliche Argument `perl = TRUE` übergeben werden.


```r
# Suche nach drei beliebigen Buchstaben aus dem kyrillischen Alphabet, angeführt und gefolgt von einem Leerzeichen
regmatches(rapunzel_rus$text[1], gregexpr("\\s\\p{Cyrillic}{3}\\s", rapunzel_rus$text[1], perl = TRUE))
```

```{style="max-height: 100px;"}
## [[1]]
##   [1] " муж "  " уже "  " лет "  " что "  " был "  " был "  " что "  " все " 
##   [9] " что "  " нее "  " она "  " так "  " она "  " что "  " она "  " муж " 
##  [17] " что "  " мне "  " что "  " мне "  " сад "  " это "  " так "  " что " 
##  [25] " еще "  " его "  " как "  " что "  " мой "  " Это "  " это "  " моя " 
##  [33] " тут "  " это "  " как "  " мне "  " нем "  " как "  " все "  " его " 
##  [41] " Бог "  " той "  " ней "  " как "  " она "  " ним "  " два "  " тем " 
##  [49] " ему "  " той "  " той "  " так "  " что "  "\nЭто " " она "  " это " 
##  [57] " так "  " его "  " что "  " лес "  " все "  " что "  " ней "  " как " 
##  [65] " ним "  " так "  " ним "  " ней "  " она "  " нею "  " как "  " как " 
##  [73] " тех "  " мог "  " нее "  " она "  " его "  " она "  " что "  " она " 
##  [81] " чем "  " она "  " ему "  "\nОна " " как "  " мне "  " мне "  " раз " 
##  [89] " она "  " ней "  "\nОни " " что "  " нее "  " что "  " ней "  " это " 
##  [97] " вас "  " Тот "  " миг "  " Что "  " что "  " все "  " она "  " ими " 
## [105] " два "  " что "  " где "  " тот "  " под "  " она "  " ним "  " там " 
## [113] " она "  " эта "  " уже "  " еще "  " что "  " тот "  " ему "  " Так " 
## [121] " лет "  " где "  " его "  " нее "  " ему "  " его "  " ему "  " Две " 
## [129] " ему "  " мог "  " все "  " как "  " где "  " они "
```

```r
# Suche nach drei beliebigen Buchstaben, egal aus welchem Alphabet, angeführt und gefolgt von einem Leerzeichen
regmatches(rapunzel_rus$text[1], gregexpr("\\s\\p{L}{3}\\s", rapunzel_rus$text[1], perl = TRUE))
```

```{style="max-height: 100px;"}
## [[1]]
##   [1] " муж "  " уже "  " лет "  " что "  " был "  " был "  " что "  " все " 
##   [9] " что "  " нее "  " она "  " так "  " она "  " что "  " она "  " муж " 
##  [17] " что "  " мне "  " что "  " мне "  " сад "  " это "  " так "  " что " 
##  [25] " еще "  " его "  " как "  " что "  " мой "  " Это "  " это "  " моя " 
##  [33] " тут "  " это "  " как "  " мне "  " нем "  " как "  " все "  " его " 
##  [41] " Бог "  " той "  " ней "  " как "  " она "  " ним "  " два "  " тем " 
##  [49] " ему "  " той "  " той "  " так "  " что "  "\nЭто " " она "  " это " 
##  [57] " так "  " его "  " что "  " лес "  " все "  " что "  " ней "  " как " 
##  [65] " ним "  " так "  " ним "  " ней "  " она "  " нею "  " как "  " как " 
##  [73] " тех "  " мог "  " нее "  " она "  " его "  " она "  " что "  " она " 
##  [81] " чем "  " она "  " ему "  "\nОна " " как "  " мне "  " мне "  " раз " 
##  [89] " она "  " ней "  "\nОни " " что "  " нее "  " что "  " ней "  " это " 
##  [97] " вас "  " Тот "  " миг "  " Что "  " что "  " все "  " она "  " ими " 
## [105] " два "  " что "  " где "  " тот "  " под "  " она "  " ним "  " там " 
## [113] " она "  " эта "  " уже "  " еще "  " что "  " тот "  " ему "  " Так " 
## [121] " лет "  " где "  " его "  " нее "  " ему "  " его "  " ему "  " Две " 
## [129] " ему "  " мог "  " все "  " как "  " где "  " они "
```

```r
# Suche mit stringr-Funktion str_extract_all()
str_extract_all(rapunzel_rus$text[1], "\\s\\p{L}{3}\\s")
```

```{style="max-height: 100px;"}
## [[1]]
##   [1] " муж "  " уже "  " лет "  " что "  " был "  " был "  " что "  " все " 
##   [9] " что "  " нее "  " она "  " так "  " она "  " что "  " она "  " муж " 
##  [17] " что "  " мне "  " что "  " мне "  " сад "  " это "  " так "  " что " 
##  [25] " еще "  " его "  " как "  " что "  " мой "  " Это "  " это "  " моя " 
##  [33] " тут "  " это "  " как "  " мне "  " нем "  " как "  " все "  " его " 
##  [41] " Бог "  " той "  " ней "  " как "  " она "  " ним "  " два "  " тем " 
##  [49] " ему "  " той "  " той "  " так "  " что "  "\nЭто " " она "  " это " 
##  [57] " так "  " его "  " что "  " лес "  " все "  " что "  " ней "  " как " 
##  [65] " ним "  " так "  " ним "  " ней "  " она "  " нею "  " как "  " как " 
##  [73] " тех "  " мог "  " нее "  " она "  " его "  " она "  " что "  " она " 
##  [81] " чем "  " она "  " ему "  "\nОна " " как "  " мне "  " мне "  " раз " 
##  [89] " она "  " ней "  "\nОни " " что "  " нее "  " что "  " ней "  " это " 
##  [97] " вас "  " Тот "  " миг "  " Что "  " что "  " все "  " она "  " ими " 
## [105] " два "  " что "  " где "  " тот "  " под "  " она "  " ним "  " там " 
## [113] " она "  " эта "  " уже "  " еще "  " что "  " тот "  " ему "  " Так " 
## [121] " лет "  " где "  " его "  " нее "  " ему "  " его "  " ему "  " Две " 
## [129] " ему "  " мог "  " все "  " как "  " где "  " они "
```


## Weiterführende Links {-}

* Wickham, Hadley, Çetinkaya-Rundel, Mine und Grolemund, Garrett (2023), *R for Data Science. Chapter 15: Regular Expressions*, https://r4ds.hadley.nz/regexps.
* Hase, Valerie (2021), *Text as Data Methods in R - Applications for Automated Analyses of News Content. Tutorial 9: Searching & Manipulating String Patterns*, https://bookdown.org/valerie_hase/TextasData_HS2021/tutorial-9-searching-manipulating-string-patterns.html.
* Regex Debugging: https://regex101.com/
* Erläuterungen zum Regex-Syntax: https://danielfett.de/2006/03/20/regulaere-ausdruecke-tutorial/
* Interaktives Tutorial zum Regex-Syntax: https://regexone.com/
* Video zu den R Base-Funktionen: https://www.youtube.com/watch?v=q8SzNKib5-4
* Lookarounds erläutert: https://www.regular-expressions.info/lookaround.html
* PCRE Cheatsheet: https://www.debuggex.com/cheatsheet/regex/pcre

