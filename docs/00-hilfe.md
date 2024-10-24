# Hilfe!! {-} 

Oft kommt es beim Coden zu komischen Fehlermeldungen, die mehr verwirren als helfen. Häufig steckt aber ein ganz simpler Flüchtigkeitsfehler dahinter. In diesem Fall gilt:

**1. Syntax und Rechtschreibung überprüfen.** Fehlt vielleicht nur eine Klammer? Ist die Variable wirklich richtig geschrieben? Sind wirklich alle notwendigen Pakete installiert und geladen?

**2. Fehlermeldung kopieren und googeln.** Bestimmt hatte schonmal jemand anderes dasselbe Problem und bestenfalls findet sich eine Lösung auf [https://stackoverflow.com/](https://stackoverflow.com/) oder in einem anderen Forum.

**3. ChatGPT fragen.** Das Codesnippet an ChatGPT senden und nach möglichen Fehlern fragen.

**4. R Hilfeseiten aufrufen.** Mit dem `?` und dem `??` Operator können über die Konsole die Dokumentation zu konkreten Funktionen aufgerufen werden. Das kann nützlich sein, um beispielsweise zu überprüfen, welchen Datentyp die Funktion als Input nimmt. Weitere Informationen zu den R Hilfeseiten: [https://www.r-project.org/help.html](https://www.r-project.org/help.html) 

Manchmal macht das Skript aber auch einfach nicht das, was es soll, ohne, dass eine Fehlermeldung entsteht. In diesem Fall liegt wahrscheinlich ein logischer Fehler im Programmablauf vor. Für diesen Fall gibt es eine Strategie, die Rubber Ducking oder Quietscheentchen-Debugging genannt wird.

**5. Rubber Ducking oder Quietscheentschen-Debugging.** Wenn nichts mehr hilft, hilft nur eins: Den Code einer Person, die nichts davon versteht - oder eben einem Quietscheentchen, Zeile für Zeile erklären. Dabei fallen oft logische Fehler auf, die das Problem verursachen.

**6. Hilfe holen.** Falls ihr im Laufe des Seminars ein Problem habt, das ihr selbst nicht lösen könnt, könnt ihr einen Screenshot für die nächste Sitzung mitbringen oder mir eine E-Mail an l.poggel@fu-berlin.de schreiben.

Falls die Konsole (Fenster Console im RStudio) plötzlich `+` statt `>` anzeigt oder sich ein Prozess durch Klick auf das rote "Stop"-Symbol nicht abbrechen lässt: Tastenkombination `Ctrl` (Control) + `C` (bzw. `Strg` + `C`).

Zuletzt kann es natürlich auch vorkommen, dass euch ein Inhalt aus dem Seminar nicht ganz klar ist oder ihr ein weiterführendes Interesse an einem Thema habt. Zum Nachlesen empfehle ich die folgenden **Lehrbücher und Ressourcen**:

Allgemein:

* Offizielle R Handbücher: https://cran.r-project.org/manuals.html
* Wickham, Hadley, Çetinkaya-Rundel, Mine und Grolemund, Garrett (2023). R for Data Science (=Official Tidyverse Book): https://r4ds.hadley.nz/
* Schmidt, Ben (ongoing). Humanities Data Fundamentals: https://hdf.benschmidt.org/R/
* Van Atteveldt, Wouter, Trilling, Damian und Arcila Calderón, Carlos (2022). Computational Analysis of Communication: https://cssbook.net/
* Stoltz, Dustin S. und Taylor, Marshall A. (2024). Mapping Texts. Computational Text Analysis for the Social Sciences, https://global.oup.com/academic/product/mapping-texts-9780197756881
* Sammlung von R "Cheatsheets": https://github.com/rstudio/cheatsheets/tree/main


Lehrbuch-Klassiker (für Grundlagen, bei speziellen Anwendungen zum Teil nicht mehr aktuell): 

* Arnold, Taylor und Tilton, Lauren (2024). Humanities Data in R. Exploring Networks, Geospatial Data, Images, and Text:  https://doi.org/10.1007/978-3-031-62566-4
* Jockers, Matthew und Thalken, Rosamond (2020). Text Analysis with R for Students of Literature: https://doi.org/10.1007/978-3-030-39643-5
* Desagulier, Guillaume (2017). Corpus Linguistics and Statistics with R: https://doi.org/10.1007/978-3-319-64572-8
* Levshina, Natalia (2015). How to Do Linguistics with R: https://doi.org/10.1075/z.195

Textanalyse mit Quanteda: 

* Offizielle Quanteda-Dokumentationsseiten: https://quanteda.io/
* Offizielles Quanteda-Tutorial:  https://tutorials.quanteda.io/
* Replikation des Codes aus Levshinas "Text Analysis with R for Students of Literature" mithilfe von Quanteda: https://quanteda.io/articles/pkgdown/replication/digital-humanities.html 
* Video-Tutorial zu Preprocessing und Textanalyse mit R von Kasper Welbers: https://www.youtube.com/playlist?list=PL-i7GM-A1wBZYRYTpem7hNVHK3hSV_1It 
* Kapitel 10: "Text as Data" aus "Computational Analysis of Communication":  https://cssbook.net/content/chapter10.html
* Kapitel 6: "From Text to Numbers" aus "Mapping Texts": https://global.oup.com/academic/product/mapping-texts-9780197756881

Datenvisualisierung mit ggplot2:

* Healy, Kieran (2019). Data Visualization. A Practical Introduction, https://socviz.co/
* Wickham, Hadley (2016). ggplot2. Elegant Graphics for Data Analysis, https://ggplot2-book.org/
* ggplot2-Dokumentationsseiten: https://ggplot2.tidyverse.org/;  https://ggplot2.tidyverse.org/articles/ggplot2.html

Statistik: 

* (grundlegend) Handl, Andreas und Kuhlenkasper, Torben (2018). Einführung in die Statistik. Theorie und Praxis mit R: https://doi.org/10.1007/978-3-662-56440-0
* (grundlegend) Gries, Stefan (2021). Statistics for Linguistics with R: https://doi.org/10.1515/9783110718256
* (grundlegend) Blitzstein, Joe. Vorlesungsvideos, interaktiver Kurs und Buch Statistics 110: Probability, https://projects.iq.harvard.edu/stat110/home
* (fortgeschritten) Kroonenberg, Pieter (2021). Multivariate Humanities: https://doi.org/10.1007/978-3-030-69150-9
* (fortgeschritten) Handl, Andreas und Kuhlenkasper, Torben (2017). Multivariate Analysemethoden. Theorie und Praxis mit R: https://doi.org/10.1007/978-3-662-54754-0

Hintergrund Preprocessing und Natural Language Processing (POS Tagging, Dependency Parsing, NER,...): 

* Jurafsky, Daniel und Martin, James H. (2023). Speech and Language Processing, https://web.stanford.edu/~jurafsky/slp3/ (insbesondere Kapitel 2, 8, 17, 18).
* Grimmer, Justin, Roberts, Margaret E. und Stewart, Brandon M. (2022), Text as Data. A New Framework for Machine Learning and the Social Sciences,  https://fu-berlin.primo.exlibrisgroup.com/permalink/49KOBV_FUB/1v1tp5h/alma9960725495502883 
* Biemann, Chris, Heyer, Gerhard und Quasthoff, Uwe (2013). Wissensrohstoff Text. Eine Einführung in das Text Mining, https://doi.org/10.1007/978-3-658-35969-0 (insbesondere Kapitel 1, 2).

Theorie (wird fortlaufend ergänzt):

* Gius, Evelyn und Jacke, Janina (2022). *Are Computational Literary Studies Structuralist?*, in: Journal of Cultural Analytics 7, no. 4, https://doi.org/10.22148/001c.46662. <mark>(Woche 1)</mark>
* Pichler, Axel und Reiter, Nils (2021), *Zur Operationalisierung literaturwissenschaftlicher Begriffe in der algorithmischen Textanalyse*, in: Journal of Literary Theory 15, no. 1-2,  https://doi.org/10.1515/jlt-2021-2008. <mark>(W1)</mark>
* Bhattacharyya, Sayan (2021). *Text Analysis for Thought in the Black Atlantic*, in: Kelly Baker Josephs und Roopika Risam, The Digital Black Atlantic, pp. 77-83, https://muse.jhu.edu/book/84470. <mark>(W1)</mark>
* Stoltz, Dustin und Taylor, Marshall (2024). *Mapping Texts*. S. XIII und Ch. 1: Text in Context, https://doi.org/10.1093/oso/9780197756874.001.0001. <mark>(W1)</mark>
* Pichler, Axel und Reiter, Nils (2022). *From Concepts to Texts and Back: Operationalization as a Core Activity of Digital Humanities*, https://culturalanalytics.org/article/57195
* Risam, Roopika (2019). *New Digital Worlds. Postcolonial Digital Humanities in Theory, Praxis, and Pedagogy.* Ch. 1: The Stakes of Postcolonial Digital Humanities (insb. Abschnitt World Making in Digital Humanities), https://fu-berlin.primo.exlibrisgroup.com/permalink/49KOBV_FUB/1v1tp5h/alma9961448530002883.
* Bubenhofer, Noah und Scharloth, Joachim (2013). *Korpuslinguistische Diskursanalyse*, in: Ulrike Hanna Meinhof, Martin Reisigl und Ingo Warnke (Hrsg.), Diskurslinguistik im Spannungsfeld von Deskription und Kritik, S. 147-167. https://doi.org/10.1524/9783050061047.147
* Bode, Katherine (2017), *The Equivalence of "Close" and "Distant" Reading; or, Toward a New Object for Data-Rich Literary History*, 
https://doi.org/10.1215/00267929-3699787. 
* Kleymann, Rabea (2022). *Datendiffraktion: Von Mixed zu Entangled Methods in den Digital Humanities*, https://doi.org/10.17175/sb005_008. 
* Eve, Martin Paul (2022). *The Digital Humanities and Literary Studies.* Introduction,  https://doi.org/10.1093/oso/9780198850489.003.0001 (für Skeptiker:innen)
* Bond, Sarah, Long, Hoyt und Underwood, Ted (2017). *'Digital' Is Not the Opposite of 'Humanities'*, https://www.chronicle.com/article/digital-is-not-the-opposite-of-humanities/. (für Skeptiker:innen)

Glossar der Zeitschrift für digitale Geisteswissenschaften (mit Beiträgen zu Theorie, Operationalisierung, Daten, ...): https://zfdg.de/wp_2023_001. 

Einige Digital Humanities Zeitschriften (für die eigene Recherche): 

* Zeitschrift für digitale Geisteswissenschaften, https://zfdg.de/
* Digital Humanities Quarterly, https://www.digitalhumanities.org/dhq/
* Journal of Cultural Analytics, https://culturalanalytics.org/
* Journal of Computational Literary Studies, https://jcls.io/
* Journal of Digital History, https://journalofdigitalhistory.org/ 
