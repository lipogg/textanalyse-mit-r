# R Basics III: Kontrollstrukturen

Bisher haben wir in unseren Beispielen immer einzelne Anweisungen oder Ausdrücke (-> Grundbegriffe!) betrachtet. Wir haben dabei gelernt, dass der Computer Code ausführen und auswerten kann: Beim Ausführen "macht" er etwas und ändert den Zustand des Speichers, zum Beispiel, wenn eine neue Variable erstellt wird. Beim Auswerten berechnet er etwas und liefert einen Wert, zum Beispiel, wenn eine logische Aussage überprüft wird. Der Computer kann Anweisungen aber auch wiederholen oder Anweisungen nur dann ausführen, wenn eine bestimmte Bedingung erfüllt ist. In R gibt es eine Reihe von Signalwörtern, die dem Computer mitteilen, dass eine bestimmte Abfolge von Anweisungen (=**Anweisungsblock**) wiederholt werden soll oder nur unter einer bestimmten Bedingung ausgeführt werden soll. 


## Grundlegende Begriffe


|Begriff         |Englische Entsprechung |Definition (im R-Kontext)                                                                                                                                                                                                                                                                                                                                                                                           |
|:---------------|:----------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Anweisungsblock |Code block             |Mehrere aufeinanderfolgende Codezeilen, zum Beispiel im Körper einer Schleife oder einer bedingten Anweisung.                                                                                                                                                                                                                                                                                                       |
|Iteration       |Iteration              |Im Kontext von Kontrollstrukturen bezeichnet Iteration zwei Dinge: in einem allgemeinen Sinn den Prozess, bei dem Anweisungen wiederholt ausgeführt werden, und zum anderen einen einzigen Schleifendurchlauf, also das einmalige Ausführen der Anweisungen im Schleifenkörper. Wenn der Code einmal ausgeführt wird, sagt man entsprechend 'eine Iteration', für das zweimalige Ausführen 'zwei Iterationen', usw. |
|Iterieren       |Iterate                |Wenn beschrieben werden soll, dass eine for-Schleife ein Objekt durchläuft, wird dazu häufig gesagt, dass die Schleife 'über das Objekt iteriert'.                                                                                                                                                                                                                                                                  |
|Terminieren     |Terminate              |Wenn eine Schleife terminiert bedeutet das, dass sie die Ausführung beendet; sie kommt zum Ende.                                                                                                                                                                                                                                                                                                                    |


:::tip
Style Tip

In Funktionsaufrufen, also z.B. `print()`, und in Zugriffsoperationen, also z.B. `var[]`, steht bekanntlich kein Leerzeichen vor der Klammer. Bei Kontrollstrukturen ist das anders. Hier empfiehlt Wickham, nach den Signalwörtern `if`, `if else`, `for` und `while` stets ein Leerzeichen einzufügen, also z.B. `if (var == 20)`. 
:::


## Bedingte Anweisungen 

Bedingte Anweisungen, oder if-Anweisungen, haben die allgemeine Form: 



```r
if (Bedingung) {
  Anweisungsblock
}

if (Bedingung) {
  Anweisungsblock
  if (Bedingung) {
    Anweisungsblock
  }
}
```

Beispiel:


```r
# Nachricht wird ausgegeben, wenn der Vektor autos ein Element mit dem Wert "Mercedes" enthält

autos <- c("Mercedes", "Fiat", "Volvo", "BMW")
if ("Mercedes" %in% autos) {
  print("Ein Mercedes steht in der Garage!")
}
```

```
## [1] "Ein Mercedes steht in der Garage!"
```

```r
# Nachricht wird ausgegeben, wenn der Wert, der dem Schlüssel "Mercedes" im Vektor autos zugeordnet ist, größer als 1 ist

autos <- c(Mercedes=2, Fiat=1, Volvo=1, BMW=3)
anzahl_mercedes <- autos["Mercedes"]
if (anzahl_mercedes > 1) {
  print(paste("Du hast", anzahl_mercedes, "Mercedes!"))
}
```

```
## [1] "Du hast 2 Mercedes!"
```

