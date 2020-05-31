## Distributions of OpenMRS

[Wiki: OpenMRS Distributions](https://wiki.openmrs.org/display/docs/OpenMRS+Distributions)

 "OpenMRS" can mean many things. OpenMRS is a flexible, modular, multi-layered system and its core platform can be used in many different configurations. 

### The OpenMRS Platform

First, there is the **OpenMRS Platform**, which is the shared platform (the "plumbing") upon which all distributions of OpenMRS are based. It is simply a minimal Java and RESTful back-end that developers and implementors can use to build their own applications on top of. It does not aim to provide a user interface or application itself.

The OpenMRS Platform contains the OpenMRS Core and the REST Web Services module.

An OpenMRS **distribution** is just a particular configuration of the OpenMRS Platform as well as certain OpenMRS modules and content (concepts, forms, reports, etc) that can be installed and upgraded as a unit.

### Modules

A **module** is a plugin or add-on consisting of packaged java code that can be installed into a running OpenMRS instance and is able to modify almost all aspects of OpenMRS. It can provide web pages, add tables, change how service calls work, and add new functionality (like report options, person attribute types, etc)

Developers can create add-on modules for OpenMRS and leverage our API to build new applications. The goal of the module system is to allow other developers to write and integrate code into OpenMRS without having to modify the core code base. 

The OpenMRS Reference Application and other distributions of OpenMRS come with modules already pre-packaged into them. These modules are packaged into the war file when it is being built if the omods are in the **/webapp/src/main/webapp/WEB-INF/bundledModules** folder.

### Reference Application

The **OpenMRS Reference Application** is the main general-purpose distribution maintained by the OpenMRS community. It provides a set of default modules that demonstrates how the platform's capabilities can be used to build an EMR. It can be used out-of-the-box as a facility EMR, or may serve as a solid base for new implementations.

Currently, Reference Application 2.10 provides modules which enable you to:

- register patients
- start and end visits
- list patients with active visits
- admit/transfer/discharge patients
- take clinical notes
- capture patient vitals
- display patient summary and visit history.
- capture allergies
- enter forms from the patient dashboard

### Development

Which distribution should I install for developing OpenMRS? *My understanding* is you should check out the core platform, as well as any modules you wish to work on. However, the reference application provides a good basis for how most interact with OpenMRS and its most-used modules, so setting up your development environment to resemble the reference application is a good idea.

Most development work on OpenMRS RA 2.x can be done *without* needing to check out all the code for all modules in the distribution ([link](https://wiki.openmrs.org/display/projects/Setting+Up+a+Development+Environment+for+OpenMRS+2.x)). You should be able to implement new features by working on the specific module that includes the app in question, or by creating a module with new functionality. 

If you want to *run* the reference application as a developer, you need *compiled* versions of all modules in the distribution. This can be done by checking out the OpenMRS Reference Application distribution [on Github](https://github.com/openmrs/openmrs-distro-referenceapplication). Running "mvn clean install" in this project will assemble the modules that make up the latest version of OpenMRS 2.x. You probably want to write a script for this, described [here](https://wiki.openmrs.org/display/projects/Setting+Up+a+Development+Environment+for+OpenMRS+2.x#SettingUpaDevelopmentEnvironmentforOpenMRS2.x-Buildingthelatestdistribution).

However that is not necessary. If you are a core developer, you will most likely need to clone the following modules:

- EMR API
- UI Commons
- App UI
- Core Apps
- Reference Application

All modules can be built by running mvn clean install. If some module is not building for you, please check its status in [our CI](https://ci-stg.openmrs.org/allPlans.action). If it is green, then it means that it is only your local problem and you need to resolve it yourself. 

### OpenMRS SDK

The **OpenMRS SDK** allows for rapid development of modules and the OpenMRS Platform code. It is an ever expanding project with a rich feature-set. Operating System compatibility was also taken into account, allowing users to install the SDK and be up and running within a few minutes on Windows, Linux and Mac OS X.

The SDK consists of a number of commands that allows you to quickly setup and configure OpenMRS servers for any major distribution. 

*Why should I use the SDK for development, instead of installing Tomcat and using my own configuration?* The SDK can set multiple self contained servers that can be run independently, at times at the same time by running each available server on a different port specifying the -Dport variable. You can even mix the servers, having those running JDK 1.7 for 1.11.x and JDK 1.8 2.x alongside each other. The SDK creates an indpendent Tomcat instance with its own database for each server. 

The SDK can be used to easily set up a development environment by creating a server and configuring debugging info in the IDE (I think?). [This is explained here](https://wiki.openmrs.org/display/docs/OpenMRS+SDK+Step+By+Step+Tutorials).

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