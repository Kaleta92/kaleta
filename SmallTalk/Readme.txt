1. 		Um das Image öffnen zu können, benötigen Sie den Pharo Launcher.
		Diesen Launcher können Sie hier downloaden: https://pharo.org/download
		Ich hatte während des Semesters mit dem "pharo-launcher-1.9.2.msi" gearbeitet.
		
2.		Um das Image nach der Installation in die VM zu bekommen, klicken Sie oben rechts auf das
		mittlere Symbol "Import". Anschließend wählen sie aus dem Ordner KdP_final die KdP_final.image-Datei aus.
		Schon sollte der System Browser und 3 Playgrounds geöffnet sein. 
		
3.		Folgende Codes, welche ich in der Ausarbeitung nicht erwähnt habe, weil diese keine Rolle gespielt haben, nur zum Spielen,
		Lernen und Testen erstellt worden sind oder einfach nicht funktioniert haben, musste ich aus dem Image hierher exportieren, 
		da die Image-Datei sonst über 100mb gewesen ist und so große Dateien bekanntlich nicht auf GitHub geuploaded werden können.

Code-Ausschnitte:

element := PPUnresolvedParser new.
open := $( asParser trim.
close := $) asParser trim.
string := ($' asParser ,
			('''''' asParser / $' asParser negate) star flatten ,
			$' asParser) trim.
natural := #digit asParser plus flatten.
e := ($e asParser / $E asParser) , ($- asParser / $+ asParser) optional , natural.
number := ($- asParser optional, natural ,
			($. asParser , natural , e optional) optional) flatten trim.
id := (open , 'id:' asParser , natural trim , close) trim.
element def: ( (open , elementName , id optional , attribute star , close) trim).
elements := open, element star , close.


stringText := PPUnresolvedParser new.
string := 	$' asParser,
				stringText,
				$' asParser
				==> [ :token | token second ].

stringText def:($'asParser negate star flatten).
string parse: '''string'''


variable :=  #digit asParser plus token trim ==> [ :token | token value asNumber ].
abc := PPUnresolvedParser new.
def := PPUnresolvedParser new.
ghi := PPUnresolvedParser new.
abc def: (abc, $+ asParser trim, term ==> [ :nodes | nodes first + nodes last]) / abc.
def def: (ghi, $* asParser trim, def ==> [ :nodes | nodes first * nodes last]) / ghi.
ghi def: ($( asParser trim, abc, $) asParser trim ==> [ :nodes | nodes second]) / variable.
go := abc end.
go parse: '(1+2) * 3'


naturalNumber := #digit asParser plus.
decimal := $. asParser.
expo := $e asParser / $E asParser.
negative := $- asParser.
number := negative optional, naturalNumber, decimal, naturalNumber, expo optional, negative optional, naturalNumber
number matches: '-312'