:::tip
Mit der Funktion `paste()` können character verkettet werden. Dabei werden automatisch  Leerzeichen zwischen den zu verkettenden charactern eingefügt. Wenn character ohne Leerzeichen verkettet werden sollen, kann stattdessen die Funktion `paste0()` verwendet werden. 
:::


```r
# Wenn der Vektor autos einen Schlüssel "Mercedes" enthält, wird überprüft, ob der Wert, der diesem Schlüssel zugeordnet ist, größer als 1 ist.
# Falls ja, wird eine Nachricht ausgegeben.

autos <- c(Mercedes=2, Fiat=1, Volvo=1, BMW=3)
if ("Mercedes" %in% names(autos)) {
  anzahl_mercedes <- autos["Mercedes"]
  if (anzahl_mercedes > 1) {
    print(paste("Du hast", anzahl_mercedes, "Mercedes!"))
  }
}
```

```
## [1] "Du hast 2 Mercedes!"
```


:::task
Verständnisfragen: 

- Welche Datenstruktur hat das Objekt `autos` im zweiten und dritten Beispiel? 
- Wie müsste man den Code im ersten Beispiel ändern, sodass eine Nachricht ausgegeben wird, wenn sowohl ein Mercedes als auch ein Fiat in der Garage stehen?  

:::

Bedingungen können auch kombiniert werden. Dazu werden einfach zwei logische Ausdrücke mithilfe der logischen Operatoren `&&` und `||` verknüpft:



```r
# Beide Ausdrücke sind wahr:  die Nachricht wird ausgegeben
autos <- c("Mercedes", "Fiat", "Volvo", "BMW")
if ("Mercedes" %in% autos && "Fiat" %in% autos) {
  print("Ein Mercedes und ein Fiat stehen in der Garage!")
}
```

```
## [1] "Ein Mercedes und ein Fiat stehen in der Garage!"
```

```r
# Einer der Ausdrücke ist falsch: es passiert nichts
autos <- c("Mercedes", "Fiat", "Volvo", "BMW")
if ("Mercedes" %in% autos && "Opel" %in% autos) {
  print("Ein Mercedes und ein Opel stehen in der Garage!")
}
```


```r
# Einer der Ausdrücke ist falsch: die Nachricht wird ausgegeben
autos <- c("Mercedes", "Fiat", "Volvo", "BMW")
if ("Mercedes" %in% autos || "Opel" %in% autos) {
  print("Ein Mercedes oder ein Opel steht in der Garage!")
}
```

```
## [1] "Ein Mercedes oder ein Opel steht in der Garage!"
```



## Verzweigungen

Es wird unterschieden zwischen einfachen und mehrfachen Verzweigungen.

**Einfache Verzweigungen**, oder if-else-Anweisungen, haben die allgemeine Form:


```r
if (Bedingung) {
  Anweisungsblock
} else {
  Anweisungsblock
}
```

Beispiel: 


```r
# Wenn der Wert, der dem Schlüssel "Ibuprofen" zugeordnet ist, größer 0 ist, wird eine Nachricht ausgegeben.
# Wenn der Wert 0 (oder im theoretischen Fall kleiner 0) ist, wird eine Warnung ausgegeben.

medikamente <- c(Ibuprofen=30, Paracetamol=23, Aspirin=28)
anzahl_ibus <- medikamente["Ibuprofen"]

if (anzahl_ibus > 0) {
  print(paste("Du hast", anzahl_ibus, "Ibuprofen!"))
} else {
  print("Warnung: kein Ibuprofen mehr da!")
}
```

```
## [1] "Du hast 30 Ibuprofen!"
```

Wenn mehr als zwei Fälle überprüft werden sollen, kann eine if-else-Anweisung mit beliebig vielen zusätzlichen bedingten Anweisungen kombiniert werden. **Mehrfache Verzweigungen** haben die allgemeine Form:


```r
if (Bedingung) {
  Anweisungsblock
} else if (Bedingung) {
  Anweisungsblock
} else {
  Anweisungsblock
}
```


