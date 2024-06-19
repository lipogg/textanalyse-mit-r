# Textanalyse II: Preprocessing

Im Kapitel "Textanalyse I" haben wir bereits die grundlegenden Quanteda-Objekte kennengelernt: korpus-Objekte, tokens-Objekte und DFM-Objekte. Bei der Erstellung des tokens-Objekts wurden die Texte tokenisiert, also in einzelne Tokens zerlegt. Vor und/oder nach der Tokenisierung erfolgen häufig noch weitere Operationen zur Bereinigung und Vorbereitung der Texte, die wir im folgenden kennenlernen werden. Solche Operationen zur Vorbereitung und Bereinigung von Texten zur Analyse werden allgemein **Preprocessing** genannt. Dazu gehören z.B. die Entfernung von Satzzeichen und von bestimmten Wörtern, die sehr häufig vorkommen (sogenannte "Stoppwörter"), die Umwandlung aller Tokens in Kleinbuchstaben, die sogenannte "Lemmatisierung" der Texte oder die manuelle Bereinigung einzelner Tokens mithilfe von speziellen Ausdrücken, die sich "Reguläre Ausdrücke" (oder engl. "Regular Expressions") nennen. Im Folgenden schauen wir uns diese und einige weitere Preprocessing-Schritte am Beispiel unseres Korpus deutschsprachiger belletristischer Texte an. 

Welche Preprocessing-Schritte im Einzelnen durchgeführt werden, hängt vom Kontext, von der Qualität der Texte und von der Forschungsfrage ab. Für manche Forschungsfragen kann es z.B. interessant sein, manche Stoppwörter beizubehalten oder zusätzliche Wörter zu entfernen. In anderen Fällen soll dagegen vielleicht mit den Grundformen der Wörter (Lemma) gearbeitet werden; die Texte müssen also "lemmatisiert" werden. 


:::tip
Der Pipe-Operator `%>%`

