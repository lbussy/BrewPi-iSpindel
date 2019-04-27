#   Installation information
## iSpindel to BrewPi Remix Integration

1. - Install numpy with 'sudo apt-get install python-numpy'
2. - Copy the files/folders fro WWW into the /var/www/html folder
3. - Copy the files in Scripts into the /home/brewpi folder.
4. - Edit ispindel.py as needed - For testing I also have CSV and Ubidots ON in this version. Add your UBI token
5. - Run the following commands:

    cd /home/brewpi
    sudo mv ./ispindle-srv /etc/init.d
    sudo chmod 755 /home/brewpi/iSpindle.py
    sudo chmod 755 /etc/init.d/ispindle-srv
    cd /etc/init.d
    sudo systemctl daemon-reload
    sudo insserv ispindle-srv
    sudo service ispindle-srv start

Now, test the iSpindel connection - go into iSpindel config mode, set it to use TCP connection and insert the IP of your BrewPI as well as port number 9501. If successful you will start to see data logged in the /var/www/html/data/iSpindel folder. One CSV with all data, another with just the most recent reading.  The full CSV is for testing and can be disabled in iSpindel.py, the single is what BrewPi pulls in.  For testing you may want to change the reporting frequency (on the iSpindel) to 30 seconds or similar.

Once all files are in place and the iSpindel is logging data to the server, stop and restart the BrewPi script.  This will make sure the web interface and the script in the back are using the new versions. 

#### Known issues:

Mobile devices are taking a long time to display. This started when I enlarged the graph to allow room for additional text.  Could be from extra logged data - plan on removing Battery and maybe Temp from graph and adding it to the header or footer as text only. Currently graphing battery to follow the battery depletion speed.

Initial logging when starting new brew will be the last reading from the iSpindel - be that from the last brew, sitting on the table or in the new brew. 

If there are NO iSpindel readings when you start a brew the graphing won't load. On a new install make sure the iSpindel has logged a data point before starting the brew..  This is only an issue on the first run after adding the integration.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjcwNDY0ODhdfQ==
-->