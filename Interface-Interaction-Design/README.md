## 1 Einleitung

- Design by Accident
	Design is inevitable. The alternative to good design is bad design, not no design at all.
- Aesthetic Design
	- Angenehmes Erscheinungsbild
	- Werden Konventionen der visuellen Gestaltung eingehalten
	- Entspricht es den Erwartungen der Benutzer
- Usability Design
	Gebrauchstauglichkeit und Benutzbarkeit
	"Most people make the mistake of thinking design is what it looks like. That's not what we think design is. It's not just what it looks like and feels like. Design is how it works." - Steve Jobs
	
	**ISO 9241-11:** Usability eines Produktes ist das Ausmaß, in dem es von einem bestimmten Benutzer verwendet werden kann, um bestimmte Ziele in einem bestimmten Kontext effektiv, effizient und zufriedenstellend zu erreichen.

### Ästhetik vs. Usability
> **Form Follows Function**
> Ornament is crime
> -Adolf Loos

### Eine Definition
> Interaction design is about shaping digital things for people's use.
> - Jonas Löwgren

## 2 Grundlagen & Dark Patterns

**User Interface:**
> Die Gesamtheit aller Mittel mit denen Menschen (die User, BenutzerInnen) mit einer Maschine, einem Gerät, einem Computerprogramm oder einem anderen komplexen Tool (dem System) interagieren.

### Mentale Modelle
- Die BenutzerInnen haben ein mentales (konzeptuelles) Modell von Dingen und ihrer Funktionsweise
- Modelle erlauben uns die Operation von einem Gerät mental durchzuspielen und die Auswirkungen unserer Handlungen vorherzusehen
- Schlechtes Design kann zu “falschen” mentalen Modellen führen
- Häufig sehr grobe Vereinfachung und nicht vollständig korrekt, müssen nur nützlich sein
- Unterscheiden sich sehr stark von Person zu Person

### Knowledge in the Head
- “Knowledge in the Head” ist Wissen in unserem Gedächtnis, das wir uns merken bzw. in Erinnerung behalten
- Effizient: Jederzeit unmittelbar und direkt abrufbar
- Aufwändig in der Aneignung und begrenzt, z.b Geburtstage, Passwörter

### Knowledge in the World
- Wissen, das aus der Welt abgeleitet werden kann
- Muss man sich nicht merken, ist aber weniger effizient, weil die externe Information gesucht und interpretiert werden muss
- Muss wahrnehmbar und verständlich sein
- Fördert Erlernbarkeit und entlastet das Gedächtnis
- z.B. Terminkalender, Labels, Zeichen, etc.

### Recall vs Recognition
Bei Recall muss der Benutzer die Befehle kennen (z.B. Terminal interface), bei Recognition kann der Benutzer die Befehle von visuellen Eindrücken ableiten (klassisches visuelles Interface).

### Two kinds of Errors
- Slips: 
	- Treten trotz korrektem mentalen Modell auf
	- User weiß was er falsch gemacht hat
	- Ursache meist überhastetes Handeln
- Mistakes:
	- Treten aufgrund von fehlerhaften mentalen Modellen auf
	- User weiß nicht was er falsch gemacht hat

### Modelle der Interaktion
> Definition:
> Interaction, noun: Reciprocal action or influence
> - Oxford Dictionary

#### The Gulfs of Execution & Evaluation
- The Gulf of Execution: Where people try to figure out how something operates.
- The Gulf of Evaluation: Where people try to figure out what happened.
![[Pasted image 20230118080423.png]]

#### Seven Stages of Action - Donal Norman
1. Goal (form goal)
2. Plan (the action)
3. Specify (action sequence)
4. Perform (action sequence)
5. Perceive (state of world)
6. Interpret (perception)
7. Compare (outcome with goal)
**Fokus auf Perspektive der BenutzerInnen**
![[Pasted image 20230118080359.png]]

### Interaction Framework
- User
- System
- Input
- Output
![[Pasted image 20230118080333.png]]