```r
# Wenn der Wert, der dem Schlüssel "Ibuprofen" zugeordnet ist, größer als 5 ist, wird eine Nachricht mit dem Wert ausgegeben.
# Wenn der Wert genau dem Integer 5 entspricht, wird eine Warnung ausgegeben.
# In allen anderen Fällen wird die Nachricht "Achtung! Weniger als 5 Ibuprofen da!" ausgegeben.

medikamente <- c(Ibuprofen=30, Paracetamol=23, Aspirin=28)
anzahl_ibus <- medikamente["Ibuprofen"]

if (anzahl_ibus > 5) {
  print(paste("Du hast", anzahl_ibus, "Ibuprofen!"))
} else if (anzahl_ibus == 5) {
  print("Warnung: Nur noch 5 Ibuprofen da!")
} else {
  print("Achtung! Weniger als 5 Ibuprofen da!")
}
```

```
## [1] "Du hast 30 Ibuprofen!"
```

:::task
Verständnisfragen: 

- Wann wird der Anweisungsblock nach `else` ausgeführt? Welche Bedingung muss erfüllt sein? 

:::


Achtung: Wenn zwei Bedingungen in einer Verzweigung zugleich erfüllt sind, wird nur die bedingte Anweisung ausgeführt, die zuerst überprüft wurde, die also "weiter oben" im Skript steht. Zur Illustration schauen wir uns nochmal unser Auto-Beispiel an. 



```r
# Sowohl die if-Bedingung als auch die if else-Bedingung werden zu True evaluiert. 
# Allerdings wird die if else-Bedingung nie ausgewertet. 
# Es wird also nur die if-Anweisung ausgeführt. 

autos <- c("Mercedes", "Fiat", "Volvo", "BMW")

if ("Mercedes" %in% autos) {
  print("Ein Mercedes steht in der Garage!")
} else if ("Fiat" %in% autos) {
  print("Ein Fiat steht in der Garage!")
} else {
  print("Weder ein Mercedes noch ein Fiat in der Garage.")
}
```

```
## [1] "Ein Mercedes steht in der Garage!"
```

Dieses Verhalten gilt allerdings nur für Verzweigungen, nicht jedoch für zwei if-Anweisungen, die aufeinander folgen: 


```r
# Beide if-Bedingungen werden nacheinander ausgewertet. Im ersten Schleifendurchlauf
# werden beide zu True evaluiert und es werden beide Anweisungen ausgeführt.

autos <- c("Mercedes", "Fiat", "Volvo", "BMW")

if ("Mercedes" %in% autos) {
  print("Ein Mercedes steht in der Garage!")
} 
```

```
## [1] "Ein Mercedes steht in der Garage!"
```

```r
if ("Fiat" %in% autos) {
  print("Ein Fiat steht in der Garage!")
} 
```

```
## [1] "Ein Fiat steht in der Garage!"
```

Eine spezielle Form von Verzweigung sind **bedingte Ausdrücke**. Diese werden verwendet, um einer Variable in Abhängigkeit von einer anderen Variable einen Wert zuzuweisen. Bedingte Ausdrücke stellen eine Kurzschreibweise für die folgende Verzweigung dar:


```r
# Kompliziert
x <- 2
if (x == 2) {
  var <- 10
} else {
  var <- 20
}
```

```r
# Einfach
x <- 2
var <- if (x == 2) 10 else 20

# Alternative (macht dasselbe): Die ifelse-Funktion
var <- ifelse(x == 2, 10, 20)
```



## while-Schleifen

while-Schleifen werden verwendet, wenn ein Code-Block (=nur ein anderes Wort für Anweisungsblock) so lange ausgeführt werden soll, wie eine bestimmte Bedingung erfüllt ist. Die Bedingung steht im **Schleifenkopf** und wird überprüft, bevor der Code im **Schleifenkörper** ausgeführt wird. Wenn die Bedingung nicht (mehr) erfüllt ist, **terminiert** die Schleife. `while`-Schleifen haben die allgemeine Form:


```r
while (Bedingung) {
  Anweisungsblock
} 
```

Beispiel: 



