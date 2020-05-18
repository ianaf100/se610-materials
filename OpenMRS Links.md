#  OpenMRS

My notes for contributing to [OpenMRS](https://openmrs.org/).

## About
"OpenMRS is a Java-based, web-based electronic medical record system.  We started from a simple (at least it used to be simple) data model, wrapped that into an API, and then built a web-based application that uses the API.  The OpenMRS API works like a "black box," hiding the complexities of the data model beneath it and ensuring that applications and modules using the API work with a similar set of business rules for managing the electronic medical record system data."

- [Github](https://github.com/openmrs/openmrs-core)
- [Demo](https://openmrs.org/demo/)
- [JIRA Issue Tracking](https://tickets.openmrs.org/secure/Dashboard.jspa)

## Developer Documentation
There's the [wiki](https://wiki.openmrs.org/display/docs/Developer+Guide) and also a separate [dev manual](http://devmanual.openmrs.org/en/)

- [Developer's Guide](https://wiki.openmrs.org/display/docs/Developer+Guide)
- [Getting Started Checklist](https://wiki.openmrs.org/display/docs/Getting+Started+as+a+Developer) 
  - [Another One](http://devmanual.openmrs.org/en/What_next/devChecklist.html)
- [A Separate Getting Started Guide](https://mauryanguidetoopenmrsdevs.wordpress.com/) 
  - Very good - walks you through everything you need to know
- [Types of Developers/Contributors](https://wiki.openmrs.org/display/docs/Different+Types+of+OpenMRS+Developers)
- [Developer Stages](https://wiki.openmrs.org/display/RES/OpenMRS+Developer+Stages)
  - Start by getting the /dev/null badge
- [Installation For Developers](http://devmanual.openmrs.org/en/Technology/getSetUp.html)
  - [IDE Info](https://wiki.openmrs.org/display/docs/How-To+Setup+And+Use+Your+IDE)
  - [Another setup guide](http://devmanual.openmrs.org/en/Technology/getSetUp.html)
  - [Github Readme](https://github.com/openmrs/openmrs-core/blob/master/README.md#build)
- [Conventions](https://wiki.openmrs.org/display/docs/Conventions)
  - [Java Conventions](https://wiki.openmrs.org/display/docs/Java+Conventions)
    - _I need to become more familiar with [javadocs](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html)_
  - [Javascript](https://wiki.openmrs.org/display/docs/JavaScript+Conventions)
  - [Github](https://wiki.openmrs.org/display/docs/GitHub+Conventions)
    - [Pull Request Instructions](https://wiki.openmrs.org/display/docs/Pull+Request+Tips)
    - [Commit Comments](https://wiki.openmrs.org/display/docs/GitHub+Conventions#GitHubConventions-CommitComments)
    - [Submitting Code How To](https://wiki.openmrs.org/display/docs/How-To+Submit+Code) (out of date?)
  - [Unit Test Conventions](https://wiki.openmrs.org/display/docs/Unit+Testing+Conventions) and [Guide](https://wiki.openmrs.org/display/docs/Unit+Tests)
- [Development screencasts on YouTube](https://www.youtube.com/user/OpenMRS)


## Communication
- [OpenMRS Talk](https://talk.openmrs.org/) - main discussion forum
  - [About](https://talk.openmrs.org/t/openmrs-talk-email-discussion-groups/1165), plus an [FAQ](https://talk.openmrs.org/faq)
  - [Introduce yourself here](https://talk.openmrs.org/t/welcome-please-introduce-yourself/)
- Chat with other developers
  - [IRC](http://irc.openmrs.org/)
  - [Telegram](http://telegram.me/openmrs)
- [Mailing List](https://wiki.openmrs.org/display/RES/Mailing+Lists)


## Contributing
"**Finding JIRA Issues**: If you don't know where to begin putting your development skills to good use, start with JIRA's Issue Navigator to view Introductory Issues. These are Ready for Work and have been deemed the right amount of complexity for a new OpenMRS developer. Review these to find an issue of interest to you that is ready for work, claim it, and make your first contribution."

"Read https://wiki.openmrs.org/display/ISM/JIRA+Issue+Tracking+System for introductory issues and other tips on getting started working with JIRA issues."

- [Video tutorial for doing this and contributing code](https://www.youtube.com/watch?v=SbbDvMVgRWo&feature=emb_logo)
- [Another Contributing Doc](https://github.com/openmrs/openmrs-core/blob/master/CONTRIBUTING.md)
- [Creating Modules](https://wiki.openmrs.org/display/docs/Creating+Modules)
  - [Your First Module](https://wiki.openmrs.org/display/docs/Creating+Your+First+Module)
- Review [Unassigned Projects](https://wiki.openmrs.org/display/projects/Available+Development+Projects) if you want to find a larger project to work on.
- [Documentation roadmap ](https://trello.com/b/uSIAARXh/openmrs-documentation-roadmap) for updating/editing documentation tasks and priorities (A GREAT START)
- [Documentation Guide](https://wiki.openmrs.org/display/docs/OpenMRS+Documentation+Guide)

## Example Commits

- https://github.com/openmrs/openmrs-contrib-android-client/pull/736
- 

## Technical
- [Technical Overview](https://wiki.openmrs.org/display/docs/Technical+Overview)
- [Architecture](http://devmanual.openmrs.org/en/Technology/architecture.html)
- [OpenMRS REST API documentation](https://rest.openmrs.org/#openmrs-rest-api)

## Directories

<table>
 <tr>
  <td>api/</td>
  <td>Java and resource files for building the java api jar file.</td>
 </tr>
 <tr>
  <td>tools/</td>
  <td>Meta code used during compiling and testing. Does not go into any released binary (like doclets).</td>
 </tr>
 <tr>
  <td>web/</td>
  <td>Java and resource files that are used in the webapp/war file.</td>
 </tr>
 <tr>
  <td>webapp/</td>
  <td>files used in building the war file (contains JSP files on older versions).</td>
 </tr>
 <tr>
  <td>pom.xml</td>
  <td>The main maven file used to build and package OpenMRS.</td>
 </tr>  
</table>

## Issues I Could Run Into

- Java versions (JDK, JRE)
- MySQL version