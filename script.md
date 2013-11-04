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