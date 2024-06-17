
# Reverse Engineering Guide

---

## Inhaltverzeichnis

1. [Was ist Reverse Engineering?](#was-ist-reverse-engineering?)  
2. [Die x86 Architektur](#die-x86-architektur)  
   2.1 [Part 1: Ziele](#part-1-ziele)  
   2.2 [Part 2: Techniken](#part-2-techniken)  
   2.3 [Part 3: Typen von Malware](#part-3-typen-von-malware)  
   2.4 [Part 4: x86 Assembly Intro](#part-4-x86-assembly-intro)  
   2.5 [Part 5: Binäres Zahlensystem](#part-5-binäres-zahlensystem)  
   2.6 [Part 6: Hexadezimalsystem](#part-6-hexadezimalsystem)  
   2.7 [Part 7: Transistoren und Speicher](#part-7-transistoren-und-speicher)  
   2.8 [Part 8: Bytes, Words, Double Words, etc...](#part-8-bytes-words-double-words-etc)  
   2.9 [Part 9: x86 Basic Architecture](#part-9-x86-basic-architecture)  
   2.10 [Part 10: Allzweck-Register](#part-10-allzweck-register)  
   2.11 [Part 11: Segment Register](#part-11-segment-register)  
   2.12 [Part 12: Instruction Pointer Register](#part-12-instruction-pointer-register)  
   2.13 [Part 13: Steuerregister](#part-13-steuerregister)  
   2.14 [Part 14: Flags](#part-14-flags)  
   2.15 [Part 15: Stack](#part-15-stack)  
   2.16 [Part 16: Heap](#part-16-heap)
   2.17 [Part 17: Linux installieren](#part-17-linux-installieren)  
   2.18 [Part 18: vim Text Editor](#part-18-vim-text-editor)  
   2.19 [Part 19: Warum Assembly?](#part-19-warum-assembly)  
   2.20 [Part 20: Instruction Code Handling](#part-20-instruction-code-handling)  
   2.21 [Part 21: Programme compilen](#part-21-programme-compilen)  


---

## Was ist Reverse Engineering?

Wikipedia definiert es wie folgt:
> Reverse Engineering, auch Rückwärtsentwicklung oder Back Engineering genannt, ist der Prozess, bei dem ein künstliches Objekt dekonstruiert wird, um seine Entwürfe, seine Architektur oder seinen Code zu enthüllen oder um Wissen aus dem Objekt zu extrahieren. Es ähnelt der wissenschaftlichen Forschung, mit dem einzigen Unterschied, dass die wissenschaftliche Forschung an einem natürlichen Phänomen durchgeführt wird.  

Das ist echt überwältigend, oder? Nun, das ist einer der Hauptgründe, warum dieser Leitfaden existiert. Er wurde geschrieben, um Reverse Engineering so einfach wie möglich zu machen.  

![How To Reverse Engineer](/img/how-to-re.png)

Hier findest du eine umfangreiche Liste mit Reverse-Engineering-Tutorials zu x86-, x64-, 32-Bit-ARM- und 64-Bit-ARM-Architekturen. Egal, ob du gerade erst anfängst oder schon ein bisschen Ahnung hast: Mit diesem Guide und den praktischen Übungen darin kannst du die Grundlagen des Reverse Engineering kennenlernen. Das ist eine Fähigkeit, die in der Welt der Cybersicherheit einfach unverzichtbar ist. Wenn du schon Profi bist und nur ein paar deiner Skills auffrischen willst, kannst du das Inhaltsverzeichnis als Leitfaden nehmen, um dir bei Bedarf dein eigenes Tempo dafür zu schaffen, was bisher in jedem Kapitel besprochen wurde.

---

## Die x86 Architektur

---

### Part 1: Ziele

Malware-Analyse ist ein wichtiger Bestandteil der Erörterung der Grundlagen des Reverse Engineering. Dabei geht es darum, Informationen zu verstehen und zu untersuchen, die notwendig sind, um in ein Netzwerk einzudringen.  

In diesem kurzen Lehrgang schauen wir uns zunächst die grundlegenden Konzepte des Malware-Reverse Engineering an und gehen dann über zu einer grundlegenden Untersuchung der Assemblersprache.  

Die Lösung liegt in der Analyse der jeweiligen verdächtigen Malware-Binärdatei. Dabei stellt sich die Frage, wie man sie in Ihrem Netzwerk findet und schließlich eindämmt.  

Wenn du die Dateien identifiziert hast, die du analysieren willst, musst du Signaturen entwickeln, um Malware-Infektionen in deinem gesamten Netzwerk zu erkennen. Egal ob zu Hause oder im Büro, für alles, was du analysieren willst, musst du hostbasierte und Netzwerk-Signaturen entwickeln.  

Um mit dem Konzept der hostbasierten Signatur anzufangen, müssen wir verstehen, dass diese verwendet werden, um bösartigen Code auf einem Zielcomputer zu finden. Host-basierte Signaturen werden auch als Indikatoren bezeichnet, die Dateien identifizieren können, die vom infizierten Code erstellt oder bearbeitet wurden und die versteckte Änderungen an der Registrierung eines Computers vornehmen können. Das ist genau der Unterschied zu Antiviren-Signaturen. Die konzentrieren sich nämlich auf die Funktionsweise der Malware, nicht auf deren Zusammensetzung. Dadurch sind sie effektiver bei der Suche nach Malware, die sich bewegen kann oder von den Datenträgern entfernt wurde.  

---

### Part 2: Techniken

Es gibt zwei grundlegende Techniken, die du bei der Analyse von Malware anwenden kannst. Die erste ist die statische Analyse und die andere die dynamische Analyse.  

Bei der statischen Analyse werden Software-Tools verwendet, um die ausführbare Datei zu untersuchen, ohne die dekompilierten Anweisungen in der Assemblerdatei auszuführen. Wir konzentrieren uns hier nicht auf diese Art der Analyse, sondern schauen uns stattdessen mal an, wie man disassemblierte Binärdateien analysiert. Das machen wir aber zu einem späteren Zeitpunkt.  

Bei der dynamischen Analyse werden Disassembler und Debugger verwendet, um Malware-Binärdateien zu analysieren, während sie tatsächlich ausgeführt werden. IDA ist der populärste Disassembler und Debugger auf dem Markt. Er läuft auf mehreren Plattformen und Prozessoren. Es gibt auch andere Disassembler/Debugger-Tools, zum Beispiel Hopper Disassembler, OllyDbg und viele andere.  

Ein Disassembler macht aus einer in Assembler, C, C++ usw. geschriebenen ausführbaren Binärdatei Assembler-Anweisungen, die man dann debuggen und manipulieren kann.  

Reverse Engineering ist viel mehr als nur Malware-Analyse. Am Ende der X86-Reihe machen wir mit dem Capstone-Tutorial weiter. Da nutzen wir IDA, um ein echtes Szenario zu erstellen. Du wirst vom CEO von ABC Biochemicals beauftragt, heimlich zu versuchen, die Software seines Unternehmens zu hacken. Die Software steuert nämlich eine kugelsichere Tür in einem sehr sensiblen biochemischen Labor. So kannst du testen, wie gut die Software gegen echte Bedrohungen funktioniert. Das Projekt ist eigentlich ganz einfach, aber es zeigt letztendlich, was Assembler-Sprache kann und wie man sie zum Reverse Engineering nutzen kann, um Lösungen zu finden, wie man den Code besser gestalten kann, um ihn sicherer zu machen.  

---
### Part 3: Typen von Malware

Malware lässt sich in mehrere Kategorien einteilen, auf die ich im Folgenden kurz eingehen werde.   

Eine Backdoor ist ein bösartiger Code, der sich selbst in einen Computer einbettet, um einem Angreifer aus der Ferne den Zugang zu ermöglichen, der nur sehr wenig oder manchmal gar keine Befugnis hat, verschieden Befehle auf dem jeweiligen lokalen Computer auszuführen.  

Ein Botnet ermöglich einem Angreifer den Zugriff auf ein System, wobei er jedoch nicht von einem einzelnen Angreifer, sondern von einem Command-and-Control Server Anweisungen erhält, der eine unbegrenzte Anzahl von Computern gleichzeitig kontrollieren kann.  

Ein Downloader ist nichts anderes als bösartiger Code, der nur einen einizgen Zweck hat, nämlich andere bösartige Software zu installieren. Downloader werden häufig installier, wenn sich ein Hacker zunächst Zugang zu einem System verschafft. Der Downloader installiert dann zusätzliche Software, um das System zu knotrollieren.  

Es gibt Malware für den Informationszugang, die Informationen von einem Computer sammelt und sie direkt an einen Host sendet, z.B. einen Keylogger oder einen Passwort-Grabber, und die normalerweise verwendet wird, um Zugang zu verschiedenen Online-Konten zu erhalten, die sehr sensibel sein können.  

Es gibt bösartige Programm, die andere bösartige Programme starten, die nicht standarmäßige Optionen verwenden, um einen erweiterten Zugang oder eine besser Tarn-/Verstecktechnik beim Eindringen in ein System zu erhalten.  

Rootkits sind eine der gefährlichsten Formen von Malware. Sie verstecken sich vor dem Benutzer und machen es extrem schwer, sie zu finden.  
Ein Rootkit kann Prozesse manipulieren, indem es z.B. die IP-Adresse bei einem IP-Scan verbirgt. So kann ein Benutzer nie erfahren, dass er eine direkte Verbindung zu einem Botnet oder einem anderen entfernten Computer hat.  

Scareware ist eine Art Software, mit der man Benutzer dazu verleiten will, zusätzliche Software zu kaufen. Die Software gaukelt ihnen vor, sie würde sie vor eine Bedrohung schützen, obwohl das gar nicht stimmt. Wenn die Benutzer dafür bezahlen, dass die Software vom Computer entfernt wird, kann sie dort bleiben und später in veränderter Form wieder auftauchen.  

Es gibt auch verschieden Arten von Malware. Die verbreiteteste ist, dass ein Zielcomputer Spam-Mails versendet. Damit kann der Angreifer Geld verdienen, indem er Benutzern verschiedene Dienste verkauft.   

Die letzte Form von Malware ist ein herkömmlicher Wurm oder Virus. Der kopiert sich selbst und greift andere Computer an.

Das wars dann erst mal mit unserer Diskussion über Malware. Wir müssen nämlich erst mal wieder ganz von vorne anfangen und verstehen, wie ein Computer auf seiner grundlegenden Ebene funktioniert

---

### Part 4: x86 Assembly Intro

Wir starten jetzt eine Reise, die euer Leben für immer verändern wird! 

Es gibt viel zu lernen, um ein gutes Verständnis der Assembler-Sprache zu bekommen und warum es wichtig ist, die Grundlagen zu verstehen.
Die erste Frage die wir beantworten müssen, ist, was x86 Assembly Language ist. Die Antwort ist eine Familie von rückwärtskompatiblen Assembler-Sprachen, die mit der Intel 8000er Serie von Mikroprozessoren kompatibel sind. x86 Assembly Languages werden verwendet, um Objektcode für die oben erwähnte Serie von Prozessoren zu erzeugen. Sie verwenden Mnemonics, um die Anweisungen darzustellen, die die CPU ausführen kann.

Die Assemblersprache für den x86-Mikroprozessor läuft auf verschiedenen Betriebssystemen. Wir konzentrieren uns auf die Linux-Assemblesprache und verwenden dabei die Intel-Syntax. Außerdem lernen wir, wie man in C programmiert, und zerlegen den Quellcode, um die jeweilige Assemblersprache zu analysieren.

Für die x86-Assemblersprache gibt es zwei verschiedene Syntaxen. Die AT&T-Syntax war in der Unix-Welt sehr verbreitet, weil das Betriebssystem in den AT&T Bell Labs entwickelt wurde. Im Gegensatz dazu wurde die Inte-Syntax ursprünglich für die Dokumentation der x86-Plattform verwendet und war in der MS-DOS- und Windows-Umgebung vorherrschend.

Der größte Unterschied zwischen den beiden ist, dass in der AT&T-Syntax die Quelle vor dem Ziel steht, während in der Intel-Syntax das Ziel vor der Quelle steht. Wir besprechen das später im Lehrang noch genauer.

Bevor du zu Tür rennst und es bereust, diese Reise angetreten zu haben, denk daran: Einige grundlegende Zusammenhänge sind hilfreich, die wir im Laufe unserer Suche entwickeln werden. Viele dieser Themen sind zu diesem Zeitpunkt vielleicht etwas verwirrend, was völlig normal ist da wir sie mit der Zeit entwickeln werden.

Wir werden uns auf Linux Assembly konzentrieren, weil Linux auf einer Vielzahl von Hardware läuft und in der Lage ist, Geräte wie ein Mobiltelefon, einen Personal Computer oder einen komplexen kommerziellen Server zu betreiben.

Linux ist außerdem Open Source und es gibt viele Versionen. Im Gegensatz dazu gehört Windows einem einzigen Unternehmen, nämlich Microsoft. Microsoft ist dafür verantwortlich, Updates, Sicherheits- und Services-Patches bereitzustellen. Linux wird hingegen von Millionen von Fachleuten kostenlos zur Verfügung gestellt!

Wir schauen uns auch die 32-Bit-Architektur an, weil die meiste Malware dafür geschrieben wird, um möglichst viele Systeme zu infizieren. 32-Bit-Anwendungen/Malware funktionieren auch auf 64-Bit-Systemen. Deshalb wollen wir die Grundlagen der 32-Bit-Wetl verstehen.

---

### Part 5: Binäres Zahlensystem

Binäre Zahlen sind das Herzstück eines Computers. Ein Bit in einem Computer ist entweder an oder aus. Ein Bit ist entweder mit Strom versorgt oder es ist nicht vorhanden. In Zukünftigen Lektionen gehen wir das noch genauer durch.

Du bist verwirrt und fragst dich, wie es jetzt weitergehen soll?

Aber keine Sorge! Das binäre Zahlensystem ist da! Im Binärsystem hat jede Spalte den doppelten Wert der Spalte zu ihrer Rechten. Außerdem gibt es nur zwei Ziffern, die zufällig 0 und 1 sind. 
Im Dezimalsystem zur Basis 10 haben wir z.B. die Zahl 15, was bedeutet: `(1 x 10) + (5 x 1) = 15`. Die 5 ist also die Zahl mal 1 und die 1 ist die Zahl mal 10.

Das Binärsystem funktioniert auf ähnliche Weise, allerdings beziehn wir uns jetzt auf die Basis 2. Die gleiche Zahl im Binärformat ist 1111. Zur Veranschaulichung:  
![Binärsystem](/img/binary.png)

Das binäre Zahlensystem ist ein Zahlensystem, das nur zwei Ziffern verwendet, um Zahlen darzustellen. Diese Zahlen sind für Computerarchitekturen notwendig und nicht die Ziffern 1 bis 9 plus 0.
Binäre Zahlen sind wichtig, weil sie die Entwicklung von Computern und verwandten Technologien vereinfachen. Das Zahlensystem besteht aus zwei Ziffern, mit denen Zahlen dargestellt werden, die für eine Computerarchitektur notwendig sind. Im Gegensatz zum Dezimalsystem werden hier die Ziffern 1 bis 9 plus 0 nicht verwendet.  

---

### Part 6: Hexadezimalsystem

Jetzt, wo wir das Binärsystem drauf haben, können wir uns mal dem Nummerierungsystem der Nummerierungssysteme widmen!

Im Binärsystem haben wir gelernt, dass jede Zahl ein Bit darstellt. Wenn wir 8 Bits kombinieren, erhalten wir ein Byte. Ein Byte kann weiter unterteilt werden in seine oberen 4 Bits und seine unteren 4 Bits. Eine Kombination von 4 Bits ist ein Nibble. Da 4 Bits den Bereich von 0 bis 15 abdecken, ist ein Zahlensystem zur Basis 16 einfacher zu handhaben. 
Du musst dir merken, dass wir bei der Basis 16 mit 0 beginnen und dass 0 bis 15 daher 16 verschiedene Zahlen sind.

Dieses spannende Zahlensystem wird Hexadezimal genannt. Der Grund, warum wir dieses Zahlensystem verwenden, ist, dass es in x86 Assembly viel einfacher ist, binäre Zahlendarstellungen in Hexadezimal auszudrücken als in jedem anderen Zahlensystem.

Hexadezimal ist ähnlich wie jedes andere Zahlensystem, mit dem Unterschied, dass im Hexadezimalsystem jede Spalte das 16-fache des Wertes der Spalte zu ihrer Rechten hat. Das Schöne an der Hexadezimaldarstellung ist, dass es nicht nur 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 gibt, sondern auch A, B, C, D, E und F und damit 16 verschiedene Symbole.

Schauen wir uns eine einfache Tabelle an, um zu sehen, wie das Hexadezimalsystem im Vergleich zum Dezimalsystem aussieht.  
![Dezimal-Hexadezimaltabelle](/img/hexdec-table.png)

Im Dezimalsystem wird alles in der Potenz von 10 behandelt. Nehmen wir die Zahl 42 und untersuchen wir sie im Dezimalsystem:

```
2 x 10 ^ 0 = 2

4 x 10 ^ 1 = 40
```

Zur Erinnerung: 10 hoch 0 ist 1 und 10 hoch 1 ist 10, also ist 2 + 40 = 42

Schnapp die deinen Kaffee, jetzt kommt der Spaßteil!

Wenn wir verstehen, dass das Dezimalsystem ein Zahlensystem zur Basis 10 ist, können wir eine einfach Formel erstellen, in der b die Basis darstellt. In diesem Fall ist b = 10.

```
(2 * b ^ 0) + (4 * b ^ 1)
(2 * 10 ^ 0) + (4 * 10 ^ 1) = 42
```
In binärer Darstellung ist, 42 dezimal 0010 1010 binär und sieht wie folgt aus:

```
0 x 2 ^ 0 = 0
1 x 2 ^ 1 = 2
0 x 2 ^ 2 = 0
1 x 2 ^ 3 = 8
0 x 2 ^ 4 = 0
1 x 2 ^ 5 = 32
0 x 2 ^ 6 = 0
0 x 2 ^ 7 = 0

0 + 2 + 0 + 8 + 0 + 32 + 0 + 0 = 42 dezimal
```

Im Hexadezimalsystem wird alles in der Potenz von 16 behandelt. Daher ist 42 dezimal gleich 2A hexadezimal:

```
10 * 16 ^ 0 = 10
2 * 16 ^ 1 = 32

10 + 32 = 42 dezimal => 2A hexadezimal
```

Das ist dasselbe wie:

```
10 * 1 = 10
2 + 16 = 32

10 + 32 = 42 dezimal => 2A hexadezimal
```

Beachte, dass 10 dezimal gleich A hexadezimal und 2 dezimal gleich 2 hexadezimal ist. Wenn wir in unserer obigen Formel mit A, B, C, D, E oder F arbeiten, müssen wir sie in ihre dezimale Entsprechung umwandeln.

Nehmen wir ein weiteres Beispiel für F5 hexadezimal. Das sieht so aus:

```
5 x 16 ^ 0 = 5
15 x 16 ^ 1 = 240

5 + 240 = 245 dezimal => F5 hexadezimal
```

Hier ist eine Tabelle von binär nach hexadezimal:
![Binär zu Hexadezimal Tabelle](/img/bin-to-hex.png)

Das ist wichtig, wenn wir unsere C-Programme in Assembler umwandeln.

Ich zeige euch das mal anders. Lassen wir uns noch ein paar Hexadezimalzahlen geben und rechnen wir sie in Dezimalzahlen um:
![Hex1](/img/hex1.png)

Um F1CD noch eimal als einfache Konvertierung hervorzuheben:

```
D --- 13 x 1 = 13
C --- 12 x 16 = 192
1 --- 1 x 256 = 256
F --- 15 x 4096 = 61,440

13 + 192 + 256 + 61,440 = 61,901
```

So funktioniert die Addition in Hexadezimal: Von nun an haben alle Zahlen im Hexadezimalsystem ein "h" neben der Zahl:
![Addition in Hex1](/img/hexadd1.png)

Ein weiteres Beispiel:
![Addition in Hex2](/img/hexadd2.png)

Jetzt schauen wir uns die Subtraktion an:
```
Sub

     7 13    
   A 8 3 h 
-  4 3 B h
-----------
   6 4 8 h

   3 - B = undefined [B steht für 11 in dezimal]  
                     [Wir können 3 nicht von 11 subtrahieren]  
                     [Wir leihen 1 von der 8 und machen daraus eine 7]  
                     [3 bedeutet, dass wir 1 brauchen, um eine Gruppe von 16 zu vervollständigen]  
                     [Wenn wir 1 zu den zusätzlichen 3 addieren, ergibt das 19]  
                     [19 - B oder 19 - 11 = 8]  
```

Du fragst dich wahrscheinlich, warum dieser Typ so viel Zeit damit verbringt, so viele verschiedene Lernmethoden durchzugehen! Die Antwort ist, dass jeder von uns ein wenig anders lernt als der andere. Ich wollte verschiedene Darstellungen von Hexadezimalzahlen im Vergleich zu Dezimal- und Binärzahlen zeigen, um das Gesamtbild zu vervollständigen.  
  
Es ist wichtig, dass du verstehst, was hier vor sich geht, um weiterzumachen.  

---  

### Part 7: Transistoren und Speicher

In der letzten Lektion haben wir uns ausführlich mit dem hexadezimalen Zahlensystem befasst. Ich fasse diese Lektion kurz, damit du die Lektion von letztem Mal noch einmal nachlesen kannst. Hexadezimale Zahlenkonvertierungen und das manuelle Addieren und Subtrahieren sind echt wichtig.  

In der Realität haben wir Taschenrechnet, in der Realität nutzen wir das Windows-Betriebssystem, in der Realität nutzen professionelle Reverse Engineers GUI-Debugger wie IDA Pro und andere.  

Die Frage ist, warum ich nicht direkt auf den Kern dessen eingehe, was echte Reverse Engineers tun? Die Antwort ist einfach: Man muss die Maschine sehr gut verstehen und respektieren, um großartig zu werden. Wir können die Welt nicht verändern, ohne sie zuerst vollstädnig zu verstehen. Geduld und Ausdauer zahlen sich am Ende aus.

Ich konzentriere mich auf Linux und die konsolenbasierte Programmierung weil die meisten professionellen Server Linux verwenden und daher die größte Bedrohung durch Malware darstellen. Wenn du Linux-Assembler verstehst, kannst du das auf Bibliotheken basierende, portable ausführbare Format von Windows-Assembler viel besser verstehen.

Jetzt mal Butter bei die Fische! Zurück zu den Grundlagen der Computertechnik!

Wenn wir uns fragen, was ein Computer ist, müssen wir so weit wie möglich auf die Grundlagen zurückgehen.

Ein elektronischer Computer besteht einfach aus Transistorschaltern. Transistoren sind mikroskopisch kleine Kristalle aus Silizium, um als Schalter zu fungieren. Moderne Computer verfügen über sogenannte Feldeffekttransistoren.

Nehmen wir als Beispiel einen Computer mit drei Pins. Wenn an Pin 1 eine elektrische Spannung angelegt wird, fließt Strom zwischen Pin 2 und Pin 3. Wird die Spannung am ersten Pin entfernt, fließt kein Strom mehr zwischen Pin 2 und Pin 3.

Wenn wir ein wenig ausholen, sehen wir, dass es auch Dioden und Kondensatoren gibt. Wenn wir diese zusammen mit den Transistorschaltern betrachten, haben wir eine Speicherzelle. Eine Speicherzelle hält einen minimalen Stromfluss aufrecht. Wenn du eine kleine Spannung an den Eingangsanschluss und eine ähnliche Spannung an den Auswahlanschluss legst, wird eine Spannung an den Ausgangsanschluss angelegt und dort beibehalten. Die Ausgangsspannung bleibt in ihrem eingestellten Zustand, bis die Spannung vom Eingangsanschluss in Verbindung mit dem Auswahlanschluss entfernt wird.  
Warum ist das wichtig? Das ist eigentlich ganz einfach: Wenn Spannung vorhanden ist, zeigt das eine binäre 1 an. Wenn keine Spannung vorhanden ist, zeigt das eine binäre 0 an. Die Speicherzelle speichert also entweder eine binäre 1 oder eine binäre 0, was "an" oder "aus" bedeutet.

---

### Part 8: Bytes, Words, Double Words, etc...

Ein Byte besteht aus 8 Bits. Zwei Bytes werden als word bezeichnet, zwei words werden als double words, das aus 4 Bytes (32 Bit) besteht, und ein quad word aus 8 Bytes (64 Bit).

Ein Byte besteht aus 8 Bits und ist eine Potenz von 2^8, was 256 ergibt. Die Anzahl der Binärzahlen mit 8 Bits ist einer von 256 Werten, die bei 0 beginnen und bis 255 gehen.

Jedes Byte des Speichers in einem Computer hat seine eigene eindeutige Adresse. Schauen wir uns mal die disassemblierten Anweisungen für eine einfache Hello-World-Anwendung unter Linux an. Wir setzen dazu einen Breakpoint bei der main-Funktion. Wir verwenden dafür den GDB-Debugger:
![Beispiel Disassembler](/img/disassembled-example.png)

Keine Sorge, wenn das noch keinen Sinn ergibt. Ich will dir damit nur einen kleinen Einblick in unser erstes Programm geben, das wir zusätzlich zum Lernen über Speicher in einem Computer untersuchen werden.  

Unten siehst du eine Untersuchung des ESP-Registers. Auch hier ist es nicht entscheidend, dass du weißt, was ein Register ist oder was ESP tut. Wir wollen einfach nur sehen, wie ein Speicherplatz aussieht:
![Speicherstelle ESP-Register](/img/memory-location.png)

Wir sehen den Speicherplatz `0xffffd040`, der natürlich in Hexadezimal angegeben ist. Wir sehen auch den Wert im ESP-Register, nämlich `0xf7fac3dc`, ebenfalls in hexadezimaler Schreibweise.

Es ist wichtig zu verstehen, dass `0xffffd040` 4 Bytes und ein **Double Word** ist. Wie wir in [Teil 6](#part-6-hexadezimalsystem) gelernt haben, wird jede hexadezimale Ziffer 4 Bits lang, auch Nibble genannt. In `0xffffd040` sehen wir uns die äußerste rechte Ziffer 0 an. In diesem Beispiel ist die 0 (hexadezimal) 4 Bits lang. Wenn wir uns 40 (hexadezimal) ansehen, sehen wir, dass sie ein Byte oder 8 Bits lang ist. Bei `d040` handelt es sich um zwei Bytes oder ein **Word**. Schließlich ist `ffffd040` ein **Double Word** oder 4 Byte lang, also 32 Bit lang. Das "0x" am Anfang der Adresse bedeutet lediglich, dass es sich um einen hexadezimalen Wert handelt.

Ein Computerprogramm ist nichts anderes als eine Reihe von Maschineenbefehlen, die im Speicher abgelegt sind. Eine 32-Bit-CPU holt sich ein **Double Word** von einer Speicheradresse. Ein **Double Word** besteht aus vier Bytes in einer Reihe, die aus dem Speicher gelesen und in die CPU geladen werden. Sobald die Ausführung beendet ist, holt sich die CPU den nächsten Maschinenbefehl aus dem Speicher vom Befehlszeiger.

Für alle, die noch nicht so firm in Assembler sind, habe ich jetzt einen ersten Einblick gegeben. Keine Sorge, wenn Du nicht alles auf Anhieb verstehst. Ich nehme mir Zeit, das Ganze Schritt für Schritt zu erklären. Und keine Bange, wir wiederholen das Ganze auch noch mal in den nächsten Lektionen.  
Wichtig ist, dass Du dir Zeit nimmst und dich mit den einzelnen Lektionen auseinandersetzt. 

---

### Part 9: x86 Basic Architecture

Eine Computeranwendung ist einfach eine Tabelle mit Maschinenanweisungen, die im Speicher abgelegt sind. Die Binärzahlen, aus denen das Programm besteht, sind nur durch die Art und Weise, wie die CPU sie verarbeitet, einzigartig.

Die grundlegende Architektur besteht aus einer CPU, einem Speicher und I/O-Devices zu Deutsch auch E/A-Geräte (Eingabe/Ausgabe), also Geräten, die Daten empfangen und senden. Sie sind alle über einen Systembus verbunden (siehe unten).
![Systembus Verbindung](/img/system-bus.png)

Die CPU besteht aus vier Teilen:  

1. Steuereinheit (Control Unit) - holt Anweisungen von der CPU ab und dekodiert sie, speichert sie im Speicher und ruft sie von dort ab.  
2. Ausführungseinheit (Execution Unit) - hier werden die abgerufenen Befehle ausgeführt.  
3. Register - Das sind Speicherplätze in der CPU, die Daten kurzzeitig speichern.  
4. Flags - zeigen an, wenn die Ausführung stattfindet.
![4 CPU Parts](/img/4-cpu-parts.png)

Als 32-Bit-x86-Prozessor holt eine 32-Bit-CPU zunächst ein **Double Word** (4 Byte oder 32 Bit lang) von einer bestimmten Adresse im Speicher ab, das aus dem Speicher gelesen und in die CPU geladen wird. Dann checkt die CPU das Binär-Bitmuster im **Double Word** und startet die Ausführung der Prozedur, die ihr durch den abgerufenen Maschinenbefehl aufgetragen wird.

Wenn ein Befehl ausgeführt wurde, geht die CPU zum Speicher und holt den nächsten Maschinenbefehl. Das Register, das wir in einer späteren Lektion besprechen, heißt EIP oder Befehlszeiger (instruction pointer). Es enthält die Adresse des nächsten Befehls, der aus dem Speicher geholt und dann ausgeführt werden soll.

Wir können sofort sehen, dass wir das Programm so verändern können, dass es Dinge tut, die es **NICHT** tun soll, wenn wir den Fluss des EIP kontrollieren. Das ist eine beliebte Technik, die von Malware verwendet wird.

Der gesamte Abruf- und Ausführungsprozess ist an die Systemuhr gekoppelt, die ein Oszillator ist, der in präzisen Intervallen Rechteckimpulse aussendet.

---

### Part 10: Allzweck-Register

Allzweckregister werden verwendet, um Daten vorübergehend zu speichern, während sie im Prozessor verarbeitet werden. Die Register haben sich im Laufer der Zeit startk weiterentwickelt und werden es auch weiterhin tun. Für unsere Zwecke konzentrieren wir uns auf die 32-Bit-x86-Architektur.

Jede neue Version von Allzweckregistern wird so entwickelt, dass sie mit früheren Prozessoren kompatibel ist. Das heißt, dass Code, der 8-Bit-Register auf den 8080-Chips verwendet, auch auf dem heutigen 64-Bit-Chipsatz funktioniert.

Allzweckregister können alle möglichen Daten speichern. Einige von ihnen haben sogar spezielle Funktionen, die in Programmen verwendet werden. Schauen wir uns mal die acht Allzweckregister in der IA-32-Architektur an.

**EAX:** ist das Hauptregister für arithmetische Berechnungen. Man nennt es auch Akkumulator, weil es die Ergebnisse von arithmetischen Operationen und die Rückgabewerte von Funktionen enthält.

**EBX:** ist das Basisregister. Darauf werden Daten im DS-Segment gespeichert. So kann man die Basisadresse des Programms speichern.

**ECX:** ist ein Register, das oft verwendet wird, um einen Wert zu speichern, der angibt, wie oft ein Vorgang wiederholt werden soll. Es wird für Schleifen- und String-Operationen verwendet.

**EDX:** ist ein echtes Multitalent. Es wird auch für E/A-Operationen verwendet. Außerdem wird EAX auf 64 Bits erweitert.

**ESI:** ist das Quellindexregister. Es zeigt auf Daten in dem Segment, auf das das DS-Register zeigt. Bei String-und Array-Operationen wird es als Offset-Adresse verwendet. Es enthält die Adresse, von der aus die Daten gelesen werden soll.

**EDI:** ist das Ziel-Index-Register. Es zeigt auf die Daten (oder das Ziel) in dem Segment, auf das das ES-Register verweist. Bei String- und Array-Operationen wird es als Offset-Adresse verwendet. Es enthält die implizite Schreibadresse für alle String-Operationen.

**EBP:** Basiszeiger. Zeigt auf Daten auf dem Stack (im Stack Segment). Er zeigt auf das Ende des aktuellen Stack-Frames. Wird verwendet, um auf lokale Variablen zu verweisen.

**ESP:** ist der Stapelzeiger (im Stack Segment). Er zeigt auf den oberen Teil des aktuellen Stack-Frames. Er wird verwendet, um auf lokale Variablen zu verweisen.

Ach ja, und denk drank: Die Register oben sind 32 Bit oder 4 Byte lang. Die unteren 2 Bytes von EAX, EBX, ECX und EDX kann man durch AX ansteuern. Dann wird das Ganze in 4 Teile aufgeteilt. AH, BH, CH und DH für die High-Bytes und AL, BL, CL und DL für die Low-Bytes. Die sind jeweils 1 Byte lang.

Außerdem können ESI, EDI, EBP und ESP durch ihr 16-Bit-Äquivalent referenziert werden, das SI, DI, BP, SP ist.

Das kann am Anfang etwas verwirrend sein. Ich versuche es mal in der folgenden Tabelle zu veranschaulichen:
![](/img/image1.png)

Bei EAX wäre AX das 16-Bit-Segment. Du kannst AX weiter unterteilen in AL für die niedrigeren 8 Bits und AH für die hohen 8 Bits. Das Gleiche gilt auch für EBX, ECX und EDX. EBX hat BX als 16-Bit-Segment. Du kannst BX weiter unterteilen in BL für die niedrigeren 8 Bits und BH für die hohen 8 Bits. ECX hat CX als 16-Bit-Segment und du kannst CX weiter unterteilen in CL für die niedrigen 8 Bits und CH für die hohen 8 Bits. EDX hat DX als 16-Bit-Segment und du kannst DX weiter unterteilen in in DL für die niedrigen 8 Bits und DH für die hohen 8 Bits.

ESI, EDI, EBP und ESP können wie folgt in ihre 16-Bit-Segmente unterteilt werden:
![](/img/image2.png)

ESI hätte also SI als 16-Bit-Segment, EDI hätte DI als 16-Bit-Segment, EBP hätte BP als 16-Bit-Segment und ESP hätte SP als 16-Bit-Segment.

---

### Part 11: Segment Register

Es gibt sech Segmentregister:

**CS:** Das Codesegmentregister speichert die Basisposition des Codeabschnitts (.text section), der für den Datenzugriff verwendet wird.

**DS:** Das Datensegmentregister speichert die Standardposition des Variablenabschnitts (.data section), der für den Datenzugriff verwendet wird.

**ES:** Extrasegmentregister, das bei Stringoperationen verwendet wird.

**SS:** Stack-Segmentregister speicher die Basisposition des Stack-Segments und wird verwendet, wenn der Stack-Pointer implizit oder der Basis-Pointer explizit verwendet wird.

**FS:** Extra-Segmentregister

**GS:** Extra-Segmentregister

Jedes Segmentregister ist 16 Bit lang und enthält den Zeiger auf den Beginn des speicherspezifischen Segments. Das CS-Register enthält den Zeiger auf das Codesegment im Speicher. Das Codesegment ist der Ort, an dem die Befehlscodes im Speicher abgelegt sind. Der Prozessor holt die Befehlscodes aus dem Speicher, indem er den Wert des CS-Registers und einen Offset-Wert aus dem Befehlszeigerregister (EIP) verwendet. Es ist zu beachten, dass kein Programm das CS-Register explizit laden oder ändern kann. Der Prozessor weist seine Werte zu, wenn dem Programm Speicherplatz zugewiesen wird.

Die Segmentregister DS, ES, FS und GS werden verwendet, um auf Datensegmente zu verweeisen. Jedes der vier separaten Datensemente hilft dem Programm, Datenelemente zu trennen, um sicherzustellen, dass sie sich nicht überlappen. Das Programm lädt die Datensegmentregister mit dem entsprechenden Zeigerwert für die Segmente und verweist dann mit einem Offsetwert auf die einzelnen Speicherplätze.

Das Stack-Segmentregister (SS) wird verwendet, um auf das Stack-Segment zu zeigen. Der Stack enhält Datenwerde, die an Funktionen und Prozeduren innerhalb des Programms übergeben werden.

Segmentregister werden als Teil des Betriebssystems betrachtet und können in den meisten Fällen weder gelesen noch direkt verändert werden. Wenn man im Protected Mode Flat Model (x86-Architektur, 32-Bit) arbeitet, wird das Programm ausgeführt und erhält einen Adressraum von 4 GB, in dem jedes 32-Bit-Register potenziell jeden der 4 Milliarden Speicherplätze adressieren kann, mit Ausnahme der vom Betriebssystem definierten geschützten Bereiche. Obwohl der physikalische Speicher größer als 4 GB sein kann, kann ein 32-Bit-Register nur `4.294.967.296` verschiedene Speicherstellen adressieren. Wenn du mehr als 4 GB Speicher in deinem Computer hast, muss das Betriebssystem einen 4-GB-Bereich im Speicher einrichten, und deine Programme sind auf diesen neuen Bereich beschränkt. Diese Aufgabe wird von den Segmentregistern übernommen und das Betriebssystem hat die genaue Kontrolle darüber. 

---

### Part 12: Instruction Pointer Register

Das EIP-Register (Instruction Point Register) ist das wichtigste Register, mit dem du beim Reverse Engineering zu tun haben wirst. Das EIP behält den Überblick über den nächsten auszuführenden Befehlscode. EIP zeigt auf den nächsten auszuführenden Befehl. Wenn due diesen Zeiger änderst, kannst du zu einem anderen Berich des Codes springen und das Programm so steuern.

Jetzt gehen wir einen Schritt weiter und tauchen in den Code ein. Hier ist ein Beispiel für eine einfach "Hello World" Anwendung in C, auf die wir später in unserem Guide noch genauer eingehen werden. Für unseren heutigen Zweck schauen wir uns die Assemblersprache an, insbesondere das EIP-Register. Wir sehen uns an was wir tun können, um die Programmkontrolle komplett zu hacken.
![eipExample](/img/eipExample.png)

Mach dir keine Sorgen, wenn du nicht verstehst, was es tut oder wie es funktioniert. Was du hier beachten solltest, ist die Tatsache, dass wir eine Funktion namens `unreachableFunction` haben, die niemals von der Hauptfunktion aufgerufen wird. Wie du sehen wirst. können wir dieses Programm hacken, um den Code auszuführen, wenn wir das EIP-Register kontrollieren können!
![eipExampleTerminal](/img/eipExampleTerminal.png)

Wir haben den Code einfach kompiliert, damit er mit dem IA32-Befehlssatz funktioniert, und ihn ausgeführt. Wir du siehst, gibt es keinen Aufruf der `unreachableFunction`, da sie unter normalen Bedingungen unerreichbar ist, wie du an dem gezeigten `Hello World` bei der Ausführung sehen kannst.

![debug eip example](/img/debugEipExample.png)

Wir haben das Programm mit dem GDB-Debugger disassembliert. Wir haben einen Breakpoint bei der `main-Function` gesetzt und das program ausgeführt. Das **=>** zeigt, wohin EIP zeigt, wenn wir zur nächsten Anweisung gehen. Wenn wir dem normalen Programmablauf folgen, wird `Hello World` auf der Konsole ausgegeben und das Programm beendet.
![eipExampleTerminal2](/img/eipExampleTerminal2.png)

Wenn wir das Programm noch mal laufen lassen und checken, wohin EIP zeigt sehen wir:
![eipExampleTerminal3](/img/eipExampleTerminal3.png)

Wir sehen, dass EIP auf main+25 oder die Adresse 0x8d0cec83.

Schauen wir uns mal die `unreachableFunction` an und notieren uns, wo sie im Speicher beginnt.
![eipExampleTerminal4](/img/eipExampleTerminal4.png)

Als nächstes setzen wir EIP auf die Adresse 0x5655619d, damit wir den Programmfluss "entführen" und die `unreachableFunction` ausführen können.
![eipExampleTerminal5](/img/eipExampleTerminal5.png)

Jetzt, wo wir die Kontrolle über EIP haben, können wir uns ansehen, wie wir den Ablauf eines laufenden Programms zu unserem Vorteil manipuliert haben!
![eipExampleTerminal6](/img/eipExampleTerminal6.png)
Wir haben das Programm gehackt!

Die Frage, die sich dir stellt, ist: Warum hast du mir das gezeigt, wenn ich keine Ahnung habe, was das alles ist? Es ist wichtig zu verstehen, dass wir bei einem so langen Guide wie diesem manchmal nach vorne schauen sollten, um zu sehen, warum wir so viele Schritte machen, um die Grundlagen zu lernen, bevor wir in die Materie eintauchen. Aber eines ist sicher: Deine Mühe lohnt sich, wenn du bei diesem Guide bleibst. Wir lernen nämlich, wie wir jedes laufende Programm hacken können, um zu tun, was wir wollen, und wie wir ein bösartiges Programm proaktiv zerlegen können, damit wir es nicht nur deaktivieren, sondern auch zu einer möglichen IP-Adresse zurückverfolgen können, von der der Hack ausgeht.

---

### Part 13: Steuerregister

Es gibt fünf Steuerregister (control registers), mit denen der Betriebsmodus der CPU und die Eigenschaften der aktuell ausgeführten Aufgabe bestimmt werden. Jedes Steuerregister ist wie folgt aufgebaut:

**CR0:** Enthält Flags, die den Betriebsmodus des Prozessors, und verschiedene Prozessorzustände steuern. Dies schließt die Aktivierung des Schutzmodus, die numerische Koprozessorsteuerung und andere systemweite Einstellungen ein.

**CR1:** (Nicht implementiert und wird in der x86-Architektur nicht verwendet.)

**CR2:** Wird zur Speicherung der Seitenfehlerinformationen (Page Fault) verwendet. Wenn eine Speicherzugriffsverletzung auftritt, speichert CR2 die fehlerhafte Speicheradresse.

**CR3:** Enthält die Basisadresse des Seitenverzeichnisses, das für die Speicherverwaltung im Seitenmodus (Paging) verwendet wird. Dies ist besonders wichtig für die virtuelle Speicherverwaltung.

**CR4:** Enthält Flags, die bestimmte Prozessoreigenschaften aktivieren oder anzeigen. Dazu gehören Erweiterungen wie das Page Size Extension (PSE), Physical Address Extension (PAE), und weitere erweiterte Funktionen des Prozessors.

Auf die Werte in den einzelnen Steuerregistern kann nicht direkt zugegriffen werden, aber die Daten im Steuerregister können in eines der Allzweckregister verschoben werden. Sobald sich die Daten in einem Allzweckregister befinden, kann ein Programm die Bit-Flags im Register untersuchen, um den Betriebsstatus des Prozessors in Verbindung mit der aktuell laufenden Aufgabe zu ermitteln.

Wenn der Wert eines Kontrollregisters geändert werden muss, kann die Änderung an den Daten im GP-Register (General-Purpose Register) vorgenommen und das Register in das CR-Register (Control Registers) verschoben werden. Low-Level-Systemprogrammierer ändern normalerweise die Werte in den Kontrollregistern. Normale Anwendungsprogramme ändern normalerweise keine Einträge in den Steuerregistern, jedoch könneten sie die Flag-Werte abfragen, um die Fähigkeiten des Host-Prozessorchips zu bestimmen, auf dem das Programm gerade läuft.

---

### Part 14: Flags

Das Thema der Flags ist eines der extrem komplexen und komplizierten Konzepte der Assemblersprache und der Programmablaufsteuerung beim Reverse Engineering. Diese Informationen werden viel klarer, wenn wir in die letzte Phase unseres Guides eintreten und C-Anwendungn in Assemblersprache zurückentwickeln.

Wichtig ist hier zu verstehen, dass Flags helfen, die Programmausführung zu steuern, zu überprüfen und zu verifizieren. Sie sind ein Mechanismus, um festzustellen, ob jede vom Prozessor durchgeführte Operation erfolgreich ist oder nicht.

Flags sind entscheidend für Anwendungen in der Assemblersprache, da sie eine Überprüfung ermöglichen, ob jede Funtkion des Programms erfolgreich ausgeführt wurde.

Wir beschäftigen uns mit 32-Bit-Assemblercode, bei dem ein einziges 32-Bit-Register eine Gruppe von Status-, Kontroll- und System-Flags enthält. Dieses Register wird als EFLAGS-Register bezeichnet, da es 32 Bit an Informationen enthält, die spezifischen Flags zugeordnet sind.

Es gibt drei Arten von Flags: Status-Flags, Kontroll-Flags und System-Flags.

#### Status-Flags sind wie folgt:

**CF:** Carry Flag
**PF:** Parity Flag
**AF:** Adjust Flag
**ZF:** Zero Flag
**SF:** Sign Flag
**OF:** Overflow Flag

Die **Carry-Flag** wird gesetzt, wenn eine mathematische Operation an einem vorzeichenlosen Integer-Wert einen Übertrag oder ein Leihen für das höchstwertige Bit erzeugt. Dies ist eine Überlaufbedingung für das an der mathematischen Operation beteiligte Register. Wenn dies geschieht, sind die verbleibenden Daten im Register nicht das korrekte Ergebnis der mathematischen Operation.

Die **Parity-Flag** wird verwendet, um korrupten Daten als Ergebnis einer mathematischen Operation in einem Register anzuzeigen. Wenn das Parity Flag überprüft wird, ist es gesetzt, wenn die Gesamtanzahl der 1-Bits im Ergebnis gerade ist, und wird gelöscht, wenn die Gesamtanzahl der 1-Bits im Ergebnis ungerade ist. Wenn die Parity Flag überprüft wird, kann eine Anwendung feststellen, ob das Register seit der Operation beschädigt wurde.

Die **Adjust-Flag** wird in binär codierten Dezimalarithmetik-Operationen verwendet und wird gesetzt, wenn ein Übertrag oder Leihen von Bit 3 des für die Berechnung verwendeten Registers erfolgt.

Die **Zero Flag** wird gesetzt, wenn das Ergebnis einer Operation null ist.

Die **Sign Flag** wird auf das höchstwertige Bit des Ergebnisses gesetzt, dass das Vorzeichenbit ist und anzeigt ob das Ergebnis Positiv oder negativ ist.

Die **Overflow Flag** wird bei vorzeichenbehafteter Ganzzahlarithmetik verwendet, wenn ein positiver Wert zu groß oder ein negativer Wert zu klein ist, um im Register dargestellt zu werden.

#### Kontroll-Flags

Kontroll-Flags werden verwendet, um spezifisches Verhalten im Prozessor zu steuern. Die DF-Flag, die die Richtungs-Flag ist, wird verwendet, um zu steuern, wie Zeichenketten vom Prozessor behandelt werden. Wenn es gesetzt ist, dekrementieren Zeichenkettenoperationen automatisch die Speicheradressen, um das nächste Byte in der Zeichenkette zu erhalten. Wenn es gelöscht ist, inkrementieren Zeichenkettenoperationen automatisch die Speicheradressen, um das nächste Byte in der Zeichenkette zu erhalten. 

#### System-Flags

System-Flags werden verwendet, um Betriebssystemebene-Operationen zu steuern, die NIEMALS von einem jeweiligen Programm oder eine Anwendung modifiziert werden sollten.

**TF:** Trap Flag
**IF:** Interrupt Enable Flag
**IOPL:** I/O Privilege Level Flag
**NT:** Nested Task Flag
**RF:** Resume Flag
**VM:** Virtual-8086 Mode Flag
**AC:** Alignment Check Flag
**VIF:** Virtual Interrupt Flag
**VIP:** Virtual Interrupt Pending Flag
**ID:** Identification Flag

Die **Trap Flag** wird gesetzt, um den Einzelschrittmodus zu aktivieren. In diesem modus führt der Prozessor jeweils nur einen einzigen befehl aus und wartet auf ein Signal, um den nächsten Befehl auszuführen. Dies ist beim Debuggen unerlässlich.

Die **Interrupt Enable Flag** steuert, wie der Prozessor auf Signale von externen Quellen reagiert.

Das **I/O Privilege Field** zeigt das Ein-/Ausgabe-Berechtigungsniveau der aktuell ausgeführten Aufgabe an und definiert die Zugriffsebenen für den Ein-/Ausgabe-Adressraum, die kleiner oder gleich dem Zugriffslevel sein müssen, das erforderlich ist, um auf den jeweiligen Adressraum zuzugreifen. Wenn es nicht kleiner oder gleich dem erforderlichen Zugriffslevel ist, wird jeder Versuch, auf den Adressraum zuzugreifen, verweigert.

Die **Nested Task Flag** steuert, ob die aktuell laufende Aufgabe mit der zuvor ausgeführten Aufgabe verknüpft ist und wird zur Verkettung von unterbrochenen und aufgerufenen Aufgaben verwendet.

Die **Resume Flag** steuert, wie der Prozessor auf Ausnahmen im Debugging-Modus reagiert.

Die **VM-Flag** zeigt an, dass der Prozessor im Virtual-8086-Modus arbeitet, anstatt im Protected- oder Real-Modus.

Die **Alignment Check Flag** wird zusammen mit dem AM-Bit im CR0-Steuerregister verwendet, um die Ausrichtungsüberprüfung von Speicherreferenzen zu aktivieren.

Die **Virtual Interrupt Flag** repliziert die IF-Flag, wenn der Prozessor im virtuellen Modus arbeitet.

Die **Virtual Interrupt Pending Flag** wird verwendet, wenn der Prozessor im virtuellen Modus arbeitet, um anzuzeigen, dass ein Interrupt aussteht.

Die **ID-Flag** zeigt an, ob der Prozessor die CPUID-Anweisung unterstützt.

--- 

### Part 15: Stack

Funktionen sind das grundlegendste Merkmal in der Softwareentwicklung. Eine Funktion ermöglicht es dir, Code auf logische Weise zu organisieren, um eine bestimmte Aufgabe auszuführen. Es ist nicht entscheiden, dass du jetzt schon verstehst, wie Funktionen funktionieren, sondern nur, dass du verstehst, dass wir beim Lernen der Entwicklung versuchen wollen, Code-Duplizierung zu minimieren, indem wir Funktionen verwenden, die mehrfach aufgerufen werden können, anstatt doppelten Code zu haben, der übermäßigen Speicherplatz beansprucht.

Wenn ein Programm startet, wird ein bestimmter zusammenhängender Speicherbereich für das Programm reserviert, der als Stack bezeichnet wird.

Der Stack-Pointer ist ein Register, das die Spitze des Stacks enthält. Der Stack-Pointer enthält die kleinste Adresse, sagen wir zum Beispiel `0x00001000`, so dass jede Adresse kleiner als `0x00001000` als Müll betrachtet wird und jede Adresse größer als `0x00001000` als gültig angesehen wird.

Die obige Adresse ist zufällig und nicht absolut, wo du den Stack-Pointer von Programm zu Programm findest, da sie variieren wird. Schauen wir uns an, wie der Stack aus einer abstrakten Perspektive aussieht.
![stackImage](/img/stackImage.png)

Das obige Diagramm ist das, was du klar im Kopf behalten solltest, da das tatsächlich im Speicher passiert. Die nächsten Diagramme werden das Gegenteil von dem zeigen, was oben gezeigt ist.

Du wirst sehen, dass der Stack in den unteren Diagrammen nach oben wächst, in Wirklichkeit wächst er jedoch von höherem zu niedrigerem Speicherplatz nach unten.

Im folgenden addMe-Beispiel zeigt der Stack-Pointer (ESP), wenn er im Speicher bei einem Breakpoint in der main-Funktion untersucht wird, 0xffffd050. Wenn das Programm die addMe-Funktion von main aufruft, ist ESP nun 0xffffd030, was im Speicher **NIEDRIGER** ist. Daher wächst der Stack nach **UNTEN**, obwohl das Diagramm zeigt, dass er nach oben zeigt. Denk daran, wenn die Pfeile unten nach oben zeigen, zeigen sie in Wirklichkeit auf niedrigere Speicheradressen.

Das Stack-Ende ist die größte gültige Adresse des Stacks und befindet sich im größeren Adressbereich oder oben im Speichermodell. Das kann verwirrend sein, da das Stack-Ende höher im Speicher liegt. Der Stack wächst im Speicher nach unten und es ist wichtig, dass du das jetzt verstehst, während wir weitermachen.

Die Stack-Grenze ist die kleinste gültige Adresse des Stacks. Wenn der Stack-Pointer kleiner wird als diese, gibt es einen Stack-Overflow, der ein Programm beschädigen und einem Angreifer ermöglichen kann, die Kontrolle über ein System zu übernehmen. Malware versucht, Stack-Overflows auszunutzen. In letzter Zeit gibt es Schutzmaßnahmen in modernen Betriebssystemen, die versuchen, dies zu verhindern.

Es gibt zwei Operationen auf dem Stack, die push und pop genannt werden. Du kannst ein oder mehrere Register pushen, indem du den Stack-Pointer auf einen kleineren Wert setzt. Dies wird normalerweise dadurch erreicht, dass man viermal die Anzahl der Register, die auf den Stack gepusht werden sollen, vom Stack-Pointer abzieht und die Register auf den Stack kopiert.

Du kannst ein oder mehrere Register poppen, indem du die Daten vom Stack in die Register kopierst und dann einen Wert zum Stack-Pointer addierst. Dies wird normalerweise dadruch erreicht, dass man viermal die Anzahl der Register, die vom Stack gepoppt werden sollen, zum Stack-Pointer addiert.

Schauen wir uns an, wie der Stack zur Implementierung von Funktionen verwendet wird. Für jeden Funktionsaufruf gibt es einen Abschnitt des Stacks, der für die Funktion reserviert ist. Dies wird als Stack-Frame bezeichnet.

Schauen wir uns das C-Programm an, das wir in Kapitel 12 erstellt haben, und untersuchen, wie die main-Funktion aussieht:
![eipExample](/img/eipExample.png)

Wir sehen zwei Funktionen. Die erste ist die `unreachableFunction`, die unter normalen Umständen nie ausgeführt wird, und wir sehen auch die `main-Function`, die immer die erste Funktion ist, die auf den Stack aufgerufen wird.

Wenn wir dieses Programm ausführen, sieht der Stack so aus:
![stackImage2](/img/stackImage2.png)

Wir können das Stack-Frame für `int main(void)` oben sehen. Es wird auch als Aktivierungsdatensatz bezeichnet. Ein Stack-Frame existiert, wann immer eine Funktion gestartet, aber noch nicht abgeschlossen ist. Zum Beispiel gibt es innerhalb des Body von `int main(void)` einen Aufruf zu `int addMe(int a, int b)`, der zwei Parameter a und b annimmt. Es muss Assemblercode in `int main(void)` geben, um die Parameter für `int addMe(int a, int b)` auf den Stack zu pushen. Untersuchen wir etwas Code.
![addMe1](/img/addMe.png)

Wenn wir dieses Programm kompilieren und ausführen, sehen wir den Wert 5 wie folgt ausgegeben:
![addMeTerminal](/img/addMeTerminal.png)

Ganz einfach, `int main(void)` ruft zuerst `int addMe(int a, int b)` auf und wird so auf den Stack gelegt:
![stackImage3](/img/stackImage3.png)

Du kannst sehen, dass durch das Platzieren der Argumente auf dem Stack das Stack-Frame für `int main(void)` größer geworden ist. Wir haben auch Platz für den Rückgabewert reserviert, der von `int addMe(int a, int b)` berechnet wird, und wenn die Funktion zurückkehrt, wird der Rückgabewert in `int main(void)` wiederhergestellt und die Ausführung in `int main(void)` fortgesetzt, bis sie abgeschlossen ist.

Sobald wir die Anweisungen für `int addMe(int a, int b)` erhalten haben, benötigt die Funktion möglicherweise lokale Variablen, so dass die Funktion etwas Platz auf dem Stack pushen muss, was so aussehen würde:
![stackImage4](/img/stackImage4.png)

`int addMe(int a, int b)` kann auf die übergebenen Argumente von `int main(void)` zugreifen, weil der Code in `int main(void)` die Argumente genau so platziert, wie `int addMe(int a, int b)` es erwartet.

FP ist der Frame-Pointer und zeigt auf die Stelle, an der sich der Stack-Pointer befand, bevor `int addMe(int a, int b)` den Stack-Pointer für seine eigenen lokalen Variablen verschoben hat.

Die Verwendung eines Frame-Pointers ist wesentlich, wenn eine Funktion den Stack-Pointer mehrmals während des Ablaufs der Funktion verschieben wird. Die Idee ist, den Frame-Pointer für die Dauer des Stack-Frames von `int addMe(int a, int b)` fixiert zu halten. In der Zwischenzeit kann der Stack-Pointer seine Werte ändern.

Wir können den Frame-Pointer verwenden, um die Speicherorte für sowohl Argumente als auch lokale Variablen zu berechnen. Da er sich nicht bewegt, sollten die Berechnungen für diese Speicherorte einen festen Offset vom Frame-Pointer haben.

Sobal es Zeit ist, `int addMe(int a, int b)` zu verlassen, wird der Stack-Pointer auf den Wert des Frame-Pointers gesetzt, was das Stack-Frame von `int addMe(int a, int b)` entfernt.

Zusammengefasst: Der Stack ist ein spezieller Speicherbereich, der temoräre Variablen speichert, die von jeder Funktion einschließlich main erstellt werden. Der Stack ist eine LIFO-Datenstruktur (Last in, First Out), die von der CPU eng verwaltet und optimiert wird. Jedes Mal, wenn eine Funktion beendet wird, werden alle Variablen, die von dieser Funktion auf den Stack gepush wurden, freigegeben oder gelöscht. Sobal eine Stack-Variable freigegeben wird, steht dieser Speicherbereich für andere Stack-Variablen zur Verfügung. 

Der Vorteil des Stacks zur Speicherung von Variablen besteht darin, dass der Speicher für dich verwaltet wird. Du musst keinen Speicher manuell allokieren oder freigeben. Die CPU verwaltet und organisiert den Stack-Speicher sehr effizient und schnell.

Es ist entscheiden, dass du verstehst, dass wenn eine Funktion endet, alle ihre Variablen vom Stack gepoppt und für immer verloren sind. Die Stack-Variablen sind lokal. Der Stack wächst und schrumpft, wenn Funktionen lokale Variablen pushen und poppen.

Dir brummt bestimmt der Schädel. Denk daran, dass diese Themen kompliziert sind und sich in zukünftigen Tutorials weiterentwickeln werden. Wir haben es mit vielen verwirrenden Themen wie Registern, Speicher und jetzt dem Stack zu tun und es kann überwältigend sein, aber nimm dir die Zeit auch Dinge zu recherchieren oder mehrmals zu lesen, falls du etwas nicht verstehst.

--- 

### Part 16: Heap

Unser nächster Schritt im Abschnitt "Grundlagen des Malware-Reverse-Engineering" konzentriert sich auf den Heap. Denke daran, dass der Stack nach unten wächst und der Heap nach oben wächst. Es ist sehr, sehr wichtig, dass du dieses Konzept verstehst, während wir voranschreiten.
![heapImage](/img/heapImage.png)

Der Heap ist der Bereich des Speichers deines Computers, der nicht automatisch für dich verwaltet wird und nicht so streng von der CPU verwaltet wird. Es ist ein frei schwebender Speicherbereich und ist größer als die Zuweisung von Speicher für den Stack.

Um Speicher auf dem Heap zuzuweisen, musst du `malloc()` oder `calloc()` verwenden, die eingebaute C-Funktionen sind. Sobald du Speicher auf dem Heap zugewiesen hast, bist du dafür veranwortlich, diesen Speicher mit `free()` wieder freizugeben, wenn du ihn nicht mehr benötigst.

Wenn du diesen Schritt nicht durchführst, wird dein Programm das haben, was als Speicherleck (memory leak) bekannt ist. Das bedeutet, dass Speicher auf dem Heap weiterhin reserviert bleibt und nicht anderen Prozessen zur Verfügung steht, die ihn benötigen.

Im Gegensatz zum Stack hat der Heap keine Größenbeschränkung für Variablen. Das einzige, was den Heap begrenzen würde, sind die physischen Einschränkungen deines Computers. Der Speicher auf dem Heap ist etwas langsamer zu lesen und zu schreiben, weil du Pointer verwenden musst, um auf den Speicher im Heap zuzugreifen. Wenn wir in unsere C-Tutorial eintauchen, werde ich dies demonstrieren.

Im Gegensatz zum Stack sind Variablen, die auf dem Heap erstellt werden, von jeder Funktion und überall in deinem Programm zugänglich. Heap-Variablen haben grundsätzlich einen globalen Geltungsbereich.

Wenn du einen großen Block Speicher für etwas wie eine Struktur oder ein großes Array zuweisen musst und du diese Variable für eine längere Dauer des Programms, auf das global zugegriffen werden muss, behalten möchtest, dann solltest du den Heap für diesen Zweck wählen. Wenn du Variablen wie Arrays und Strukturen benötigst, dann wirst du sie wahrscheinlich auf dem Heap zuweisen müssen und dynamische Speicherverwaltungsfunktionen wie `malloc(), calloc(), realloc()` und `free()` verwenden, um diesen Speicher manuell zu verwalten.

Der nächste Schritt ist das Programmieren von C in der Linux-Umgebung, wo wir Schritt für Schritt jedes C-Programm disassemblieren. So lernst du sowohl C-Porgrammierung als auch Assembler.

---

### Part 17: Linux installieren

---

### Part 18: vim Text Editor

Jetzt, da wir eine funktionierende Version von Linux haben, brauchen wir einen Texteditor, mit dem wir im Terminal arbeiten können.

Um zu beginnen, öffne dein Termina und tippe:

```
cd ~
vi .vimrc
```

Dies öffnet den vi-Texteditor. Das erste, was du tippen musst, ist der Buchstabe **i**, um den Editor in den Eingabemodus zu versetzen, damit du mit dem Tippen beginnen kannst.

Gebe nun folgendes ein:
```
synax on
set number
set smartident
set tabstop=4
set shiftwidth=4
set expandtab
```

Nachdem du fertig getippt hast, drücke die **ESC**-Taste und tipp **:wq** und drücke Enter.

Herzlichen Glückwunsch! Du hast deine erste Datei erstellt! Dies ist eine einmalige Datei, die wir erstellen müssen, um unseren Texteditor so zu konfigurieren, wie wir ihn haben möchten.

Die erste Zeile lautet `syntax on`, was einfach nur dafür sorgt, dass der Syntax farbig dargestellt wird. Die zweite Zeile `set number` bedeutet, dass wir möchten dass jede Datei Zeilennummern anzeigt, da dies für das Debuggen von Code unerlässlich ist. Die Anweisungen `set smartident, set tabstop, set shiftwidth` und `set expandtab` legen Regeln für die ordnungsgemäße Formatierung des Codes fest und ermöglichen 4 Leerzeichen pro Tab-Einrückung, worduch unser Code sauber aussieht.

Es gibt meherer Befehle, die du kennen musst. Denke daran, dass du, um in den Befehlsmodus zu wechseln, die **ESC**-Taste drücken musst. Unten sind die häufigsten Befehle:

+ **j** oder Pfeil-Unten [Bewegt den Cursort eine Zeile nach unten]
+ **k** oder Pfeil-Oben [Bewegt den Cursor eine Zeile nach oben]
+ **h** oder Pfeil-Links [Bewegt den Curser ein Zeichen nach links]
+ **l** oder Pfeil-Rechst [Bewegt den Cursor ein Zeichen nach rechts]
+ **0** [Bewegt den Cursor zum Anfang der aktuellen Zeile]
+ **$** [Bewegt den Cursor zum Ende der aktuellen Zeile]
+ **b** [Bewegt den Cursor zum Anfang des vorhergehenden Wortes]
+ **dd** [Löscht die Zeile, in der sich der Cursor befindet]
+ **D** [Löscht vom Cursor bis zum Ende der Zeile]
+ **yy** [Kopiert die aktuelle Zeile]
+ **p** [Fügt den kopierten Text nach dem Curor ein]
+ **u** [Macht die letzte Änderung an der Datei rückgängig]
+ **:w** [Speichert die Datei]
+ **:wq** [Speichert die Datei und beendet den Texteditor]
+ **:q!** [Beendet den Texteditor ohne Änderungen zu speichern]

Du wirst ständig zwischen dem Befehlsmodus **ESC** und dem Eingabemodus **i** wechseln. Denke daran, dass du im Eingabemodus sein musst, wenn du Zeichen einfügen möchtest, und im Befehlsmodus, wenn du den Cursor bewegen möchtest, außer du bewegst dich zur nächsten Zeile.

Jetzt, da wir vi konfiguriert haben, lass uns vim installieren, das einige bessere Funktionen hat. Tippe einfach:
`sudo apt-get install vim`
Sobald das installiert ist, verwenden wir anstelle von vi nun vim.

---

### Part 19: Warum Assembly?

Warum sollte man die Assemblersprache lernen? Es gibt viele Programmiersprachen die mir sofort einen Job verschaffen, also warum sollte ich meine Zeit damit verschwenden, diese archaische Assembly-Sprache zu lernen?

Das stimmt und es ist auch nichts falsch daran, Python, C++, Java oder sonstige Programmiersprachen zu lernen. Allerdings ist die Bedrohung durch Cybersecurity das größte Problem, das die Gesellschaft heutzutage.

Die meiste Malware wird in höheren Programmiersprachen geschrieben, aber die meisten Malware-Autoren geben den Angreifern nicht ihren Quellcode, damit sie ihren Angriff richtig analysieren können.

Wenn wir Malware untersuchen, bekommen wir meistens nur ein kompiliertes Binary. Das Einzige, was wir mit einem kompilierten Binary machen können, ist, es Befehl für Befehl in der Assemblersprache zu zerlegen, denn **ALLES** läuft letzendlich auf die Assemblersprache hinaus.

Das Verstehen der Assemblersprache ermöglich es einem, einen Debugger auf einem laufenden Prozess zu öffnen. Jedes laufende Programm hate eine PID, die einen numerischen Wert darstellt und ein laufendes Programm bezeichnet. Wenn wir einen laufenden Prozess oder ein beliebiges Stück Malware mit einem professionellen oder Open-Source-Tool wie GDB öffnen, können wir **GENAU** sehen, was vor sich geht, und dann den EIP-Befehlspointer ergreifen, um dorthin zu gelangen, wo wir hin müssen, um die **KOMPLETTE** Kontrolle über den Programmablauf zu haben.

Die meiste Malware wird, wie ich bereits erwähnt habe, in einer Hochsprache geschrieben und nach dem Kompilieren kann sie von der Hardware oder dem Betriebssystem gelesen werden, da sie nicht menschenlesbar ist. Damit man dies verstehen kann, muss man lernen, Assembly zu lesen, zu schreiben und richtig zu debuggen.

Die Assemblersprache ist niedrigstufig und hat viel mehr Anweisungen, als man in einer höher-leveligen Anwendung sehen würde.

Die vorherigen 18 Lektionen dieses Guides haben dir die Grundlagen der x86-Hardware vermittelt. Wie ich in frühren Parts bereits erwähnt habe, werden wir uns auf das Debuggen von 32-Bit-Assembly konzentrieren, da die meiste Malware versucht, so viele Systeme wie möglich zu beeinträchtigen, und obwohl es 64-Bit-Malware gibt, ist 32-Bit-Malware erheblich destruktiver und gefährlicher und wird der Fokus dieses Guides sein.

---

### Part 20: Instruction Code Handling

Eine CPU liest Instruction Codes, die im Speicher gespeichert sind, da jeder Codesatz ein oder mehrere Bytes an Informationen enthalten kann, die den Prozessor anleiten, eine sehr spezifische Aufgabe auszuführen. Während jeder Instruction Code aus dem Speicher gelesen wird, werden auch alle Daten, die für den Instruction Code benötigt werden, in den Speicher gespeichert und gelesen.

Denke daran, dass der Speicher, der Instruction Code enthält, sich nicht von den Bytes unterscheidet, die die vom Prozessor verwendeten Daten enthalten, und spezielle Zeige helfen der CPU, den Überblick darüber zu behalten, wo im Speicher die Daten und wo die Instruction Codes gespeichert sind.

Ein Daten-Pointer hilft der CPU, den Anfang des Datenbereichs im Speicher zu verfolgen, was der Stack ist. Wenn neue Datenelemente in den Stack gelesen werden, bewegt sich der Stack-Pointer nach oben im Speicher. Falls du dieses Konzept nicht verstehst, schaue dir noch einmal [Part 15: Stack](#part-15-stack) an.

Der Instruction-Pointer wird verwendet, um der CPU zu helfen, den Überblick darüber zu behalten, welche Instruction Codes bereits verarbeitet wurden und welche als nächstes verarbeitet werden sollen. Wenn du dieses Konzept nicht verstehst, schaue dir noch einmal [Part 12: Instruction Pointer Register](#part-12-instruction-pointer-register) an.

Jeder Instruction Code muss ein Opcode enthalten, der die grundlegende Funktion oder Aufgabe definiert, die von der CPU ausgeführt werden soll. Opcodes sind zwischen 1 und 3 Bytes lang und definieren eindeutig die auszuführende Funktion.

Lass uns nun ein einfaches C-Programm namens test.c untersuchen, um anzufangen.
![test.c](/img/testC.png)

Alles, was wir tun, ist eine main-Funktion vom Typ Integer zu erstellen, die einen void-Parameter hat und 0 zurückgibt. Dieses Programm beendet einfach das Betriebssystem.

Lass uns dieses Programm kompilieren und ausführen:
![testCTerminal](/img/testCTerminal.png)

Lass uns das objdump-Tool verwenden, um die main-Funktion darin zu finden:
![testCTerminal2](/img/testCTerminal2.png)

Hier ist ein Ausschnitt der Ergebnisse, die du durch das Ausführen des obigen Befehl erhalten würdest. Hier sind die Inhalte der main-Funktion. Denk daran, dass das Folgende in Intel-Syntax ist, wie wir bereits besprochen haben.
![testCTerminal3](/img/testCTerminal3.png)

Ganz links haben wir die entsprechenden Speicheradressen. In der Mitte haben wir die Opcodes und schließlich ganz recht die entsprechende Assemblersprache in Intel-Syntax.

Um es einfach zu halten, lass uns die Speicheradresse `0000118a` (im Screenshot steht diese einfach nur als 118a, da die Nullen vor der Zahl wegfallen können) untersuchen, wo wir die Opcodes `b8 00 00 00 00` sehen. Wir können sehen, dass der Opcode `b8` der Anweisung `mov eax, 0x0` auf der rechten Seite entspricht. Die nächste Serie von `00 00 00 00` repräsentiert 4 Bytes des Wertes 0. Wir sehen `mov eax, 0x0`, daher wird der Wert 0 in `eax` bewegt, was den obigen Code repräsentiert. Denke daran, dass die IA-32-Plattform das verwendet, was wir Little-Endian-Notation nennen, was bedeutet, dass die niederwertigen Bytes zuerst erscheinen, wenn man von rechts nach links liest.

Ich möchte sicherstellen, dass du das richtig im Kopf hast, also lass uns so tun, als wäre der obige Wert:

**`mov eax, 0x1`**

In diesem Szenario wäre der entsprechende Opcode:

**`b8 01 00 00 00`**

Wenn du verwirrt bist, ist dass vollkommen in Ordnung. Erinnerst du dich an Little-Endian? Denke daran, dass `eax` 32 Bit breit ist, was 4 Bytes sind (8 Bits = 1 Byte). Die Werte werden in umgekehrter Reihenfolge aufgelistet, daher sehen wir die obige Darstellung. 

---

### Part 21: Programme compilen

Schauen wir uns noch einmal das C-Programm der letzten Lektion an und betrachten genauer, wie wir diesen Quellcode in eine ausführbare Datei umwandeln.
![test.c](/img/testC.png)

Um dieses Programm in C zu kompilieren, tippen wir einfach:
![compile](/img/compile1.png)

Dieser einzelne Schritt erstellt dann **`test.o`**, die binäre Objektdate, und **`test`**, die binäre ausführbare Datei.

Wenn wir diesen C-Quellcode in Assembly umwandeln möchten, müssen wir den GNU-Compiler auf folgende Weise verwenden. Lass uns mit dem folgenden Befehl im Terminal beginnen:
![compile2](/img/compile2.png)

Beginnen wir mit dem **`-S`** Switch. Der **`-S`** Switch erstellt vergleichbaren Assembly-Quellcode in der AT&T-Syntax. Der **`-m32`** Switch erstellt eine 32-Bit-Ausführungsdatei und der **`-O0`** Switch sagt dem Compiler, wie viel Optimierung beim Kompilieren des Binaries verwendet werden soll. Das ist das große O und die Ziffer 0. Die Ziffer 0 in diesem Fall bedeutet keine Optimierung, was bedeutet, dass es der am besten menschenlesbare Befehlssatz ist. Wenn due eine 1,2 oder 3 einsetzt, nimmt die Menge der Optimierung zu, je höher die Werte sind.

![testCAssembly](/img/testCAssembly.png)
Dieser Schritt oben erstellt **`test.s`**, was der äquivalente Assembly-Quellcode ist, wie wir oben erwähnt haben.

Dann müssen wir den Assembly-Quellcode in eine binäre Objektdatei kompilieren, die eine **`test.o`** Datei erzeugt.
![testCAssembly2](/img/testCAssembly2.png)

Schließlich müssen wir einen Linker verwenden, um den tatsächlichen binären ausführbaren Code aus der binären Objektdatei zu erstellen, was eine ausführbare Datei namens **`test`** erzeugt.
![testCAssembly3](/img/testCAssembly3.png)

Letztes mal, als wir die ausführbare Datei **`test`** in einem Programm namens objdump untersucht haben und den Main-bereich betrachteten, sahen wir Folgendes, nur dass wir diesmal die AT&T-Assembly-Syntax verwenden werden:
![testCAssembly4](/img/testCAssembly4.png)

Dieser Befehl oben erzeugt die folgende Ausgabe:
![testCAssembly5](/img/testCAssembly5.png)

Lass uns den Code im Debugger untersuchen. Lass uns GDB starten, den GNU-Debugger, und zuerst den Quellcode auflisten, indem wir **`l`** tippen, dann einen Breakpoint auf **`main`** setzen und das Programm ausführen. Schließlich disassemblieren wir und überprüfen die Ausgabe unten: