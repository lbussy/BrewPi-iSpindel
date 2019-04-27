# Installation Guide for sanleitung für Raspberry Pi (Raspbian )
### Step-by-Step
chritt-für-Schritt

[English Version](INSTALL_en.md)

### Update toauf Firmware 5.x:
If you already have this running and want to update to the new firmware, you'll need to add a new column to the Data tableVersion 5.x:
Um die neue Firmware einsetzen zu können, bitte das Skript auf die neueste Version aktualisieren.     
Falls die Datenbank schon besteht, muss in die Tabelle "Data" ein neues Feld eingefügt werden:

	USE iSpindle;
	ALTER TABLE Data ADD Gravity double NOT NULL DEFAULT 0;

This is already taken into account if you newlyBei einer Neuinstallation install this and follow the instructions below dies bereits im Folgenden berücksichtigt.

### Update foür Resetflag for charts:
To use the charts with the parameter Resetflag, you'll need to add a new column to thbei Grafiken:
Um bei den Grafiken das Resetflag nutzen zu können, muss, falls die Datenbank schon besteht, in die Tabelle "Data table" ein neues Feld eingefügt werden:

	USE iSpindle;
	ALTER TABLE Data ADD ResetFlag boolean;

This is already taken into account if you newlyBei einer Neuinstallation install this and follow the instructions below.

### Preliminary Remarks:

These recommended software requirements might seem like overkill, but this solution is highly flexible and wide open for future enhancements.    
Most Raspberry owners will use Raspbian, but you should be able to get this to work just fine under any other Linux based distribution.    
Of course you can use slimmer solutions for the database and the web server.    
I am using MySQL and Apache because they're quite standard and there's a lot of potential for future development.    
The whol dies bereits im Folgenden berücksichtigt.

### Vorbemerkung:
Es mag so aussehen, als würde hier viel zu viel Ballast installiert, das geht alles auch schlanker.
Stimmt.
Aber die meisten werden ja Raspbian verwenden, und das ist schon so aufgebläht dass ich da kein schlechtes Gewissen habe, noch Apache, Samba und MySQL mit draufzupacken.
Der Raspi 3 schafft das sowieso problemlos, aber auch die kleineren Modelle sollten klarkommen.
Die ganze iInstall will use roughly 5 GB on the sd card, so a 8 GB one should suffice,ation braucht gute 5GB, also eine SD mit 8 GB sollte reichen, ab 16 GB breing just perfect.
If you don't need phpmyadmin and the visualization charts you can safely omit both mySql and Apache, of courcht der Speicherplatz auf jeden Fall.
Wer auf die Diagramme und auf phpmyadmin verzichten will, also auf die Möglichkeit, die Datenbank mit einer html Oberfläche zu administrieren, kann den Apache2 und phpmyadmin auch weglassen.

### Prepare Raspbian vorbereiten
- raspi-config: activate ssh, establish network connection (via Ethernet or WiFi). You'll probably need a keyboard and an HDMI capable display for this step. Or you create an empty file called "ssh" in the boot directory of the SD card prior to booting the Raspi from it.
- connect viainschalten, Netzwerkverbindung herstellen (hierfür braucht man einmalig eine Tastatur und einen HDMI Bildschirm)             
- Oder: Beim Vorbereiten der SD Karte im /boot Verzeichnis eine leere Datei "ssh" anlegen. Dann kann man sich direkt über einen anderen Rechner im Netzwerk per SSH einloggen und braucht weder Bildschirm noch Tastatur am Raspi.
- Verbinden mit putty (Windows) oder Terminal and SSH (Mac OS X, Linux):
```           
    und ssh:

	ssh pi@[ip-addresse oder hostname] 
   	Passwordt: raspberry (change this)
```
Update your systemändern)

System auf neuesten Stand bringen:

	sudo apt-get update
	sudo apt-get dist-upgrade

### MySQL, Apache2 a Datenbank, Apache2 Webserver und phpMyAdmin dDatabaseenbank GUI 

#### Installieren:

	sudo apt-get install apache2 mysql-server mysql-client php5-mysql python-mysql.connector

When asked, choose a pPasswordt foür the database admin accountDatenbank root Benutzer eingeben.

	sudo apt-get install phpmyadmin

When asked, choose Apache2 als wWeb server, and enter the database root user's password (as above).
MySQL is now managable throughuswählen, Datenbank root Passwort wieder eingeben.
MySQL kann nun über http://[myeinraspi]/phpmyadmin.

You can now complete the following steps either by using erreicht werden.

Die folgenden Schritte lassen sich über phpmyadmin (logging inim Browser erledigen (dazu als root, then click on the "SQL" tab) or directly in mysql via command line:

	mysql -u root -p

You'll have to enter the root user's password again.    
Now you should see a einloggen und oben den Reiter "SQL" anklicken) oder über die Kommandozeile machen, dazu einfach aus dem Terminal aufrufen:

	mysql -u root -p

Ihr werdet aufgefordert, das obige Passwort wieder einzugeben.        
Danach landet Ihr auf einem **mysql>** pPrompt.

#### Create and Select the DatabaseDatenbank erstellen und auswählen:
	CREATE DATABASE iSpindle;
	USE iSpindle;

#### Create Tables:

If you wish to use the charts with density calibration, you should create both tables now.Tabelle(n) anlegen:

