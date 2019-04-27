# genericTCP
#### (iSpindle.py Version 1.0.0)

[Install InstructEnglish Versions](INSTALL.md)      README_en.md)

**Neu: Diagramme, siehe:**
[Charts Readme](web/README.md)

NOTE: If you are updating from**Installations Anleitung für Raspbian:**
[INSTALL.md](INSTALL.md)

**Achtung: Update auf Firmware < 5.x please see [Install Instructions](INSTALL.md).

This script was written in Python and its purpose is to accept raw data from an iSpindle via a:**
Bitte die [Installations Anleitung](INSTALL.md) beachten.      

Dieses in Python geschriebene Server Skript dient dazu, von der iSpindel kommende Rohdaten über eine genericsche TCP connection, usually in a local network environmVerbindung zu empfangent.
It purposely avoids unneccessary higher-level pAuf zusätzlichen, unnötigen Overhead durch Protockols such as http, in order to maximize the battery lifetime of the iSpindle and generally make things more easy and transparent, also for you fellow developers out there.

The data received can be stored (or forwarded) in three different ways, non exclusively.    
You can enable or disable them separately, one byle wie http wird hierbei bewusst verzichtet, um den Stromverbrauch der iSpindel so gut es geht zu minimieren.
Die empfangenen Daten können als CSV (“Comma Separated Values”, also durch Kommas getrennte Werte) in Textdateien gespeichert (und so zum Beispiel in Excel leicht importiert) werden.
Ebenso ist es möglich, die Daten in einer MySQL Datenbank abzulegen.    
Somit hat man einen lokalen Server, der auch ohne.   
One option is to save incoming data to a CSV (comma separated values) file, one per iSpindle.
This might be useful for example to do a quick import in Excel.   
The second one allows you to Internet Anbindung funktioniert und den Einsatz der iSpindel im Heimnetzwerk ermöglicht.
Die Zugriffszeiten sind kürzer und dadurch sinkt natürlich auch der (ohnehin geringe) Stromverbrauch der iSpindel noch wreite it to a MySQL table.   
And finally, in order to get the best out of two worlds and not have to say goodbye to Ubidots, you can configure the script to foward it all transparently, so Ubidots won't even know it's not connected directly to your iSpindle, with the added advantage of being able to also process your data locally, so, for example, you could come up with some new super nice way to calibrate it.   

In addition, the time your iSpindle has to wait for a connection will decrease, further enhancing its battery life.   
But even without Internet access, you'll be able to process the data your iSpindle sends.

The script is completely platform independent and should run on any OS that supports Python.
It has been tested onr.

Um nicht auf die Anbindung an Ubidots verzichten zu müssen, besteht aber auch die Option, die Daten zusätzlich dorthin weiterzuleiten.    
Das geschieht transparent, ohne dass die iSpindel auf die Verbindung warten muss. Der lokale Server fungiert dann sozusagen als Proxy.

Das Skript ist plattformunabhängig und kann z.B. auf einem Raspberry Pi, eingesetzt werden.
Aber auch der Einsatz auf einem evtl. gemieteten dedizierten (oder virtuellen) Server oder einem beliebigen Rechner im Heimnetz ist möglich.
Der Betrieb mehrerer iSpindeln gleichzeitig funktioniert problemlos und ohne Verzögerungen, da Multithreading implementiert ist.    
Getestet wurde es unter Mac OS X (Sierra) aund Linux (Debian), but it should workes sollte aber auch undter Windows just as well.
If you own or have rented a dedicated or virtual server, or if there is any computer in your home network that is running 24/7, this should work for you.    
A Raspberry Pi will always do the trick.
Just make sure you have Python installed, and if you are using the MySQL feature, don't forget to install the `python-mysql.connector`, too.
Multithreading is implemented, so even if your multiple iSpindles send their data at the same time, there should be no delays or other problems during the transmission.

When configuring your iSpindle, choose **TCP** as protocol, enter thproblemlos laufen.    
Die einzige Voraussetzung ist, dass Python installiert ist.

Für die Anbindung an MySQL muss auch der `python-mysql.connector` installiert sein.
In der Konfiguration der iSpindel wählt man **TCP** aus, und trägt die IP adAdress of the server the script is running on, and enter **9501** as TCP port (which is the default port the script will listen to).

Then, configure the script by opening e des Servers ein, auf dem das Skript läuft.
Als Port wählt man am besten die Voreinstellung **9501**.