### Dark Patterns
- “Dark Patterns are features of interface design crafted to trick users into doing things they might not want to do, but which benefit the business in question”
- User Interfaces die Benutzerinnen und Benutzer manipulieren etwas zu tun, das sie gar nicht tun wollen bzw. das nicht in ihrem besten Interesse ist
- Eine Art von Persuasive Design
- Häufiger Grund: Interessen der User und Interessen des Unternehmens sind gegensätzlich
- Dark Patterns setzen voraus:
	- eine böse oder tückische Absicht
	- ein legitimes Interesse des Anbieters bzw. Betreibers
- Nicht immer einfach oder eindeutig zu identifizieren:
	- Böse Absicht oder schlechtes Design?
	- Legitimer Interessenskonflikt?
	- Dark Pattern oder Betrug?

#### Dark Pattern Strategien
- **Nagging:** A minor redirection of expected functionality that may pesist over one or more interactions.
- **Obstruction:** Impeding a task flow, making an interaction more difficult than it needs to be, with the intent to dissuade an action.
- **Sneaking:** Hiding, disguising, or delaying the divulging if relevant information. Often intended to force uninformed decisions.
- **Interface Interference:** Privileging specific actions over others, thereby confusing the user or limiting discoverability of important actions.
- **Forced Action:** Requiring users to perform a specific action to access (or continue to access) specific functionality.

## 3 Designkonzepte nach Donald Norman

### Affordances & Signifier
- **Affordance:** Handlungsmöglichkeiten, die ein Objekt einem/ einer AkteurIn zur Verfügung stellt (unabhängig davon ob wahrnehmbar oder nicht)
  Eigenschaften eines Objektes <--> Fähigkeiten eines Benutzers
- **Perceivable Affordance:** Von einem/einer AkteurIn wahrnehmbare Handlungsmöglichkeiten
- **Signifier:** Die Repräsentation der vorhandenen Handlungsmöglichkeiten, wodurch diese für den/die AkteurIn wahrnehmbar werden

### Mapping
Nach Norman: Beschreibt die Beziehung zwischen Kontrollelementen, ihrer Anordnung und ihrer Bewegung und den Resultaten in der Welt.
Drei Ebenen:
- Beste Mappings: Kontrollmechanismus direkt am kontrollierten Objekt befestigt
- Zweitbestes Mapping: Kontrollmechanismus möglichst nahe am kontrollierten Objekt
- Drittbestes Mapping: Kontrollmechanismus in räumlicher Analogie zu kontrollierten Objekten angeordnet

Mapping - Scrollen: 
	1. Bezugspunkt Viewport
	2. Bezugspunkt Dokument

### Contraints
- Grenzen die möglichen Aktionen ein, die ausgeführt werden können
- Verhindern die Selektion von ungültigen Optionen oder das Ausführen von fehlerhaften Operationen
- Fehler verhindern bevor sie passieren
- Verschiedene Arten:
	- Physical Constraints
		- USB Stecker kann nicht falsch eingesteck werden
		- Datumseingabe in Eingabefeld muss bestimmtes Format haben
		- Forcing Functions: Änderung Speichern Dialog bevor Fenster geschlossen wird, Benutzer kann Datei an der gerade gearbeitet wird nicht umbennen, Bankomat gibt erst Geld aus nachdem die Karte entnommen wurde
	- Cultural Constraints
	- Logical Constraints

#### Poka-Yoke Prinzip
= Fehler-sicher (in der Bedienung) - Ursprung Toyota Produktionssystem (Shigeo Shingo)
- **Contact Method:** Überprüfung der Form, Größe, anderer physischer Attribute
- **Fixed-Value Method:** Überprüfung ob die richtige Anzahl von Schritten durchlaufen wurde
- **Motion-Step Method:** Eine bestimmte Abfolge von Schritten muss in der richtigen Reihenfolge durchlaufen werden

