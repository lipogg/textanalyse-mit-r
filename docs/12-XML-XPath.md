# Exkurs: XML, TEI und XPath

## Was ist XML? Was ist TEI?

XML = Extensible Markup Language </br>
Formale Definition ([Wikipedia](https://de.wikipedia.org/wiki/Extensible_Markup_Language)): 

> Auszeichnungssprache zur Darstellung hierarchisch strukturierter Daten im Format einer Textdatei, die sowohl von Menschen als auch von Maschinen lesbar ist.

TEI = Text Encoding Initiative </br> 
Formale Definition ([Wikipedia](https://de.wikipedia.org/wiki/Text_Encoding_Initiative)): 

> eine 1987 gegründete Organisation (seit 2000 als TEI-Konsortium organisiert) und ein gleichnamiges Dokumentenformat zur Kodierung und zum Austausch von Texten, das diese entwickelt hat und weiterentwickelt. [...] TEI hat sich zu einem De-facto-Standard innerhalb der Geisteswissenschaften entwickelt, wo es zum Beispiel zur Kodierung von gedruckten Werken (Editionswissenschaft) oder zur Auszeichnung von sprachlichen Informationen (Linguistik) in Texten verwendet wird.

## Aufbau von XML-TEI Dokumenten

XML-Dokumente sind streng hierarchisch aufgebaut und bestehen aus verschiedenen, in einer Baumstruktur ineinander verschachtelten Elementen. Diese Baumstruktur bedeutet, dass jedes Element, das "geöffnet" wird, auch "geschlossen" werden muss, es besteht also aus einem Öffnungstag (oder "opening tag") und einem Schließtag (oder "closing tag"). Elemente können Fließtext, Attribute oder auch weitere Elemente enthalten; sie können aber auch leer sein. Attribute werden direkt in den Öffnungstag hineingeschrieben und definieren Eigenschaften des entsprechenden Elements.

Aufbau eines XML-Dokuments: 
````xml
<!-- Prolog: optional; definiert die verwendete XML-Version -->
<?xml version="1.0" encoding="UTF-8"?> 
<!-- Wurzelelement -->
<root> 
<!-- irgendein Element -->
  <child> 
  <!-- noch ein Element, mit Attribut "attribute" und dem Attributwert "some_property" -->
    <subchild attribute="some_property">...some content...</subchild>
  </child>
</root>
````

Mehr zur XML-Syntax [hier](https://www.w3schools.com/xml/xml_syntax.asp). 

Die konkrete Benennung der Elemente ist dabei nicht durch die XML-Syntax vorgegeben und ist grundsätzlich beliebig, muss jedoch einheitlich sein. Der Benennungsstil und Regeln für die Verwendung bestimmter Elemente und Attribute bei der Auszeichnung von Texten können auch durch ein Kodierungsschema und einen XML-Auszeichnungsstandard am Anfang des Dokuments festgelegt werden. In den Geisteswissenschaften hat sich mit TEI ein solcher einheitlicher Auszeichnungsstandard für Texte durchgesetzt.

Nach den TEI-Guidelines besteht jedes TEI-Dokument aus einem Wurzelelement, welches das TEI-Dokument mit den Tags ``<TEI></TEI>`` öffnet und schließt, sowie aus zwei Kindelementen, dem Header (``<teiHeader></teiHeader>``) und einem Text-Element (``<text></text>``). Der Header beinhaltet Metadaten zu dem ausgezeichneten Text, beispielsweise eine "file description" (``<fileDesc></fileDesc>``), die bibliographische Informationen zur ausgezeichneten Datei enthält. Die file description ist als einziges Header-Element für alle TEI-Header zwingend erforderlich, damit das Dokument als valide gelten kann (zur Validierung siehe nächster Abschnitt). Der ``<TEI>``-Öffnungstag enthält standardmäßig den [TEI-Namensraum](https://de.wikipedia.org/wiki/Namensraum_(XML)) (oder "namespace") http://www.tei-c.org/ns/1.0, manchmal auch die Sprache des ausgezeichneten Texts und die verwendete TEI-Version. Der Namensraum dient dazu, die verwendeten Elemente und Attribute eindeutig einer bestimmten XML-Sprache bzw. einem Auszeichnungsstandard zuzuordnen, in diesem Fall also TEI. 

In den TEI-Guidelines findet sich das folgende Beispiel für ein [minimales valides TEI-Dokument](https://tei-c.org/release/doc/tei-p5-doc/en/html/ref-TEI.html#index-egXML-d54e49512):  

````xml
<TEI version="3.3.0" xmlns="http://www.tei-c.org/ns/1.0">
 <teiHeader>
  <fileDesc>
   <titleStmt>
    <title>The shortest TEI Document Imaginable</title>
   </titleStmt>
   <publicationStmt>
    <p>First published as part of TEI P2, this is the P5
         version using a namespace.</p>
   </publicationStmt>
   <sourceDesc>
    <p>No source: this is an original work.</p>
   </sourceDesc>
  </fileDesc>
 </teiHeader>
 <text>
  <body>
   <p>This is about the shortest TEI document imaginable.</p>
  </body>
 </text>
</TEI>
````

Für ein [Minimalbeispiel für einen validen Header](https://tei-c.org/release/doc/tei-p5-doc/en/html/HD.html#index-egXML-d54e16887) siehe den Abschnitt "2 The TEI Header" in den TEI-Guidelines. Für Informationen zum Aufbau des Text-Elements siehe ["4 Default Text Structure"](https://tei-c.org/release/doc/tei-p5-doc/en/html/DS.html).

Während dieser Minimalaufbau für alle TEI-Dokumente einheitlich vorgegeben ist, hängt die Wahl der konkreten Elemente und Attribute innerhalb des Headers und des Textkörpers davon ab, welche Textart ausgezeichnet wird. Für bestimmte Textarten finden sich in den [TEI-Guidelines](https://tei-c.org/release/doc/tei-p5-doc/en/html/) verschiedene [Module](https://tei-c.org/release/doc/tei-p5-doc/en/html/ST.html#STMA), die flexibel zu einem [TEI Schema](https://tei-c.org/release/doc/tei-p5-doc/en/html/ST.html#STIN) kombiniert werden können. Alle Elemente und Attribute, die in einem TEI-Header verwendet werden können, finden sich beispielsweise im Modul "header". Elemente und Attribute, die zur Auszeichnung von "Performance Texts" wie Dramen verwendet werden können, finden sich im Modul "drama".   

Der [Text eines einzelnen Gedichts](https://tei-c.org/release/doc/tei-p5-doc/en/html/VE.html#index-egXML-d54e59340) würde unter Verwendung der Moduls "verse" wie folgt ausgezeichnet werden:

````xml
<text>
 <front>
  <head>1755</head>
 </front>
 <body>
  <l>To make a prairie it takes a clover and one bee,</l>
  <l>One clover, and a bee,</l>
  <l>And revery.</l>
  <l>The revery alone will do,</l>
  <l>If bees are few.</l>
 </body>
</text>
````

:::task
Verständnisfragen:

Die XML-Syntax erinnert an HTML, das ihr in der letzten Woche kennengelernt habt. Lest [diesen Beitrag](https://aws.amazon.com/de/compare/the-difference-between-html-and-xml/) und beantwortet anschließend die folgenden Fragen: 

- Was ist der Oberbegriff über XML, HTML und ähnliche Sprachen?
- Was sind Ähnlichkeiten zwischen XML und HTML? 
- Worin unterscheiden sie sich? 

:::

## Wohlgeformtheit und Validität 

Ein XML-Dokument gilt als **wohlgeformt**, wenn alle [XML-Syntaxregeln](https://www.w3schools.com/xml/xml_syntax.asp) eingehalten werden, das heißt:

* Es gibt genau ein Wurzelelement 
* die Baumstruktur ist eingehalten, das heißt alle Elemente sind richtig ineinander geschachtelt und haben je Öffnungs- und Schließtags
* Groß- und Kleinschreibung von Element- und Attributnamen wurde beachtet 
* Attributwerte stehen in Anführungszeichen
* Sonderzeichen wurden ggf. durch Entitätenreferenzen ersetzt

Ein XML-Dokument ist **valide** (auch: "gültig"), wenn alle Regeln eingehalten werden, die jeweils in einem dem Dokument zugewiesenen Schema definiert sind. Das heißt für TEI-Dokumente: Ein TEI-Dokument ist valide, wenn seine Struktur und die Benennung der verwendeten Attribute und Elemente den Regeln des zugewiesenen Schemas entsprechen.   

Es gilt: Ein valides XML-Dokument ist immer auch wohlgeformt, ein wohlgeformtes XML-Dokument muss jedoch nicht unbedingt valide sein. 

Um zu überprüfen, ob ein XML-Dokument valide ist ("Validierung"), kann das Dokument beispielsweise in den oXygen XML Editor importiert werden (siehe Abschnitt 5). Eine Anleitung zur Validierung findet sich [hier als Kurzversion](https://guides.library.illinois.edu/xml/oxygen-validate) sowie ausführlich in den [oXygen-Hilfeseiten](https://www.oxygenxml.com/xml_editor/validation.html).  

## Wo findet man Korpora, die in XML-TEI ausgezeichnet sind? 

* Literarische Textsammlungen, z.B.: 
  + DraCor: https://dracor.org/ 
  + ELTeC: https://github.com/COST-ELTeC
  + TextGrid Repository: https://textgridrep.org/
* Wissenschaftliche Editionen, z.B.:
  + Theodor Fontanes Notizbücher: https://fontane-nb.dariah.eu/index.html
  + Hannah Arendt Gesamtausgabe: https://hannah-arendt-edition.net/index.html
  + Alexander von Humboldt digitale Edition: https://edition-humboldt.de/
* Katalog [Digital Scholarly Editions](https://www.digitale-edition.de/exist/apps/editions-browser/index.html)

...und über die auf der [Hilfeseite](https://lipogg.github.io/textanalyse-mit-r/hilfe.html) verlinkten Ressourcen. 

## Was ist XPath? 

Zur Suche in Zeichenketten (character strings) haben wir bereits Reguläre Ausdrücke kennengelernt. Um bestimmte Elemente eines XML-Dokuments oder deren Attribute zu adressieren oder um Informationen zu finden, kann dagegen die Abfragesprache XPath  verwendet werden. 

XPath steht für "XML Path Language" und ist eine sogenante Pfadbeschreibungssprache, die zur Suche in XML und HTML Dateien verwendet wird. XPath-Ausdrücke erlauben es, XML-Elemente anhand ihrer Hierarchie, Attributen, Textinhalten und weiteren Eigenschaften in der XML-Baumstruktur zu lokalisieren. Wie wir bereits gesehen haben, sind XML-Dokumente als hierarchische Baumstruktur aufgebaut (genau wir HTML-Dokumente, denn XML und HTML sind aus derselben Sprache hervorgegangen). 

## Basics XPath Syntax 

Ein paar der wichtigsten XPath-Ausdrücke sind: 

  | Ausdruck           |  Bedeutung                                   |
  |------------------|----------------------------------------------|
  | ``bookstore``           | Alle Elemente mit dem Namen bookstore auswählen                       |              
  | ``bookstore/book``           | Alle book-Elemente, die Kindelemente von bookstore sind                          |
  | ``bookstore/book[1]``   | Das erste Element auswählen, wenn bookstore mehrere Kindelemente mit dem Namen book hat
  | ``//book``           | Alle book-Elemente irgendwo im  XML-Baum                             |   
  | ``.//book``           | Alle book-Elemente innerhalb des aktuellen Elements 
  | ``bookstore//book``           | Alle book-Elemente, die sich irgendwoe im Element bookstore befinden                                 |
  | ``//@lang``           | Alle Attribute mit dem Namen lang auswählen                             |
  | ``//title[@lang]``           | Alle title-Elemente, die ein Attribut lang haben |
  | ``//title[@lang='en']``           | Alle title-Elemente, die ein Attribut lang mit dem Wert "en" haben              |
  | ``//*[@lang]``           | Irgendein Element auswählen, das ein Attribut lang hat                            |
  | ``title[contains(text(), 'History')]``    | Alle title-Elemente, die den Text "History" enthalten

Quelle: [W3Schools XPath Tutorial: XPath Syntax](https://www.w3schools.com/xml/xpath_syntax.asp). 


:::task
Verständnisfragen:

Schreibt XPath-Ausdrücke zur Suche nach folgenden XML-TEI-Elementen: 

- Ein p-Element irgendwo im XML-Baum
- Ein date-Element mit einem p-Element als Elternelement. Das p-Element ist irgendwo im XML-Baum
- Ein name-Element mit dem Attribut "type='person'", irgendwo im XML-Baum
- Ein title-Element mit dem Text "Babel", irgendwo im XML-Baum

:::



## Weiterführende Links und Literatur {-}

* "A Gentle Introduction to XML" der Text Encoding Initiative: https://tei-c.org/release/doc/tei-p5-doc/en/html/SG.html
* Komplette TEI-Guidelines: https://tei-c.org/release/doc/tei-p5-doc/en/html/index.html
* Einführung in XML und TEI von Henriette Mehn/SLUB Dresden: https://bildungsportal.sachsen.de/opal/auth/RepositoryEntry/17544740866/CourseNode/92610534105762
* Linksammlung und Selbstlernressourcen auf der Seite der Text Encoding Initiative: https://tei-c.org/support/learn/teach-yourself-tei/ 
* Selbstlernkurse zum Thema TEI auf der Seite von DARIAH-CAMPUS: https://campus.dariah.eu/en/tag/tei/page/1
* Tutorial "Digitale Textedition mit TEI" von Christof Schöch/DARIAH-DE: https://de.dariah.eu/tei-tutorial
* Tutorial des "TEI by Example Project": https://teibyexample.org/tutorials/
* "An Introduction to XML and TEI" der University of Illinois Library: https://guides.library.illinois.edu/c.php?g=347707&p=2344750
* Textdaten-Wiki des Instituts für Romanistik der FU Berlin, Abschnitt "Datenkodierung und XML": https://wikis.fu-berlin.de/display/tdr/Datenkodierung+und+XML
* Übersicht XPath Syntax: https://www.w3schools.com/xml/xpath_syntax.asp
* XPath Online Tester: https://extendsclass.com/xpath-tester.html
* Übersicht XPath Funktionen:  https://developer.mozilla.org/en-US/docs/Web/XPath/Functions.
* Leicht verständliches XPath Tutorial: https://www.youtube.com/watch?v=0QHmDvc9abs. 
* Vortrag: "Introduction to TEI" von Christopher Oghe:  https://www.youtube.com/watch?v=HSxnNK3ail4
* Folien zum Vortrag "Einführung in die Textauszeichnung mit TEI" von Agnes Brauer, Universitätsbibliothek Frankfurt: https://moodle.studiumdigitale.uni-frankfurt.de/moodle/pluginfile.php/77425/mod_resource/content/1/DH%40ubffmTEI_Brauer.pdf
