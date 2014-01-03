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

## Minimal Coded Units