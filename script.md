% Vorlesungen zu Rechnernetze II \
 *Eine Zusammenfassung aus dem Wintersemester 2013/2014*
% Christian Müller


# ISO Layer 6
Diese Schicht wird im Internet durch zahlreiche Einezellösungen nachgebaut,
da sie nicht implementiert wird.
Daher wird die Funktion der Schicht 6,
die Kompression der Daten,
anhand von Lösungen dargestellt die im Internet in Layer 7 eingesetzt werden.

## Multimediale Daten
Bei Multimedialen Daten werden mindestens ein zeitdiskretes und
ein zeitkontinuierliches Medium für die Darstellung von Information verwendet.
Das heisst die Anzahl der gleichzeitig verwendeten Medien ist immmer >= 2.
Problematisch ist die zeitliche Synchronisation der Medien,
was durch Echtzeitprotkolle ermöglicht wird.
Diese sind aber nicht durch das Internet implementiert.

## Prinzip der Datenkompression
Durch die Datenkompression wird die Datenrate des Multimedialen Ausgangssignlas reduziert.
Dies geschieht idealerweise mit kurzer Latenz zwischen Unkomprimierten und komprimierten Daten.
Dabei ist zu beachten das die zeitliche Synchronisation zwischen den verschiedenen Teilen der multimedialen Daten. 
Grundsätzlich unterscheidet man zwischen verlustbehafteten (lossy)
und verlustfreien (loss-less) Kompressionsverfahren.
Dabei erreichen nur verlustbehaftete Verfahren die nötige Kompression von > 10,
um für den Transport über das Internet zu erzielen.

Weiterhin werden die Kompressionsalgorithmen die folgende vier Kategorien aufgeteilt
	
	* Entropie-Kodierungen
	* Quellen-Kodierung
	* Kanal-Kodierung
	* Hybride-Kodierung

Diese werden im folgenden Kurz vorgestellt.

## Entropie Kodierung
Diese Art der Kodierung ist immer verlustfrei und erreicht Kompressionen um den Faktor 3.
Die Semantik der Daten spielt für sie keine Rolle.
Ein typisches Beispiel ist die Lauflängekodierung wie beim Fax.

Gliederung der Entropie Kodierung:

	* Entropie Kodierung
		+ Lauflängenkodierung
		+ Color-Lookup-Tabke Kodierung
		+ Statistische Kodierung
			- Morse Kode
			- Huffman Kodierung
			- Ziv-Lempel Kode
			- Arithmetischer Kode

#### Lauflängenkodierung
Wiederholen sich identische Zeichen im Kode häufig,
lassen sie sich mit einer Lauflängenkodierung komprimieren.
Dies geschiet durch das Übertragen der Häufigkeit der identischen Zeichen
an statt der Zeichen selbst.
Entweder werden dazu Steuerzeichen verwendet 
oder Byte-Stuffing kommt zum Einsatz.

### Huffman Kodierung
Huffmann Kodierung erreicht verlustfrei eine Kompressionsrate um etwa 40%.
Die Kodewörter müssen *präfixfrei* sein,
das heisst das kein Kodewort zugleich Angang eines anderen Kodewortes sein darf.
Dies wird erreicht durch einen Entscheidungsbaum.

#### Entscheidungsbaum {-}
Ein Entscheidungsbaum ist ein Binärbaum, 
dessen Kanten mit 1 oder 0 beschriftet sind.
Jeder Pfad von der Wurzel zu einem Blatt stellt ein Kodewort dar.
So ergibt sich der Wert des Kodewors aus der Aneinanderreihnung der
auf dem Pfad durchlaufenen Kanten.
Die inneren Knoten sind mit der Häufigkeit des Auftretens der kodierten 
Zeichen im darunterliegenden Teilbaum.
Ziel iste es den häufigsten Buchstaben die kürzesten Pfade zuzuweisen,
den seltensten Buchstaben den längsten
und dabei die Präfixfreiheit der Kodeworte bewahren.

