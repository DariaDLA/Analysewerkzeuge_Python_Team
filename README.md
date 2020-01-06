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
* Ein klassifizierendes System bauen, das auf Interaktionen des Users reagiert und dementsprechend die zu erwartende Relevanz von Artikeln berechnet. 
* _Vorgehensweise derzeit im Prototyp_: Artikel, die der Benutzer bereits gelesen hat, müssen für das Clustering verwendet werden. Auf den sich daraus ergebenden Klassenlabels wird ein klassifizierender Algorithmus trainiert. Dieser wiederum wird verwendet, um neue Nachrichten zu klassifizieren. Basierend auf den Klassenlabels und Klassengewichtungen sollten die neuen Nachrichten dem Benutzer in der "personalisierten" Kategorie angezeigt werden. Wenn eine genügende Anzahl an neuen Artikeln von dem Benutzer gelesen wurde -> update clusters, classifier.

##### Classificator - Aufgaben:
* Vektorrepräsentationen der Texte und Distanzen _(derzeit im Prototyp: Pre-trained Word2Vec -> L2 -> cosine distance; theoretisch auch möglich: Word2Vec -> WM distance ABER in diesem Fall ist mir momentan die Vorgehensweise bei der Klassifizierung unklar)_
* Passendes Modell für das Clustering (erforderlich: Metrik mehr oder weniger frei wählbar und/oder das Modell kann mit Distanzmatrix arbeiten; wünschenswert: "outlier"-Cluster) 
* und für die Klassifizierung ("outlier"-Klasse erforderlich, wahrscheinlich RadiusNeighborsClassifier denkbar - oder auch custom) 

#### 3. User Interface:
* Passendes high-level package für GUI in Jupyter Notebook finden
* Interface skizzieren, Funktionalität festlegen
* Interface bauen

#### Testen...