Am besten gleich beide Tabellen (Daten und Kalibrierung) anlegen.       
[Hiere](./MySQL_CreateTables.sql) you can find scripts with both definitions.
Otherwise, the main data table will suffice:ist ein [SQL Skript](./MySQL_CreateTables.sql) für beide.        
Die Datentabelle folgt diesem Schema:      

	CREATE TABLE `Data` (
 		`Timestamp` datetime NOT NULL,
 		`Name` varchar(64) COLLATE ascii_bin NOT NULL,
 		`ID` varchar(64) COLLATE ascii_bin NOT NULL,
 		`Angle` double NOT NULL,
 		`Temperature` double NOT NULL,
 		`Battery` double NOT NULL,
		`ResetFlag` boolean,
		`Gravity` double NOT NULL DEFAULT 0,
 	PRIMARY KEY (`Timestamp`,`Name`,`ID`)
	) ENGINE=InnoDB DEFAULT CHARSET=ascii COLLATE=ascii_bin COMMENT='iSpindle Data';

The fi(Im Feld "ID" stores the iSpindle's uniqu wird die hHardware ID abgelegt, whielche we'll need in order to use calibration.ir zum Hinterlegen der Kalibrierung benötigen.)     

	CREATE TABLE `Calibration` (
		`ID` varchar(64) COLLATE ascii_bin NOT NULL,
		`const1` double NOT NULL,
		`const2` double NOT NULL,
		`const3` double NOT NULL,
		PRIMARY KEY (`ID`)
		) ENGINE=InnoDB DEFAULT CHARSET=ascii COLLATE=ascii_bin COMMENT='iSpindle Calibration Data';

#### Create a Database User, Grant Permissions, SetBenutzer anlegen und berechtigen (und ihm ein eigenes Passwordt geben):

	CREATE USER 'iSpindle' IDENTIFIED BY 'passwordxxxxxxxxxx';
	GRANT USAGE ON *.* TO 'iSpindle';
	GRANT ALL PRIVILEGES ON `iSpindle`.* TO 'iSpindle' WITH GRANT OPTION;

Now the database is accessible by the Python sAb sofort steht die MySQL Datenbank für die iSpindel zur Verfügung.        
Das Server scSkript.    
Configure it as explained here: [README](./README.md).
See section below on how to install it.


### Optional: Install Samba (Recommended hierzu wie im [README](./README.md) beschrieben anpassen und neu starten.


### Optional: Samba installieren (empfohlen):

	sudo apt-get install samba samba-common-bin

#### Share pi User's Home Directory and Log FilesHome Verzeichnis im Netzwerk freigeben:

/etc/samba/smb.conf:

	[global]
 	server string = RASPBIAN
 	guest ok = yes
 	security = user
 	socket options = TCP_NODELAY SO_RCVBUF=65535 SO_SNDBUF=65535
 	registry shares = yes
 	syslog = 0
 	map to guest = bad user
 	workgroup = WORKGROUP
 	bind interfaces only = No
 	encrypt passwords = true
 	log level = 0
	# smb ports = 445
 	unix extensions = No
 	wide links = yes

 	include = /etc/samba/user.conf
 	include = /etc/samba/shares.conf


/etc/samba/shares.conf:

	[pi-home]
    	path = /home/pi
    	guest ok = yes
    	read only = no
    	force user = pi
    	browseable = yes

	[system-logs]
    	path = /var/log
    	guest ok = yes
    	read only = yes
    	force user = root
    	browseable = yes

#### Start Samba Ddaemon: (smbd) starten

	sudo insserv smbd
	sudo service smbd start

TheDas pi user's home and system log directories are now being shared and you should be able to see themHome Verzeichnis ist nun im Heimnetzwerk freigegeben und Ihr könnt es inm Explorer/Finder.

### Install the Python Server Script for genericTCP:
Configure the script as explained here: [README](./README.md).
If you're not too familiar with Unix and the shell, you could follow this guide below:

Copy both Eures Computers sehen.

### Das genericTCP Skript installieren
Zunächst das Skript konfigurieren, wie im README beschrieben.    
Die Dateien iSpindle.py aund ispindle-serv to the pi home directory.
Then, within the SSH terminal session, type:

    cd /home/pi
    in das mit Samba freigegebene Verzeichnis kopieren.    
Dann wieder auf dem Raspi im ssh Terminal eingeben:

	sudo mv ~./iSpindle.py /usr/local/bin
	sudo mv .~/ispindle-srv /etc/init.d
	sudo chmod 755 /usr/local/bin/iSpindle.py
	sudo chmod 755 /etc/init.d/ispindle-srv
	cd /etc/init.d
	sudo systemctl daeamon-reload
	sudo insserv ispindle-srv
	sudo service ispindle-srv start

Now would be a good time to reboot the Raspi (should noEin guter Zeitpunkt, den Raspi neu zu starten (ist abe required, though).    
You should be able to see the script running now:

    ps -ax | grep iSpindle

Done.
If everything is configured correctly, the database should receive the iSpindle data and your device(s) should show up in Ubidots, if you have enabled forwarding.    
[Here](web/README.md) are some charts I made for visualization I found essential.    

Have fun!

Yours,
r nicht nötig):

	sudo reboot

Nach erneuter Verbindung sollte nun "ps -ax | grep iSpindle" einen laufenden iSpindle.py Prozess anzeigen, so in der Art:     

	23826 ?        S      0:00 python2.7 /usr/local/bin/iSpindle.py
	23845 pts/0    R+     0:00 grep iSpindle

Ihr habt jetzt die längstmögliche Batterielaufzeit für die iSpindel und habt Eure Daten auch lokal vorhanden, falls Ubidots mal aussetzen sollte oder Ihr Eure eigenen Visualisierungen machen wollt.

Ein paar (für mich ausreichende) Diagramme habe ich [hier](/web) bereitgestellt.

Euer Tozzi (stephan@sschreiber.de)



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMwNzcyNjk3NV19
-->