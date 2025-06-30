# Web Application Layout
## Web Application Infrastructure
The most common ones can be grouped into the following four types:

- `Client-Server`
- `One Server`
- `Many Servers - One Database`
- `Many Servers - Many Databases`
### Client-Server
A server hosts the web application in a client-server model and distributes it to any clients trying to access it.
![[client-server-model.jpg]]
### One Server
In this architecture, the entire web application or even several web applications and their components, including the database, are hosted on a single server. Though this design is straightforward and easy to implement, it is also the riskiest design.

If any web application hosted on this server is compromised in this architecture, then all web applications' data will be compromised.
![[one-server-arch.jpg]]
### Many Servers - One Database
This model separates the database onto its own database server and allows the web applications' hosting server to access the database server to store and retrieve data.
![[many-server-one-db-arch.jpg]]
### Many Servers - Many Databases
![[many-server-many-db-arch.jpg]]
## Web Application Components
4 components:
1. `Client`
2. `Server`
    - Webserver
    - Web Application Logic
    - Database
3. `Services` (Microservices)
    - 3rd Party Integrations
    - Web Application Integrations
4. `Functions` (Serverless)
## Web Application Architecture
Divided into three different layers (AKA Three Tier Architecture).

|**Layer**|**Description**|
|---|---|
|`Presentation Layer`|Consists of UI process components that enable communication with the application and the system. These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS.|
|`Application Layer`|This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.|
|`Data Layer`|The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.|
### Microservices
Independent components of the web application, which in most cases are programmed for one task only.

For example, for an online store, we can decompose core tasks into the following components:
- Registration
- Search
- Payments
- Ratings
- Reviews

The communication between these microservices is `stateless`, which means that the request and response are independent. This is because the stored data is `stored separately` from the respective microservices.
### Serverless
Cloud providers such as AWS, GCP, Azure, among others, offer serverless architectures. These platforms provide application frameworks to build such web applications without having to worry about the servers themselves. These web applications then run in stateless computing containers (Docker, for example).