Bei der Huffmann Kodierung wird der Binärbaum rekursiv von den Blättern zur Wurzel aufgebaut.
Begonnen wird mit den beiden seltensten Zeichen,
sie sind dienen 2 benachbarten Blättern als Beschriftung,
hiebei sind Mehrdeutigkeiten möglich
und es könnnen mehrer gleichwertige Binärbäume endstehen.
Die summierte Häufigkeit des Teilbaumes dient als Knotenmarkierung.
Eine Kante wird mit 0,
die andere mit 1 beschriftet.
Mit den nächsthäufigen Zeichen und dem Teilbaum wird der Baum
nun weiter aufgebaut bis keine Zeichen mehr Übrig sind.

Das Verfahren stellt die Präfixfreitheit der Kodeworte sicher
und erzeugt sie in einer eindeutigen Weise.
Zur Kommunikation wird dabei zunächst das Kodebuch übertragen.

### Lempel-Ziv Kodierung
Eine Familie von Kodierungen,
welche im Gegensatz zu Huffmann ganze Zeichenketten als Kodewörter verwendet.
Dies erlaubt auch höhere Kompressionsraten als bei der Huffmann Kodierung.
Es verwendet jedoch Präfixe von Kodeworten,
so dass ein Kodewort zugleich auch der Anfang eines anderen Kodewortes sein kann.

## Qullen Kodierung
Verfahren der Quellen Kodierung können,
auch in Abhängigkeit von der Wahl der Parameter,
verlustfrei oder verlustbehaftet sein.
Die Semantik ist wichtig für die verlustbehaftete Kompression.
Die Kompressionsraten können aber größer als 10 sein 
und sind somit geeignet für das Internet.
Die Kompression beruht oft auf der *Diskreten Cosinus Transformation*
und dem Weglassen von Fourierkoeffizienten mit hoher Frequenz und kleiner Amplitude.
Die Kompression führt bei Bildern zu unschaerfe bei feinen Details,
was in der Regel kaum auffält sofern nicht zu stark komprimiert wird.
Ein typisches Beispiel für Quellen Kodierung ist das Bildformat JPEG.

Gliederung der Quellenkodierung:

	* Quellen Kodierung
		+ Prädikative Kodierung
		+ Vekorquantisierung
		+ Transformations Kodierung
			- Diskrete Fouriertransfromation
			- Schnelle Fouriertransformation (FFT)
			- Diskrete Cosinustransformation

#### Prädikative Kodierung {-}
Das jeweils nächste Zeichen im Datenstrom wird vorhergesagt von einem
Prädikator der bei Empfänger und Sender identisch ist.
Nun werden nurnoch Abweichungen übetragen,
was in der Regel weniger Zeichen benötigt als der Datenstrom selbst.
Die Kompressionsraten ist letztlich abhängig,
davon wie leicht sich die Daten vorhersagen lassen
und wie exakt der Prädikator ist.

## Kanal Kodierung
Mit der verlustbehafteten Kanal Kodierung können Kompressionsraten von über 10 erzielt werden.
Dazu werden Einzelne Teile der Daten weggelasen,
deren physiologische Wirkung als Teil des Datenstromes nicht von Bedeutung sind.
Beispielsweise sehr leise Töne die gleichzeitig
oder kurz nach lauten Tönen folgen da sie vom Ohr nicht wargenommen werden können.
Dies geschieht beispielsweise bei MP3s oder der Subbandkodierung.

Gliederung der Kanalkodierung

	* Kanal Kodierung
		+ Subsampling
		+ Subband Kodierung

## Hybride Kodierung
Die Kombination mehrer der vorher vorgestellten Verfahren.
Dies sind in der Kombination häufig verlustbehaftet.
Bekannte Verfahren sind JPEG und MPEG.

Verfahren der Hybriden Kodierung

	* Hybride Kodierung
		+ JPEG
		+ MPEG1, 2, 4, 7
		+ H.261, ...

# JPEG
<!--- Vorlesung vom 07.11.13 -->
Hybrides Kodierungsverfahren für Farbphotos welches schon 1993 standardiesiert,
von einer Untergruppe der ISO.
Nutz den kontinuierlichen Farbverlauf von "normalen" Bildern,
ohne binäre Übergängen zwischen Pixeln.
Weniger geeignet für Bilder mit hohen Kontrasten
oder hohem Detailreichtum.
Eine Verlustfreie Kompression ist bis 3:1 möglich,
darüberhinaus ist verlustbehaftet Kompression bis ca 40:1.
In der Regel wird jedoch verlustbehaftet komprimiert.
Bei der hybriden Kompression kommen diverse Algortihmen zu einsatz,
wie die Discrete Cosinustransformation und die Huffmann kodierung,
sowie diverse andere Verfahren.

