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

Modules of interest for development:

- uicommons - Contains the general CSS and Javascript files that are used in the reference application
- referencemetadata - Adds required metadata, sets required [settings](https://wiki.openmrs.org/display/docs/Settings+Descriptions) (formerly global properties from platform 1.8 downwards) and any necessary configurations that are required for the application to be functional.
- referencedemodata - Adds demodata that is uses in the dev and test environments.
- referenceapplication - This is the one that glues everything together to form the reference application.

And the reference application distro is a utility module that contains the scripts that build and packages the archive that contains all the modules, it also contains the UI tests for the reference application.

### Development

Which distribution should I install for developing OpenMRS? 

*My understanding* is you should check out the core platform, as well as any modules you wish to work on. However, the reference application provides a good basis for how most interact with OpenMRS and its most-used modules, so setting up your development environment to resemble the reference application is a good idea.

### Reference Application Development

Most development work on OpenMRS RA 2.x can be done *without* needing to check out all the code for all modules in the distribution ([link](https://wiki.openmrs.org/display/projects/Setting+Up+a+Development+Environment+for+OpenMRS+2.x)). You should be able to implement new features by working on the specific module that includes the app in question, or by creating a module with new functionality. 

If you want to *run* the reference application as a developer, you need *compiled* versions of all modules in the distribution. This can be done by checking out the OpenMRS Reference Application distribution [on Github](https://github.com/openmrs/openmrs-distro-referenceapplication). Running "mvn clean install" in this project will assemble the modules that make up the latest version of OpenMRS 2.x. You probably want to write a script for this, described [here](https://wiki.openmrs.org/display/projects/Setting+Up+a+Development+Environment+for+OpenMRS+2.x#SettingUpaDevelopmentEnvironmentforOpenMRS2.x-Buildingthelatestdistribution).

However that is not necessary. If you are a core developer, you will most likely need to clone the following modules:

- UI Commons
- Reference Application
- EMR API
- App UI
- Core Apps

All modules can be built by running mvn clean install. If some module is not building for you, please check its status in [our CI](https://ci-stg.openmrs.org/allPlans.action). If it is green, then it means that it is only your local problem and you need to resolve it yourself. 

### OpenMRS SDK

The **OpenMRS SDK** allows for rapid development of modules and the OpenMRS Platform code. It is an ever expanding project with a rich feature-set. Operating System compatibility was also taken into account, allowing users to install the SDK and be up and running within a few minutes on Windows, Linux and Mac OS X.

The SDK consists of a number of commands that allows you to quickly setup and configure OpenMRS servers for any major distribution. 

*Why should I use the SDK for development, instead of installing Tomcat and using my own configuration?* The SDK can set multiple self contained servers that can be run independently, at times at the same time by running each available server on a different port specifying the -Dport variable. You can even mix the servers, having those running JDK 1.7 for 1.11.x and JDK 1.8 2.x alongside each other. The SDK creates an indpendent Tomcat instance with its own database for each server. 

The SDK can be used to easily set up a development environment by creating a server and configuring debugging info in the IDE (I think?). [This is explained here](https://wiki.openmrs.org/display/docs/OpenMRS+SDK+Step+By+Step+Tutorials).