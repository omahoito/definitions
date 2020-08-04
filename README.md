# ODA definitions

This repository contains ODA architecture and UI definitions.

## Design principles 

See [suunnitteluperiaatteet.md](suunnitteluperiaatteet.md) (in Finnish)

## Architecture

The architecture is guided by the following principles:
* Modular API first system
* Composed of microservices
* Simple and minimalistic user experience
* User centric
* Multilingual
* Agile development
* Systems design
* Small is beautiful
* Open data
* Open source
* Open API
* Open standards
* My Data.

Further reading (in Finnish): [Arkkitehtuuriperiaatteet](arkkitehtuuriperiaatteet.md)

### Modules

Currently implemented modules are listed below. See their repositories for more
detailed software achitecture description.

![](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/omahoito/definitions/master/modules.plantuml?13) 
<!-- To invalidate caches change the counter in the url above, i.e. modules.plantuml?13 -> modules.plantuml?14 -->

#### [oda-analytics-service](https://github.com/omahoito/oda-analytics-service)
Oda-analytics-service generates dashboard reports from ODA log data.

#### [oda-backend](https://github.com/omahoito/oda-backend) 
Backend provides static resources, API gateway and integration to Suomi.fi 
SSO. Backend is the contact point for web browsers and handles security aspects
such as CSRF protection.

#### [oda-cds-service](https://github.com/omahoito/oda-cds-service)
Oda-cds-service acts as a decision making service for oda-fhir-service
regarding specific questionnaires stored in oda-phr.

#### [oda-context-client](https://github.com/omahoito/oda-context-client) 

#### [oda-esb](https://github.com/omahoito/oda-esb) 

ODA ESB provides XML interfaces for external services via KaPa and converts
external XML interfaces to FHIR/JSON interfaces for internal services.

#### [oda-feedback-service](https://github.com/omahoito/oda-feedback-service)

#### [oda-fhir-service](https://github.com/omahoito/oda-fhir-service) 

FHIR Service Provides FHIR resource APIs on top of oda-phr and external systems 
that provide FHIR APIs.

#### [oda-cqrs-service](https://github.com/omahoito/oda-cqrs-service) 

Provides DTO interfaces for oda-web-front using CQRS pattern.

#### [oda-fhir-service-common](https://github.com/omahoito/oda-fhir-service-common)

Java library for providing FHIR services.

#### [oda-form-generator](https://github.com/omahoito/oda-form-generator) 

#### [oda-gateway](https://github.com/omahoito/oda-gateway) 

#### [oda-idp](https://github.com/omahoito/oda-idp)
ODA OpenID Connect Provider for authenticating and authorizing end users.


#### [oda-logging-service](https://github.com/omahoito/oda-logging-service) 
Logging Service provides a centralized logging server and a client library that
handles server communication when built into other services.

#### [oda-media-storage](https://github.com/omahoito/oda-media-storage)
Provides an API to file storage. Checks authorization using oda-fhir-service.

#### [oda-notification-service](https://github.com/omahoito/oda-notification-service)
Oda-notification-service sends notifications to end users. 

#### [oda-phr](https://github.com/omahoito/oda-phr) 
Personal health record database. 
 
#### [oda-service-common](https://github.com/omahoito/oda-service-common) 
Shared server side Java library.

#### [oda-ui-common](https://github.com/omahoito/oda-ui-common) 

#### [oda-web-front](https://github.com/omahoito/oda-web-front) 
Web Browser UI that is served from oda-backend. 

#### [oda-covid-contact-service](https://github.com/omahoito/oda-covid-contact-service) 
Implements storing and distributing contact requests from citizen to health care personel from Finnish national Covid tracking app (koronavilkku) and handles publish token generation and distribution by professionals.

### Information architecture

Information is stored primarily as FHIR resources.
Data model is described in [ODA RFC repository](https://github.com/omahoito/rfc/blob/master/README.md).

ODA uses standardized codesets whenever possible. See full [codes and code systems listing](codesets.md).

### Roles and authorizations
Documented (in Finnish): [Roolit ja valtuutukset](roolit-ja-valtuutukset.md)

### Integrations
See [Integrations](integrations.md) for details.

### Functional architecture

Some of the module interactions are described with 
[sequence diagrams](sequence-diagrams/).

How a service provider is selected for a customer is described in 
[customer-to-service.md](customer-to-service.md).

### Key technologies

Current microservices are implemented as standalone Java 8 applications 
with [Spring Boot](https://projects.spring.io/spring-boot/).
[PostgreSQL 9.6](https://www.postgresql.org/) is used as database.

Web frontend is implemented as an EcmaScript 6/HTML 5 single page application.
Key libraries include 
* [react](https://facebook.github.io/react/) UI-component library
* [react-router v4](https://github.com/ReactTraining/react-router/) front-end routing and
* [redux](http://redux.js.org/) for application state handling.


### Deployment architecture
See [Deployment architecture](deployment.md)

## Visual design

* [Wireframes](sketch-wireframes/)
* [Style guide](style-guide/)

## Tools and conventions

* Wireframes are produced with [Sketch](https://www.sketchapp.com/).
* [Working with diagrams](diagrams.md)
* [Code Conventions](codeconventions.md)
* [Working with FHIR profiles](https://github.com/omahoito/rfc#tools-for-editing-profiles)

## Glossary

See [glossary.md](glossary.md)