Das Kompressionsschema ist unabhängig von:
 * Bildauflösung
 * Bildseitenverhältnis
 * Pixelgröße
 * Farbrepresentation
 * Bildkomplexität und statistische Eigenschaften des Bildes

## Farbmodell
Auch Farbraumdarstellung.
Feine Fabrverläufe werden vom Auge kaum wargenommen.
Hell Dunkel Unterschiede können jedoch besser erkannt werden.
Koppelung von Farbe und Helligkeit bei RGB.
Daher wird die Luminanz, das Helligkeitssignal, verwendet.
Die Signale werden Abgekürzt mit: Y, Cb und Cr.
Das erste Signal Y steht für die Luminanz oder auch Chrominanz des Signales,
was die Heeligkeit eines Bildpunktes codiert.
Cb und Cr (auch u,v) definieren eindeutig eine Farbe in einem 2D-Farbraum.
Ohne die Farbinformationen ergibt sich aus dem Y-Signal ein Schwarz-Weiss Bild.

Dieses System hat verschieden Bezeichungen,
funktioniert aber scheinbar ähnlich.
Mögliche Bezeichungen sind:
	* YUV
	* Y Cb Cr

## x:y:z Notation für Fabsubsampling
Die Farben von benachbarten Pixeln werden gemittelt.
In dieser Notation wird angegeben,
wie das Subsampling zwischen den Pixeln angewendet wird.
Die erste Zahl steht für die Anzahl der gemittelten Pixel im Y.
Die zweite Zahl bezeichnet die Anzahl der Pixel über die in Cb und Cr gemittelt wird.
Die letze Zahl bezeichnet die Richtung der Mittelung über die Pixel (?).

Typische Muster sind 4:2:2, wo pro 4 Pixel im Y-Signal 2 Pixel es bei Cb und CR gemittelt Pixel gibt. Wobei die Mitteliung in x-Richtung horizontal über jeweils zwei Nachbarpixel erfolgt.
Bei 4:1:1 werden pro 4-Pixel im Y-Signal gibt es ein gemitteles Pixel bei Cb und Cr. Hier wird auch in x-Richtung horizontal über je 4 Nachbarpixel gemittelt.

Die Bestandteile Helligkeit (Y), Farbe Cb und Farbe Cr werden als JPEG-Komponenten bezeichnet und als solche abgespeichert.
Dabei ist jede Komponente als 2-dimensionales Array implementiert,
welche die Bildinformationen enthält und diese anteilig zum Gesamtbild beisteuert.
Die Arrays werden getrennt abgelegt und komprimiert,
was die Rechenzeit niedrig hält ( O(n log n)).
In JEPG-Bilder sind bis zu 255 solcher Felder bzw. Komponenten möglich.
Angeblich lässt sich damit dann auch Transluzenz implementieren (auch wenn das ausser dem Prof noch keiner gesehen hat).

### JPEG Farb Subsampling
Beim Farb-Subsampling haben die Chrominanz einen geringe Auflösung als die Luminanzkomponente.
Dabei ist für den Komrepssionsfaktor wichtig wie die Auflösungen im Verhältnis zueinander stehen.
Dabei steht die Zahl Hi für das Verhältnis der Auflösung der Komponente i in der Horizontalen Richtung zur Vertikalen Richtung Vi.

Somit gehört beim Farbsubsampling jedes Element einer Chrominanzkomponente zu mehreren Elementen der Luminanzkomponente, es wird als der selbe Farbwert für mehrere benachbarte Pixel verwendet.

## Dateineinheiten
Kompressionsalgorithmen können bei JPEG auf 2 Datetypen angewendet werden:
 * Einzelne Komponentenelemente
 * einen Block von Komponetenelementen
In den JPEG-Algorithmen wird deshalb allgemein von Dateneinheiten gesprochen (Data Units).

