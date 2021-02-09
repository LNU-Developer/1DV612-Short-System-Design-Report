# Task 1
- What does serverless architecture refer to?
  - Serverless architectures are application designs that incorporate third-party “Backend as a Service” (BaaS) services, and/or that include custom code run in managed, ephemeral containers on a “Functions as a Service” (FaaS) platform.
- What's the trend over the last years where a web application's code is placed?
  -
- What's a server-side web framework (a.k.a. "web application framework")?
  - Server-side web frameworks (a.k.a. "web application frameworks") are software frameworks that make it easier to write, maintain and scale web applications
- What can a server-side web framework do for you?
  - Web frameworks allow you to write simplified syntax that will generate server-side code to work with these requests and responses. This means that you will have an easier job, interacting with easier, higher-level code rather than lower level networking primitives
- What's the difference between an ORM and an ODM?
  - Two types of database set ups which are ORM (object Relation Mapping) and ODM (Object Document Mapping)
    ORM which is to map an object with a relational world, it basically converts data between incompatible types in object oriented programming languages. ORM wraps the implementation specific details of storage drivers in an API (application program interface), and maps the relational fields to an object members. For example if I have a table of employees, it is mapped to a single object for all employees, with various methods associated with it.
    ODM on the other hand is an Object Document Mapper, which maps objects with a Document Database like MongoDB.
- What factors may affect your selection of a server-side web framework?
  - Effort to learn
  - Productivity
  - Performance of the framework/programming language
  - Caching support
  - Scalability
  - Web security
- Whats server-side web frameworks are there for programming languages such as Phyton, Java, JavaScript, C#, PHP, Ruby, etc.?
  - Phyton - Bottle, BlueBream, CherryPy, CubicWeb, Django etc
  - Java - Brutos Framework, Eclipse RAP, Google Web Toolkit etc
  - JavaScript - Express.js, Ember.js, Next.js etc
  - C# - OpenRasta, Base One Foundation Component Library, MonoRail etc
  - PHP - Li3 (Lithium), CodeIgniter, Fat-Free etc
  - Ruby - PureMVC, Ruby on Rails etc
- Frameworks for persistent data?
- What frameworks are there for Node.js, except for Express?
  - AdonisJs
  - Meteor.js
  - NestJs
  - Sails.js
  - Koa.js
- Frameworks for persistent data?




# Task 2
Create an artifact describing the architecture of the server-side of the examination application. The following requirements must be met.

The documentation for your web application must be stored on the GitLab wiki associated with your students group of repos for this course.
The documentation must contain a picture describing the server-side subsystem.
Describe (data flow diagram?) how the data is processed.
If there is a need for persistent data on the server-side, it should be described.

## **Data flow diagram**
Please see the attached schemas around the dataflow.

## **KickAssPayoutsBackend**

**Description**
<br/>
A web API capable of persisting user preferences and user input to a MySQL database, as well as specifying data to be retreived by API. The backend should continuously poll the game servers through the swArenaApi websocket in order to save and store data needed to be provided. Whenever data is accessed through the API the most current data is sent back for that perticular request (being either Discord or web browser).

**Frameworks and dependencies**
<br/>
In order to satisfy an easy way of communicating with the MySQL database the Microsoft.EntityFrameworkCore has been choosen as a serve-side framework. Entity Framework (EF) Core is a lightweight, extensible, open source and cross-platform version of Entity Framework data access technology. EF Core can serve as an object-relational mapper (O/RM).
- Enables .NET developers to work with a database using .NET objects.
- Eliminates the need for most of the data-access code that typically needs to be written.
To create on the fly documentation around the web API created Swashbuckle.AspNetCore will be used. Swashbuckle is an open source project for generating Swagger documents for Web APIs that are built with ASP.NET Core.
A project SDK, Microsoft.NET.Sdk.Web, is used for easy build and deployment as a dotnet webapp.

**Persistent data**
<br/>
The value of this application comes through the consistant value of current data which only few people can access through a computer backend. In order to support user preference as well as the added value of data history a database is needed to persist data. For this project a MySQL database has been selected as a database. Please see the attached database schema around how the database should be structured.

## **swArenaApi**
**Description**
<br/>
An application able to communicate with the Star Wars Galaxy of Heroes game servers using the RPC protocol. The application is wrapped around a websocket enabling fast and consistant communication with whichever service that connects to it (currently only the KickAssPayoutsBackend service)

**Frameworks and dependencies**
<br/>
In order to serilize and deserilize requests from and to the games servers the protobufjs framework is used to translate data structures into readable JSON objects.
As mentioned above the application is wrapped around a websocket, as such the ws dependency is used.
To create make calls to the games RPC node-fetch has been chosen as a dependency.