Nun muss das Skript selbst unbedingt noch konfiguriert werden.
Dazu öffnet man es mit ein a tem Text eEditor.
Make sure you adjust all the settings according to the d und bearbeitet die gleich beschriptions following below.

Finally, copy it to some path on your server. If using a Raspi, good choices would be `/usr/local/bin` or simply `/home/pi`.

Make it executable by typing `chmod 755 iSpindle.py` on the command line inside the path you've chosen.
Then start it by typing `./iSpindle.py`.
Alternatively (or when using Windows), you can start it by typing `python iSpindle.py`.    
Once it all worksebenen Einstellungen.

Dann wird es in einen beliebigen Pfad auf dem Zielsystem kopiert, `/usr/local/bin` bietet sich an, oder einfach `/home/pi`.
Mit `chmod 755 iSpindle.py` macht man es ausführbar und startet es mit `./iSpindle.py`.
Alternativ (z.B. unter Windows) startet man es mit `python iSpindle.py`.
Wenn alles funktioniert, beendet man das Skript, set zt `DEBUG to 0, restart it in the back= 0` und startet es im Hinterground and enjoy.
neu mit `./iSpindle.py &`.

### CKonfiguration:

#### GeneralAllgemeines:

	DEBUG = 0      
	PORT = 9501    
	HOST = '0.0.0.0'

Wenn **DEBUG** = 1 allows detailed output on the console.
You'll want this when first configuring the script and your iSpindle.
After that, not so much, probably.   
If TCP **Port** is already in use (unlikely), you can change it here to another value.   
**HOST** determines tauf 1 gesetzt wird, werden auf der Konsole detaillierte Informationen ausgegeben.    
Während der ersten Inbetriebnahme ist dies sehr empfehlenswert.    
Falls der TCP **Port** 9501 bereits belegt ist (was unwahrscheinlich ist), kann man diesen auf einen anderen Wert einstellen.    
**HOST** legt fest, von welchen IP range clients have to be in in order to be allowed to connect. Leave this at 0.0.0.0 for no restrictions.
Port 9501 is usually not reachable from the (potentially hostile) outside unless you are explicitly forwarding it through your router (firewall settings: port forwarding), so, no worries there, usually.
And if you've read this far, you'll probably know what you're doing, anyway... ;)

#### CSV:

	CSV = 0
	OUTPATH = '/home/pi/iSpindel'
	DELIMITER = ';'
	NEWLINE = '\r\n'
	DATETIME = 1    

Set **CSV** to 1 if you want CSV files to be generated.    
**OUTPATH** configures the path CSV files will be stored at. Share it in your local network to allow easy import for Excel or whatever frontend you want to use.    
**DELIMITER** sets the character to be used to separate the data fields. ';' is usually good for Excel. Common choices would be ',' or ';'.    
**NEWLINE** is normally '\n', but if you're using anything made by Microsoft, uAdressen aus der Server erreichbar sein soll.    
Am besten lässt man das auf der Voreinstellung, also keine Einschränkungen.    
Von außerhalb des eigenen Netzwerks ist Port 9501 normalerweise sowieso nicht zu erreichen, es sei denn man konfiguriert den Router entsprechend (Port Forwarding).

#### CSV:

	CSV = 0
	OUTPATH = '/home/pi/iSpindel'
	DELIMITER = ';'
	NEWLINE = '\r\n'
	DATETIME = 1    

Um die Ausgabe von CSV Dateien einzuschalten, muss **CSV** = 1 gesetzt werden.   
**OUTPATH** ist der Pfad, unter dem die CSV Datei gespeichert wird. Pro Spindel wird hierbei jeweils eine eigene Datei angelegt.
Wenn OUTPATH im Netzwerk freigegeben ist kann man sehr leicht eine solche Datei z.B. in Excel importieren.   
**DELIMITER** legt das Zeichen fest, durch welches die einzelnen Datenfelder separiert sind. Üblicherweise wählt man hier Komma oder Semikolon.   
**NEWLINE** definiert das Zeilenende. Ist die CSV Datei für Windows oder Office für Mac bestimmt, gilt die Voreinstellung.   
Für UNIX Systeme (Linux, Mac OS X) wählt man besser '\n\r'.    
**DATETIME** should be left at its default setting of 1, unless for some reason you don't want timestamps being added to the data output.
legt fest, ob das aktuelle Datum und Uhrzeit mit in die CSV Datei geschrieben werden sollen. Normalerweise dürfte das der Fall sein.

