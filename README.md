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

#### GUI - Funktionalität und Interaktionen:
* Es werden Nachrichten-Items in verschiedenen **Kategorien (Sektionen)** angezeigt
* Jedes **Nachricht-Item** besteht aus Titel, Summary, Link, Datum und Uhrzeit 
* Jedes Nachricht-Item besitzt außerdem einen **ID**, der zwar dem User nicht angezeigt wird aber für Kommunikation mit dem System benötigt wird
* **Update-Button**, um Nachrichten erneut herunterzuladen
* _(optional)_ **Show more-Button** in jeder Kategorie
* _(optional)_ **Show similar-Button**, um ähnliche Nachrichten zu finden und zu zeigen
* _(SEHR optional)_ **User Profile** - die Möglichkeit, das Benutzerprofil zu wählen

_Datenformat_

* Nachrichten-Items werden als pandas-DataFrame Objekte bereitsgestellt
* DataFrame enthält u.a. Spalten: `title`, `summary`, `link`, `datetime` und `ID`
* `ID` ist derzeit ein hash-Code für jede Nachricht und sollte nicht angezeigt sondern nur für die innere Kommunikation mit dem System verwendet werden
* Nachrichten aus den in rss-feeds pre-definierten Kategorien (wie Politik, Kultur, ...) werden gesamt als DataFrame mit entsprechender `category`-Spalte bereitgestellt
* Nachrichten aus der personalisierten Kategorie ("interesting for you/ based on recent viewed/...") werden im separaten DataFrame bereitgestellt
* DataFrames können auch **leer sein** bzw. einige Kategorien können nicht präsent sein (keine Einträge)

_Interaktionen_

0. Die **Applikation** wird **gestartet**:  
    _Das System versucht, Daten und Modelle einzulesen, wenn OK -> das existierende System wird gestartet, wenn etwas fehlt -> ein neues wird initialisiert._
    - Input System -> GUI: Zwei DataFrames - DataFrame mit pre-definierten Kategorien und DataFrame mit Nachrichten für die personalisierte Kategorie, bereitsgestellte Nachrichten-Items müssen nun angezeigt werden

1. Der **Link** zur Nachricht wird **angeklickt**: 
    - Link muss geöffnet werden (z.B. in Browser, bitte überlegt euch, wir ihr das implementiert)
    - Output GUI -> System: Nachricht-ID
    - Input System -> GUI: Updated DataFrames mit Nachrichten-Items, die nun anstatt "alten" angezeigt werden sollen _(dabei wurde die angeklickte Nachricht als "viewed" wahrgenommen und deshalb entfernt; evtl hat sich das klassifizierende System verändert und andere Nachrichten als "interessante" gewählt)_
    
2. **Update-Button** wird **angeklickt**:
    - Output GUI -> System: Irgendein Identifikator, z.B. string "update", der als entsprechender Befehl erkannt wird und Nachrichten-Update triggert
    - Input System -> GUI: Zwei DataFrames - DataFrame mit pre-definierten Kategorien und DataFrame mit Nachrichten für die personalisierte Kategorie, bereitsgestellte Nachrichten-Items müssen nun angezeigt werden
    
3. **Session** wird **beendet** (**Close-Button** angeklickt):
    - Output GUI -> System: Entsprechender Identifikator, der als Befehl erkannt wird, dass die Daten und Modelle gespeichert werden müssen
    - Input System -> GUI: Bestätigung, dass alles notwendige gespeichert wurde oder Fehlermeldung _(die eigentlich irgendwie bearbeitet werden muss aber lass'ma des...)_
    - Applikation wird geschlossen


#### Testen...
