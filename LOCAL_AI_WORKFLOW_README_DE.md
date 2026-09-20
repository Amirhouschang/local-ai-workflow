# Wie ich lokale und Cloud-KI tatsächlich benutze

**English:** [README.md](README.md)


> **Persönlicher Praxisbericht – kein allgemeiner KI-Benchmark**  
> Stand: September 2026

Dieses README beschreibt meinen eigenen Umgang mit KI bei Datenanalyse, Python, SQL, Dokumentation, Sprache und alltäglichen Aufgaben. Es ist kein wissenschaftlicher Benchmark und keine Empfehlung für jede Hardware. Die Ergebnisse gelten für **meinen Rechner, meine Modelle, meine Einstellungen und meine Arbeitsweise**.

Ich bin kein KI-Ingenieur und will auch nicht so tun. Ich benutze KI als Werkzeug. Manche Dinge macht sie extrem schnell. Manche Dinge macht sie überzeugend falsch. Beides ist wichtig zu wissen.

Diesen Laptop habe ich nicht mit Blick auf lokale KI gekauft. Zum Kaufzeitpunkt hatte ich weder vor, lokale KI zu betreiben, noch KI-Ingenieur zu werden. Er hat nicht einmal eine dedizierte GPU – eine der wichtigsten Komponenten, um KI-Modelle effizient laufen zu lassen.

Mein wichtigster Grundsatz ist deshalb:

> **KI kann Arbeit beschleunigen. Sie übernimmt aber nicht die Verantwortung dafür, ob ein Ergebnis stimmt.**

Das gilt für lokale Modelle genauso wie für Cloud-KI.

> English version: `LOCAL_AI_WORKFLOW_README_EN.md`

## Inhaltsverzeichnis

- [Kurzfassung](#kurzfassung)
- [1. Was dieses README ist – und was nicht](#1-was-dieses-readme-ist--und-was-nicht)
- [2. Mein System](#2-mein-system)
- [3. Erst musste die lokale Ausführung sauber funktionieren](#3-erst-musste-die-lokale-ausführung-sauber-funktionieren)
- [4. Warum ich derzeit lieber direkt mit Ollama arbeite](#4-warum-ich-derzeit-lieber-direkt-mit-ollama-arbeite)
- [5. Mensch, deterministischer Code, lokale KI und Cloud-KI](#5-mensch-deterministischer-code-lokale-ki-und-cloud-ki)
- [6. Warum ich überhaupt lokale KI will](#6-warum-ich-überhaupt-lokale-ki-will)
- [7. Warum Cloud-KI trotzdem eine sehr große Rolle behält](#7-warum-cloud-ki-trotzdem-eine-sehr-große-rolle-behält)
- [8. Mein eigentlicher Projekt-Workflow](#8-mein-eigentlicher-projekt-workflow)
- [9. Mein lokaler Data-Analysis-Workflow](#9-mein-lokaler-data-analysis-workflow)
- [10. Wie ich die lokalen Modelle getestet habe](#10-wie-ich-die-lokalen-modelle-getestet-habe)
- [11. Ein sehr wichtiger Test: `COUNT(*)` war korrekt](#11-ein-sehr-wichtiger-test-count-war-korrekt)
- [12. Zweiter wichtiger Test: Many-to-Many Merge](#12-zweiter-wichtiger-test-many-to-many-merge)
- [13. Geschwindigkeit: meine beobachteten Zeiten](#13-geschwindigkeit-meine-beobachteten-zeiten)
- [14. Die getesteten Modelle](#14-die-getesteten-modelle)
- [15. Meine aktuelle Modell-Aufteilung](#15-meine-aktuelle-modell-aufteilung)
- [16. Was ich aus dem Vergleich gelernt habe](#16-was-ich-aus-dem-vergleich-gelernt-habe)
- [17. KI und Genauigkeit](#17-ki-und-genauigkeit)
- [18. Diagramme: ein Bereich, in dem der Mensch sehr wichtig bleibt](#18-diagramme-ein-bereich-in-dem-der-mensch-sehr-wichtig-bleibt)
- [19. Wie ich AI-generierten Code behandle](#19-wie-ich-ai-generierten-code-behandle)
- [20. Warum ich trotzdem gerne mit KI programmiere](#20-warum-ich-trotzdem-gerne-mit-ki-programmiere)
- [21. Mein praktischer Qualitätscheck](#21-mein-praktischer-qualitätscheck)
- [22. Ein kleines Beispiel für einen guten KI-Test](#22-ein-kleines-beispiel-für-einen-guten-ki-test)
- [23. Was lokale KI bei mir nicht ersetzen soll](#23-was-lokale-ki-bei-mir-nicht-ersetzen-soll)
- [24. Aktueller praktischer Ablauf im Alltag](#24-aktueller-praktischer-ablauf-im-alltag)
- [25. Ein paar einfache Ollama-Befehle](#25-ein-paar-einfache-ollama-befehle)
- [26. Was ich noch verbessern kann](#26-was-ich-noch-verbessern-kann)
- [27. Was dieses Experiment für mich gezeigt hat](#27-was-dieses-experiment-für-mich-gezeigt-hat)
- [28. Schluss](#28-schluss)

---

## Kurzfassung

Mein Workflow ist inzwischen ungefähr so:

1. Ich denke zuerst selbst über die Frage, das Projekt und die Struktur nach.
2. Ich baue einen möglichst konkreten Projektplan.
3. KI hilft mir, Lücken, technische Probleme und mögliche Fehler im Plan zu finden.
4. Erst wenn der Plan stabil ist, beginne ich mit dem eigentlichen Code oder SQL.
5. Für exakte Berechnungen benutze ich Pandas, SQL oder andere deterministische Werkzeuge.
6. Lokale KI hilft bei Code, Debugging, Erklärungen, Texten und erster Analyse.
7. Für schwierige oder wichtige Fragen benutze ich mehrere Modelle oder zusätzlich Cloud-KI.
8. Ich prüfe Zahlen, Tabellen, Diagramme und Aussagen selbst.
9. Wenn etwas logisch oder visuell nicht stimmt, ist syntaktisch korrekter Code für mich noch kein Beweis.
10. Am Ende entscheide ich selbst, was in ein Projekt, einen Bericht oder ein Dashboard gehört.

Ich versuche also nicht, **eine KI zu finden, der ich alles glauben kann**. Ich versuche, für unterschiedliche Aufgaben passende Werkzeuge zu haben.

---

## 1. Was dieses README ist – und was nicht

Dieses Dokument ist eine Zusammenfassung meiner privaten Tests mit lokalen Sprachmodellen über Ollama sowie meiner praktischen Arbeit mit Cloud-KI.

Es soll zeigen:

- warum ich lokale KI benutze,
- warum ich trotzdem Cloud-KI weiter benutze,
- wie ich verschiedene Modelle getestet habe,
- welche Modelle bei mir für welche Aufgaben funktionieren,
- wo sie Fehler gemacht haben,
- warum Geschwindigkeit allein nicht reicht,
- warum größere Modelle nicht automatisch besser sind,
- warum ein spezielles Coding-Modell trotzdem Logikfehler machen kann,
- wie ich KI in echte Data-Analysis-Projekte integriere,
- und warum der Mensch am Ende weiterhin prüfen muss.

Es soll **nicht** zeigen:

- dass ein Modell generell „das beste Modell" ist,
- dass meine Zeiten auf anderer Hardware gleich wären,
- dass Open WebUI grundsätzlich schlecht ist,
- dass lokale KI automatisch sicher oder in jeder Firma erlaubt ist,
- oder dass ich ein professionelles KI-Benchmarking durchgeführt habe.

Die Tests waren bewusst praxisnah. Ich habe Modelle mit Aufgaben getestet, die tatsächlich zu meinem Alltag passen.

---

## 2. Mein System

Die Resultate in diesem README beziehen sich auf ungefähr folgende lokale Umgebung:

| Komponente | Mein System |
|---|---|
| Betriebssystem | EndeavourOS / Arch Linux |
| CPU | AMD Ryzen AI 9 HX 370, 12 Kerne / 24 Threads |
| RAM | ca. 93 GiB nutzbar |
| GPU | integrierte AMD Radeon 890M (`gfx1150`) |
| GPU-Speicher | Shared Memory |
| NVIDIA | keine |
| Ollama | lokale Modell-Ausführung |
| Docker | für weitere lokale Dienste |
| Oberfläche | Terminal direkt über Ollama; Open WebUI zusätzlich getestet |

Das ist wichtig. Ein Modell, das auf meinem Rechner 20 Minuten braucht, kann auf einer starken dedizierten GPU völlig anders aussehen.

Deshalb sind Aussagen wie „Modell X ist langsam" in diesem README eigentlich Abkürzungen für:

> **Modell X war bei dieser Aufgabe auf meinem System und mit meiner Konfiguration langsam.**

---

## 3. Erst musste die lokale Ausführung sauber funktionieren

Ein Teil des Experiments war nicht das Modell selbst, sondern die Umgebung.

Meine integrierte AMD-GPU wurde anfangs von Ollama nicht sinnvoll verwendet. Nach Aktivierung der iGPU-Nutzung konnte ich mit `ollama ps` sehen, dass Modelle tatsächlich auf der GPU lagen.

Für mich war diese Kontrolle wichtig. Sonst vergleicht man am Ende Modelle, obwohl man in Wirklichkeit unterschiedliche Hardware-Pfade vergleicht.

Ein zweiter Punkt war der Kontext.

Bei einem Modell zeigte Open WebUI einen Kontext von `32768`, obwohl ich mit kleineren Einstellungen arbeiten wollte. Eine direkte Anfrage an Ollama mit `num_ctx=8192` wurde dagegen korrekt mit 8192 ausgeführt.

Das war einer der Gründe, warum ich meine eigentlichen Vergleichstests später **direkt im Terminal** gemacht habe.

---

## 4. Warum ich derzeit lieber direkt mit Ollama arbeite

Ich hatte zunächst viel über Open WebUI getestet. Dabei wirkten manche großen Modelle fast unbrauchbar langsam: mehrere Minuten, teilweise Timeouts.

Dann habe ich dieselben Modelle direkt gestartet:

```bash
ollama run MODELNAME
```

Und plötzlich war das Bild teilweise komplett anders.

Einige Modelle begannen direkt zu antworten und streamten Wort für Wort. Für mich fühlt sich das viel besser an, weil ich sofort sehe, dass etwas passiert.

Das heißt **nicht**, dass Open WebUI schlecht ist.

Mögliche Gründe für den Unterschied sind zum Beispiel:

- längere System-Prompts,
- Chat-History,
- Tools / Function Calling,
- zusätzliche Kontextdaten,
- eine größere Context-Window-Einstellung,
- meine eigene Konfiguration,
- oder Dinge, die ich in Open WebUI noch nicht optimal eingestellt habe.

Ich will daraus keine allgemeine Behauptung machen.

Für **meine aktuelle Arbeitsweise** ist die direkte Ollama-Nutzung aber einfacher, transparenter und schneller.

Open WebUI kann weiterhin sinnvoll sein, besonders wenn ich später Dateien, RAG, Tools oder eine komfortable Oberfläche brauche. Für reine Modelltests möchte ich aber möglichst wenig zusätzliche Schichten zwischen Prompt und Modell haben.

---

## 5. Mensch, deterministischer Code, lokale KI und Cloud-KI

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

Dieses Prinzip habe ich auch in meinem lokalen Data-Analysis-Workflow umgesetzt: Pandas übernimmt deterministische Berechnungen; das lokale Modell interpretiert die berechneten Fakten. AI-generierter Python-Code wird nicht automatisch ausgeführt, sondern zuerst geprüft.

---

## 6. Warum ich überhaupt lokale KI will

### 6.1 Datenschutz und Gewohnheit

Nicht jede Datei ist streng geheim. Trotzdem möchte ich nicht automatisch alles an einen externen Dienst schicken.

Für manche Sachen gilt für mich einfach:

> Wenn es lokal bleiben kann, warum soll es überhaupt meinen Rechner verlassen?

Das ist nicht immer eine Frage von „hoch sensibel" oder „nicht sensibel". Es geht auch um persönliche Gewohnheit und Datenminimierung.

Natürlich gilt: **lokal bedeutet nicht automatisch erlaubt**. In einer Firma entscheidet die interne IT-/Datenschutz-/Compliance-Regel. Wenn Cloud-KI verboten ist, bedeutet das nicht automatisch, dass jedes lokale Modell erlaubt ist.

Aber ich möchte technisch in der Lage sein, auch ohne Cloud-KI sinnvoll zu arbeiten.

### 6.2 Kosten und Limits

Cloud-KI ist sehr leistungsfähig, aber lange Arbeitssitzungen produzieren sehr viele Nachrichten:

- eine SQL-Abfrage,
- Fehler,
- neue Version,
- neue Fehlermeldung,
- Daten prüfen,
- Text korrigieren,
- Diagramm ändern,
- noch einmal prüfen,
- README überarbeiten,
- wieder zurück zum Code.

Wenn jede kleine Frage in die Cloud muss, kann ein Nutzungs- oder API-Limit sehr schnell unnötig belastet werden.

Eine kleine Syntaxfrage oder eine normale E-Mail muss für mich nicht immer die stärkste Cloud-KI verbrauchen.

Lokale KI ist deshalb auch eine Art **Arbeitsentlastung für die Cloud**.

### 6.3 Unabhängigkeit

Es kann Umgebungen geben, in denen externe KI nicht genutzt werden darf oder zeitweise nicht verfügbar ist.

Ich will deshalb nicht in einer Situation sein, in der mein gesamter Workflow zusammenbricht, sobald ChatGPT oder ein anderer Cloud-Dienst nicht verfügbar ist.

Lokale Modelle müssen nicht alles können. Sie müssen genug können, damit ich weiterarbeiten kann.

---

## 7. Warum Cloud-KI trotzdem eine sehr große Rolle behält

Lokale KI ersetzt für mich Cloud-KI nicht vollständig.

Cloud-KI bleibt besonders wichtig für:

- komplexe Aufgaben mit vielen Schritten,
- starke zweite oder dritte Kontrolle,
- aktuelle Recherche im Web,
- Arbeiten mit vielen Dateien,
- umfangreiche Formatierung,
- fertige Dokumente und Artefakte,
- schwierige methodische Diskussionen,
- längere Projektplanung,
- und Fälle, in denen lokale Modelle qualitativ nicht reichen.

Mein Ziel ist deshalb kein **Local-only-System**.

Mein Ziel ist eher:

> **Benutze lokal, was lokal gut funktioniert. Benutze Cloud dort, wo die zusätzliche Qualität oder Funktion wirklich etwas bringt.**

---

## 8. Mein eigentlicher Projekt-Workflow

Das ist für mich inzwischen wichtiger als die Frage, welches Modell gerade „Nummer 1" ist.

### Phase 1 – Erst selbst denken

Bevor ich Code generieren lasse, versuche ich zu klären:

- Was ist eigentlich meine Frage?
- Was will ich zeigen?
- Welche Daten habe ich?
- Was kann ich mit diesen Daten nicht zeigen?
- Welche Kennzahlen brauche ich?
- Welche Begriffe müssen sauber definiert werden?
- Welche Risiken für Double Counting, falsche Granularität oder falsche Interpretation gibt es?

Wenn diese Dinge unklar sind, produziert KI sehr schnell sehr viel Code für ein schlecht definiertes Problem.

Das spart keine Zeit.

### Phase 2 – Aus der Idee einen echten Projektplan machen

Danach diskutiere ich den Plan mit KI.

Ein guter technischer Plan enthält für mich zum Beispiel:

- Forschungsfrage,
- Zeitraum,
- Datenquellen,
- Tabellen,
- notwendige Spalten,
- Definition jeder Kennzahl,
- Aggregationsebene,
- SQL-/Python-Schritte,
- Query-Namen,
- Diagramm-Titel,
- erwartete Outputs,
- Validierungschecks,
- bekannte Grenzen,
- Dateistruktur,
- README-Struktur.

Dieser Teil kann erstaunlich viel Zeit sparen.

Wenn der Plan fertig ist, muss ich beim eigentlichen Projekt nicht bei jedem kleinen Schritt neu überlegen, wie eine Tabelle heißen soll oder welche Granularität ich brauche.

Gerade weil Deutsch und Englisch nicht meine Muttersprache sind, hilft mir ein sauber vorbereiteter Plan auch bei konsistenten:

- Titeln,
- Variablennamen,
- Query-Namen,
- Diagramm-Bezeichnungen,
- kurzen Erklärungen,
- README-Abschnitten.

Dann kann ich vieles kontrolliert übernehmen, statt dieselbe sprachliche Kleinarbeit zwanzigmal neu zu formulieren.

### Phase 3 – Code erzeugen, aber nicht blind ausführen

KI ist für mich sehr nützlich beim ersten Entwurf von:

- Python,
- Pandas,
- SQL,
- Dune/Trino SQL,
- Datenbereinigung,
- Visualisierungscode,
- Funktionen,
- Dateiexporten.

Aber ein Codeblock ist noch kein Ergebnis.

Ich schaue:

- Welche Spalten werden benutzt?
- Ist die Granularität richtig?
- Wird vor oder nach einem Join aggregiert?
- Kann ein Merge Zeilen vervielfachen?
- Wird `count` benutzt, obwohl `nunique` gemeint ist?
- Wird ein Durchschnitt auf Transaktions- oder Nutzerebene berechnet?
- Stimmen Zähler und Nenner?
- Passt die Zahl überhaupt zur Realität?

### Phase 4 – Fehler schneller finden

Natürlich gibt es Fehler.

Ohne KI kann ich Dokumentation lesen, Foren durchsuchen, Websuche benutzen oder die Pandas-/Dune-/Trino-Dokumentation prüfen. Das mache ich auch weiterhin.

Aber KI kann den Debugging-Prozess stark verkürzen. Statt zwei Stunden nach einem kleinen Syntax- oder Typfehler zu suchen, kann ich häufig in wenigen Minuten verstehen, wo das Problem wahrscheinlich liegt.

Das ist für mich einer der größten praktischen Vorteile.

### Phase 5 – Ergebnis ansehen, nicht nur Code ansehen

Ein Punkt ist für mich besonders wichtig:

> **Ein Code kann plausibel aussehen und trotzdem ein absurdes Ergebnis produzieren.**

Ein Sprachmodell sieht oft einen sauberen Codeblock und erklärt sehr überzeugend, warum er gut sei.

Ein Mensch schaut auf das Diagramm und denkt:

> Moment. Das kann doch nicht stimmen.

Zum Beispiel:

- ein Anteil über einer mathematisch möglichen Grenze,
- ein Diagramm mit merkwürdiger Skala,
- Zahlen, die sich gegenseitig widersprechen,
- ein Token mit falschem Label,
- ein Mittelwert, der nicht zur Verteilung passt,
- ein Top-10-Anteil, der offensichtlich unplausibel ist.

Deshalb gehören für mich **visuelle Prüfung und Plausibilitätsprüfung** zur Analyse und nicht nur zur Präsentation.

### Phase 6 – Zweites Modell oder zweite Methode

Wenn etwas wichtig ist, frage ich nicht unbedingt dasselbe Modell noch fünfmal.

Ich kann:

- ein anderes lokales Modell benutzen,
- Cloud-KI als zweite Meinung nehmen,
- die Zahl selbst nachrechnen,
- eine kleine Testtabelle bauen,
- Dokumentation lesen,
- eine alternative Query schreiben.

Mehrere unabhängige Wege sind oft besser als mehr Selbstvertrauen im ersten Ergebnis.

Und ganz wichtig:

> **Wenn drei Modelle denselben Fehler machen, wird der Fehler nicht demokratisch richtig.**

Diesen Gedanken habe ich nicht aus KI-Tests, sondern aus meinem Geschichtsstudium mitgenommen: Auch wenn sich viele Quellen oder viele Historiker in derselben falschen Einschätzung einig sind, macht die Übereinstimmung die Einschätzung nicht richtig.

---

## 9. Mein lokaler Data-Analysis-Workflow

Ich habe bereits einen allgemeinen lokalen Workflow gebaut, der genau diese Trennung versucht umzusetzen.

Die Grundidee:

- CSV laden und bereinigen → Pandas
- Datentypen / Missing Values / Duplikate → Pandas
- Statistiken und Korrelationen → Pandas
- erste Diagramme → Matplotlib
- strukturelle Fragen → lokales RAG
- Interpretation → lokales Ollama-Modell
- sichere natürliche Berechnungsfragen → vordefinierte Pandas-Operationen
- Code-Vorschläge → lokales Code-Modell
- AI-generierten Code → **nicht automatisch ausführen**

Für einfache natürliche Berechnungsfragen erzeugt das Modell nur einen kleinen strukturierten Plan. Das Notebook prüft diesen Plan und führt nur erlaubte Pandas-Operationen aus.

Das gefällt mir wesentlich besser als:

```text
User fragt etwas
→ LLM schreibt beliebigen Python-Code
→ Code wird automatisch ausgeführt
→ hoffen, dass alles stimmt
```

Mein Ziel ist nicht maximale „Agenten-Magie", sondern ein Workflow, den ich verstehen und kontrollieren kann.

---

## 10. Wie ich die lokalen Modelle getestet habe

Ich wollte nicht einfach Online-Rankings übernehmen.

Ein Modell kann auf einem Benchmark sehr gut sein und für meine tatsächliche Arbeit trotzdem nervig sein.

Deshalb habe ich Aufgaben genommen, die ich wirklich brauche.

### Sprach- und Alltagstests

Unter anderem:

- kurze professionelle E-Mail auf Deutsch,
- fehlerhaften englischen Text natürlich korrigieren,
- API in einfachem Deutsch erklären,
- vier kurze Bullet Points zusammenfassen,
- auf normales Alltagsenglisch reagieren, ohne wie ein Lehrer zu klingen.

### Analyse-Test

Ich gab zwei hypothetische DeFi-Lending-Protokolle mit unterschiedlichem Borrow Volume, unterschiedlicher Zahl von Borrowern, unterschiedlichen Positionsgrößen, Liquidationsvolumen und Borrower Concentration.

Das Modell sollte:

- Beobachtung und Interpretation trennen,
- alternative Erklärungen nennen,
- zusätzliche Metriken vorschlagen,
- problematische Kennzahlen erkennen,
- keine Kausalität erfinden.

### Coding- und Debugging-Tests

Hier wurde es interessanter.

Ich testete unter anderem:

1. **Pandas Duplicate Weighting**  
   Ein `transform("sum")` erzeugt Borrower-Gesamtsummen auf mehreren Transaktionszeilen. Danach wird fälschlich der Mittelwert dieser wiederholten Werte berechnet.

2. **Many-to-Many Merge / Double Counting**  
   Mehrere Borrow-Zeilen und mehrere Liquidation-Zeilen desselben Borrowers werden direkt gemerged. Dadurch entsteht ein kartesisches Produkt.

3. **SQL ohne Fehler**  
   Eine Query war bewusst korrekt. Das Modell sollte nur einen Fehler melden, wenn wirklich einer vorhanden war.

4. **Code selbst schreiben**  
   Das Modell sollte aus einer einfachen DataFrame-Struktur korrekte Monatsmetriken inklusive Top-10-Borrower-Share bauen.

5. **Mehrere Bugs gleichzeitig**  
   Ein Test kombinierte Merge Explosion, falsches `count`, falsche Durchschnittsebene und Liquidationsvolumen.

Gerade die **No-Bug-Aufgabe** war sehr hilfreich. Sie zeigte, dass Modelle nicht nur echte Fehler übersehen, sondern auch Fehler erfinden können.

---

## 11. Ein sehr wichtiger Test: `COUNT(*)` war korrekt

Die SQL-Query aggregierte zuerst auf:

```text
month + protocol + borrower
```

Danach wurde auf:

```text
month + protocol
```

gruppiert.

Damit entspricht jede Zeile der CTE bereits genau einem eindeutigen Borrower im jeweiligen Monat und Protokoll.

Deshalb war:

```sql
COUNT(*)
```

vollkommen korrekt.

Trotzdem behaupteten sowohl:

- `qwen3-coder:30b`
- `qwen2.5:32b`

noch, dass `COUNT(*)` falsch sei und durch `COUNT(DISTINCT borrower)` ersetzt werden müsse.

Das neue SQL hätte in diesem Fall zwar denselben Wert geliefert, aber die Begründung war falsch.

Für mich war das ein wichtiges Ergebnis:

> Ein Modell kann einen „Fix" liefern, der das Ergebnis nicht kaputtmacht, und trotzdem die Logik falsch verstanden haben.

`qwen3.6:27b` erkannte dagegen ziemlich früh, dass die Query korrekt war. Das Problem: Es dachte danach sehr lange weiter und überprüfte dieselbe Sache immer wieder.

**Richtig, aber 30 Minuten darüber nachdenken, ob etwas richtig ist, ist im Alltag auch nicht ideal.**

---

## 12. Zweiter wichtiger Test: Many-to-Many Merge

Das Beispiel:

- Borrower `0x1` hat zwei Borrow-Zeilen.
- Derselbe Borrower hat zwei Liquidation-Zeilen.

Direkter Merge auf:

```python
["month", "protocol", "borrower"]
```

erzeugt:

```text
2 Borrows × 2 Liquidations = 4 Zeilen
```

Damit werden sowohl Borrow- als auch Liquidation-Werte vervielfacht.

### Ergebnis der Modelle

`qwen3-coder:30b` erkannte den Merge-Fehler, schrieb danach aber selbst einen unvollständigen Fix: Es aggregierte nur eine Seite korrekt und konnte dadurch wieder Werte vervielfachen.

`qwen2.5:32b` übersah in einem späteren Mehrfach-Bug-Test den zentralen Merge-Fehler komplett.

`qwen3.6:27b` erkannte:

- den Many-to-Many-Join,
- die Inflation des Borrow Volume,
- die Inflation des Liquidation Volume,
- `count` statt `nunique`,
- die falsche Durchschnittsebene,

und schlug die saubere Lösung vor:

1. Borrows separat aggregieren.
2. Liquidations separat aggregieren.
3. Erst danach die aggregierten Tabellen verbinden.

Das war qualitativ die beste Antwort – aber deutlich langsamer.

---

## 13. Geschwindigkeit: meine beobachteten Zeiten

Diese Zeiten sind **keine standardisierten Benchmarks**. Sie sind einzelne reale Durchläufe auf meinem System.

| Test | Modell | Einzelner Durchlauf (n=1) | Ergebnis |
|---|---|---:|---|
| SQL „kein Fehler vorhanden" | Qwen3-Coder 30B | ca. 1:12 min | schnell, aber erfand einen Bug |
| SQL „kein Fehler vorhanden" | Qwen2.5 32B | ca. 5:54 min | langsamer, erfand denselben Bug |
| SQL „kein Fehler vorhanden" | Qwen3.6 27B | >30 min, abgebrochen | erkannte früh, dass Query korrekt war, blieb aber im Thinking |
| Pandas-Code neu schreiben | Qwen3-Coder 30B | ca. 36 s | gute Lösung |
| Pandas-Code neu schreiben | Qwen2.5 32B | ca. 4:10 min | brauchbar, aber fragiler Teil |
| Pandas-Code neu schreiben | Qwen3.6 27B | ca. 20:44 min | logisch sauber, extrem langsam |
| Mehrfach-Bug-Pandas | Qwen3-Coder 30B | ca. 1:44 min | mehrere echte Punkte erkannt, aber neue Fehler im Fix |
| Mehrfach-Bug-Pandas | Qwen2.5 32B | ca. 5:35 min | zentrale Merge-Problematik nicht sauber behoben |
| Mehrfach-Bug-Pandas | Qwen3.6 27B | ca. 18:24 min | beste Logik und sauberer Fix |
| 4 Sprach-/Alltagsfragen | Mistral Small 3.1 | ca. 3:38 min gesamt | sprachlich sehr gut |

Das zeigt für mich sehr deutlich:

> **Geschwindigkeit, Modellgröße und Genauigkeit sind drei verschiedene Dinge.**

---

## 14. Die getesteten Modelle

### `mistral-small3.1`

**Getestet für:** deutsche E-Mail, Englischkorrektur, Alltagssprache, einfache technische Erklärung.

**Stärken:** natürliche Sprache, kontrollierte Korrekturen, freundlich ohne unnötig formell zu werden.

**Schwäche:** nicht das absolut schnellste kleine Modell auf meinem System.

**Rolle:** **Hauptkandidat für Alltag, Schreiben, Sprache und normale Gespräche.**

### `gemma2:9b-instruct`

**Stärken:** sehr schnell, gute kurze E-Mails, gute einfache Erklärungen, gutes natürliches Englisch.

**Schwächen:** bei Tools / Function Calling in Open WebUI gab es Probleme; verändert den Stil manchmal stärker als gewünscht.

**Rolle:** **Sehr schnelles Alltags- und Büromodell.**

### `qwen2.5:14b-instruct`

**Stärken:** schnell, sachlich, gute Zusammenfassungen, gute E-Mail- und Erklärungstexte.

**Schwächen:** teilweise etwas steifer; überschneidet sich stark mit anderen Alltagsmodellen.

**Rolle:** **Sachliches General-/Büromodell, möglicherweise später redundant.**

### `gpt-oss:20b`

**Stärken:** gute natürliche Sprache, gute Korrekturen, angenehmer Allrounder.

**Schwächen:** bei technischen Fragen gab es mehrere selbstbewusst formulierte Fehler oder Übertreibungen, unter anderem bei RAM/VRAM, Ollama/GPU, Modellgrößen und Ethereum-Historie.

**Rolle:** **Guter Generalist für Sprache und Alltag, aber technische Aussagen prüfe ich.**

### `llama3.1:8b-instruct`

**Stärken:** sehr schnell, brauchbar für einfache Fragen, gute einfache Englischkorrektur.

**Schwächen:** schwächer bei technischer Präzision; sprachlich teilweise etwas holprig.

**Rolle:** **Sehr schneller kleiner Fallback, aber möglicherweise redundant zu Gemma.**

### `qwen2.5:32b`

**Stärken:** direkte Antworten, gute allgemeine Analyse, im Terminal deutlich angenehmer als vorher in Open WebUI.

**Schwächen:** im Coding-Vergleich oft deutlich langsamer als Qwen3-Coder und nicht besser; erfand beim korrekten SQL-Test einen Fehler; übersah im komplexeren Pandas-Test den wichtigsten strukturellen Fehler.

**Rolle:** **Allgemeine größere Analyse, nicht mein primäres Coding-Modell.**

### `qwen3-coder:30b`

**Stärken:** auf meinem System überraschend schnell, guter Code-Entwurf, Python und SQL, sehr praktisch im täglichen Coding.

**Schwächen:** kein automatischer Logikprüfer. Es erfand bei einer korrekten SQL-Query einen Fehler, reparierte einen Many-to-Many-Fehler zunächst unvollständig und erzeugte in einem späteren Test sogar Code, der so nicht funktionieren konnte.

**Rolle:** **Mein Hauptmodell für Code schreiben, schnelle Python-/SQL-Hilfe und normales Debugging.**

> **Code vom Coder-Modell ist ein Entwurf, keine Freigabe.**

### `qwen3.6:27b`

Das war das interessanteste Modell im Test.

**Stärken:** gute methodische Analyse, trennt Beobachtung und Interpretation besser, findet alternative Erklärungen, erkennt Aggregationsebenen, war bei schwierigen Pandas-/SQL-Logiktests qualitativ am stärksten.

**Schwäche:** **Thinking. Sehr viel Thinking.** Es kann eine richtige Antwort relativ früh erkennen und danach minutenlang weiterprüfen und wiederholen. Bei manchen Aufgaben 18, 20 oder über 30 Minuten.

**Rolle:** **Deep Reviewer.** Nicht für jede kleine Frage, sondern für wichtige Logikprüfungen.

### `qwen3.5:27b`

Auch direkt über Ollama war die Generierung bei meinem Test sehr langsam. Kein klarer Vorteil gegenüber meinen anderen Modellen.

**Rolle:** **Löschkandidat.**

### `deepcoder:14b`

Erkannte in einem Pandas-Test früh einen subtilen Fehler, geriet danach aber in eine sehr lange Thinking-Schleife und kam praktisch nicht zum Ende.

**Rolle:** **Löschkandidat.**

### `qwen2.5-coder:7b`

**Vorteil:** sehr schnell.

**Problem:** erkannte die entscheidende Duplicate-Weighting-Logik nicht sauber und schlug eine falsche Korrektur vor.

**Rolle:** **Löschkandidat.**

### `nomic-embed-text`

Kein Chatmodell. Es dient für Embeddings, semantische Suche und lokales RAG.

**Rolle:** **Behalten.**

---

## 15. Meine aktuelle Modell-Aufteilung

Das ist keine ewige Wahrheit. Es ist mein aktueller Stand nach diesen Tests.

| Rolle | Modell | Warum |
|---|---|---|
| Alltag / Schreiben / Sprache | **Mistral Small 3.1** | natürlich, kontrolliert, gute Korrekturen |
| sehr schnelle Alltagsaufgaben | **Gemma 2 9B** | sehr schnell und ausreichend gut |
| Code schreiben / normales Debugging | **Qwen3-Coder 30B** | beste Mischung aus Geschwindigkeit und Coding-Nutzen |
| schwierige Logikprüfung | **Qwen3.6 27B** | qualitativ stärkster Reviewer in meinen Tests |
| allgemeine größere Analyse | **Qwen2.5 32B** | brauchbarer großer Generalist |
| Embeddings / RAG | **nomic-embed-text** | andere technische Rolle |

Weitere Modelle wie Qwen2.5 14B, GPT-OSS 20B oder Llama 3.1 8B können weiterhin nützlich sein, überschneiden sich aber stärker mit den Rollen oben.

Meine klarsten Löschkandidaten nach den bisherigen Tests sind:

- `deepcoder:14b`
- `qwen2.5-coder:7b`
- `qwen3.5:27b`

---

## 16. Was ich aus dem Vergleich gelernt habe

### 16.1 Größer ist nicht automatisch besser

32B kann langsamer sein und denselben Fehler machen wie ein 30B-Modell. Ein kleineres Modell kann bei einer klaren Alltagsaufgabe angenehmer sein als ein viel größeres.

### 16.2 „Coder" bedeutet nicht „logisch immer korrekt"

Ein Coding-Modell kann sehr gut Syntax, Bibliotheken und Strukturen kennen und trotzdem eine falsche Aggregation bauen. Das ist besonders gefährlich bei Datenanalyse, weil der Code trotzdem ohne Fehler laufen kann.

### 16.3 Ein laufender Code ist nicht automatisch ein richtiger Code

Ein Query kann kompilieren, laufen und eine schöne Tabelle liefern – und trotzdem methodisch falsch sein.

### 16.4 Negative Tests sind wichtig

Nicht nur fragen: „Findest du den Fehler?"

Sondern manchmal absichtlich einen korrekten Code geben und fragen: „Gibt es einen **bestätigten** Fehler?"

So sieht man, ob das Modell wirklich prüft oder einfach einen Fehler erfinden will, weil der Prompt nach einem Fehler klingt.

### 16.5 Mehr Thinking ist nicht automatisch mehr Wahrheit

Qwen3.6 zeigte, dass lange Thinking-Phasen sehr gute Ergebnisse liefern können. Aber es zeigte auch, dass ein Modell dieselbe richtige Überlegung sehr lange wiederholen kann.

> **Thinking ist kein Qualitätszertifikat.**

### 16.6 Streaming verändert die Nutzbarkeit stark

Für mich ist ein Modell, das sofort anfängt und langsam weiterschreibt, oft angenehmer als ein Modell, das fünf Minuten scheinbar nichts macht und dann alles auf einmal liefert.

### 16.7 Mehrere Modelle können denselben Denkfehler haben

Qwen3-Coder und Qwen2.5 32B machten beim `COUNT(*)`-Test praktisch denselben falschen Schluss.

Deshalb ist „zweites Modell fragen" gut, aber keine Garantie für unabhängige Kontrolle.

Manchmal braucht man statt eines zweiten LLM einfach Mathematik, kleine Testdaten, Dokumentation oder eigenes Nachrechnen.

### 16.8 Warum ich die Thinking-Ausgabe trotzdem gerne lese

Ich lese die Denk-/Thinking-Ausgabe eines Modells gerne, auch wenn sie bei einer einfachen Frage oder einer Code-Korrektur eine halbe Stunde dauert.

Der Grund: Man lernt dabei viel.

Man sieht:

- Widersprüche im eigenen Gedankengang des Modells,
- Zweifel, die im fertigen Antworttext später gar nicht mehr auftauchen,
- wie das Modell tatsächlich denkt und welche Logik dahintersteckt,
- wie gefährlich es wäre, diese Zwischenschritte einfach als Fakten zu nehmen.

Das gilt für mich nicht nur bei technischen Fragen. Gerade bei ganz allgemeinen Fragen finde ich die Frage und den Denkprozess dahinter extrem interessant: Man sieht etwas davon, wie Entwickler denken, und man merkt, wo große Unternehmen oder Staaten möglicherweise bestimmte Narrative durchsetzen oder Werte verschieben wollen.

Deshalb meine Empfehlung: Lies die Thinking-Ausgabe, wenn ein Modell sie anzeigt. Es hilft zu verstehen, wie ein Modell wirklich arbeitet – und vielleicht auch dabei, nicht alles zu glauben, was eine KI sagt.

---

## 17. KI und Genauigkeit

Ich bin bei Projekten sehr genau. Das kann manchmal Arbeit langsamer machen, ist bei Datenanalyse aber auch ein Vorteil.

Wenn mir eine Zahl komisch vorkommt, akzeptiere ich sie nicht nur deshalb, weil:

- der Code läuft,
- zwei Modelle sagen, er sei richtig,
- oder der Text professionell klingt.

Ich prüfe lieber noch einmal.

In einem früheren Solana-Projekt war genau das entscheidend: Ein Wert war mathematisch nicht plausibel. Der SQL-Code war syntaktisch gültig, aber das Ergebnis konnte so nicht stimmen.

Ein anderes Beispiel waren Token-Metadaten. Ein Label in einer Datenquelle sah offiziell aus, war aber falsch. Das musste außerhalb der Query manuell geprüft werden.

Das ist für mich der Unterschied zwischen:

```text
Code erzeugen
```

und:

```text
Analyse machen
```

Das erste kann KI sehr schnell.

Das zweite braucht weiterhin Urteil.

---

## 18. Diagramme: ein Bereich, in dem der Mensch sehr wichtig bleibt

KI kann guten Matplotlib-, Plotly- oder Power-BI-Code vorschlagen.

Aber ein Diagramm kann technisch korrekt erzeugt worden sein und trotzdem schlecht sein.

Ich schaue zum Beispiel auf:

- Achsenskalierung,
- unplausible Sprünge,
- falsche Normalisierung,
- abgeschnittene Werte,
- Kategorien, die nicht zusammenpassen,
- Farben / Legende / Lesbarkeit,
- ob die visuelle Aussage überhaupt zu den Zahlen passt.

Ein Sprachmodell kann Code sehen und sagen: „Das sieht korrekt aus."

Ein Mensch kann das fertige Bild sehen und sofort merken: „Nein. Irgendwas stimmt hier nicht."

Deshalb gehört die visuelle Endkontrolle für mich nicht an KI.

---

## 19. Wie ich AI-generierten Code behandle

Mein bevorzugtes Muster:

```text
Frage / Plan
    ↓
AI schlägt Code vor
    ↓
Ich lese den Code
    ↓
Code läuft
    ↓
Ich prüfe die Ausgabe
    ↓
Plausibilitätscheck
    ↓
bei wichtigen Metriken: zweite Kontrolle
    ↓
erst dann Dokumentation / Dashboard
```

Nicht:

```text
AI schreibt Code
    ↓
läuft
    ↓
fertig
```

---

## 20. Warum ich trotzdem gerne mit KI programmiere

Weil es schnell ist.

Ich muss nicht jede Pandas-Syntax auswendig kennen, um zu verstehen:

- welche Aggregation ich brauche,
- warum die Granularität wichtig ist,
- warum ein Merge gefährlich sein kann,
- oder warum ein Ergebnis falsch aussieht.

Wenn ich weiß, **was ich analytisch erreichen will**, kann KI mir die technische Umsetzung stark beschleunigen.

Das ist für mich keine Schwäche des Workflows, sondern genau der Nutzen des Werkzeugs.

Ich würde meine Arbeitsweise ungefähr so beschreiben:

> Ich benutze KI regelmäßig für Code, Debugging und Dokumentation. Ich definiere die analytische Frage, prüfe die Outputs und treffe die endgültigen methodischen Entscheidungen selbst.

---

## 21. Mein praktischer Qualitätscheck

Bevor ich eine wichtige Kennzahl akzeptiere, frage ich mich:

1. Was ist die Granularität der Rohdaten?
2. Was ist die Granularität des gewünschten Ergebnisses?
3. Habe ich vor dem Join korrekt aggregiert?
4. Ist der Join one-to-one, one-to-many oder many-to-many?
5. Kann dadurch etwas doppelt gezählt werden?
6. Bedeutet `count` wirklich das, was ich „Anzahl Nutzer" nenne?
7. Brauche ich `nunique`?
8. Ist ein Durchschnitt pro Event, pro User, pro Tag oder pro Position?
9. Was genau ist der Nenner eines Anteils?
10. Kann ich das Ergebnis an einem Mini-Beispiel von Hand nachrechnen?
11. Passt die Zahl zum Diagramm?
12. Ist eine Interpretation wirklich aus den Daten ableitbar oder nur plausibel?
13. Gibt es alternative Erklärungen?
14. Was kann die Datenquelle überhaupt nicht beantworten?

Wenn diese Fragen beantwortet sind, ist mir die Modellmarke wesentlich weniger wichtig.

---

## 22. Ein kleines Beispiel für einen guten KI-Test

Statt nur:

```text
Ist dieser Code richtig?
```

lieber:

```text
Goal:
- total borrow volume
- unique borrowers
- average total borrowed amount per borrower

Identify only confirmed logic errors.
Do not invent problems.
Explain the aggregation level.
Give corrected code only if needed.
```

Noch besser: eine kleine Testtabelle bauen, bei der ich das richtige Ergebnis vorher kenne.

Dann teste ich nicht nur, ob das Modell schön erklärt.

Ich teste, ob es richtig liegt.

---

## 23. Was lokale KI bei mir nicht ersetzen soll

Lokale KI ersetzt für mich nicht:

- Dokumentation,
- Quellenkritik,
- eigene Plausibilitätsprüfung,
- Web-Recherche,
- menschliches Urteil,
- Datenschutzregeln eines Arbeitgebers,
- fachliche Verantwortung,
- oder starke Cloud-Modelle bei wirklich schwierigen Aufgaben.

Sie ist eine zusätzliche Schicht in meinem Werkzeugkasten.

Nicht mehr und nicht weniger.

---

## 24. Aktueller praktischer Ablauf im Alltag

### Kleine E-Mail / Englischkorrektur

```text
Mistral Small 3.1
oder
Gemma 2 9B
```

### Kleine Linux-/Alltagsfrage

```text
Gemma / Mistral / kleiner Generalist
```

### Python- oder SQL-Code schreiben

```text
Qwen3-Coder 30B
```

### Komische Aggregation oder wichtige Metrik prüfen

```text
Qwen3.6 27B
```

und wenn nötig zusätzlich:

```text
Cloud-KI / Dokumentation / eigener Mini-Test
```

### Größere allgemeine Analyse

```text
Qwen2.5 32B
oder
Cloud-KI
```

### RAG / semantische Suche in lokalen Daten

```text
nomic-embed-text + lokales LLM
```

---

## 25. Ein paar einfache Ollama-Befehle

Modell starten:

```bash
ollama run qwen3-coder:30b
```

Session verlassen:

```text
/bye
```

Geladene Modelle / Ausführung prüfen:

```bash
ollama ps
```

Installierte Modelle:

```bash
ollama list
```

Modell aus dem Speicher stoppen:

```bash
ollama stop qwen3.6:27b
```

Eine einzelne Frage direkt stellen, ohne in den Chat-Modus zu wechseln:

```bash
ollama run gemma2:9b "Was ist ein DataFrame? Erkläre es kurz."
```

Antwort direkt als Datei speichern:

```bash
ollama run mistral-small3.1:latest "Schreibe eine kurze Zusammenfassung über Berlin." > berlin.txt
```

Datei lesen:

```bash
cat berlin.txt
```

Antwort gleichzeitig sehen und speichern:

```bash
ollama run mistral-small3.1:latest "Erkläre mir kurz, was SQL ist." | tee sql.txt
```

Eine Code- oder Textdatei an die KI übergeben:

```bash
cat analysis.py | ollama run qwen3-coder:30b "Prüfe diesen Python-Code auf bestätigte Fehler."
```

Codeanalyse direkt in eine Datei speichern:

```bash
cat analysis.py | ollama run qwen3-coder:30b "Prüfe den Code und erkläre die Fehler." > code_review.txt
```

Für mich war `ollama ps` besonders nützlich, um zu sehen, welches Modell geladen ist, welchen Kontext es nutzt und ob es auf GPU oder CPU liegt.

---

## 26. Was ich noch verbessern kann

Dieser Workflow ist nicht fertig.

Ehrlich gesagt: Aktuell liefert mir die direkte Nutzung einzelner Modelle über WebUI oder Terminal oft bessere Ergebnisse als meine eigene automatisierte Pipeline. Der geplante Ablauf – das Modell erzeugt einen kleinen strukturierten Plan, das Notebook führt nur erlaubte Pandas-Operationen aus – funktioniert noch nicht zuverlässig genug.

Mögliche nächste Schritte:

- Modelle nach Updates erneut testen,
- reproduzierbarere Benchmarks mit identischen Prompts speichern,
- Kontextlänge und Quantisierung systematischer vergleichen,
- RAM-/GPU-Auslastung protokollieren,
- Open WebUI sauberer konfigurieren und erneut gegen direkten Ollama-Betrieb testen,
- automatische Testfälle mit bekannten richtigen Ergebnissen bauen,
- lokale Modelle stärker nach Rollen statt nach „einem besten Modell" organisieren.

Ich habe tagelang sehr intensiv an dieser automatisierten Pipeline gearbeitet. Irgendwann zeigte sich aber ein klassisches 80/20-Verhältnis: Die letzten Prozent an Automatisierung hätten noch einmal viele Stunden gekostet, die ich inzwischen lieber woanders investiere. Ich arbeite deshalb nur noch ab und zu daran, nicht mehr so intensiv wie am Anfang.

Was tatsächlich geblieben und zu meinem eigentlichen Workflow geworden ist, ist nicht die fertige Automatisierung, sondern die Methode dahinter: verschiedene Modelle gezielt für unterschiedliche Aufgaben zu nutzen, so wie in diesem README beschrieben. Das klingt komplizierter, als es ist. Im Alltag ist es einfach: das passende Modell – oder Cloud-KI – für die jeweilige Aufgabe wählen und die Ergebnisse selbst prüfen.

Ich sehe das als Experimentierumgebung, nicht als fertiges KI-System.

---

## 27. Was dieses Experiment für mich gezeigt hat

Ich habe nicht das eine perfekte lokale Modell gefunden.

Das war am Ende aber auch nicht das wichtigste Ergebnis.

Ich habe etwas Nützlicheres gefunden: eine Arbeitsteilung. Welches Modell – oder Cloud-KI – wofür taugt, steht in Abschnitt 15 und 24. Am Ende rechnen Pandas und SQL, und ich entscheide, was ich glaube und was ich veröffentliche.

Das ist für mich realistischer als die Vorstellung, eine einzige KI müsse alles können.

---

## 28. Schluss

Ich benutze KI viel.

Aber „viel KI benutzen" bedeutet für mich nicht:

> Ich frage eine KI und übernehme ihre Antwort.

Es bedeutet eher:

> Ich benutze verschiedene KI-Werkzeuge, Code, Dokumentation und eigene Kontrolle, um schneller zu einem Ergebnis zu kommen – und prüfe besonders dort genauer, wo ein Fehler leicht professionell aussehen kann.

Die Modelle sparen mir Zeit.

Sie sparen mir aber nicht das Denken.

Und genau das ist die Kombination, die ich haben will.

> **Use AI. But use your brain. Please!!!**