```r
# In jedem Schleifendurchlauf wird zunächst der Ausdruck anzahl_ibus > 0 ausgewertet.
# Wenn der Ausdruck zu True evaluiert wird, wird die Anweisung im Schleifenkörper ausgeführt, d.h. der Wert der Variable anzahl_ibus wird um 1 verringert.
# Wenn die Variable anzahl_ibus den Wert 0 annimmt, wird der Ausdruck im Schleifenkopf zu False evaluiert und die Schleife terminiert.
# Nach dem Terminieren der Schleife wird eine Warnung ausgegeben.


medikamente <- c(Ibuprofen=30, Paracetamol=23, Aspirin=28)
anzahl_ibus <- medikamente["Ibuprofen"]

while (anzahl_ibus > 0) {
  anzahl_ibus <- anzahl_ibus - 1
}

print("Warnung: kein Ibuprofen mehr da!")
```

```
## [1] "Warnung: kein Ibuprofen mehr da!"
```


:::task
Verständnisfragen: 

- Nach wie vielen Schleifendurchläufen wird die Meldung "Achtung! Kein Ibuprofen mehr da!" ausgegeben? 
- Nach wie vielen Schleifendurchläufen terminiert die Schleife? 

:::


Es ist auch möglich, `while`-Schleifen und bedingte Anweisungen zu kombinieren:


```r
# In jedem Schleifendurchlauf wird zunächst der Ausdruck anzahl_ibus > 0 ausgewertet.
# Wenn der Ausdruck zu True evaluiert wird, wird überprüft, ob der Wert dem Integer 5 entspricht.
# Falls ja, wird eine Warnmeldung ausgegeben.
# Danach wird der Wert der Variable anzahl_ibus um 1 verringert.
# Wenn die Variable anzahl_ibus den Wert 0 annimmt, wird der Ausdruck im Schleifenkopf zu False evaluiert und die Schleife terminiert.
# Nach dem Terminieren der Schleife wird eine weitere Warnung ausgegeben.

medikamente <- c(Ibuprofen=30, Paracetamol=23, Aspirin=28)
anzahl_ibus <- medikamente["Ibuprofen"]

while (anzahl_ibus > 0) {
  if (anzahl_ibus == 5) {
    print("Warnung: nur noch 5 Ibuprofen da!")
  }
  anzahl_ibus <- anzahl_ibus - 1
}
```

```
## [1] "Warnung: nur noch 5 Ibuprofen da!"
```

```r
print("Achtung! Kein Ibuprofen mehr da!")
```

```
## [1] "Achtung! Kein Ibuprofen mehr da!"
```

:::task
Verständnisfrage: 

- Wir wollen sichergehen, dass die Warnung wirklich im 26. Schleifendurchlauf ausgegeben wird. Wie könnten wir den Code ändern, damit in jedem Schleifendurchgang zusätzlich ein Wert ausgegeben wird? 

:::

Achtung: while-Schleifen laufen unendlich lange, wenn die Abbruchbedingung nie erfüllt ist:


```r
# while (TRUE) {
#   print("Dies ist eine unendliche Schleife")
#}
```

:::tip
Falls ihr versehentlich doch mal eine unendliche Schleife im RStudio ausgeführt haben solltet, könnt ihr die Ausführung der Schleife im RStudio abbrechen. Dazu könnt ihr entweder auf das rote Stopp-Zeichen oben rechts im Konsole-Fenster klicken oder ihr gebt dir Tastenkombination `CTRL + C` (Mac) bzw. `Escape` (Windows) ein.
:::

## for-Schleifen

`for`-Schleifen werden verwendet, um **iterierbare Objekte** zu durchlaufen. Listen und Vektoren sind in R iterierbar; und weil Matrizen und Dataframes aus Vektoren bzw. Listen bestehen, sind auch Matrizen und Dataframes iterierbar. Iterieren bedeutet einfach "durchlaufen". Im Kopf der for-Schleife steht anders als bei der while-Schleife keine Bedingung, sondern es wird eine Variable festgelegt, die als Platzhalter für den Wert der Elemente des Objekts dient. Diese Variable nennt man auch **Laufvariable**. Die for-Schleife “durchläuft” also im Grunde das Objekt, indem in jedem Schleifendurchlauf die Laufvariable den Wert des aktuellen Elements annimmt – und das so lange, bis es keine weiteren Elemente mehr gibt. Mithilfe des Variablennamens kann im Schleifenkörper auf den Wert des aktuellen Elements zugegriffen werden. Einen Schleifendurchlauf nennt man auch eine **Iteration**.

