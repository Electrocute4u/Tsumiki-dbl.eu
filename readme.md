# tsumiki-dbl-eu
A rewritten and **highly improved npm package** for posting bot stats to **discord-botlist.eu** API endpoint. \
Mainly rewritten for [Tsumiki](https://github.com/Electrocute4u/Tsumiki), a Discord Bot written in Javascript.

**This npm package can be used by anybody and is not just made for** [Tsumiki](https://github.com/Electrocute4u/Tsumiki).
This is a better and more improved version of [discord-botlist-api](https://www.npmjs.com/package/discord-botlist-api) npm package. \
All credit where credit is due to the initial framework layed out by [lptp1](https://www.npmjs.com/~lptp1), which I reworked to some extent.

I've also added a more *suitable documentation* for making sure even the newest developer can follow through.

**Installation**:
To install the npm package: do the following command in a terminal: \
```npm i tsumiki-dbl-eu```

## Table of contents
<details>
<summary>"Click to expand"</summary>
 
- [Dependencies](#dependencies)
- [Autopost Options](#autopost-options)
- [Manual post Options](#manual-post-options)
- [Supported Autopost Intervals](#supported-autopost-intervals)
- [Example for a Discord bot with a client](#example-for-a-discord-bot-with-a-client)
  - [Automatically post to the API](#automatically-post-to-the-api)
  - [Manually post to the API](#manually-post-to-the-api)
  - [Get Votes](#get-votes)
    - [The returned Vote object](#the-returned-vote-object)

</details>


## Dependencies
- `request` v^2.88.2 ([LINK](https://www.npmjs.com/package/request))
- `tough-cookie` v^4.0.0 ([LINK](https://www.npmjs.com/package/tough-cookie))
- `websocket` v^1.0.32 ([LINK](https://www.npmjs.com/package/websocket))

## Autopost Options

| FIELD          | TYPE          | DESCRIPTION | DEFAULT |
| :------------- |:-------------:|:-----------:|:-------:|
| client      | `string` | The client that you initialize discord with. (e.g bot, client etc) |
| postTime           | `string?`      |   A [supported interval](#supported-autopost-intervals) format (e.g 5min, 1h, 12hours etc) | 1 hour (60 min)
| log           | `boolean?`      |   Whether or not to log when it posts to the API every x. | true
| date      | `boolean?`     |    Whether or not to include the date in the console.log message if "log" is set to true. | false

## Manual post Options
| FIELD          | TYPE          | DESCRIPTION |
| :------------- |:-------------:|:-----------:|
| serverCount      | `Object` | The client server cache size or guilds size, depending on version |
| shardCount           | `Object?`      | The client shard cache size or shard size, depending on version |

**On v12:** you can use `<client>.guilds.cache.size` and  `<client>.shards.cache.size` \
**On v11 or below:** use `<client>.guilds.size` and  `<client>.shards.size`

## Supported Autopost Intervals
These are the supported auto post intervals and their respective aliases that you can use for postTime.

**5 Minutes:** \
`5` , `5min`, `5m`, `5minutes`, `05m`, `05min`, `05minutes`, `five`, `five minutes`

**15 Minutes:** \
`15` , `15min`, `15m`, `15minutes`, `fifteen`, `fifteen minutes`

**30 Minutes:** \
`30` , `30min`, `30m`, `30minutes` `thirty`, `thirty minutes`

**60 Minutes (1 hour):** \
`1` , `1h`, `1 hour`, `01h`, `one hour`, `60`, `60m`, `60min`, `60 minutes`, `sixty`, `sixty minutes`

**2 Hours:** \
`2` , `2h`, `2 hours`, `02h`, `two hours`

**3 Hours:** \
`3` , `3h`, `3 hours`, `03h`, `three hours`

**4 Hours:** \
`4` , `4h`, `4 hours`, `04h`, `four hours`

**5 Hours:** \
`5` , `5h`, `5 hours`, `05h`, `five hours`

**10 Hours:** \
`10` , `10h`, `10 hours`, `10h`, `ten hours`

**12 Hours:** \
`12` , `12h`, `12 hours`, `12h`, `halfday`, `half-day`, `twelve hours`

**24 Hours:** \
`24` , `24h`, `24 hours`, `24h`, `1day`, `1d`, `entire day`

## Example for a Discord bot with a client
### Automatically post to the API
```javascript
// Calling in discord.js and creating the client for it
const Discord = require('discord.js')
const client = new Discord.Client();

// Calling in the module and creating the client
const dbeu = require('tsumiki-dbl-eu')
const dbapi = new dbeu.Client();

// Your discord-botlist-eu token:
const dbl_token = "Your-api-token-here"

// This is the discord.js client ready state.
// Important to run the code within this or after the ready state.
client.on("ready", async () => {

// Important this is called within the ready or after the ready state.
// Otherwise it'll return null/error the application out.
const botID = client.user.id

// The dbapi client will emit a ready event when it has sucessfully logged in to the API.
dbapi.on("ready", () => {

     // Posts to console when the bot has sucessfully connected.
    console.log("Sucessfully logged in to: Discord-botlist.eu API")
    
    // Automatically posts to the botlist API every x with logging set to "true" with fancy date set to "true"
    dbapi.autoPost(client, "12 hours", true, true)

    // This will log to the console every 12 hours that it has posted to the botlist with a fancy date (converted to GMT+1)
});

    // Logging in to the API
    dbapi.login(dbl_token, botID)
});
```
### Manually post to the API
```javascript
// Calling in discord.js and creating the client for it
const Discord = require('discord.js')
const client = new Discord.Client();

// Calling in the module and creating the client
const dbeu = require('tsumiki-dbl-eu')
const dbapi = new dbeu.Client();

// Your discord-botlist-eu token:
const dbl_token = "Your-api-token-here"

// This is the discord.js client ready state.
// Important to run the code within this or after the ready state.
client.on("ready", async () => {

// Important this is called within the ready or after the ready state.
// Otherwise it'll return null/error the application out.
const botID = client.user.id

// The dbapi client will emit a ready event when it has sucessfully logged in to the API.
dbapi.on("ready", () => {

     // Posts to console when the bot has sucessfully connected.
    console.log("Sucessfully logged in to: Discord-botlist.eu API")
    
    // This set's the post interval.
    // replace "30000" with your own time, remember that it is in ms (milliseconds)
    setInterval(() => {
    
    // Tip: If you haven't sharded yet, don't include the shard property (or leave it as 0).

    // Use this when on v12 or above:
    dbapi.postData(client.guilds.cache.size, client.shards.cache.size)
    
    // Use this when on v11 and below:
    dbapi.postData(client.guilds.size, client.shards.size)
    
    console.log("Sucessfully posted to Discord-botlist.eu!")
    }, 30000);
})
    // Logging in to the API
    dbapi.login(dbl_token, botID)

});
```
## Get votes
```javascript
dbapi.on("vote", vote => {
    console.log(vote)
})
```
### Getting all bot votes example
```javascript
// Calling in discord.js and creating the client for it
const Discord = require('discord.js')
const client = new Discord.Client();

// Calling in the module and creating the client
const dbeu = require('tsumiki-dbl-eu')
const dbapi = new dbeu.Client();

// Your discord-botlist-eu token:
const dbl_token = "Your-api-token-here"

// This is the discord.js client ready state.
// Important to run the code within this or after the ready state.
client.on("ready", async () => {

// Important this is called within the ready or after the ready state.
// Otherwise it'll return null/error the application out.
const botID = client.user.id

// The dbapi client will emit a ready event when it has sucessfully logged in to the API.
dbapi.on("ready", () => {

     // Posts to console when the bot has sucessfully connected.
    console.log("Sucessfully logged in to: Discord-botlist.eu API")
    
    // Do something with ready event ...
})

dbapi.on("vote", vote => {
    console.log(vote)
    // Do something with vote ...
})

dbapi.login(dbl_token, botID)
```
### The returned Vote object
```javascript
botID: "some-bot-id",
user: {
    "name": "VOTED_USER_NAME",
    "id": "VOTED_USER_ID",
    "else": "OTHER_STUFF"
}
```