Dabei werden die einzelnen Komponentenelemente nur mit verlustfreier Kompression verarbeitet, einer prädiktiven Vorhersage.
Zur verlustbehafeten Kompression wird in der Regel Farb-Subsampling verwendet und Blöcke der Größe 8x8 mit einer Diskreten Cosinus Transformation verarbeitet.

Die Verarbeitung erfolgt jedoch immer getrennt für jede Komponente.

## Minimal Coded Units
MCUs sind die Elemente aller Komponenten die logisch zusammengehören,
aber aus Gründen der Effizienz getrennt komprimiert und verarbeitet werden.
Die sind Beispielsweise die Farbkomponenten und Helligkeitskomponenten eines Pixels.

Werden einzenele Komponenten als Datenelemente und Farb-Subsampling verwedent, besteht eine MCU aus der Menge der zusammengehörigen Elemente aller Komponenten.

	*4*2 Bild mit 4:2:2 Farbsubsampling*
	Die Helligkeitskompenente ist ein 4+2 Feld mit den Elementen Y1 bis Y4 in der Ersten und Y5-Y8 ind der Zweiten Reihe.
	Die Farbkomponenten Cb is t ein 2x2 Feld mit den Elemente Cb1 und Cb2 in der Ersten Zeile und Cb3 und Cb4 in der 2. Zeile (analog mit Cr).
	Somit ergebn sich 4 MCUs:
		1. MCU1={Y1, Y2, Cb1, Cr1}
		2. MCU1={Y3, Y4, Cb2, Cr2}
		3. MCU1={Y5, Y6, Cb3, Cr3}
		4. MCU1={Y7, Y8, Cb4, Cr4}

Werden jedoch Blöcke von Komponenten als Datenelemente und Farbsubsampling verwendet, enhält eine MCU die Farb- und Helligkeitssignale eines Blocks von Pixeln.

## Kompression mit diskreter Cosinustransformation
Das Auge ist nicht in der Lage geringe Farbunterschiede wahr zu nehmen.
Geringe Unterschiede in der Helligkeit werden besser erkannt.
Wenn nun Chrominanzsignal fouriertransfomiert werden,
haben die Fourierkoeffizienten eine unterschiedliche Wichtigkeit,
da Chrominanzkoeffizieten mit hoher Frequenz feine Farbverläuf repreäsentieren 
können sie mit reduizerter Genauigket abgespeichert oder sogar verworfen werden.

In der Regel ändern sich Photos relativ wenig von Pixel zu Pixel.
Wird das Luminanzsignal fouriertransformiert haben die Koeffizienten meist nur geringe Amplituden, auf Grund der geringen Menge an Änderungen.
Somit können auch Luminanz-Koeffizienten mit hoher Frequenz in einer reduzierten Genauigkeit abgespeichert oder verworfen werden.

Die DCT trägt wesentlich zur Datenkompression bei und ist somit ein wichtiger Teil des JPEG Verfahrens.

## Verlustfreie Kompression in JPEG
Ermöglicht Kompressionsraten bis 3:1.
Dabei kommt prädikative Kodierung zum Einsatz, wobei die Daten anschließend mit per Huffman- oder arithmetischer-Kodierung weiter verdichtet werden.
Insgesamt gibt es 8 verschiedene Prädikatormodi.
<!--TODO Bild für Prädikatoren-->

## Verlustbehaftete Kompression in JPEG
Dient hauptsächlich der Kompression von Photos.
Die verschiedenen Modi verwenden unter anderem die DCT um das 2-dimensionale Bild in den 2-dimensionalen Spektralraum zu transformieren

### Sequentieller Modus
Daten werden in Blöcken von Pixeln der Größe 8x8 oder 16x16 verwendet und Farbsubsampling.
Das Bild wird Block für Block von links oben nach rechts unten zum komprimiert.

Dieser Modus ist am leichtesten zu implementiern und liefert die besten Kompressionsraten
und ist in den anderen Beiden Modi enthalten.

### Progressiver Modus
Bild wird mehrmals komprimiert, jedesmal etwas schärfer.
Übertragen werden dann immer weniger stark komprimierte Bilder.
Somit baut sich aus einem unscharfen ein scharfes Bild auf,
je nach Bandbreite und dauer der Übertragung.
Es werden Bilder immer schärfer durch schrittweise Hinzunahme von Fourierkoeffizienten mit hoher Frequenz oder durch Erhöhung der Genauigkeit der Fourierkoeffizienten.

