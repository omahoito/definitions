# Integrations

![](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/omahoito/definitions/master/masterdata.plantuml?3) 
<!-- This generates a picture based on masterdata.plantuml. To change the counter in the url above, i.e. masterdata.plantuml?13 -> masterdata.plantuml?14 -->

| System | Description | Authentication |
| ------ | ----------- | -------------- |
| Suomi.fi identification | SAML 2.0 SSO. https://esuomi.fi/palveluntarjoajille/tunnistaminen/ | Public key |
| Suomi.fi e-Authorizations | Enables users to act on behalf of other persons in digital services, and authorize others to act on their behalf. https://esuomi.fi/palveluntarjoajille/asiointivaltuudet/  | X-Road or OAuth 2 |
| Kanta | Electronic health records archive. http://www.kanta.fi/web/ammattilaisille/tietotekniikan-ja-yhteyksien-toteuttajille | OAuth 2 |
| Kela Koronahaavi | API meant for health authorities, for creating a publish token for a diagnosis report after verifying the infection.  | Mutual TLS|
| My kanta | http://www.kanta.fi/web/ammattilaisille/tyokalut-kehittajille | OAuth 2 or SSL client cert |
| ODA 1 | Provides questionnaires and decision support | (firewall) |
| Code server | National code server https://koodistopalvelu.kanta.fi/codeserver/ | Open data |
| Terhikki | Practitioner registry | ? |
| VTJ | Provides person information of citizen | X-Road or SSL client certificate |
| Appointment reservations | E.g. AVPH that provides FHIR API for booking appointments | (firewall) |
| Minimum context management | http://www.hl7.fi/hl7-rajapintakartta/minimikontekstinhallinnan-maarittely/  | ? | 

## KaPa integration