`for`-Schleifen haben in R die allgemeine Form: 


```r
for (Element in Objekt) {
  Anweisungsblock
}
```

Beispiel: 


```r
# In jedem Schleifendurchlauf nimmt die Laufvariable zahl den Wert eines Elements aus dem
# Vektor zahlen an und es wird der aktuelle Wert der Variable zahl, addiert mit 1, ausgegeben.
woerter <- c("Netzwerkdurchsetzungsgesetz", "Abfallverzeichnisverordnung", "Haftpflichtversicherung", "Antivirenprogramm")

for (wort in woerter) {
  print(paste(wort, nchar(wort)))
}
```

```
## [1] "Netzwerkdurchsetzungsgesetz 27"
## [1] "Abfallverzeichnisverordnung 27"
## [1] "Haftpflichtversicherung 23"
## [1] "Antivirenprogramm 17"
```
:::tip
Mithilfe der Funktion `nchar()` kann die Anzahl der Zeichen in einer Zeichenkette (=character) bestimmt werden. 
:::

Wie while-Schleifen können auch for-Schleifen mit bedingten Anweisungen kombiniert werden:


```r
# In jedem Schleifendurchlauf nimmt die Laufvariable wort den Wert eines Elements aus dem Vektor woerter (s.o.) an.
# Zunächst wird die Zeichenanzahl des aktuellen Strings der Variable laenge als Wert zugewiesen.
# Dann wird geprüft, ob der Wert der Variable zeichen größer als 20 ist.
# Falls ja, wird der Liste lange_woerter ein Element mit dem aktuellen Wert der Variable wort als Schlüssel und der Zeichenanzahl als Wert hinzugefügt.
# Wenn der Vektor woerter keine weiteren Elemente mehr enthält, terminiert die Schleife.
# Danach wird die Liste lange_woerter ausgegeben.
woerter <- c("Netzwerkdurchsetzungsgesetz", "Abfallverzeichnisverordnung", "Haftpflichtversicherung", "Antivirenprogramm")
lange_woerter <- list()

for (wort in woerter) {
  zeichen <- nchar(wort)
  if (zeichen > 20) {
    lange_woerter[[wort]] <- zeichen
  }
}

print(lange_woerter)
```

```
## $Netzwerkdurchsetzungsgesetz
## [1] 27
## 
## $Abfallverzeichnisverordnung
## [1] 27
## 
## $Haftpflichtversicherung
## [1] 23
```

Es muss jedoch beachtet werden, dass Elemente eines iterierbaren Objekts im Laufe einer Iteration nicht direkt verändert werden können:


```r
tiere <- c("Hund", "Elefant", "Igel", "Katze")

for (tier in tiere) {
  tier <- tolower(tier)
}

# Änderungen wurden nicht übernommen 
print(tiere)
```

```
## [1] "Hund"    "Elefant" "Igel"    "Katze"
```
:::tip
Mithilfe der Funktion `tolower()` können Buchstaben in einer Zeichenkette (=character) automatisch in Kleinbuchstaben umgewandelt werden. 
:::

Das liegt daran, dass die Laufvariable nur für den Wert des aktuellen Elements steht, nicht für dieses Element selbst. Die Laufvariable haben wir also eigentlich bisher als **"Wertvariable"** verwendet. 

**Um Elemente im Schleifendurchlauf zu verändern, muss mithilfe der Indexposition direkt auf ein Element aus dem Vektor oder der Liste zugegriffen werden**, also genau so, wie wir bisher auch auf Elemente zugegriffen haben. Doch mit jedem Schleifendurchlauf nimmt die Laufvariable ja den Wert eines anderen Elements an; die Indexposition des aktuellen Elements ändert sich also fortlaufend. Um auf Elemente in einem Schleifendurchlauf zuzugreifen, brauchen wir also eine Variable, die nicht den Wert des aktuellen Elements annimmt, sondern dessen Indexposition.

`for`-Schleifen werden deswegen in R meist als sogennante **Zählschleife** verwendet. Dazu wird die Funktion `seq_along()` verwendet, die einen Integer-Vektor erstellt, der genau so lang ist, wie das Objekt, das der Funktion übergeben wird. Solche `for`-Schleifen haben die allgemeine Form:


```r
for (i in seq_along(Objekt)) {
  Anweisungsblock
}
```

Das `i` steht für **iterator** und dient als **Zählvariable**: mit jedem Schleifendurchlauf (Iteration) wird i um Eins erhöht; der Zählbeginn ist 1. In jeder Iteration wird auf das Element `i` des iterierbaren Objekts zugegriffen. Mithilfe des Iterators `i` kann also in jeder Iteration auf ein Element aus dem Objekt zugegriffen werden.

Klingt kompliziert, wird aber am folgenden Beispiel direkt deutlich:  


```r
tiere <- c("Hund", "Elefant", "Igel", "Katze")

for (i in seq_along(tiere)) {
  tiere[i] <- tolower(tiere[i])
}

# Änderungen wurden übernommen 
print(tiere)
```

```
## [1] "hund"    "elefant" "igel"    "katze"
```


:::task
Verständnisfragen: 

- Welchen Wert nimmt `i` im ersten Schleifendurchlauf an? Auf welches Element wird zugegriffen?
- Welchen Wert nimmt `i` im dritten Schleifendurchlauf an? Auf welches Element wird zugegriffen? 
- Kann man eine `for`-Schleife auch dazu verwenden, um über die Spalten einer Matrix / eines Dataframes zu iterieren?  
- Was macht die Funktion `seq_along()`?
- Wann empfiehlt es sich, `seq_along()` in Verbindung mit einer `for`-Schleife zu verwenden?
:::

Auch Zählschleifen können mit bedingten Anweisungen oder Verzweigungen kombiniert werden: 


```r
tiere <- c("Hund", "Elefant", "Igel", "Katze")

for (i in seq_along(tiere)) {
  if (tiere[i] == "Hund") {
    tiere[i] <- tolower(tiere[i])
  } else {
    tiere[i] <- toupper(tiere[i])
  }
}

# Änderungen wurden übernommen 
print(tiere)
```

```
## [1] "hund"    "ELEFANT" "IGEL"    "KATZE"
```

Im Beispiel oben haben wir mehrmals `tiere[i]` verwendet. Falls wir uns irgendwann später dazu entscheiden sollten, die Variable `tiere` umzubenennen, müssten wir alle Vorkommnisse von `tiere[i]` im Schleifenkörper finden und austauschen. So entstehen schnell Fehler. Um das Problem zu umgehen, könnten wir jetzt auf die Idee kommen, eine "temporäre" Variable zu verwenden, der am Anfang jedes Schleifendurchlaufs das aktuelle Element zugewiesen wird:  


```r
tiere <- c("Hund", "Elefant", "Igel", "Katze")

for (i in seq_along(tiere)) {
  tier <- tiere[i]
  if (tier == "Hund") {
    tier <- tolower(tier)
  } else {
    tier <- toupper(tier)
  }
}

# Änderungen wurden nicht übernommen! 
print(tiere)
```

```
## [1] "Hund"    "Elefant" "Igel"    "Katze"
```

Aber wenn wir uns am Ende den Vektor `tiere` ausgeben lassen, sehen wir, dass wir den Vektor gar nicht verändert haben! Woran liegt das? 

:::task
Verständnisfragen: 

- Warum wird der Vektor `tiere` durch das Einfügen einer temporären Variable nicht mehr verändert? 
- Wie könnten wir den Code im Beispiel umschreiben, um das Problem zu beheben? 
:::

Der Grund für dieses Verhalten liegt wieder in der Unveränderbarkeit der Objekte in R. 

Um das Problem zu beheben, gibt es verschiedene Möglichkeiten. Die Änderungen, die am Objekt `tier` vorgenommen wurden, könnten zum Beispiel am Ende der Schleife wieder dem Objekt `tiere[i]` zugewiesen werden:


```r
tiere <- c("Hund", "Elefant", "Igel", "Katze")

for (i in seq_along(tiere)) {
  tier <- tiere[i]
  if (tier == "Hund") {
    tier <- tolower(tier)
  } else {
    tier <- toupper(tier)
  }
  # Wert der Variable tier wieder dem Element tiere[i] zuweisen
  tiere[i] <- tier
}

# Änderungen wurden übernommen! 
print(tiere)
```