### Hierarchischer Modus
In diesem Modus werden kontinuierlich Bilder mit höherer Auflösung übertragen.
Somit gewinnt das Bild an schärfe durch die Erhöhung der Anzahl an Pixeln,
durch Optimierung reicht es auch die Differenz zur gröberen Auflösung.
Somit werden keine Daten doppelt Übertragen wenn sich keine Daten ändern.

<!--TODO:Erweiterte Beschreibung des sequentiellen Modus -->

# MPEG
Beschreibt diverse Standards zur Kompression von Audio und Video Datein.
Diese basieren auf der JPEG-Kompression und erlauben eine verlustbehaftete Kompresion von 75:1 für Video und 10:1 bei Audio.
Die Qualität kann durch die Kompressionsrate beliebig gewählte werden.

 * MPEG-1 -- Videos in mittlere Qualität für Konsumelektronik
 * MPEG-2 -- Studio-Qualität für HD-TV
 * MPEG-4 -- niedirge Qualität bei extrem niedriger Bitrate

MPEG-1 und -2 erlauben den wahlfreien Zugriff auf jedes einzelne Bild innerhalb eines Videofilms und erlauben somit schnelles Vor- und Rückspulen.
Das Verfahren hat eine hohe Fehlertoleranz gegenüber Übertragungsfehlern.
Es erlaub eine Vielfalt hinsichtlich Bildgröße und Bildwiederholrate.
Beide Verfahren basieren auf ähnlichen Algorithmen,
haben jedoch einige spezifische Details.

## MPEG-1
Umfasst die drei Teile Audio, Video und System.
Die Zeitbasis arbeitet mit 90kHz und verteilt 33 Bit große Zeitstempel an Datenpakete.
Der Systemteil integriert die komprimierten Datenströme von Audio- und Video zu einem ununterbrochenen Paketstrom.
Der Systemmultiplexer besteth aus 2 Schichten.
Die Packetschicht gliedert die Bitströme aus Audio und Video-Teil in Pakete und versieht jedes Paket mit einem Header.
Die Packschicht fasst mehere Pakte der Paketschicht zu einem übergeordneten Paket zusammen das ebenfalls einen Header enthält.

### MPEG-1/Video
Der Videoteil von MPEG-1 macht sich zwei Fakten zu Nutze:
 * Benachbarte Pixel eines Bildes unterscheiden sich nur wenig voneinander
 * Aufeinanderfolgende Bilder eiens Videofilms unterscheiden sich auch nur wenig
 
Die örtliche Redundanz wird druch die Kompression der einzelnen Bildern als JPEG reduziert.
Die zeitliche Redundanz wird durch eine Kompensation in den Bewegungen eliminiert.
Es werden bei diesem Verfahren nur die Bilddetails erfasst die sich auch ändern.
So werden die Bildteile die (nahezu) unverändert bleiben, sich aber in ihrer Position ändern, durch die Angabe der neuen Position chrakterisiert.
Es gibt in MPEG-1/Video verschiedene Bildtypen, solche für verschobene Bildteile und "normale" vollständige Bilder.
Durch den Vergleich mit den vollständigen Bildern, lassen sich die unvollständigen, verschobenen Bilder aufbauen.
Der Vergleich erfolgt mittels einer Bewegungskompensation.

#### Bewegungskompensation
Die Bewegungskompensation ist besonders effektiv wenn sich nur eine Objekte im Vordergrund bewegen der die Kamera über eine Szenerie mit festem Bildhinter- und -vordergrund fährt.

Das Bild wird dazu in macroblöcke zerlegt, ähnlich wie bei JPEG.
Nun wird versucht dieser Macroblock in Bild n+1 wiederzufinden.
Um Rechenzeit zu sparen wird dabei nur die Umgebung des Macroblockes abgesucht.
Wird der Macroblock wiedergefunden wird die Postionsverschiebung mit Hilfe eines Bewegungsvektors dargestellt. 
Dieser und die Teile des Bildes die sich garnicht verändern müsssen nur einmal kodoiert und übertragen werden.