Zwei Wirkungsweisen:
- Warning Poka-Yoke: AkteurIn wird auf den Fehler aufmerksam gemacht
- Control Poka-Yoke: Fehler werden unmöglich gemacht

### Feedback
- Zurücksenden von Informationen an den User über:
	- das Ergebnis einer Aktion
	- darüber, dass eine Aktion ausgeführt wird/wurde
- Feedback sollte möglichst unmittelbar sein
-> "Bridging the Gulf of Evaluation"

Zeit:
- < 0,1 Sek: unmittelbar, kein spezieller Status Indikator notwendig
- 0,1 - 1,0 Sek: User verliert das Gefühl direkter Interaktion, aber kein spezieller Indikator zwingend notwendig
- 1,0 - 10 Sek: Fokus bleibt auf Dialog, aber ein Statusindikator ist angebracht
- > 10 Sek: User verliert Fokus, Fortschrittsindikator ist angebracht

### Conventions & Consistency
- Konsistenz...
	- innerhalb einer Applikation
	- innerhalb einer Plattform
- Dinge, die gleich aussehen, sollen sich auch gleich verhalten (und umgekehrt)
- Dinge, die unterschiedlich aussehen, sollen sich auch unterschiedlich verhalten (und umgekehrt)
- Vorteil: Konsistente Interfaces sind einfacher zu erlernen und benutzen

## 4 Sketching & Prototyping

### Prototype
- Repräsentieren ein fertiges Produtk bzw. System
- Haben unterschiedliche Arten von Detailgraden
- Werden in vielen Gebieten des Designs und Engineerings verwendet

#### Evolutionäre Prototypen
- Konvergiert gegen das finale System durch iteratives Design & Entwicklung
- Weiterentwicklung des Prototypen in jeder Iterationsphase

#### Inkrementelles Prototyping
- Teilen des Systems in Komponenten
- Fertigstellung der einzelnen Komponenten
- Einzelne Komponenten werden zu Gesamtsystem zusammengefügt

#### Wegwerf Prototypen
Nachdem der Prototyp evaluiert und optimiert wurde, werden auf dessen Basis finale Voraussetzungen erstellt, mit denen weitergearbeitet wird, und der Prototyp wird vernichtet.

### Was kann (soll) man prototypen?
- Technische Aspekte
- Funktionalität
- Interaktionsverhalten
- Layout am Bildschirm (Wo werden welche Informationen angezeigt)
- Graphisches Design und "Look and Feel"
- Arbeitsablauf und einzlne Aufgaben
- Inhalte, Bezeichnungen, Abkürzungen
- Kontroversielle und kritische Aspekte
-> Man kann alles prototypen!

### Sketching vs. Prototyping
- **Sketching:**
	- in den frühen Phasen
	- wenig ingestiert
	- erst wenige Details
	- viele Möglichkeiten
- **Prototyping:**
	- bereits viel investiert
	- viele Details
	- genaue Ziele und Auswahl-Kriterien

### Breite und Tiefe der Funktionalität
- Vertikal:
	- Beschränkt auf ein Subset an Funktionen
	- Aber dieses Subset bietet Interface und Funktion
	- Realistisches Testszenario
- Horizontal:
	- Komplettes User Interface
	- Keine oder limitierte Funktionalität
	- Schnell implementiert
	- Komplettes Interface kann getestet werden, jedoch nicht in einem realistischen Testszenario

### Storyboard
- Präsentiert Sequenz con Aktivitäten in einem UI
- "Fluß" von einem Zustand zum nächsten (vgl. Zustandsdiagramme)
- Detailgrade:
	- Abstrakt (lediglich Zustandsdiagramm mit Text in Feldern)
	- Visuell (statt Text, mit Sketch der einzelnen Zustände)
	- Annotiert (Visuell mit erklärendem Text dazu)
	- Indizes (Visuell inkl. Alternativen für die einzelnen Sketches)
	- Narrativ (Geschickte inkl. Benutzer und Situationen, in denen diese die Applikation verwenden)