#### MySQL

	SQL = 1
	SQL_HOST = '127.0.0.1'
	SQL_DB = 'iSpindle'
	SQL_TABLE = 'Data'
	SQL_USER = 'iSpindle'
	SQL_PASSWORD = 'xxxxxxxx'


If you want to switch off MySQL connectivity, set **SQL** to 0 and all other settings will be ignored.     
**SQL\_HOST** defines the DB host's IP address. Usually, this will be 'Will man auf MySQL verzichten, setzt man **SQL** = 0.    
**SQL\_HOST** definiert die IP Adresse der Datenbank. Normalerweise ist das derselbe Rechner auf dem auch das Skript läuft; dies ist die Voreinstellung (“localhost'” oder 127.0.0.1).    
ThDie remaining fistlichen Feldser define the connection to the databasieren den Zugang zur Datenbanktabelle.    
By dDie Default, we assume the database name and user ID are 'iSpindle', and the table name is 'Data'.

In order to create a table inside your MySQL database accordingly, use thi Einstellung geht davon aus, dass die Datenbank “iSpindle”; heißt, ebenso die ID des Datenbank Users.    
Die Tabelle, in welcher die Daten landen, heißt “Data”.    
Um die Tabelle mit den grundlegenden Feldern anzulegen, verwendet man am besten dieses SQL sStatement:

	CREATE TABLE 'Data' (
		'Timestamp' datetime NOT NULL,
		'Name' varchar(64) COLLATE ascii_bin NOT NULL,
		'ID' varchar(64) COLLATE ascii_bin NOT NULL,
		'Angle' double NOT NULL,
		'Temperature' double NOT NULL,
		'Battery' double NOT NULL,
		'ResetFlag' boolean,
		'Gravity' double NOT NULL,
		PRIMARY KEY ('Timestamp', 'Name', 'ID')
	) 
	ENGINE=InnoDB DEFAULT CHARSET=ascii 
	COLLATE=ascii_bin COMMENT='iSpindle Data';

Of course you could always just log in to the database using your default admin account, butAls Datenbankbenutzer kann man natürlich das vordefinierte Admin Konto verwenden, oder a better solution is to create a dedicated user for the server scriptr (schöner) man legt einen Benutzer an, mit Zugriff auf diese Datenbank:

	CREATE USER 'iSpindle'@'localhost' IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON iSpindle . * TO 'iSpindle'@'localhost';
	FLUSH PRIVILEGES;

The script is able to create additional table columns dynamically from the received JSON dataset.    
This is, however, only recommended if you are developing your own firmware and wish to store some variables not being exported by default.
If youWeitere Felder können vom Skript dynamisch angelegt werden; dies ist aber nur zu empfehlen, wenn man eine eigene Firmware testen will, die zusätzliche Variablen ausgibt.    
Hierzu wird **ENABLE\_ADDCOLS** = 1 gesetzt. Für den Normalbetrieb ist egal, ob hier 0 oder 1 eingestellt ist.    
Falls der sServer is reachable from the Internet, make sure **ENABLE\_ADDCOLS** is 0.
In its current version I cannot guarantee the script is not vulnerable to SQL Injection attacks when this is enabled (set to 1).
If unsure, set it to 0nach außen offen ist (z.B. extern gehostet), empfehle ich aber (in dieser Version des Skripts) ENABLE\_ADDCOLS auf 0 zu setzen um eventuelle SQL Injections zu verhindern(!).


#### Ubidots ForwardiAnbindung

	UBIDOTS = 1
	UBI_TOKEN = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

**UBIDOTS** = 0 will switch off Ubidots Forwardingschaltet die Ubidots Weiterleitung aus.    
In **UBI\_TOKEN** is where youdas eigene Token einter your personal Ubidots Token (see iSpindle docs).

Your data should nowragen (siehe Dokumentation der iSpindel) und das war's auch schon.

Die Daten sollten nun show up in Ubidots as usual, plus you have it available locally to fiddle around with.    
Ubidots will not know the difference and even create new devices just as well.

Have Fun!    
ohl wie gewohnt in Ubidots erscheinen als auch auf Eurem lokalen Server.
Auch neue iSpindeln (Devices) lassen sich so problemlos anlegen, für Ubidots macht es keinen Unterschied, ob die Daten von der iSpindel direkt kommen oder vom lokalen Server.

Viel Spaß,     
Euer Tozzi (stephan@sschreiber.de)
<!--stackedit_data:
eyJoaXN0b3J5IjpbODg1ODcxOTUyXX0=
-->