# Analysewerkzeuge (Python) News Aggregator Projekt

Das Ziel: Eine funktionierende Software, die es ermöglicht, Nachrichten aus verschiedenen Quellen zu aggregieren. Dabei sollten die Nachrichten sinnvollen Kategorien zugeordnet werden. Das System sollte außerdem Präferenzen des Users berücksichtigen und die relevanten Artikel z.B. in einer speziellen personalisierten Kategorie anzeigen.

#### Das Projekt benötigt folgende Komponenten:
* *Daten* aus rss feeds, aufbereitet für weitere Analyse
* *Logik*, bestehend aus einfachem Sortieren von Artikeln nach Kategorien und einem (selbst-)lernenden System, das auf Ähnlichkeit basierend versucht, Artikel zu finden, die den Interessen des Users entsprechen
* *User Interface*

### Aufgaben
#### 0. Architektur:
Da die benötigte Software offensichtlich aus mehreren Modulen besteht, sollten jeweilige Schnittstellen definiert werden und sichergestellt werden, dass etwa Daten die Anforderungen der logischen Komponente abdecken. Wiederum sollte das User Interface die Funktionalität der logischen Komponente in einer geeigneten Form repräsentieren können.

#### 1. Daten: 
* Quelle(n) finden und stabiles Download von Daten gewährleisten
* Daten bereinigen und in einem strukturierten Format alle wichtigen Informationen wie etwa Link, Quelle, Artikelkategorie, Beschreibung etc bereitstellen

#### 2. Logik:
* Kategorien zum Anzeigen bestimmen, am einfachsten wäre die Kategorien aus rss feeds zu nehmen - dann erfordert es keine weitere Sortierung
* Ein klassifizierendes System bauen, das auf Interaktionen des Users reagiert und dementsprechend die zu erwartende Relevanz von Artikeln berechnet. Z.B., ein Ansatz basierend auf Distanzen: Die Artikel, die User gelesen hat, bilden lokale Zentren von seinen Interessen; Artikel in einem bestimmten Radius vom Zentrum werden höher gewichtet als Artikel außerhalb des Interessenradius. Gespeichert wird allerdings aufgrund der Größe nicht das ganze Model wie bei KNN, sondern nur die Zentren, Radien, Gewichtungen des Raumes und einige "frische" Artikel. In der personalisierten Kategorie bekommt der User in erster Linie die hochgewichteten Artikel angeboten.
* Artikel aus dem Pool mit der den Gewichtungen entsprechenden Häufigkeit wählen. *Wichtig: Auch irrelevante Artikel sollten ab und zu angezeigt werden*
##### Classificator - Aufgaben:
* Anhand von vorhandenen Informationen über Artikel aus rss feeds geeignete Features fürs Modell finden. Möglicher Ansatz: Term-Doc-Matrix -> Metrik wie TF-IDF, PMI, PPMI,... -> Dimensionality reduction, z.B. LSA -> Distanzmatrix (wie KNN)
* Am Anfang sind alle Artikel (= alle Teile des Raumes) gleich gewichtet. Wenn der User einen Artikel zum Lesen wählt, sollte entsprechender Teil des "Artikelraumes" leicht höher gewichtet werden; sollte das Interesse aufrecht bleiben (der User bevorzugt auch andere Artikel aus der Umgebung), wird die Gewichtung signifikanter erhöht. *Wichtig: sinnvollen Radius bestimmen*
* Zu berücksichtigen: Bereits gelesene Artikel nicht wieder anbieten (aber evtl in der zusätzlichen Kategorie speichern)
* Das Modell soll gespeichert werden: Lage der Interessenzentren, entsprechende Radien und Gewichtungen, sodass die Gewichtung des jeweiligen Teilraums wiederhergestellt wird und neue Artikel entsprechend behandelt werden.

#### 3. User Interface:
* Passendes high-level package für GUI in Jupyter Notebook finden
* Interface skizzieren, Funktionalität festlegen
* Interface bauen

#### Testen...
