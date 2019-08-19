# Glossary
Explanations of terms used within the book.

## Cache
The [Cache] saves received information from the Discord-API and enables us to access it, avoiding API-requests. It does not cache messages by default.

To learn more about the cache, read the [Cache-chapter].

## Rate Limit
Every API-interaction has a limited amount of usages within a certain time frame. These limitations are dynamically sent as response after issuing an API-request.

## CacheRwLock
A newtype wrapping around an inner cache to be allowed implementing [AsRef] on parking_lot's RwLock<Cache>.
It automatically dereferences to RwLock<Cache> and thus behaving as such.

## HTTP-Client
Communicates with Discord's REST-API.

## REST-API
The bot can issue requests via this to e.g. send messages. An error can occur when the request lacks permission or sent JSON is malformed. Serenity makes sure the latter won't happen, if it does, it's a bug.

## Gateway
A bot connects to Discord via shards to the gateway. Bots receive their events from this.

## Shard
A shard is a connection to Discord. There is a recommended/mandatory amount of shards dictated by Discord based on how many guilds your bot is serving (2,500 guilds per shard).
Each connection handles events and commands for its assigned guilds.

To learn more about sharding, read Serenity's [sharding-documentation].

## Sharding
The process of composing the Discord bot's connection into multiple shards. Serenity automatically shards by default, this can be changed.

To learn more about sharding, read Serenity's [sharding-documentation].

## Command Framework
There is a [Framework]-trait, it provides the basic concept on how a utility can be built around the Discord API.

## Standard Framework
Serenity's own provided implementation to utilise the Discord API.
It takes receiced Discord messages, parses them, and acts accordingly.
Users can define the actions done by the framework. Commands can be defined or dispatch failure handled.

## Events
Describes special circumstances defined by either Discord or Serenity.
Discord's circumstances cause data to be sent. These contain information about what or who did something.
A few examples are: A member joined a guild, a message has been sent or deleted, a shard has connected, ...\

## Guild
Often called servers. Discord refers to them as guilds. The technical difference is that a hardware server potentially serves multiple guilds, turning guild into another word for a community.

## Member
A Discord user who is part of a guild is considered a member of given guild.

[AsRef]: https://doc.rust-lang.org/std/convert/trait.AsRef.html
[Cache]: https://docs.rs/serenity/0.6/serenity/cache/struct.Cache.html
[Cache-chapter]: ../features/context/cache.md
[Framework]: https://docs.rs/serenity/0.6/serenity/framework/trait.Framework.html
[sharding-documentation]: https://docs.rs/serenity/0.6/serenity/gateway/index.html#sharding