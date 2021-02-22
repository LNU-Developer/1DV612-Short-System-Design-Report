<img src="BD-1-AG.png" width="200">

# KickAssProject

[[_TOC_]]

## Explaining the business case
Due to the popularity of the mobile game [Star Wars Galaxy of Heroes](https://www.ea.com/games/starwars/galaxy-of-heroes) and the game developers not prioritizing certain player needs, the author of this design project identified a few areas that could help the whole player base. Back in July 2020 a work began of reverse engineering the game to understand how packages are sent between the client and game server to see what kind of data could be retreived through an self-developed backend. After many hours of frustration reading up on [serilization](https://en.wikipedia.org/wiki/Serialization), [protobuffers](https://developers.google.com/protocol-buffers) and [RPCs](https://en.wikipedia.org/wiki/Remote_procedure_call) the project KickAssProject was born to help the general player base of the game.

### Game mechanics to expand upon
The game has different arenas where players that belong to the same "shard" (a bracket of around 50 000 players) can combat other players to get a better arena placement. At a certain point in time a payout is made based on the current arena placement. There is therefore an incentive to have the best arena placement to get the biggest reward. This payout-time is different for each player (but different payout times can be shared between persons causing them to compete), and there is no way of telling who has which payout through the game interface. This means that players that doesn't have close payout might harm other players that have a payout in the next hour. The player base quickly understood this and different shard chats were formed outside of the game to better synchronize and make sure that no-one is harmed in their own payout hour, maximizing the payouts for all players.

### Discord as the main communication method
Since the beginning Discord has been the main communication method for these shard chats and there have been different ways of displaying when everyone has their different payout times, but the most basic form was lists where everyone had to write their own payout time.

### How to help the player base
In order to help player base as much as possible a Discord bot was created, called "KickAss Payouts Bot", expanding upon the reverse-engineered solution, which was supposed to show live payouts data (updating every other minute) as well as direct message users whenever they lose rank in-game. Due to the author being junior at the time the solution might not have been the best one from both an architectural standpoint but also code quality. There are many other features to the original bot, but these two (Live Payout Data and DM on drop) the areas in scope for this course. The below pictures illustrates how the bot is functioning today where a message is updated for each shard chat to illustrate live payout data and a direct message is sent to a user on drop.

![Payout](KickAssPayoutBotPayout.PNG "Payout") ![DM](KickAssPayoutBotDm.PNG "DM")
## Architectural patterns
There are a lot to think about when deciding which architecture pattern to use for an application. Most of the time it would be recomendet to go for one of the predefined [Architecture Patterns](Architecture-Patterns), since not doing so could easily lead to the use of one of the well know anti-patterns (e.g. BBoM, or "Big Ball of Mud") [1].

Research was made, [here](Architecture-Patterns), around the different predefined patterns that can be used as well as the pros and cons of using these patterns. Looking at the needs of my application I quickly fell for the **layerd based design** toppled of with some **event-driven aspects** as well as one part of the application being a **microservice**. This was due to the challenges faced when building this application as a monolith. Some of the experienced problems were the fact that I had to poll the backend when to send a message and everything was beginning from the frontend in this aspect instead of originating from the backend.

By clearly seperating the application into layers (e.g. bot being the view and backend being a model/controller), I feel that the application has a better structure and divided responsibility. This will make the different parts smaller and easier to handle and debug [2]. I also like the fact that I would be able to avoid polling by instead opting for a more event-drive design [3]. I was planning to solve this through a websocket where the bot would connect to the backend and receive messages whenever it is suppose to act. This could be seen as a simplified handling of the observer pattern [3]. A few areas of the application cant avoid the polling scenario, this is due to the fact that there is no way to receive info on an event-basis from the game servers. The microservice that pulls game data will do this through the use of polling and keeping data up to date in the database, so whenever data is requested it is pulled from the database containing fresh data.

## Overall structure of the application

![Overall structure of the application](DesignReportOverviewOverApplication.PNG "Overall structure of the application")

The application will consist of four different virtual machines hosted on my own dedicated server that are communicating with each other in different ways. In its core an MVC architectural pattern has been choosen clearly seperating different virtual machines into either Model/Controller or View. A sense of event-driven architecture can also be seen since the backend (KickAssBackend) is suppose to communicate with primarily the discord view through a websocket, telling the application when to communicate either through editing a shard payout message or direct messaging a user that their rank has dropped. Secondly a website will be created where users can login through Discord OAuth and see all shards/servers that they have access to. Finally the application communicating with the game server (swArenaApi) can be seen as a microservice, as its sole purpose is to fetch data from the game server based on input, and it has a very standardized way of doing so (through websockets). Please see the below high level dataflow overview showing this.

![High level data flow](HDataflow.PNG "High level data flow")

## Frontend
The frontend consist of two different areas, one discord interface, where the user can login through the discord application, invite the KickAssPayoutsBot to their server and set up the bot to provide the data that is requested. Secondly a React.JS application is used to allow the user to login through Discord OAuth to show the same information as prodvided in the discord application.

The different views are communicating with the backend through web APIs and websockets as per the below endpoints (please see below under the backend section). Below an illustration of the frontend application can be shown.

![Frontend flows](FrontendFlows.PNG "Frontend flows")

### Discord flow

### Description
A bot is created in Node.JS using the popular Discord.JS framework, which is wrapping Discords own APIs to communicate to Discord easily (e.g. sending messages, receive and interpet messages etc). By creating a discord bot I can simulate a user that can communicate just like any other person. The focus of the application will lie in this part of the frontend.

### Frameworks and dependencies
Discord.JS will be used to easily handle discords complex API structure and to easily create a bot that can listen to and act upon commands of the user.

### Explaining the Discord View flow
1. The user logs into the application in Discord to identify itself as a specific user.
2. The user invites the bot using and invite link to its server to be able to communicate with the bot.
3. The user sets up the bot by entering different commands, like creating shards, adding users opting in for direct messages etc. The bot would listen for certain commands and send requests through to the backend using a rest API where data would be stored/processed to set up the bot for that specific shards purpose.
4. The bot is connected to the backend through a websocket and receives messages whenever it needs to act (e.g. update a message or send a direct message to a user).

### **Webbrowser flow**
### Description
A webpage is created using the popular React.JS framework to create a SPA application. The user is supposed to login using Discords OAuth service and by doing this information is sent to backend on which servers the user has access to (in order to show the correct shards that has been created through Discord). The webpage will not be focused on in this project, but rather demonstrating the important and powerful OAuth technology.

### Frameworks and dependencies

As mentioned above React.JS will be used to create a simple SPA application.

### Explaining the Webpage View flow
1. The user accesses the webpage.
2. The user logs in through Discord OAuth.
3. The user is displayed all shards and the current payout information for these shards.

## Backend

![Backend flows](BackendFlows.PNG "Backend flows")

### **KickAssPayoutsBackend**

### Description
A web API capable of persisting user preferences and user input to a MySQL database, as well as specifying data to be retreived by API. The backend should continuously poll the game servers through the swArenaApi websocket in order to save and store data needed to be provided. Whenever data is accessed through the API the most current data is sent back for that perticular request (being either Discord or web browser).

### Frameworks and dependencies
ASP.Net Core will be used as a web application framework in order to easily handle the creation and usage of APIs.
In order to satisfy an easy way of communicating with the MySQL database the MySQLConnector package has been choosen as a serve-side framework.
To create on the fly documentation around the web API created Swashbuckle.AspNetCore will be used. Swashbuckle is an open source project for generating Swagger documents for Web APIs that are built with ASP.NET Core.
A project SDK, Microsoft.NET.Sdk.Web, is used for easy build and deployment as a dotnet webapp.

### API
The backend has a few endpoints to handle the users request to add/remove and update data.

| Component | Endpoint | Method | Description |
|----------|----------|----------|----------|
| Oauth | /discord-login | GET   | User login through a redirect to Discord login |
| Oauth | /discord-redirect | GET | Callback redirect after login |
| Shards | /shards | GET | Fetch all shard information |
| Shards | /shards/{id} | GET | Fetch shard information for a specific ID |
| Shards | /shards/create | POST | Create a new shard |
| Shards | /shards/change | PUT | Change shard type |
| Shards | /shards | DELETE | Delete a shard with a specific ID |
| Players | /players/add | POST | Add a new player to a specific shard |
| Players | /players | DELETE | Delete a player from a specific shard |
| WebSocket | /websockets | GET | Connect to the websocket server |

### Persistent data
The value of this application comes through the consistant polling of current data which only few people can access through a computer backend. In order to support user preference as well as the added value of data history a database is needed to persist data. For this project a MySQL database has been selected as a database. Please see the attached database schema around how the database should be structured.
![Database schema](Database.PNG "Database schema")

### **swArenaApi**
### Description
An application built in Node.JS able to communicate with the Star Wars Galaxy of Heroes game servers using the RPC protocol. The application is wrapped around a websocket enabling fast and consistant communication with whichever service that connects to it (currently only the KickAssPayoutsBackend service). This project is the core of the architecture and quite a few trade secrets are stored within its source-code. As such this code will not be pushed to the repository in the final handin of the project.

### Frameworks and dependencies
In order to serilize and deserilize requests from and to the games servers the protobufjs framework is used to translate data structures into readable JSON objects.
As mentioned above the application is wrapped around a websocket, as such the ws dependency is used.
To create make calls to the games RPC node-fetch has been chosen as a dependency.

### **Data flow diagram**
Please see the attached schemas around the dataflow.

![Low level data flow](LDataflow.PNG "Low level data flow")

## References
[1] The Big Ball of Mud and Other Architectural Disasters, Jeff Atwood, 26 Nov 2007,  https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/
(accessed Feb. 20, 2021).

[2] The top 5 software architecture patterns: How to make the right choice, Peter Wayner,  https://techbeacon.com/app-dev-testing/top-5-software-architecture-patterns-how-make-right-choice (accessed Feb. 20, 2021).

[3] Understanding the Observer Design Pattern, Carlos Caballero, 22 Jan, 2021,
https://medium.com/better-programming/understanding-the-observer-design-pattern-f621b1d0b6c9 (accessed Feb. 20, 2021).