Die Entscheidung wann zwei Macroblöcke identische sind wird an Hand eines Entscheidungskriteriums fest gemacht, so gelten zwei Blöcke als identisch wenn ein bestimmter Schwellenwert überschritten ist.
Dieses Entscheidungskriterium ist bspw. die Zahl gleicher Pixel in einem Macroblock.
Alternativ kann auch der mittlere quadratische Abstand der Pixelwerte der beiden Macroblöcke herrangezogen werden (euklidische Metrik).
Hier werden dann die Abstände der Chrominanz und Liuminanz Werte verglichen.

Die Macroblöcke können entweder über eine *Brute-Force Methode* innerhalb eines bestimmten Suchbereiches oder durch eine Methode mit meheren Gittern gefunden werden.
Dabei benötigt die Brute-Force Methode eine höhere Rechenleistung bzw. mehr Zeit (O(n^3)), wärend die Gittermethode weniger Rechenleistung benötigt aber unter bestimmten umstände Blöcke nicht findet.

Letztlich ist es nötig verschiedene Bildtypen zu unterscheiden,
da der Video-Kodierer nur einen Bitstrom aus Bewegungsvektoren und JPEG-komprimierten Differenz-Macroblöcken oder nur den Bitstrom für einen Orginalen JPEG-Macroblock liefert.
Die Wiederverwendung von Blöcken ist aber nicht beliebig of möglich,
da sich immer kleine Fehler in die Blöcke einschleichen.
Deshalb liefert der MPEG-Video-Kodiere einen Bitstrom mit unterschiedlich kodierten Bildern, die so genannten MPEG-Frames.

### MPEG-1-Frames
Die verschiedenden Frames kommen im Wechsel vor nehmmen unterschieldiche Aufgaben war.
Sie unterscheiden sich in der Menge an Informationen und "Orginalität",
also der Menge der Bewegungskompensation.

#### Intracoded Frames
I-Frames enthalten ein Bild komplett ohne Bewegungskompensation als JPEG kodiert.
Sie dienen als Ausgangspunkt für P- und B-Frames.
Weiterhin wird mit I-Frames die Synchronisation hergestellt und sie sind der Ausgangspunkt zum Einklinken neuer Teilnehmer in einen Übertragung,
bzw weiner Resynchronisation nach Übertragungsfehlern.
Auch eignen sie sich zum Spulen, da sie keinen Bezug auf andere Frames nehmen.

#### Predicted Frames
Ein P-Frame besteht aus Orginal-JPEG-Macroblöcken und Differenz-JPEG-Macroblöcken mit Bewegungsvektoren.
Sie werden durch Bewegungskompensation aus zeitlich zurückliegenden I- oder P-Frames gebildet.
Dabei werden jedoch andere JPEG-Quantisierungstabellen verwendet,
welche geringe Differenz zu Null macht.
Da das Auge diese Unterschiede nur schlecht wahrnehmen kann führt das zu einer weiter verbesserten Kompression.

#### Bidirectional predicted Frames
B-Frames sind P-Frames sehr ähnlich, sie können sich jedoch sowohl auf das Vorgänger als auch das Nachfolgerbild (oder beide) beziehen.
B-Frames tragen jedoch nicht zur Fehlerfortpflanzung bei,
da von ihnen keine weitern Blöcke abgeleitet werden.
B-Frames werden in der Regel zwischen einem I- oder P-Referenzbild platziert.

#### DC-coded Frames
Jeder Pixel hat in einem DC-kodierten Rahmen den gleichen Wert.
Dieser ergibt sich aus dem Mittelwert aller Elemente eines Blockes.
Daduch haben DC-Rahmen eine sehr viel niedrigere Auflösung
und dienen haupstächlich zum sehr schnellen Spulen durch den Film.

