# Manual Install - Setup

**[Installation Troubleshooting Wiki](https://wiki.openmrs.org/display/docs/Troubleshooting+Installation)**	

The latest platform release (Platform 2.3) is made for Java 8 and does not support later versions. The next platform release (Platform 2.4) will support Java 8, 11, 13, and 14 (most importantly Java 8, which is in most production environments, and Java 11, as our goal is to support LTS versions).

The latest Reference Application 2.10 uses Platform 2.3, so will need Java 8 and will undoubtedly require old versions of Tomcat & MySQL.

## Requirements

### Java

OpenMRS platform 2.x requries Java 1.8 for both the JDK and JRE (but not higher). This is recommended [here](https://wiki.openmrs.org/display/docs/OpenMRS+Installation+for+Developers+on+Ubuntu), [here](https://wiki.openmrs.org/display/docs/Step+2+-+Install+Java), [here](https://talk.openmrs.org/t/toward-a-more-approachable-install/28702/4?u=ianaf100), etc.

- JDK: Version 8
  - `java -version`
- JRE: Version 11
  - `javac -version`
  - I think this needs to be 8 as well

[How to configure Java versions](https://docs.azul.com/zulu/zuludocs/ZuluUserGuide/SwitchingBetweenJavaAlternatives/SwitchBetweenYourLinuxZuluInstallations.htm)

### MySQL

OpenMRS requires old versions of MySQL, version 5.6 (*or 5.7?*). Installing old versions on Ubuntu is described well [here](https://talk.openmrs.org/t/breaking-down-walls-and-attracting-more-devs-to-openmrs/28502/7?u=ianaf100).

- MySQL Community, Version 5.7.29, as recommended [here](https://wiki.openmrs.org/display/docs/Step+4+-+Install+MySQL) 

- MySQL Workbench provides a GUI for viewing and editing the databases

```bash
sudo mysql
```

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

## Build

During build, a web archive (.war) file is created called openmrs.war, which consists of all the pages and classes needed by a server to deploy OpenMRS.

To build OpenMRS and create openmrs.war:

```
mvn clean install
```

```
mvn clean install -DskipTests
```

*(This should be configuired in your IDE pretty easily)*

## Deploy

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

# SDK

- "server1"
- Port: 8081
- Remote debugging port: 1044

*Doesn't work*

# Standalone

```
java -jar ~/Documents/referenceapplication-standalone-2.10.0/openmrs-standalone.jar
```

http://localhost:8081/openmrs-standalone

# Todo

- Try docker version of the SDK
- Try installing the reference application modules manually
- 