- Branching Diagram: Diagramm in dem alle möglichen Schritte dargestellt sind, wieder ähnlich einem Zustandsdiagramm

### Interaktivität simulieren
- Papier Prototyp
- Click.Through Prototyp (Figma, Präsentation, statische HTML Seite)
- Wizard of Oz: Mensch führt im Hintergrund berechnung aus und antwortet (z.B. zum testen von AI Anwendungen, Spracherkennung etc.)


## 5 Grundlagen der visuellen Gestaltung

### Gestaltungsprizipien
- Prinzip der Nähe
	- Näher zusammen liegende Elemente werden als zusammengehörig empfunden
	- Eines der wirksamsten Prinzipien
	- Eine der einfachsten Möglichkeiten um eine enge Beziehung zu Elementen auszudrücken
- Prinzip der Ähnlichkeit
	- Gleiche oder fast gleiche Elemente werden als zusammengehörig empfunden
	- Farbe gruppiert stärker als Form
- Prinzip der gemeinsamen Region
	- Elemente in derselben abgegrenzten Region werden als zusammengehörig empfunden
- Prinzip der stetigen Fortsetzung
	- Konturen mit kontinuierlichem Verlauf werden gegenüber Konturen mit gebrochenem Verlauf oder abruptem Richtungswechsel bevorzugt
	- Bei mehreren Interpretationsmöglichkeiten wird immer die einfachste und regelmäßigste Variante bevorzugt
- Prinzip der Geschlossenheit
	- Geschlossene Formen werden bevorzugt wahrgenommen
- Prinzip der Prägnanz
	- Figur vs. Hintergrund
	- Manche Objekte treten in der visuellen Wharnehmung in den Vordergrund
	- Üblicherweise kann nur eine "Interpretation" wahrgenommen werden
- Prinzip des gemeinsamen Schicksals
	- Elemente, die sich gleichzeitig in die selbe Richtung bewegen, werden als zusammengehörig empfunden
	- z.B. nützlich für Animationen und Übergänge

### Anwendung der Gestaltungsprinzipien
- **C**ontrast
	- Farbe, Form, Größe, etc.
	- Erweckt Aufmerksamkeit
	- Schafft Ordnung und Hierarchie
	- Kontrast muss eindeutig und erkennbar sein
- **R**epetition
	- Ein grafisches Konstrukt oder eine Eigenschaft wird im gesamten Design wiederkehrend verwendet
	- Atomic Design - Brad Frost:
		- Atoms: Grundbausteine (Labels, Buttons, etc.)
		- Molecules: Wiederkehrende Komponenten aus Grundbausteien
		- Organism: Komplexe, eigenständige, spezifische Bereiche, zusammengesetzt aus "Molecules"
		- Templates: Vorlage für einzelne Seite bzw. Bildschirmmasken, zusammengesetzt aus "Organisms"
		- Pages: Konkrete Seiten bzw. Bildschirmmasken, bestehend aus Templates befüllt mit Inhalten
- **A**lignment
	- Kein Element der Seite darf beliebig platziert werden
	- Jedes Element muss eine visuelle Verbindung mit einem anderen Element haben
	- Ausrichtung an einem Grid
- **P**roximity
	- Inhaltlich zusammen-gehörende Elemente befinden sich in räumlicher Nähe
	- Nähe impliziert Zusammengehörigkeit
	- Entfernung zu nicht zusammengehörenden Elementen groß genug und eindeutig
->CRAP

### Farben

#### Farbwahrnehmung
- Stäbchen: Hell - Dunkel Wahrnehmung
- Zapfen: Farbwahrnehmung
	- L-Zapfen: ~560 nm (Gelb)
	- M-Zapfen: ~530 nm (Grün)
	- S-Zapfen: ~420 nm (Blau)

#### Farbmischung
- Subtraktive Farbmischung - Pigmentfarben (CMYK):
	- Cyan
	- Magenta
	- Yellow
	- (Schwarz) - zusätzlich im Druck