### MPEG-Datenstrom
Der MPEG-Datenstrom ist hierarchisch in 6 Schichten eingeteilt.
Jede Schicht aht dabei einen eigenen Header, welcher Verwaltungsinfomationen enthält.
Die Daten aller Schichten bilden gemeinsam ein MPEG-Video Paket.
Jedes Video-Paket wird vom MPEG-Systemteil über eine 90kHz-Systemuhr mit einem MPEG-Audio Pakte starr gekoppelt (~synchronisiert).
Im folgenden sind die die MPEG-Layer beschrieben.

 * Sequence Layer
	+ Fasst Bildgruppen zu einem Film zusammen
	+ Header enthält allgemeine Information zum Film (Bildgröße. Bildformat, Wiederholrate, Bit-Rate, nötige Puffergröße)
 * Group-of-Picture Layer
	+ enthält mindestens einen I-Frame zur Synchronisation
	+ Ohne Bezug auf andere Gruppen dekodierbar (paralellisierbar)
 * Picture Layer
	+ enthält alle Informationen die benötigt werden um *ein* Bild zu dekodieren
	+ enthält Postionsinformationen für seine Gruppe
 * Slice Layer
	+ Sequenzen von aufeinanderfolgenden Macroblöcken im selben Bild
	+ Header definiert Beginn und Ende von Slice (paralellisierbar)
 * Macro Layer
	+ Beschreibt die Art und Lage von Macroblöcken
	+ Verschienden Macroblöcke für verschieden Fames
 * Block Layer
	+ Enthält die verschiedenen Frames

## MPEG-1/Audio
Standardformat zum Ablegen von Audio Daten,
insbesondere MPEG-1/Audio Level III (Layer III) auch bekannt als MP3.

Bei MPEG-1/Audio werden Abtastraten von 48kHz, 44.1kHz und 32kHz unterstütz.
Das Format is bitorientiert und die Audiodaten werden hintereinander in Frames übertragen,
welche abhängig vom Level eine feste Anzahl an Abtastpunkten haben.
Zusätzlich werden noch Metainformationen übertragen.

### Funktionsweise
MP3 macht sich dabei Eigenarten des Ohres zu nutze,
änlich wie MPEG bei den Augen.
So kann das menschliche Gehör kleine Lautstärkesprünge nach einem
großen Lautstärkesprung nicht wahrnehmen.
Weiterhin können leise Töne in einer ähnlichen Frequnzen von lauten Tönen nicht wahrnehmen.
Auch Rauschen wird bei lauten Tönen weniger als störend Wahrgenommen.

So ergibt siche in *psycho akustisches Modell* welches zur Komrepssion der Daten beiträgt.
Dies wird implementiert durch getrennte Kodierung von Subbändern.
Dabei werden die Frequenzen zwischen 20Hz und 20kHz in je 32 Unterbänder zu je 625Hz aufgeteilt und getrennt kodiert.
An dieser Stelle erfolgt auch das Weglassen leiser Töne neben lauten Tönen.

Die Abtastwerde werden dann je nach Subband quantisiert und mit DCT die Fourierkoeffizienten errechnet.
Anschließend erfolgt die Entropie-Kodierung der Fourierkoeffizienten mittel Huffman-Kode.

## MPEG-2
Bei MPEG-2 handelt es sich um eine Variante von MPEG-1 mit zusätzlichen leistungsmerkmalen.
Dazu gibt es neben dem Programmstrom,
welcher einem MPEG-1-Systemstron entspricht einen zweiten Transportstrom.
Dieser enthält Pakete fester länge, koppelt Audio und Video jedoch nur asynchron.
Das ist wiederum gut für die Übertragung im Netz,
aber schlecht für eine Lippensynchrone übertregung von Inhalten.
Der Programmstron kann hingegen Audio und Video über einen Systemtak synchron halten,
was die Lippensynchronizität erhöht aber schlechter für die Üertragung im Netz ist.
Unter Verlust der Synchronizität ist ein wechsel zwischen den Strömen möglich.

#### MPEG-1 vs MPEG-2 {-}
 * MPEG-2 unterstütz verschiedene Anwendungsgebiete
	+ 5 verschiedene Profile
 * 4 verschiedene Auflösungsstufen
 	+ niedrig, mittel, hoch, sehr hoch
	+ Datenraten höher als bei MPEG-1
 * D-Rahmen entfallen
 * Interlacing möglich (Halbbilder)

