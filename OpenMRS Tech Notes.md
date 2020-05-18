Some tech notes I've compiled (from various documentation sources) to help me understand and remember the technical overview of OpenMRS.



# Architecture

![architecture](http://devmanual.openmrs.org/en/assets/OpenMRS-architecture.png)

## Data Layer

The Data Access layer is an abstraction layer from the actual data model and its changes. It uses **Hibernate** as the Object Relational mapping tool, and Liquibase to manage relational database changes in a database-independent way.

MySQL is used to represent OpenMRS's data model. To view the relations in the data model, see [the wiki](https://wiki.openmrs.org/display/docs/Data+Model).

### Hibernate

[Hibernate](http://hibernate.org/) is an excellent Object Relational Mapper. Using just xml files, we are able to describe the relationship between all of our tables and our domain objects in Java (POJO). *POJO: Plain Old Java Objects are the objects we understand like Patient.java, Concept.java, etc.*

For example, it would be very difficult to keep up with where to store each part of a domain object and all the relations between them. Using Hibernate, an xml mapping file does the hard work of knowing the tables behind the object, so we only have to concern ourselves with the given domain object and not its specific relations. 

## Service Layer

The Service layer is responsible for managing the business logic of the application. It is built around the Spring framework. 

### Spring

The **Spring Framework** is an application framework for Java. In a nutshell, Spring is a way of making it easy to work with databases and create websites and web services using Java. Spring provides you anything you want/need to build an ecosystem of services for an enterprise-level application. Spring modules provide a range of services, such as:

- **Database connections and manipulations**
  - The Spring Data module can communicate with databases, SQL or NoSQL
  - Oracle, MySQL, MongoDB, Redis, Hibernate, MyBatis, etc...
- **Build REST API**
- **Build web applications**
- Handle external resources or systems you have to work with
- Authentication, security, testing, and many other services...

Specifically, The OpenMRS service layer classes make extensive use of the Spring framework for a number of tasks including the following:

- Spring Aspect Oriented Programming (AOP) is used to provide separate cross cutting functions (for example: authentication, logging)

- Spring Dependency Injection (DI) is used to provide dependencies between components.
- Spring is used to manage transactions in between service layer classes

OpenMRS also uses **Spring MVC** in the presentation layer, a module which provides a Model-View-Controller (MVC) architecture and several features that can be used to develop flexible and loosely coupled web applications. (***Note:*** *not sure if this is still the case*)

## Presentation Layer

### Legacy

The presentation layer for the *legacy application* was built upon HTML, Spring MVC, JSP and JavaScript. 

- DWR (Direct Web Rounting) was used for AJAX functionality and provided the mapping between Java objects/methods to JavaScript objects/methods respectively.

- HTML and JSP pages are what you view when you use OpenMRS. Most of these pages are in JSP.
- [JQuery](https://jquery.com/) is just a simple, fast JS Library which simplifies JavaScript programming. Used to simplify the interactions with Javascript and the browser.
- The Spring MVC module was used to provide the Model-View-Controller architecture.

#### JSP

*JSP is only used in the legacy application*

**JavaServer Pages (JSP)** is a technology for developing Webpages that supports dynamic content. This helps developers insert java code in HTML pages by making use of special JSP tags, most of which start with <% and end with %>. Unlike static HTML or JavaScript, JSP can interact with the web server to perform complex tasks like database access and image processing etc. JSP tags can be used for: 

- Retrieving information from a database 
- Registering user preferences
- Passing control between pages
- Sharing information between requests, pages etc.

JavaServer Pages often serve the same purpose as programs implemented using the [Common Gateway Interface (CGI)](https://en.wikipedia.org/wiki/Common_Gateway_Interface), but JSP offers certain advantages. 

### Current

For the current reference application user interface, HTML and JavScript are primarily used instead of JSP, MVC, and AJAX. 

**AngularJS, JQuery, and Apache Groovy** are heavily used. 

*I'm having trouble finding more info on the current UI architecture*