In den Beispielen in diesem Kapitel kommt manchmal der sogenannte Pipe-Operator `%>%` vor. Diesen Operator habt ihr bereits im Kapitel ["R Basics IV: Funktionen und Pakete"](https://lipogg.github.io/textanalyse-mit-r/r-basics-iv-funktionen-und-pakete.html#wozu-werden-pakete-verwendet) kurz kennengelernt. Der Pipe-Operator wird verwendet, um mehrere Funktionsaufrufe miteinander zu verketten. Dabei übernimmt die nachfolgende Funktion als erstes Argument jeweils den Rückgabewert der vorhergehenden Funktion. Im folgenden Beispiel übergibt der Pipe-Operator der Funktion `paste0()` das Objekt `satz` als Argument. Die `paste0()`-Funktion fügt an den Satz ein Fragezeichen an und übergibt die bearbeitete Zeichenkette "Hallo, wie geht es dir?" an die Funktion `strsplit()`. Die Funktion `strsplit()` teilt dann den Satz anhand der Leerzeichen in einzelne Einheiten auf und gibt einen character-Vektor zurück. Dieser character-Vektor wird zuletzt der Variable `woerter` zugewiesen. 

```
satz <- "Hallo, wie geht es dir"

woerter <- satz %>%
  paste0("?") %>%
  strsplit(" ")
```

Ein Ausdruck der Art `x %>% f` ist also äquivalent zu `f(x)`.

:::




## Tokenisieren und segmentieren

Das Tokenisieren, also das Zerlegen von Zeichenketten in Tokens, haben wir schon kennengelernt. Wir schauen uns als Beispiel wieder den Beispielsatz aus dem letzten Übungsblatt an, mit ein paar Zusätzen:


```r
library(quanteda)

beispiel_1 <- "Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322. Meine E-Mail-Adresse ist niro@gmail.com und ich bin geboren am 02-04-1965. #callme"

beispiel_toks <- tokens(beispiel_1)
print(beispiel_toks, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "Hallo"          "mein"           "Name"           "ist"           
##  [5] "Mr"             "."              "Robert"         "De"            
##  [9] "Niro"           "und"            "das"            "ist"           
## [13] "meine"          "Telefonnummer"  ":"              "0164-452954322"
## [17] "."              "Meine"          "E-Mail-Adresse" "ist"           
## [21] "niro@gmail.com" "und"            "ich"            "bin"           
## [25] "geboren"        "am"             "02-04-1965"     "."             
## [29] "#callme"
```

In dem Beispielsatz werden alle Sinneinheiten richtig als Tokens erkannt. Aber was passiert, wenn z.B. die Telefonnummer und das Geburtsdatum etwas anders aussehen und anstelle eines Trennstrichs ein Schrägstrich verwendet wird? 


```r
beispiel_2 <- "Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164/452954322. Meine E-Mail-Adresse ist niro@gmail.com und ich bin geboren am 02/04/1965. #callme"

beispiel_toks <- tokens(beispiel_2)

print(beispiel_toks, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "Hallo"          "mein"           "Name"           "ist"           
##  [5] "Mr"             "."              "Robert"         "De"            
##  [9] "Niro"           "und"            "das"            "ist"           
## [13] "meine"          "Telefonnummer"  ":"              "0164"          
## [17] "/"              "452954322"      "."              "Meine"         
## [21] "E-Mail-Adresse" "ist"            "niro@gmail.com" "und"           
## [25] "ich"            "bin"            "geboren"        "am"            
## [29] "02"             "/"              "04"             "/"             
## [33] "1965"           "."              "#callme"
```

Dann werden die Telefonnummer und das Geburtsdatum nicht mehr als Sinneinheiten erkannt. In diesem Fall gibt es zwei Möglichkeiten: Entweder die Tokenisierungsregeln werden angepasst, oder die Tokens, die falsch erkannt werden, werden vor dem Tokenisieren so bearbeitet, dass sie nach den bestehenden Tokenisierungsregeln als Sinneinheit erkannt werden. Die manuelle Anpassung der Tokenisierungsregeln ist recht komplex und würde den Rahmen etwas sprengen; ihr könnt allerdings bei Interesse  [hier](https://quanteda.io/reference/tokenize_custom.html) nachlesen, wie das geht. Wir schauen uns stattdessen an, wie die Tokens so bearbeitet werden können, dass sie richtig erkannt werden. In unserem Beispiel geht das ganz einfach mit der R-Basisfunktion `gsub()`: 


```r
beispiel_2 <- gsub("/", "-", beispiel_2)
beispiel_toks <- tokens(beispiel_2) 
print(beispiel_toks, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "Hallo"          "mein"           "Name"           "ist"           
##  [5] "Mr"             "."              "Robert"         "De"            
##  [9] "Niro"           "und"            "das"            "ist"           
## [13] "meine"          "Telefonnummer"  ":"              "0164-452954322"
## [17] "."              "Meine"          "E-Mail-Adresse" "ist"           
## [21] "niro@gmail.com" "und"            "ich"            "bin"           
## [25] "geboren"        "am"             "02-04-1965"     "."             
## [29] "#callme"
```

Wenn dagegen aus irgendeinem Grund die Telefonnummer und das Geburtsdatum nicht als Sinneinheit behandelt werden sollen, sondern die Zeichen jeweils eigene Tokens bilden sollen, kann dagegen einfach beim Aufruf der `tokens()`-Funktion das zusätzliche Argument `split_hyphens` übergeben werden:


```r
beispiel_toks <- tokens(beispiel_1, split_tags = TRUE, split_hyphens = TRUE )
print(beispiel_toks, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "Hallo"          "mein"           "Name"           "ist"           
##  [5] "Mr"             "."              "Robert"         "De"            
##  [9] "Niro"           "und"            "das"            "ist"           
## [13] "meine"          "Telefonnummer"  ":"              "0164"          
## [17] "-"              "452954322"      "."              "Meine"         
## [21] "E"              "-"              "Mail"           "-"             
## [25] "Adresse"        "ist"            "niro@gmail.com" "und"           
## [29] "ich"            "bin"            "geboren"        "am"            
## [33] "02"             "-"              "04"             "-"             
## [37] "1965"           "."              "#"              "callme"
```

Zeichenketten können nicht nur wie bisher in Wörter und andere Äußerungen zerlegt werden. Manchmal ist es sinnvoll, Texte in kleinere oder größere Einheiten zu zerlegen, also z.B. in einzelne Zeichen oder einzelne Sätze. Wenn Texte in größere Segmente wie Sätze zerlegt werden, nennt man diese Operation **Segmentieren** (oder engl. "segmentation"). Dazu kann ebenfalls die Funktion `tokens()` verwendet werden, mit dem zusätzlichen Argument `what = "character"` bzw. `what = "sentence"`: 


```r
# Segmentieren auf Satzebene
beispiel_segments <- tokens(beispiel_1, what = "sentence")
print(beispiel_segments, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
## [1] "Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322."
## [2] "Meine E-Mail-Adresse ist niro@gmail.com und ich bin geboren am 02-04-1965. #callme"
```

```r
# Tokenisieren auf Zeichenebene
beispiel_chars <-  tokens(beispiel_1, what = "character")
print(beispiel_chars, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##   [1] "H" "a" "l" "l" "o" "m" "e" "i" "n" "N" "a" "m" "e" "i" "s" "t" "M" "r"
##  [19] "." "R" "o" "b" "e" "r" "t" "D" "e" "N" "i" "r" "o" "u" "n" "d" "d" "a"
##  [37] "s" "i" "s" "t" "m" "e" "i" "n" "e" "T" "e" "l" "e" "f" "o" "n" "n" "u"
##  [55] "m" "m" "e" "r" ":" "0" "1" "6" "4" "-" "4" "5" "2" "9" "5" "4" "3" "2"
##  [73] "2" "." "M" "e" "i" "n" "e" "E" "-" "M" "a" "i" "l" "-" "A" "d" "r" "e"
##  [91] "s" "s" "e" "i" "s" "t" "n" "i" "r" "o" "@" "g" "m" "a" "i" "l" "." "c"
## [109] "o" "m" "u" "n" "d" "i" "c" "h" "b" "i" "n" "g" "e" "b" "o" "r" "e" "n"
## [127] "a" "m" "0" "2" "-" "0" "4" "-" "1" "9" "6" "5" "." "#" "c" "a" "l" "l"
## [145] "m" "e"
```

Da Tokenisieren eine komplexe Operation ist, dauert es je nach Anzahl und Länge der Texte lange, bis der Computer ein komplettes Korpus tokenisiert hat. Wenn die  `tokens()`-Funktion mit dem zusätzlichen Argument `verbose = TRUE` aufgerufen wird, werden beim Ausführen der Funktion Updates zu jedem Bearbeitungsschritt auf der Konsole ausgegeben: 


```r
tokens(beispiel_1, verbose = TRUE)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "Hallo"  "mein"   "Name"   "ist"    "Mr"     "."      "Robert" "De"    
##  [9] "Niro"   "und"    "das"    "ist"   
## [ ... and 17 more ]
```

Die `tokens()`-Funktion kann auch zum Tokenisieren von japanisch- und chinesischsprachigen Texten verwendet werden, hierbei wird unter der Motorhaube eine morphologische Analyse durchgeführt. Ein Beispiel aus den [Quanteda-Dokumentationsseiten](https://tutorials.quanteda.io/multilingual/overview/): 


```r
library(readtext)
declaration <- readtext("data/declaration_rights.txt")
cor <- corpus(declaration)
toks <- tokens(cor)
toks
```

```
## Tokens consisting of 1 document.
## declaration_rights.txt :
##  [1] "鉴于" "对"   "人类" "家庭" "所有" "成员" "的"   "固有" "尊严" "及其"
## [11] "平等" "的"  
## [ ... and 281 more ]
```


## Reguläre Ausdrücke im Preprocessing

Manchmal ist es notwendig, eine Zeichenkette vor dem Tokenisieren manuell zu bearbeiten oder bereinigen, damit beim Preprocessing die Tokens für den jeweiligen Kontext richtig erkannt werden. Wir haben zum Beispiel gesehen, dass beim Tokenisieren manche Sinneinheiten richtig erfasst werden (z.B. Hashtags oder Telefonnummern mit `-`), aber andere nicht (z.B. Telefonnummern mit `/`, der Punkt nach einer Abkürzung wie Mr., der Nachname De Niro). Um eines dieser Probleme zu beheben, haben wir den Text manuell bearbeitet und mithilfe der Funktion `gsub()` alle Schrägstriche gegen Trennstriche ausgetauscht. Es kommt daneben auch vor, dass Texte bestimmte Zeichen enthalten, die keine inhaltliche Bedeutung tragen, zum Beispiel Fußnoten oder Seitenzahlen. Solche Zeichen können die Ergebnisse der Textanalyse beeinflussen und sollten deswegen im Rahmen des Preprocessing entfernt werden. Zur Suche, zum Bearbeiten und zur Entfernung von Zeichen in Zeichenketten können reguläre Ausdrücke (engl. "regular expressions") verwendet werden. Eine ausführliche Einführung in reguläre Ausdrücke findet ihr im Kapitel "Exkurs: Reguläre Ausdrücke". In diesem Abschnitt schauen wir uns nur an einem Beipsiel an, wie reguläre Ausdrücke beim Preprocessing zur Anwendung kommen können.  

Der Beispieltext `froschkoenig` enthält Verweise auf Fußnoten in eckigen Klammern. Diese Verweise wollen wir nun entfernen. Die bereits bekannte Funktion `gsub()` kann verwendet werden, um mithilfe von regulären Ausdrücken Muster zu definieren, die in einer Zeichenkette ausgetauscht werden sollen. Um alle Verweise zu entfernen, definieren wir einen regulären Ausdruck, der nach einem Leerzeichen gefolgt von mehr als einer (`+`) Zahl zwischen 0 und 9 (`[0-9]`) innerhalb von eckigen Klammern (`\\[` oder `\\]`) sucht und durch einen leeren String (`""`) austauscht. Bevor wir die Seitenzahlen entfernen, sollten wir uns allerdings die Suchergebnisse anzeigen lassen, um zu überprüfen, ob der reguläre Ausdruck die richtigen Zeichenketten findet:  


```r
froschkoenig <- "In den alten Zeiten [1], wo das Wünschen noch geholfen hat, lebte ein König [2], dessen Töchter waren alle schön, aber die jüngste Tochter [3] war so schön, daß die Sonne selber, die doch so vieles gesehen hat, sich verwunderte so oft sie ihr ins Gesicht schien."

regmatches(froschkoenig, gregexpr(" \\[[0-9]+\\]", froschkoenig))
```

```
## [[1]]
## [1] " [1]" " [2]" " [3]"
```

```r
froschkoenig <- gsub(" \\[[0-9]+\\]", "", froschkoenig)
froschkoenig
```

```
## [1] "In den alten Zeiten, wo das Wünschen noch geholfen hat, lebte ein König, dessen Töchter waren alle schön, aber die jüngste Tochter war so schön, daß die Sonne selber, die doch so vieles gesehen hat, sich verwunderte so oft sie ihr ins Gesicht schien."
```


Die eckige Klammer steht in einem Regex-Ausdruck für Zeichenklassen (s. "Exkurs: Reguläre Ausdrücke"). Die doppelten \\\\ werden verwendet, damit die eckige Klammer nicht als Regex-Symbol erkannt wird, sondern als ganz normales Satzzeichen. 

:::task
Verständnisfragen: 

* Was passiert, wenn man die \\\\ weglässt?
* Was passiert, wenn man das Leerzeichen am Anfang des regulären Ausdrucks `" \\[[1-9]+\\]"` weglässt?
* Was machen die Funktionen `gregexpr()` und `regmatches()`?

:::


## Satzzeichen, Zahlen und Sonderzeichen entfernen 

Für viele Analysemethoden spielen nur Wörter im eigentlichen Sinne eine Rolle. Satzzeichen, Zahlen und Sonderzeichen werden deswegen häufig beim Preprocessing entfernt. Dieser Vorbereitungsschritt ist so verbreitet, dass die Entwickler:innen des Pakets quanteda Parameter für die `tokens()`-Funktion definiert haben, die steuern, ob bei der Tokenisierung diese Zeichen direkt entfernt werden sollen oder nicht.  


```r
froschkoenig_toks <- tokens(froschkoenig, remove_punct = TRUE, remove_numbers = TRUE, remove_symbols = TRUE)
froschkoenig_toks
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "In"       "den"      "alten"    "Zeiten"   "wo"       "das"     
##  [7] "Wünschen" "noch"     "geholfen" "hat"      "lebte"    "ein"     
## [ ... and 32 more ]
```


## Stoppwörter entfernen 

Wenn Texte zur Analyse als "Bag of Words" repräsentiert werden sollen, dann spielt die Reihenfolge der einzelnen Worte keine Rolle und Funktionswörter wie Artikel, Konjunktionen, Präpositionen u.ä. sind außerhalb des Satzzusammenhangs für diese Analysemethoden häufig nicht von Interesse. Zugleich kommen diese Wörter aber deutlich häufiger vor als "bedeutungstragende" Wörter. Solche Wörter werden deswegen häufig beim Preprocessing entfernt. Dabei werden sogenannte Stoppwortlisten verwendet: Alle Tokens, die in der Liste vorkommen, werden nach dem Tokenisieren entfernt. Die Funktionen zum Tokenisieren und zum Entfernen der Stoppwörter kann in Quanteda mithilfe des Pipe-Operators verkettet werden:


```r
# Funktion tokens_remove() zum entfernen der Stoppwörter verwenden 
froschkoenig_toks <- tokens(froschkoenig) %>%
  tokens_remove(pattern = stopwords("de"))
print(froschkoenig_toks, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "alten"       "Zeiten"      ","           "Wünschen"    "geholfen"   
##  [6] ","           "lebte"       "König"       ","           "Töchter"    
## [11] "schön"       ","           "jüngste"     "Tochter"     "schön"      
## [16] ","           "Sonne"       "selber"      ","           "vieles"     
## [21] "gesehen"     ","           "verwunderte" "oft"         "Gesicht"    
## [26] "schien"      "."
```

Beim Aufruf der Funktion `tokens_remove()` wird mithilfe des Arguments `pattern = stopwords("de")` eine Stoppwortliste mit deutschen Stoppwörtern übergeben. Welche Wörter die  `tokens_remove()`-Funktion entfernt, hängt also davon ab, welche Stoppwörter auf dieser Liste stehen. Der [Funktionsdokumentation](https://quanteda.io/reference/stopwords.html) können wir entnehmen, dass die Stoppwortliste einem weiteren R Paket entnommen wird. Die Liste deutscher Stoppwörter ist demzufolge: 
http://snowball.tartarus.org/algorithms/german/stop.txt

Eine Stoppwortliste ist also im Grunde nur eine Plaintextdatei, in der in jeder Zeile ein Wort steht. Anstelle der vordefinierten Stoppwortliste kann auch eine eigene Stoppwortliste eingelesen werden. Dazu kann entweder eine Liste komplett selbst erstellt werden, oder es wird zunächst eine Stoppwortliste heruntergeladen und dann angepasst, zum Beispiel: 

* https://github.com/solariz/german_stopwords
* https://github.com/stopwords-iso/stopwords-de/blob/master/stopwords-de.txt

Die Liste kann angepasst werden, indem einfach Wörter in der Plaintext-Datei hinzugefügt oder entfernt werden. 


```r
# Eigene Stoppwortliste einlesen
custom_stopwords <- readLines("stopwords.txt", encoding = "UTF-8") 
custom_stopwords <- readtext("stopwords.txt") 
custom_stopwords
# Importierte Stoppwortliste and die tokens_remove()-Funktion übergeben
froschkoenig_toks <- tokens_remove(froschkoenig_toks, pattern = custom_stopwords, padding=F)
```

Alternativ kann auch die Default-Stoppwortliste der Funktion `stopwords()` durch eine andere Stoppwortliste ausgetauscht werden. Um zu überprüfen, welche Stoppwortlisten es gibt: 


```r
library(stopwords)
stopwords_getsources()
```

```
## [1] "snowball"      "stopwords-iso" "misc"          "smart"        
## [5] "marimo"        "ancient"       "nltk"          "perseus"
```

```r
froschkoenig_toks <- tokens(froschkoenig) %>%
  tokens_remove(pattern = stopwords("de", source="nltk"))
```

Zuletzt können mit der `tokens_remove()`-Funktion auch nachträglich einzelne Tokens entfernt werden, die in einem Text vielleicht besonders häufig vorkommen, aber nicht auf der Stoppwortliste stehen: 


```r
froschkoenig_toks <- tokens_remove(froschkoenig_toks, pattern = "daß", padding=F)
```

## Groß- und Kleinschreibung anpassen

Wir haben bereits gesehen, dass dasselbe Wort groß- und kleingeschrieben als zwei verschiedene Types gezählt wird. Dieses Verhalten ist bei der Analyse oft nicht gewünscht, da die unterschiedliche Schreibweise meist nicht als inhaltlich bedeutungstragend angesehen wird. Beim Preprocessing kann deswegen zusätzlich der gesamte Text in Kleinbuchstaben umgewandelt werden: 


```r
froschkoenig_toks <- froschkoenig %>% 
  tokens(remove_punct = TRUE, remove_numbers = TRUE, remove_symbols = TRUE) %>% 
  tokens_tolower()
froschkoenig_toks
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "in"       "den"      "alten"    "zeiten"   "wo"       "das"     
##  [7] "wünschen" "noch"     "geholfen" "hat"      "lebte"    "ein"     
## [ ... and 32 more ]
```

## Stemming 

> "a simpler version of lemmatization in which we mainly just strip suffixes from the end of the word" 

Quelle: [Jurafsky/Martin 2023, S. 2](https://web.stanford.edu/~jurafsky/slp3/2.pdf)

Beim Stemming werden Wörter auf ihren Wortstamm reduziert, indem Wortendungen nach bestimmten Regeln entfernt werden. Das Stemming schauen wir uns nur äußerst kurz an, denn in der Praxis lohnt es sich selten, dieses Verfahren anzuwenden. Zum Stemming kann die quanteda-Funktion `tokens_wordstem()` verwendet werden: 


```r
library(quanteda)

beispiel <- "Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322. Meine E-Mail-Adresse ist niro@gmail.com und ich bin geboren am 02/04/1965. #callme"

beispiel_stem <- tokens_wordstem(tokens(beispiel), language="ger")
print(beispiel_stem, max_ntoken = 200)
```

```
## Tokens consisting of 1 document.
## text1 :
##  [1] "Hallo"          "mein"           "Nam"            "ist"           
##  [5] "Mr"             "."              "Robert"         "De"            
##  [9] "Niro"           "und"            "das"            "ist"           
## [13] "mein"           "Telefonnumm"    ":"              "0164-452954322"
## [17] "."              "Mein"           "E-Mail-Adress"  "ist"           
## [21] "niro@gmail.com" "und"            "ich"            "bin"           
## [25] "gebor"          "am"             "02"             "/"             
## [29] "04"             "/"              "1965"           "."             
## [33] "#callm"
```


## Lemmatisierung

 > "the task of determining that two words have the same root, despite their surface differences" 
 
Quelle: [Jurafsky/Martin 2023, S. 2](https://web.stanford.edu/~jurafsky/slp3/2.pdf)

Lemmatisierung (engl. lemmatization) hat im Grunde dasselbe Ziel wie Stemming: Bei der Lemmatisierung werden Wörter auf ihre Grundform reduziert. Dazu werden jedoch komplexere Algorithmen angewandt, sodass auch Grundformen erkannt werden, die durch die einfache Entfernung von Endungen nicht richtig gebildet würden.

### Methode 1: Lemmatisierung mit Lexikon

Diese Methode kann zur Lemmatisierung englischsprachiger Texte angewandt werden.


```r
install.packages("lexicon")
```


```r
library(lexicon)
```


```r
beispiel_engl <- "Hello I went swimming yesterday"

beispiel_lemmatized_2 <- tokens_replace(tokens(beispiel_engl), pattern = lexicon::hash_lemmas$token, replacement = lexicon::hash_lemmas$lemma)
print(beispiel_lemmatized_2, max_ntoken = 200)

# dasselbe geht auch mit der Funktion dfm_replace()
```


### Methode 2: Lemmatisierung mit UDPipe 

Diese Methode kann zur Lemmatisierung auch von deutsch- und anderssprachigen Texten angewandt werden. Das Verfahren ist ausführlich dokumentiert in den UDPipe-Dokumentationsseiten: https://cran.r-project.org/web/packages/udpipe/vignettes/udpipe-annotation.html. In der Sitzung zum fortgeschrittenen Preprocessing werden wir noch einmal darauf zurückkommen und erarbeiten uns in diesem Abschnitt nur ein grundlegendes Verständnis der Lemmatisierung mithilfe des R Pakets `udpipe`. Zunächst muss das Paket `udpipe` installiert und geladen werden. Anschließend muss ein sogenanntes Sprachmodell heruntergeladen und eingelesen werden. Die `udpipe`-Sprachmodelle sind statistische Modelle, die auf einem bestimmten Datensatz "trainiert" wurden, also zum Beispiel auf einem Korpus deutschsprachiger Texte, die mit linguistischen Annotationen versehen wurden. Auf der Grundlage der Trainingsdaten können danach Bestandteile wie Wörter und Satzzeichen auch in unbekannten Texten erkannt werden: Zum Beispiel Wortarten, Grundformen und syntaktische Beziehungen. Genau das passiert, wenn wir einer udpipe-Funktion wie `udpipe()` oder `udpipe_annotate()` auf unser Beispielkorpus anwenden. 



```r
# 0. Vorbereitung
install.packages("udpipe")
library(udpipe)

# Deutsches Sprachmodell herunterladen und laden
ud_model <- udpipe_download_model("german")
ud_model <- udpipe_load_model(ud_model)
```



Es gibt zwei Funktionen aus dem Paket udpipe, mit denen Texte lemmatisiert werden können, also mit denen die Wörter in einem Text auf ihre Grundformen reduziert werden können. Die Funktion `udpipe()` gibt direkt einen Dataframe zurück (s.u.). Die Funktion `udpipe_annotate()` gibt eine Liste zurück, die in einem folgenden Schritt in einen Dataframe umgewandelt werden kann. Beide Funktionen lemmatisieren den Text nicht nur, sondern tokenisieren ihn auch und führen weitere Verarbeitungsschritte durch, auf die wir an dieser Stelle nicht weiter eingehen. Die Funktion `udpipe_annotate()` erlaubt es, mithilfe verschiedener Funktionsparameter festzulegen, welche dieser Verarbeitungsschritte beim Aufruf der Funktion durchgeführt werden sollen. 

Wir betrachten zunächst wieder unseren Beispielsatz: 


```r
# 1. Mit Beispieltext
beispiel <- "Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322. Meine E-Mail-Adresse ist niro@gmail.com und ich bin geboren am 02/04/1965. #callme"
```


```r
beispiel_df <- udpipe(beispiel, ud_model)
head(beispiel_df) # erste fünf Zeilen des Dataframes anzeigen 
```

```{style="max-height: 200px;"}
##   doc_id paragraph_id sentence_id
## 1   doc1            1           1
## 2   doc1            1           1
## 3   doc1            1           1
## 4   doc1            1           1
## 5   doc1            1           1
## 6   doc1            1           1
##                                                                                  sentence
## 1 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 2 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 3 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 4 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 5 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 6 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
##   start end term_id token_id  token  lemma  upos   xpos
## 1     1   5       1        1  Hallo  hallo     X    ITJ
## 2     7  10       2        2   mein   mein   DET PPOSAT
## 3    12  15       3        3   Name   Name  NOUN     NN
## 4    17  19       4        4    ist   sein   AUX  VVFIN
## 5    21  23       5        5    Mr.    Mr. PROPN     NE
## 6    25  30       6        6 Robert Robert PROPN     NE
##                                                   feats head_token_id  dep_rel
## 1                                                  <NA>             3      dep
## 2             Case=Nom|Gender=Masc|Number=Sing|Poss=Yes             3 det:poss
## 3                      Case=Nom|Gender=Masc|Number=Sing             5    nsubj
## 4 Mood=Ind|Number=Sing|Person=3|Tense=Pres|VerbForm=Fin             5      cop
## 5                      Case=Nom|Gender=Masc|Number=Sing             0     root
## 6                      Case=Nom|Gender=Masc|Number=Sing             5     nmod
##   deps misc
## 1 <NA> <NA>
## 2 <NA> <NA>
## 3 <NA> <NA>
## 4 <NA> <NA>
## 5 <NA> <NA>
## 6 <NA> <NA>
```

```r
beispiel_annotated <- udpipe_annotate(ud_model, beispiel, tagger="default", parser="none")
beispiel_df <- as.data.frame(beispiel_annotated)
head(beispiel_df) # erste fünf Zeilen des Dataframes anzeigen
```

```{style="max-height: 200px;"}
##   doc_id paragraph_id sentence_id
## 1   doc1            1           1
## 2   doc1            1           1
## 3   doc1            1           1
## 4   doc1            1           1
## 5   doc1            1           1
## 6   doc1            1           1
##                                                                                  sentence
## 1 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 2 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 3 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 4 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 5 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
## 6 Hallo mein Name ist Mr. Robert De Niro und das ist meine Telefonnummer: 0164-452954322.
##   token_id  token  lemma  upos   xpos
## 1        1  Hallo  hallo     X    ITJ
## 2        2   mein   mein   DET PPOSAT
## 3        3   Name   Name  NOUN     NN
## 4        4    ist   sein   AUX  VVFIN
## 5        5    Mr.    Mr. PROPN     NE
## 6        6 Robert Robert PROPN     NE
##                                                   feats head_token_id dep_rel
## 1                                                  <NA>          <NA>    <NA>
## 2             Case=Nom|Gender=Masc|Number=Sing|Poss=Yes          <NA>    <NA>
## 3                      Case=Nom|Gender=Masc|Number=Sing          <NA>    <NA>
## 4 Mood=Ind|Number=Sing|Person=3|Tense=Pres|VerbForm=Fin          <NA>    <NA>
## 5                      Case=Nom|Gender=Masc|Number=Sing          <NA>    <NA>
## 6                      Case=Nom|Gender=Masc|Number=Sing          <NA>    <NA>
##   deps misc
## 1 <NA> <NA>
## 2 <NA> <NA>
## 3 <NA> <NA>
## 4 <NA> <NA>
## 5 <NA> <NA>
## 6 <NA> <NA>
```

```r
#View(beispiel_df)
# ?udpipe_annotate
```

Beachtet Zeile 29: Hier wurde das Token "am" in zwei Lemmata aufgeteilt: "an" und "der". Dieses Verhalten müssen wir bei der Weiterverarbeitung der Lemmata beachten!

Jetzt schauen wir uns an, wie nicht nur ein einziger Text, sondern ein ganzes Korpus mithilfe von udpipe lemmatisiert werden kann. Als Beispiel dient uns das Teilkorpus mit Kafka-Texten aus der letzten Stunde. Dazu erstellen wir, analog zur letzten Stunde, zunächst ein Teilkorpus aus Kafka-Texten: 


```r
library(readtext)
library(quanteda)

ger_texte <- readtext("korpus/*.txt", docvarsfrom = "filenames", dvsep = "_", docvarnames = c("Autor_in", "Titel", "Jahr"), encoding = "UTF-8")
ger_korpus <- corpus(ger_texte)
kafka_korpus <- corpus_subset(ger_korpus, Autor_in == "kafka")
```

Anschließend können wir die Funktion `udpipe_annotate()` auf unser Kafka-Korpus anwenden: 


```r
library(udpipe)

# 3. Mit Korpus zur Weiterverarbeitung in quanteda
kafka_annotated <- udpipe_annotate(ud_model, kafka_korpus, tagger="default", parser="none", doc_id = kafka_korpus$Titel)
kafka_df <- as.data.frame(kafka_annotated)
```

Da bei der Lemmatisierung Tokens wie "am" in zwei Lemmata aufgeteilt werden ("an" und "dem"), gibt es im Dataframe `kafka_df` einige Zeilen, in denen in der Spalte `lemma` der Wert `NA` steht. Außerdem fällt auf, dass es einige Zeilen gibt, in denen zwei mögliche Lemmata angegeben werden, die mit einem `|` getrennt sind, zum Beispiel fallen|fällen. Diese Zeilen sollten zunächst bereinigt werden und es muss entschieden werden, welche Variante die richtige ist. 


```r
# Zeilen mit NA-Werten entfernen
kafka_cleaned_df <- kafka_df[!is.na(kafka_df$lemma), ]

# Alle Zeilen mit zwei verschiedenen Varianten in der Spalte lemma auswählen: Hier muss ggf. im Einzelfall entschieden werden, welche Variante richtig ist!
kafka_cleaned_df[grep("\\|", kafka_cleaned_df$lemma), ]

# Als "quick and dirty" Methode kann z.B. einfach immer die letzte Variante ausgewählt werden
kafka_cleaned_df$lemma <- gsub("\\w+\\|(\\|\\w+)?", "", kafka_cleaned_df$lemma)
```

Wir haben jetzt einen bereinigten Dataframe `kafka_cleaned_df`, der Lemmata zu jedem der Texte in unserem Korpus enthält. Die Lemmata liegen aber immer noch als Elemente der Spalte `lemma` vor. Einen Dataframe dieser Form können wir nicht mithilfe von quanteda-Funktionen weiter bearbeiten. Wir müssen also irgendwie den Dataframe in eine Form bringen, die mit quanteda-Funktionen kompatibel ist. Dazu kombinieren wir die Lemmata aus jedem der Texte in einer einzigen Zeile, sodass wir einen Dataframe mit einer Zeile je Text erhalten, der in einer neuen Spalte "text" einen character Vektor mit den Lemmata aus diesem Text enthält. Ein Dataframe mit dieser Struktur ist kompatibel mit der Quanteda-`corpus()`-Funktion. Um unseren Dataframe zu bearbeiten, verwenden wir die Funktionen `group_by()` und `summarise()` aus dem Paket `dplyr`: 


```r
library(dplyr)
# Dataframe umformen, sodass jede Zeile einem Dokument entspricht und die Lemmata zu einem zusammenhängenden Text zusammengefügt werden 
kafka_grouped <- kafka_cleaned_df %>% 
  group_by(doc_id) %>% 
  summarise(text = paste(lemma, collapse = " ")) 
View(kafka_grouped)
# In Quanteda corpus-Objekt umwandeln
kafka_lemmatized <- corpus(kafka_grouped, docnames = maerchen_grouped$doc_id)
# Korpus-Objekt mit den lemmatisierten Texten speichern
saveRDS(kafka_lemmatized, file="kafka_lemmatized.rds")
```

Mit dem Objekt `kafka_lemmatized` könnte jetzt ganz regulär mit Quanteda-Funktionen weitergearbeitet werden. Wir werden uns diesen Code und auch den Dataframe `kafka_df` in der Sitzung zum fortgeschrittenen Preprocessing mit UDPipe noch einmal genauer ansehen. 


## Quellen {-}

- Jurafsky, Daniel und Martin, James H. (2023), *Speech and Language Processing. Chapter 2: Regular Expressions, Text Normalization, Edit Distance*, https://web.stanford.edu/~jurafsky/slp3/2.pdf. 
- ForText (2016), Glossar: Preprocessing, https://fortext.net/ueber-fortext/glossar/preprocessing. 
- Jünger, J. and Gärtner, C. (2023), *Computational Methods für die Sozial- und Geisteswissenschaften. Kapitel 9: Textanalyse,* S. 361-364, https://doi.org/10.1007/978-3-658-37747-2_9.
- Hase, Valerie (2021), *Text as Data Methods in R - Applications for Automated Analyses of News Content. Tutorial 11: Preprocessing*, https://bookdown.org/valerie_hase/TextasData_HS2021/tutorial-11-preprocessing.html.
- Welbers, Kasper (2020), Text Analysis in R. Part 1: Preprocessing, https://www.youtube.com/watch?v=O6CGXnxPHok.
- Wijffels, Jan (2023), *UDPipe Natural Language Processing - Text Annotation*, https://cran.r-project.org/web/packages/udpipe/vignettes/udpipe-annotation.html.
- Van Atteweldt, Wouter et al. (2022). *Computational Analysis of Communication. Ch. 10.3.4: Linguistic Preprocessing*, https://cssbook.net/content/chapter10.html#sec-nlp.
- Desagulier, Guillaume (2017). *Corpus Linguistics and Statistics with R. Ch. 4.4. Regular Expressions*, pp. 73-82, https://doi.org/10.1007/978-3-319-64572-8.