KaPa services are provided and consumed via 
[oda-esb](https://github.com/omahoito/oda-esb) and an X-Road Security Server.

![](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/omahoito/definitions/master/kapa.plantuml?3) 
<!-- To invalidate caches change the counter in the url above, i.e. kapa.plantuml?13 -> kapa.plantuml?14 -->

The proposed services that could be published to KaPa are:

* CarePlan Service
  * Get service requests
  * Get care plan
* Observation Service
  * Get observations
  * Create observation
* Communication Service
  * Get notifications
  * Get comments

![](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/omahoito/definitions/master/kapa-services.plantuml?-2) 
<!-- To invalidate caches change the counter in the url above, i.e. kapa-services.plantuml?13 -> kapa-services.plantuml?14 -->

## Preparing for third party downtime

Downtime could be due to any planned or unplanned incident that prevents ODA 
from using the remote system. The fault may be for example in the remote 
system, in some ODA component or connection between them.

All failures and failed attempts must be logged and monitored. If failures are 
rare, it might not be cost-effective to implement failback mechanisms. On the 
other hand, if failures are common, they degrade ODA user experience.

### Suomi.fi identification

ODA depends on Suomi.fi e-Identification for strong authentication and citizen 
cannot start new sessions. The ODA error page should instruct citizen to use 
the functionality allowed for anonymous users.
However the existing sessions won’t be affected. Mobile applications could 
have long sessions, if the users is identified again with a PIN.

_TBD: How long session ODA should allow? What data access requires a valid 
suomi.fi session?_

### Suomi.fi Digital Authorizations

Downtime prevents citizen from acting in ODA on behalf of another person.
ODA could keep the authorization in a cache, provided that the information is 
adequately shielded.

_TBD: To what extend should the cache be protected? For example, does it have 
to be encrypted?_

### Kela Koronahaavi Publish Token API

In the case of Publish Token API being down, all related actions for professionals are unavailable.

### Kanta
#### Read operations
ODA will read prescriptions and information about previous encounters from 
Kanta. Some of this information could be used to prefill forms on behalf of 
citizen. If Kanta data can’t be read the user would have to fill in the 
information manually.

_TODO: Analyze the effects on the UI components used by professionals_
_TBD: Can the data be kept in a cache and on what conditions?_

#### Write operations
_TODO: Analyze from the perspective of professionals’ UI components_

### My Kanta

My Kanta will contain care plans, observations and other patient data that will 
be displayed and edited in ODA. My Kanta will provide FHIR (STU 3) API. FHIR 
resources, including their version information and timestamps, could be kept in 
cache in ODA - perhaps even in the client. FHIR standard specifies versioning 
and mechanisms for managing lost updates. On the other hand, FHIR specifies 
that the FHIR server may change the resource during create or update. This 
means that the unsaved (cached) resources should be refreshed from the server 
after saving.

_TBD: Security considerations of caching resources on server and client side_

### ODA 1 knowledgebase
#### Forms

The knowledgebase contains symptom check forms. Forms are versioned and there 
is an API for checking the current versions, which makes it easy to cache them 
in ODA. The cache can be populated with a batch run that can be retried if the 
knowledgebase cannot be accessed.

#### Decision support
Processing symptom checks takes place in the knowledgebase. If the 
knowledgebase doesn’t respond, ODA is unable to present treatment 
recommendations. The input should be kept safe and the end user should be able 
to try again. 

### Code server

Code values change rarely and they could be cached in ODA database. This way 
the downtime would affect only the patch download process that could be retried.

### Terhikki

Terhikki is a registry of social welfare and health care professionals. Access 
to Terhikki is needed when person’s rights to act as a registered 
professional is checked. The check could be done when new professionals are 
added to system and organizations. This reduces dependency to Terhikki, but 
revoked and granted rights must be handled. System must not under any 
circumstances allow some person to act as a professional if that person is not 
a professional. 

_TBD: How and when are rights updated? Professional’s profile view could have 
an action to fetch rights from Terhikki. Additionally, system/organization 
level batch action or an automated job could refresh the data?_

### VTJ

VTJ contains basic information about citizen [(name, address, home municipality 
etc.)](https://esuomi.fi/palveluntarjoajille/tunnistaminen/tekninen-aineisto/tunnistetusta-kayttajasta-valitettavat-attribuutit/). 
Suomi.fi identification passes most of this information to ODA when user logs 
in. Authentication fails in Suomi.fi if VTJ is down. Additional data might also
be required directly from VTJ, for example mother tongue. The additional data 
could be cached to user profile in ODA.

### Appointment reservations

Downtime prevents appointment reservation functionality from working: list of 
free appointments will be unavailable and/or the reservation fails.

User should be able to try again. If the problem persists, alternative contact 
information should be presented to user. The user could reserve the appointment 
via chat and a phone number (that is saved in ODA) could be displayed.

### VRK geolocation data

Health center areas, addresses etc. should be downloaded into ODA database. The 
downloaded data must be refreshed regularly, because the data set might contain 
outdated, incorrect or missing values.

### Minimum context management

Reliance on a third party context server prevents SSO and context switch from 
an EHR system, if that server is unavailable. As a fallback the professional 
could log in via suomi.fi. ODA could also mandate EHR systems to use ODA’s 
context server. This would reduce the number of dependencies.

_TBD: How context is passed along if the professional logs in via suomi.fi? 
Option include at least URL encoding and session._

## Caching

When third party data is cached in ODA, sizing, consistency and security must 
be considered. 

Cache sizing is a tradeoff between the need and cost of resources. The cache 
should be big enough to accommodate all relevant data. When the cache is full 
data is replaced with a selected caching algorithm.

Ideally items in cache and the lower level storage should be in the same state. 
Changes made by ODA users can be tackled by writing modifications through the 
cache, but ODA does not get notified by changes made by other systems. Thus 
some validity period must be considered. The cached objects should be in a 
consistent state also between difference ODA processes. If the objects are 
mutable, the cache should be distributed.

The same security constraints must be apply whether the data is accessed in the 
master data source or the cache. Per user cache would be the most secure but 
also the most resource intensive. The data must not leak from the cache. 
Special care should be taken if the cache is in client side (browser or mobile 
application). The data must be deleted from client as soon as it is no longer 
needed to avoid leaks. The server must check the validity of everything coming 
from the client side.

All cache actions must be logged and some need to be monitored perhaps even on 
a real-time basis. In particular, all failures and failed attempts need to be 
logged.

## Guaranteed asynchronous persistence to master data stores

In case a master data store is down, data should be persisted locally to avoid 
losing it. Update to master should be retried when it comes available. 
Storing unsaved data locally also enables read operations. Whether these dirty 
reads are a good idea or not is another thing.

The retry strategy depends on whether the modifications are interdependent or 
not.

**When events must be processed in the same order they occur**

ODA must persist events in a particular order. Further related updates must not 
be saved before the first failed event succeeds. New dependent events must not 
be attempted, but put to retry queue instead. Because of the dependencies, 
processing of the failed events must be serialized to a certain degree. 
If one of these events is a creation of a resource and further events update 
the same resource, ODA must assign some internal identifier to this resource 
and map this ID to the official identifier assigned by the master data store.

In the following example CaraPlan creation (POST) fails and ODA can’t get 
resource ID from PHR. A subsequent update (PUT) to the same resource could be 
done by using an internal ID that is saved to a custom extended attribute. If 
the resource ID still can’t be resolved, the client has to wait for the retry 
mechanism to work. Once the resource is created, the internal ID can be mapped 
to resource id and the update can be retried.

![](http://www.plantuml.com/plantuml/svg/dL5BJm8n4BxlhvXmqiJT4CE911A3-8Y948MJ6bEsWpGkRNTskkA_j_l82hWaoAsTxpLVQmMnYfKLcc6KADGSsArHnqWHBbUAmWmfLp5sG6JWXxiiGgPlI1swYf2smL-6Ff5Al1piSAPtJq6L3-6m280-J2VFCnW9mcaYT53C6kWgozI7BYxlIW9k6BL42OAPrC8nrknDuNl8AVN0EFU9UpYSAJdGBaakQl-1ADEylvBdbThBwstxn-h-j9l_Bt9OTxaTps1BsCOlY64JrElsuD4mt1YdPLlvunU9eYX82tdreoUzNoLjwkn93zRvpRpiTc7o46Z322P7ge1-wUUFZWgTyCVCE8eHxg-1Lu9VTMnS8c61i1RMUWwRUi43fUOdTzfNlYp9hC4IvX5aZFQjrUO7)

**When order of events is not important**

For example Observation resources could be created in any order, because 
Observations have a natural order (by date) separate from the creation order or 
surrogate ID. Ideally the Observations would also be immutable.

A retry queue would likely be the simplest to implement. (1) An event (e.g., 
create an Observation) is send to queue. (2) Event is processed. If the 
processing fails (resource can’t be saved because the remote system doesn’t 
respond), the event is returned to the queue. The queue could also be priority 
based, e.g., the oldest event can have the highest priority.

### Pit falls

Remote system might be slow to respond. If a resource is created or modified 
after a client connection times out, the client would duplicate the event.

The remote system might have some logic and side effects tied to modification 
events. For example, the remote system might transform the resource some way, 
or the operation could be denied because the end user has no right to perform 
it. These would go unnoticed by the ODA end user if the persistence is 
asynchronous, unless handled separately.

If the remote system is available to other users, ODA users would miss the 
updates. Conflicting edits might occur and they could be hard to solve without 
user interaction. 

### FHIR

FHIR standard specifies two ways a resource can be created: create operation 
(HTTP POST) and conditional update (PUT). Resource ID is assigned by the FHIR 
server. The create operation can also be optional: a search condition can be 
provided by the client.

Single FHIR resources can be updated by ID or a query. If no resources match 
the query a new resource will be created.

An update can contain a new version of the whole resource (regular update) or 
just parts of it (patch update). If patch updated target separate paths of a 
resource, the updates can be made and retried independently. 

### Recommendations

The safest option is to limit delayed persistence to creation of immutable 
resources. Even then, the risk of duplicated resources should be managed. For 
example, the creation timeout could be set high enough (but not infinite) to 
avoid duplicates in practical scenarios, or if the resources are identical for 
practical purposes, they could be merged by ODA.

Converting synchronous operations to asynchronous causes unavoidable complexity 
and therefore increases risk of bugs and failures. On the other hand, third 
party downtime can cause significant disturbance to ODA users. To find a 
compromise, third party service levels should be monitored and analyzed in 
production. Before ODA itself goes to production this analysis could be based 
on service proved reports.

#### MVP scope

As defined in MVP requirement PHR3, CaraPlan FHIR resources are saved 
synchronously and related Observation resources asynchronously if the PHR is 
unavailable.

The following diagram illustrates two alternatives for implementing the 
asynchronous retrying of Observation creation. In the first alternative 
failures are saved to database. An external retry worker process polls the 
database and tries to call the FHIR service again. As there might be multiple 
workers (ofr high availability and performance) the failure entries must be 
locked and assigned to a single worker. The persistence guarantees that data 
won’t be lost. However, if a worker crashes, the entries might stay locked.

The second alternative uses a queue as described previously. A simple way to 
implement this would be to use Java Concurrency Framework’s Executors. Events 
would be modeled as Java Runnable tasks. By using a Redis database and 
[Redisson](https://github.com/redisson/redisson/wiki/9.-distributed-services) 
library a distribution and persistence layer could be added to this approach.

![](http://www.plantuml.com/plantuml/svg/nLBBJiCm4BpxArRSGmN77hZm40YaLj3Uk3ZnLhNgsEdkfXL_ZnyffAM28paaaMTcTsVMdIQnZUsq5xWmMdIj6Iz0CvXQTMEGKEW3jZts1M-tv_1ze0rI9kI3OqumkbgjBLMCjBDDAHGFZrdpnrV8IBScfI8CrKtwXodCbmkOrKcjnGQlb7O2t3SDCYl8f2gIasOAisU5ZX6kBc6bdQjriu6zbJLeE3Y53eaj2_e6etxePUgeVjCxZ3hhq03kq4l4NGWTBBrO1sb8Gkw2DvmwGNnA18S2y-0Sh08T5U4pn9SG1pk75XktX5hmo7PY_PfB8fo6CNdoWkIrWtkYG9E5RJ7qCYvMEcTovcGiHJmOF9ty3Xt6qGxn0EIylwMd_ALtNruBtnjLtbbv_NDJ-SBz7VrWeDscpVLrQsLeJHWhSEaMqP79Xz5Qm39m_Bvx2S2XnU4Y_yy-SXxUV080)