- Additive Farbmischung - Lichtfarben (RGB):
	- Rot
	- Grün
	- Blau

### Typografie - Klassifizierung nach Form
- Schriftklassifikation nach Wilberg & Kupferschmid
- Formgruppen:
	- Serifen vs. Serifenlos
		- Serifen: im Druck und hochauflösenden Bildschirmen
		- Serifenlos: geringen Größe auf niedrigauflösenden Bildschirmen
	- Strichkontrast
- Stilgruppen:
	- Dynamisch (schräge Kontrastachse, organische Form)
	- Statisch (gerade Kontrastachse, hoher Strichkontrast)
	- Geometrisch (konstruierte Formen, O und andere Buchstaben fast zirkelrund, wenig Strichkontrast)
	- Dekorativ
	- Provozierend
- Schriftarten kombinieren:
	- Auf eine Schriftart bzw. Schriftfamilie beschränken
	- Zwei Schriftarten können kombiniert werden, um eine Hierarchie & Spannung zu erzeugen
	- Ähnliche Schriften vermeiden zu kombinieren
	- Einfacher Ansatz: Serif & Sans-serif kombinieren
- Zeilenlänge:
	- Saccade: Augenbewegung - lediglich 4-5 Buchstaben können gliechzeitig fixiert werden
	- Zu lange: Auge verliert Zeile
	- Zu kurz: Auge muss ständig Zeile wechseln
	- Optimal 40-80 Zeichen
- Zeilenabstand:
	- Wichtigstes Kirterium für die Lesbarkeit
	- Je größer die Schrift desto **geringer** der Zeilenabstand
- Schriftfamilie:
	- Verschiedene "Schnitte" einer Schrift (Breite, Stärke, Lage)
	- Schrift niemals verzerren -> dafür vorgesehenen Schnitt verwenden
- Vertikaler Rhythmus - Größenskalierung
	- Mithilfe von "Baseline-Grid"
	- Bestimmt typografische Hierarchie
	- Harmonie innerhalb der Komposition
	- Definiert Zeilenhöhe und Abstand zwischen Absätzen
	 -> Konstanter vertikaler Rhythmus 
	 - Methoden: Traditionell, Fibonacci, LeCorbusier

## 6 GUI & WIMP User Interfaces

### WIMP User Interfaces
- **W**indow
- **I**con
- **M**enu
- **P**ointer
-> WIMP UIs sind graphische User Interfaces, aber nicht jedes GUI ist ein WIMP UI

### Fitts' Law
Grob vereinfacht:
- Je größer ein Ziel ist, desto schneller kann man es auswählen.
- Je näher ein Ziel liegt, desto schneller kann man es auswählen.
![[Pasted image 20230119203255.png]]
Ein Modell des Bewegungsapparats, welches die Zeit zur Auswahl eines Zielbereichs in Abhängigkeit von Distanz zum Ziel und Größe des Ziels setzt.
$$MT = a + b · ID = a + b · log_2(\frac{2D}{W})$$
- MT = durschnittliche Dauer der Bewegung zur Zielauswahl
- a,b = Konstanten, die durch empirische Versuche und lineare Regressionsanalyse bestimmt werden
- D = Distanz vom Startpunkt zum Mittelpunkt des Ziels
- W = Breite des Ziels entlang der Bewegungsachse
![[Pasted image 20230119203520.png]]
![[Pasted image 20230119203542.png]]
![[Pasted image 20230119203557.png]]

### Accot-Zhai Steering Law
- Kernaussage: Je kürzer der Pfad und je breiter der Tunnel, desto schneller kann man sich fehlerfrei hindurch bewegen.
- Accot & Zhai (1997): Mathematisch hergeleitet von Fitts' Law und empirisch verifiziert
- Allgemeine Form: $T=a+b\ \int_C \frac{ds}{W(s)}$ 
- Vereinfachte Form für gerade Tunnel mit fixer Breite: $T=a+b \frac{A}{W}$
	- T = Zeit; a, b = Kostanten; A = Länge des Pfades; W = Breite des Tunnels