```
## [1] "hund"    "ELEFANT" "IGEL"    "KATZE"
```

Alternativ kann auch der ursprüngliche Vektor beibehalten werden und die Änderungen in jedem Schleifendurchgang einem neuen Vektor `tiere_neu()` hinzuzufügt werden. Dafür kann in diesem Fall eine einfache for-Schleife verwendet werden:  


```r
tiere <- c("Hund", "Elefant", "Igel", "Katze")
tiere_neu <- c() # leeren Vektor erstellen

for (tier in tiere) {
  if (tier == "Hund") {
    tier <- tolower(tier)
  } else {
    tier <- toupper(tier)
  }
  # Wert der Variable tier als neues Element dem Vektor tiere_neu hinzufügen
  tiere_neu <- c(tiere_neu, tier)
}

print(tiere_neu)
```

```
## [1] "hund"    "ELEFANT" "IGEL"    "KATZE"
```

```r
print(tiere) # Vektor tiere ist unverändert
```

```
## [1] "Hund"    "Elefant" "Igel"    "Katze"
```


## Schleifen abbrechen

Es ist zudem möglich, einen Schleifendurchlauf (Iteration) oder die gesamte Schleife abhängig von einer Bedingung frühzeitig abzubrechen. Eine `next`-Anweisung kann verwendet werden, um den aktuellen Schleifendurchlauf abzubrechen und direkt zum Schleifenkopf zu springen (= Iterationsabbruch). Eine `break`-Anweisung wird verwendet, um die Schleife komplett abzubrechen (= Schleifenabbruch). Das kann nützlich sein, wenn man fehlende Werte in einem Objekt hat und diese überspringen möchte, damit es zu keiner Fehlermeldung kommt. 

Die Signalworte `next` und `break` können sowohl mit while-Schleifen als auch mit for-Schleifen verwendet werden. Hier ein Beispiel mit einer for-Schleife:


```r
# next Anweisung

# Vektor mit NA-Werten
beliebige_woerter <- c("Baum", "Wald", NA, "Meer", NA, "Sonne")

# Funktion is.na() überprüft, ob der Wert des Elements wort NA ist
for (wort in beliebige_woerter) {
  if (is.na(wort)) { 
    next
  } else {
    print(wort)
  }
}
```

```
## [1] "Baum"
## [1] "Wald"
## [1] "Meer"
## [1] "Sonne"
```


```r
# break Anweisung

# Vektor mit NA-Werten
beliebige_woerter <- c("Baum", "Wald", NA, "Meer", NA, "Sonne")

# Funktion is.na() überprüft, ob der Wert des Elements wort NA ist
for (wort in beliebige_woerter) {
  if (is.na(wort)) { 
    break
  } else {
    print(wort)
  }
}
```

```
## [1] "Baum"
## [1] "Wald"
```


:::tip
Schleifen in R

Für die meisten Anwendungen verwendet man in R eher `for`-Schleifen als `while`-Schleifen. In vielen Fällen sind Schleifen aber sogar überhaupt nicht notwendig. In der nächsten Stunde werden wir sehen, dass Schleifen in vielen Fällen durch bestimmte Funktionen ersetzt werden können. Da R im Kern eine sogenannte "funktionale" Programmiersprache ist, werden Schleifen weniger verwendet als in anderen Programmiersprachen.  
:::



## Quellen {-}

* Venables, W.N. and Smith, D.M. and the R Core Team. An Introduction to R, [https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf](https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf)
* Wickham, Hadley. The Tidyverse Style Guide, [https://style.tidyverse.org/](https://style.tidyverse.org/)
* Reussner, Ralf H. Mitschnitt zur Vorlesung "Programmieren" im WiSe 2015/2016. 03: Kontrollstrukturen, [https://www.youtube.com/watch?v=-ZvrABPvUdA](https://www.youtube.com/watch?v=-ZvrABPvUdA)
* Wickham, Hadley and Grolemund, Garrett. R for Data Science: Iteration, [https://r4ds.had.co.nz/iteration.html](https://r4ds.had.co.nz/iteration.html)
