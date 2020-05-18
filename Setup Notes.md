*Setup notes*

### Java

Java Runtime Environment: Version 8, as recommended [here](https://wiki.openmrs.org/display/docs/Step+2+-+Install+Java)

Java Dev Kit: Version 11 (this might need to be 8)

[How to configure Java versions](https://docs.azul.com/zulu/zuludocs/ZuluUserGuide/SwitchingBetweenJavaAlternatives/SwitchBetweenYourLinuxZuluInstallations.htm)

### MySQL

MySQL Community, as well as MySQL Workbench which provides a great GUI for viewing and editing the databases

Root password: (same as linux password)

```bash
sudo mysql
```

**I need to downgrade to version 5 most likely**

### Apache Tomcat

Installed locally in `%HOME%/.local/share/apache-tomcat-8.5.55` (Version 8)

Also properly installed here: (Version 9) 

- `/usr/share/tomcat9/` 
- `/etc/tomcat9/`
- `/var/lib/tomcat9/`

`tomcat-users.xml` contains the user roles

- Admin: *tomcat*
- Password: *tomcat*

```
./bin/startup.sh
```
```
sudo service tomcat9 start
sudo service tomcat9 stop
sudo service tomcat9 restart
```

Starts a server on http://localhost:8080/

### IntelliJ

https://wiki.openmrs.org/display/docs/Developer+How-To+Setup+And+Use+IntelliJ

### Other Things For Later

- Logging is kind of covered [here](https://wiki.openmrs.org/display/docs/Developer+How-To+Setup+And+Use+IntelliJ#DeveloperHow-ToSetupAndUseIntelliJ-Addloggingtoclass)
- I still need to import templates

### Build

During build, a web archive (.war) file is created called openmrs.war, which consists of all the pages and classes needed by a server to deploy OpenMRS.

To build OpenMRS and create openmrs.war:

```
mvn clean install
```

```
mvn clean install -DskipTests
```

*(This should be configuired in your IDE pretty easily, stupid)*

### Deploy

The OpenMRS source code contains a dependency to a  Jetty server, and for development purposes you can simply deploy the `openmrs.war`  in that application server:

```
cd webapp
mvn jetty:run
```

However to build in Tomcat, (which offers the production environment most firms deploy OpenMRS in), we need to do some more work. 

- Copy the file `openmrs/webapp/target/openmrs.war` into your `/var/lib/tomcat9/webapps` directory.
- Now start the server with `bin/startup.sh`, or by starting the server from your IDE (haven't figured that out yet).
- Now go to your browser and type in the url http://localhost:8080/openmrs 
- If this is your first time, this will open up the initial installation of OpenMRS.
