# Wie ich lokale und Cloud-KI tatsächlich benutze

[English](README.md) | **Deutsch** | [Modelltests](MODELLTESTS_DE.md)

> **Persönlicher Praxisbericht – kein allgemeiner KI-Benchmark**  
> Stand: Oktober 2026

Dieses README beschreibt, wie ich mit KI an meinen Datenprojekten arbeite: von der ersten Frage bis zur letzten Prüfung. Es gilt für meine Projekte, meine Werkzeuge und meine Arbeitsweise.

Ich bin Analyst, kein Software-Entwickler und kein KI-Ingenieur. Den größten Teil meines Codes schreibt KI. Meine Arbeit liegt davor und danach: die Frage, die Daten, die Prüfung und die Entscheidung, was veröffentlicht wird.

Mein wichtigster Grundsatz:

> **KI kann Arbeit beschleunigen. Sie übernimmt aber nicht die Verantwortung dafür, ob ein Ergebnis stimmt.**

Die Tests meiner lokalen Modelle (Zeiten, Fehler, welches Modell wofür) stehen vollständig in einer eigenen Datei: [Modelltests](MODELLTESTS_DE.md).

---

## Inhaltsverzeichnis

- [Kurzfassung](#kurzfassung)
- [1. Warum ich so viel mit KI arbeite](#1-warum-ich-so-viel-mit-ki-arbeite)
- [2. Welche KI ich wofür benutze](#2-welche-ki-ich-wofür-benutze)
- [3. Vor dem Projekt: Frage, Wissen, Quellen](#3-vor-dem-projekt-frage-wissen-quellen)
- [4. Daten holen und prüfen](#4-daten-holen-und-prüfen)
- [5. Analyse: jede Frage einzeln](#5-analyse-jede-frage-einzeln)
- [6. Diagramme und Notebook](#6-diagramme-und-notebook)
- [7. Das fertige Notebook ist der Anfang](#7-das-fertige-notebook-ist-der-anfang)
- [8. Wann ich aufhöre](#8-wann-ich-aufhöre)
- [9. Was das für die Zusammenarbeit mit mir bedeutet](#9-was-das-für-die-zusammenarbeit-mit-mir-bedeutet)
- [10. Schluss](#10-schluss)

---

## Kurzfassung

1. Am Anfang steht eine echte eigene Frage.
2. Ich prüfe, ob ich das Thema verstehe und ob es Daten und Belege gibt.
3. KI schlägt Quellen vor. Ich prüfe jede Quelle und entscheide immer selbst, woher die Daten kommen.
4. Mit den Daten vor mir lege ich die Fragen neu fest: Was lässt sich realistisch beantworten?
5. KI schreibt den Code. Ich lasse mir jedes Ergebnis erklären und frage jedes Mal: Wie kann man das prüfen?
6. Zahlen und Diagramme vergleiche ich miteinander.
7. Wenn das Notebook fertig ist, beginnt die eigentliche Prüfung: mit einem festen Prüfauftrag, mit mehreren Modellen und in mehreren Runden.
8. Was ich nicht klären kann, steht offen bei den Einschränkungen des Projekts.

---

## 1. Warum ich so viel mit KI arbeite

Ich komme nicht aus der Wirtschaft und nicht aus der Blockchain-Welt. Ich bin Historiker. Mich interessiert aber fast jede Frage, und wenn ich eine Frage habe, will ich sie beantworten. KI gibt mir die Möglichkeit, das so genau wie möglich zu tun, auch in Bereichen, die nicht mein Fach sind.

Der zweite Grund ist persönlich. Ich bin neurodivers. Meine Gedanken sind schneller als meine Sprache. Im normalen Gespräch mit Kollegen fällt das nicht auf. In Test- und Interviewsituationen, besonders am Anfang, kann ich meine Arbeit mündlich aber nicht im Detail erklären. Was ich dann sage, klingt einfacher, als die Arbeit ist.

Dazu kommt: Ich verwende selten Fachbegriffe. Das liegt nicht daran, dass ich die Sache nicht verstehe. Ich muss einen Begriff nicht kennen, um zu sehen, was in den Daten passiert. Wenn ich etwas nicht verstehe, lasse ich es mir so lange einfacher erklären, bis ich es verstehe.

KI hilft mir, schnelle Gedanken und den fehlenden Hintergrund so aufzuschreiben, mit Zahlen und Diagrammen, dass andere sehen können, was ich untersucht und gefunden habe. Deshalb sind meine Projekte ausführlich dokumentiert. Die schriftliche Form zeigt meine Arbeit genauer als ein Gespräch.

---

## 2. Welche KI ich wofür benutze

| Werkzeug | Rolle |
|---|---|
| **Claude** | Hauptwerkzeug für den ganzen Ablauf: Planung, Code, SQL, Fehlersuche, Korrekturen, Dokumentation |
| **ChatGPT** | Suche nach Quellen; letzter Prüfer am Ende eines Projekts |
| **Lokale Modelle (Ollama)** | In VS Code eingebunden: ein Modell zum Chatten, eines zum Korrigieren und Kommentieren. Dazu gelegentlich eine Code-Prüfung. Ich benutze keinen Copilot. |

Lokale Modelle benutze ich nicht nur in VS Code, sondern sehr oft direkt im Terminal. Das hat für mich einen großen Vorteil: Was ich dort mit der lokalen KI ausprobiere, muss ich weder speichern noch löschen. Strg+C, das Terminal ist zu, und alles ist weg. Ich habe keine Hunderte gespeicherter Chats, die ich einordnen und sortieren muss. Bei so intensiver Arbeit ist das ein Vorteil und kein Nachteil: Nicht jeder Gedanke muss in einer Datei und einem Ordner landen.

Welche lokalen Modelle ich getestet habe und wie sie abgeschnitten haben, steht in den [Modelltests](MODELLTESTS_DE.md).

Ich vertraue keinem einzelnen Ergebnis, egal ob es von einem Menschen oder von einer KI kommt. Deshalb arbeite ich mit mehreren Modellen und prüfe mit Code nach.

### Wer macht was

Ich trenne diese Rollen bewusst.

| Aufgabe | Mensch | Pandas / SQL / Code | Lokale KI | Cloud-KI |
|---|---:|---:|---:|---:|
| Forschungsfrage wählen | **Hauptrolle** | – | Ideen möglich | Ideen / Diskussion |
| Projekt strukturieren | **Hauptrolle** | – | Unterstützung | starke Unterstützung |
| Exakte Summen / Mittelwerte / Counts | prüfen | **Hauptrolle** | nicht als Rechenquelle | nicht als Rechenquelle |
| SQL/Python schreiben | entscheiden / prüfen | ausführen | **sehr nützlich** | **sehr nützlich** |
| Debugging | logisch prüfen | Fehlermeldungen liefern | **sehr nützlich** | **sehr nützlich** |
| Interpretation | **entscheidend** | Fakten liefern | erste / zweite Analyse | tiefe zweite Meinung |
| Diagramme erzeugen | visuell prüfen | **Hauptrolle** | Code-Vorschläge | Code + Formatierung |
| Diagramme beurteilen | **Hauptrolle** | – | begrenzt | begrenzt |
| Text korrigieren | Endkontrolle | – | **sehr gut** | **sehr gut** |
| Dokumentation | Inhalt entscheiden | – | Entwurf | Entwurf / Formatierung |
| Aktuelle Web-Recherche | Quellen prüfen | – | meist nein | **wichtig** |
| Private lokale Daten | entscheiden | lokal | **Vorteil** | nur wenn passend/erlaubt |

Der Kern dahinter:

> **Berechnung und Interpretation sind nicht dasselbe.**

Wenn ich einen Durchschnitt brauche, soll Pandas ihn berechnen. Das Sprachmodell darf erklären, was er vielleicht bedeutet. Es soll aber nicht so tun, als wäre seine sprachliche Antwort selbst die Datenquelle.

Dieses Prinzip habe ich auch in meinem lokalen Data-Analysis-Workflow umgesetzt (siehe [Modelltests](MODELLTESTS_DE.md)): Pandas übernimmt deterministische Berechnungen; das lokale Modell interpretiert die berechneten Fakten. AI-generierter Python-Code wird nicht automatisch ausgeführt, sondern zuerst geprüft.

---

## 3. Vor dem Projekt: Frage, Wissen, Quellen

**Die Frage.** Meine Projekte beginnen oft spontan: mit einer Nachricht, mit etwas aus den sozialen Medien, mit einer Beobachtung im Solana-Ökosystem. Es sind immer meine echten Fragen, auch wenn sie für andere vielleicht seltsam klingen.

**Kann man sie beantworten?** Darüber denke ich nach, manchmal eine halbe Stunde, manchmal ein paar Tage. Dann prüfe ich zwei Dinge:

- Weiß ich genug über das Thema, um zu verstehen, worum es geht?
- Gibt es Daten, Belege und Quellen für das, was ich untersuchen will?

**Das Gespräch mit der KI.** Erst danach gehe ich zur KI. Ich beschreibe meine Idee, meine Fragestellung und wie das Ergebnis aussehen soll. Die KI prüft die Fragestellung, und wir diskutieren sie.

**Quellen.** Ich frage die KI, wo es passende Daten gibt. Sie macht Vorschläge. Ich sehe mir jede Quelle selbst an und entscheide immer selbst, woher die Daten kommen.

**Die Fragen neu stellen.** Wenn ich die Daten kenne, lege ich die Fragen noch einmal fest. Erst jetzt sehe ich, welche Fragen zu den Daten passen und welche realistisch sind.

**Zusammenfassung.** Am Ende dieser Phase lasse ich mir von der KI alles zusammenfassen: Fragestellung, Daten, Quellen. Ich prüfe, ob das logisch ist. Erst dann beginnt das Projekt.

In dieser ersten Phase wird fast alles festgelegt: welche Daten, welcher Markt, welcher Handelsplatz, welche Adressen. Das dauert lange. Dafür geht das Holen der Daten danach schnell.

---

## 4. Daten holen und prüfen

**Holen.** Wenn es eine Schnittstelle gibt (zum Beispiel Yahoo Finance, Dune, die Bundesbank), schreibt die KI den Code dafür. Wenn es keine gibt, suche ich die Dateien selbst (CSV, Excel). Alles kommt in eine feste Ordnerstruktur, damit nichts durcheinandergerät.

**Prüfen.** Wenn die KI mir sagt, die Daten seien in Ordnung, reicht mir das nicht. Ich lasse eigenen Prüfcode laufen und sehe mir das Ergebnis an:

- Duplikate
- fehlende Werte
- fehlende Tage
- Zahl der Zeilen und Spalten
- Median und Durchschnitt
- ob die Form der Daten zu dem passt, was ich erwarte

Das ist allgemeines Grundwissen, und ich mache es bei jedem Projekt.

---

## 5. Analyse: jede Frage einzeln

Ich gehe meine Fragen eine nach der anderen durch. Jede Frage bekommt ihren eigenen Schritt und ihren eigenen Code.

Das gilt auch für die Arbeit mit der KI selbst. Wenn es mehrere Punkte gibt, soll die KI einen Punkt nach dem anderen behandeln: erst wenn ein Punkt ganz erledigt ist, kommt der nächste. Das muss ich ihr in jedem Projekt mehrmals sagen, was ziemlich lästig ist.

Gleichzeitig ist die KI genau hier eine große Hilfe. In der Regel kommen mehrere Fragen auf einmal. Die KI hilft mir, Fragen, die nichts miteinander zu tun haben, getrennt zu behandeln und am Ende das Ergebnis zusammen zu sehen.

Zu jedem Ergebnis stelle ich der KI dieselben Fragen:

- Was bedeutet dieses Ergebnis?
- Wie kann man prüfen, ob es stimmt?

Jeder Punkt wird erklärt, bis ich ihn verstehe. Wenn eine Zahl nicht plausibel ist oder zwei Prüfungen sich widersprechen, mache ich weiter, bis es zusammenpasst.

Die Ergebnisse in meinen Projekten sind deshalb nicht in einem Schritt entstanden. Sie sind das Ergebnis vieler Runden.

---

## 6. Diagramme und Notebook

**Diagramme.** Die Diagramme entstehen meistens mit Matplotlib. Farben und Titel passe ich selbst an. Dann vergleiche ich jedes Diagramm mit den Zahlen darüber: Zeigt das Bild dasselbe wie die Tabelle?

**Notebook.** Am Ende gebe ich das ganze Notebook an die KI, mit einem Beispiel für meine feste Struktur:

- Einführung und Fragestellung am Anfang
- Bibliotheken nur einmal laden
- jeder Schritt getrennt
- keine Wiederholungen im Code

Die KI räumt auf. Die Logik des Codes darf sich dabei nicht ändern.

---

## 7. Das fertige Notebook ist der Anfang

Wer ein fertiges Notebook sieht, denkt: Das Projekt ist fertig. Bei mir fängt die eigentliche Arbeit an dieser Stelle an.

Der Ablauf am Ende ist bei allen Projekten gleich:

1. **Lokale KI.** Code und Notebook gehen noch einmal an ein lokales Modell.
2. **Claude mit festem Prüfauftrag.** Ich habe einen langen, festen Prüfauftrag. Er verlangt: Zahlen prüfen, Logik prüfen, Ungenauigkeiten finden. Jeder Fehler muss belegt sein. Erfundene Fehler zählen nicht. Das läuft ein- bis zweimal, auch mit verschiedenen Claude-Modellen und Denkstufen.
3. **Korrektur.** Die gefundenen Fehler werden nacheinander bearbeitet.
4. **ChatGPT als letzter Prüfer.** ChatGPT bekommt denselben Prüfauftrag mit dem Hinweis, dass es die finale Version ist. Es findet fast immer noch etwas. In dieser Rolle ist ChatGPT nach meiner Erfahrung stärker als Claude.
5. **Gegenprüfung.** Die Funde von ChatGPT gehen zurück an Claude mit der Frage: Stimmen diese Fehler? Bisher haben sie immer gestimmt. Claude korrigiert sie.
6. **Kontrolle der Korrektur.** Zum Schluss prüft ChatGPT, ob die Korrektur stimmt.

Warum ChatGPT erst am Ende kommt: Würde ich ihm ein Projekt früher geben, wären es zu viele Funde auf einmal, und ich müsste das ganze Projekt neu erklären.

**Was ChatGPT nicht prüfen kann.** Manchmal nennt ChatGPT Punkte, die es nicht prüfen kann, weil Daten fehlen oder weil ihm eine Frage unklar ist. Dann frage ich, was genau es braucht. Die nötigen Daten gebe ich ihm, wenn es möglich ist. Es gibt aber Daten, die ich nicht an ChatGPT geben will, und die bekommt es nicht. Offene Fragen beantworte ich, bis ChatGPT alles geprüft hat, was sich prüfen lässt.

**Was ich selbst prüfen muss.** Claude und ChatGPT können nicht alles öffnen, lesen oder sehen: zum Beispiel meine Dashboards und manche Links und Quellen. Diese Dinge prüfe ich im letzten Durchgang selbst noch einmal. Wenn etwas ungenau oder falsch ist, korrigiere ich es oder frage die KI, wo der Fehler liegt und wie ich ihn korrigieren kann.

Was in den letzten Runden gefunden wird, sind meistens Kleinigkeiten: eine ungenaue Formulierung, eine gerundete Zahl, eine alte Version eines Dashboards. Ich korrigiere sie trotzdem, weil ich sehr genau bin.

Danach schreibe ich das README. Die Struktur bespreche ich mit der KI, der Text entsteht Schritt für Schritt, und ich korrigiere ihn.

In einem Projekt stecken auf diese Weise viele Stunden: manchmal 20, manchmal 50 oder mehr, und beim Telegram-Projekt (persian-media-analysis) waren es rund 100. Das sind realistische Zeiten. Kein Projekt ist die Arbeit von einer Stunde.

Das Datum eines Repos zeigt nicht, wann die Arbeit begonnen hat. Ein Projekt kommt erst auf GitHub, wenn es zu etwa 90 % fertig ist.

Dass alle Repos seit September 2026 entstanden sind, hat einen einfachen Grund: Im September habe ich angefangen, mein Portfolio aufzubauen. Bei einigen Themen war ein Teil der Arbeit schon vorher erledigt. Im September habe ich dann Tag und Nacht gearbeitet, um die Projekte nacheinander fertigzustellen und zu veröffentlichen.

Manche Projekte laufen parallel. Wenn die lokale KI zum Beispiel stundenlang Texte analysiert, warte ich nicht ab. Ich arbeite am nächsten Projekt und erledige zwischendurch Kleinigkeiten am ersten. Wer sich wundert, woher die Zeit kommt: Jeder Tag hat 24 Stunden, und das Wochenende hat zwei Tage.

Die meiste Zeit arbeite ich aber an einem Projekt, bis es fertig ist. Mein Fokus ist besser, wenn ich nur eine Aufgabe habe und nicht mehrere gleichzeitig.

---

## 8. Wann ich aufhöre

KI sagt oft: Das ist ausreichend. Mir reicht das meistens nicht. Ich suche weiter, bis meine Frage beantwortet ist.

Es gibt zwei Gründe, aus denen ich aufhöre:

- **Die Daten gibt es nicht.** Ein Beispiel sind Gebühren: Wenn in einem Projekt steht, dass keine Gebührendaten vorhanden sind, habe ich vorher mehrere Abfragen probiert, und die Tabelle war jedes Mal leer.
- **Die Frage ergibt keinen Sinn.** Weil viele Themen nicht mein Fach sind, kann es passieren, dass ich nach etwas suche, das es so nicht gibt. Wenn ich das merke, höre ich auf.

Und es gibt einen dritten Fall: Ein großes Projekt ist fertig, und es bleibt eine Kleinigkeit, die das Ergebnis nicht verändert. Dann baue ich nicht das ganze Projekt um. Ich nenne den Punkt offen bei den Einschränkungen.

---

## 9. Was das für die Zusammenarbeit mit mir bedeutet

- Ich arbeite schnell, prüfe aber alles mehrmals.
- Aufgaben bekomme ich am liebsten schriftlich.
- Ergebnisse liefere ich am besten schriftlich: als Bericht, mit Zahlen und Diagrammen.
- Wenn jemand wissen will, wie ein Ergebnis entstanden ist, steht es in der Dokumentation des Projekts. Ich zeige es gern dort.

---

## 10. Schluss

Ich benutze KI sehr viel. Das bedeutet nicht, dass ich eine KI frage und ihre Antwort übernehme. Es bedeutet, dass ich mit mehreren Modellen, mit Code und mit eigenen Prüfungen arbeite, bis ich einem Ergebnis glaube.

Die Modelle sparen mir Zeit. Das Denken sparen sie mir nicht.

> **Benutze KI. Aber benutze dein Gehirn.**

---

## Rechte

© 2026 Amirhoushang Rahmannejad. Alle Rechte vorbehalten. Ansehen und Prüfen ist ausdrücklich erwünscht. Kopieren, Ändern oder Weiterverbreiten nur mit meiner schriftlichen Erlaubnis.
