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
In order to help player base as much as possible a Discord bot was created, called "KickAss Payouts Bot", expanding upon the reverse-engineered solution, which was supposed to show live payouts data (updating every other minute) as well as direct message users whenever they lose rank in-game. Due to the author being junior at the time the solution might not have been the best one from both an architectural standpoint but also code quality. There are many other features to the original bot, but these two (Live Payout Data and DM on drop) the areas in scope for this course.

![Payout](KickAssPayoutBotPayout.PNG "Payout") ![DM](KickAssPayoutBotDm.PNG "DM")

## Overall structure of the application

![Overall structure of the application](DesignReportOverviewOverApplication.PNG "Overall structure of the application")

The application consist of four different virtual machines that are communicating with each other in different ways.

## Frontend

## Backend