### MPEG-2/Audio
Es sind bis zu 5 Audiokanäle mit voller Bandbreite möglich,
was Surroundsound erlaubt.
Weiterhin sind 7 mehrsprachige Tonkanäle möglich
und die Tonqualität ist besser bei niedrigen Datenrante (weniger als 64 kBit/s).
Dabei bleibt MPEG-2 aufwärtskompatibel zu MPEG-1
und alle MPEG-1 Audioformate könne auch von MPEG-2 verarbeitet werden.

## MPEG-4
MPEG-4 (ISO 14496) war ursprünglich für Systeme mit wenigen Ressourcen gedacht
und sollte in der Mobilkommunikaion und Videotelefonie eingesetzt werden.
Die ursprüngliche Datenraten lagen zwischen 4800 und 64000 bit/s.
Mittlerweile sind etwa 25 Unterstandards zu MPEG-4 vorhanden.
Darin ist auch der H.264 Standard (AVC) für HDTV und bluray enthalten,
sowie Spezifikationen für 3D-Videos.
AVC und der Audioteil AAC erlauben höhere Kompressionsraten bei bessere Qualität,
benötigen jedoch auch einen höheren Rechenaufwand im Vergleich zu MPEG-1/2.

### Kompression in MEPG-4
In MPEG-4 werden die Algorithemen aus mehreren Standards gleichzeitg verwendet
um besoders hohe Komrepssionsraten zu erzielen (MPEG-1/2, H261, H263, H264).
Zusätzlich werden folgende Techniken eingesetzt:

 * Analyse der Semantik der Bildinhalte
 * Formale Sprache zur Beschreibung der Szenen (MSDL)
	+ MPEG-4 Syntactic Description Language
	+ MSDL erlaub beschreibung von Objekten und Operationen auf ihnen
	+ Diese Objekte werden kodiert und manipuliert wie Pixel in MPEG-1

# ISO Layer 7 - Die Anwendungsschicht
Im Gegensatz zum Internet ist der Layer 7 im Internet sehr stark ausgeprägt,
da die Layer 5 und 6 dort nicht implementiert sind.

## Klassifikation nach Diensten und Anwendungen
Dienst

:	Programme und Protokolle, die allgemein im Internet verfügbar sind und der Datenaustauschformat in einer Norm oder einem sog. RFC festgelegt ist

Anwendung

:	Individuelle Programme von Firmern, Institutionen oder Einzelpersonen, die auf Grund ihrer Client/Server-Struktur ein Rechnernetz brauch und die netweder propriertär oder für einen begrenzt öffentlichen Benutzerkreis bestmmt sind.

---------------------- -------------------------------
Dienste					  Anwendungen
---------------------- -------------------------------
DNS						  VOD

www						  web-TV

news						  Systeme zur Flugbuchung

ftp						  Video-Konferenzsysteme

E-Mail					  Verteilte Dokumentenbearbeitung

Zeitverteilung (ntp)			

telnet
---------------------- -------------------------------

Table: Anwendungen und Diensten auf Layer 7

Im weiteren werden noch einige der Dienste und Anwenungen näher beschrieben.

## Domain Name System
Jeder Rechner des Internet wird mit einer eindeutigen IPv4 oder IPv6 angesprochen.
Das Domain Name System übersetzt diese Art von nummerischen Adressen in literale,
also eine Zurodnung von foobar.de zu einer IP-Adresse.

Die Vergabe und Zurdnung erfolgt modular und hierarchisch in einer Baumstruktur.
Dabei enstsprechen die Blätter den IP-Adressen
und alle Knoten stehen für eine ganzen Bereich von Namen.
Dabei ist die Baumtiefe unbeschränkt,
genauso wie die Anzahl der Söhne pro Knoten.

Die zweitoberste Baumebene wird als Top-Level Domain bezeichnet (TLD)
und hat einen festgelegten Aufbau.
Die oberste Baumebene besteht aus 13 festgelegten Root-Servern,
welche Anfragen an alle darunterliegenden weiterleitet.
Der Baumdurchlauf geht maximal bis Top-Level Domain,
nicht bis zum Root-Server.
Insgesamt gibt es mehr als 200 TLDs welche alle den Root-Servern bekannt sind.

Der Name resultiert aus einer Verkettung der durchlaufenen Knotennamen,
welche jeweils durch einen Punkt getrennt sind.