### Menüs
- Cascading Menus:
	- Problem: Direkter Pfad verlässt den Korridor
	- Lösung: Delay bevor das Untermenü aufklappt
- Menüstruktur:
	- Tiefe Hierarchien sorgen in der Regel für größere Schwierigkeiten als brete Strukturen
	- Guidelines: max. 2 (Apple) - 3 (Microsoft) Ebenen

#### Hick-Hyman Law
- Zeit die benötigt wird, um eine Entscheidung auf Basis mehrerer Auswahlmöglichkeiten zu treffen:
	- Zeit nimmt mit jeder zusätzlichen Möglichkeit zu
	- Zeit steigt mit steigender Anzahl an (strukturierten) Möglichkeiten logarithmisch (vgl. binäre Suchstrategien)
- Übung verringert Reaktionszeit (näherungsweise an einen fixen Wert)
$$T=b · log_2(n+1)$$
![[Pasted image 20230119205509.png]]

#### Menüleisten
- Einzelne Wörter als Kategorie-Bezeichnung
- Vorhandene/bekannte Kategorienamen verwenden, wenn möglich
- Sortierung:
	- Best-practices folgen, logische Reihenefolge, Häufigkeit, alphabetisch, chronologisch
- Ribbonmenü
	- Empfohlen für dokumentzentrierte Anwendungen
	- Kombination aus Toolbar und Menüleiste
- Kontextmenüs:
	- Operationen für bestimmtes Objekt
	- Sehr effizient (direkt, kontextbezogen)
	- Guidelines: 
		- Auf die wichtigsten Befehle beschränken
		- Befehle nicht auschließlich in Kontextmenüs, sindern reguläre Menübefehle duplizieren
		- Keine Default-Auswahl setzen
		- Nicht anwendbare Befehle ausblenden
- Adaptive Menüs:
	- Filterung nach Häufigkeit der Verwendung
	- Problem: Menü ändert sich ständig
	- Problem: Automatische Filterung macht Fehler
- Kreismenüs:
	- Schneller: Alle items gleich nahe, und größer
	- Lerneffekt (wenn konsistent): Richtung prägt sich ein
	- Probleme: Platz, Skalierung, Beschriftung, Sortierung

### Icons
- Ikonisch:
	- Optisch-abstrahierte Darstellung
	- Optische Ähnlichkeit zu reellem Objekt
- Indexalisch:
	- Kausaler Zusammenhang
	- Totenkopf steht für Gefahr
- Symbolisch
	- Form und Aussehen hängen nicht mit der Idee oder der Bedeutung zusammen
	- Muss gelernt werden - oft von kultureller/sozialer Umgebung abhängig

#### Icons Usability
- Beschriften!!!
- Labels wenn Platz ist
- Ansonsten Tool Tips
-> Wen Tool Tips nicht möglich sind und zu wenig Platz ist: Besser Labels anstatt Icons

#### Heterogenität vs. Homogenität
- Heterogenität: 
	- Unterschiedliche Icons sollen gut unterscheidbar sein
	- z.B. durch Farbe, Form, Detailgrad, Stil, Perspektive, Licht & Schatten
- Homogenität: 
	- Icons innerhalb eines Icon-Sets sollen ein stimmiges Gesamtbild ergeben
	- z.B. durch Geometrie, modulare Zusammensetzung
- Trend: Heterogenität -> Homogenität

#### Abstraktionsgrad
- Repräsentativ
- Ikonographisch
- Abstrakt
-> Passender Grade der Abstraktion. Nicht so abstrakt, dass man es nicht versteht, aber auch keine fotorealistischen Icons

### Windows
- Container zur räumlichen Organisation von Inhalten
- Overlapping Windows
- Tiled Windows
- Multiple Document Interface (MDI)
- Single Dokument Interface (SDI)
- Tabbed Document Interface (TDI)
-> In der Praxis werden häufig mehrere Konzepte in einer Anwendung kombiniert