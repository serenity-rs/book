# Client

### Good to know:
`struct`.

### Quick overview:
* Contains cache and HTTP-client.

## About
What if we want a simple way of connecting to Discord? The `client`-feature
allows us to create a [Client]-instance and this struct keeps our connection to
Discord. This feature is enabled by default!

A client empowers us to issue authenticated REST-requests but also manage
gateway-connections to Discord, also known as shards.\
Oh, while we are talking about shards, shard-management is automatic by
default. Therefore, if your bot grows, Serenity will spin up more shards, each shard handles a certain set of guilds.

The concept of a client is represented with the [Client]-type in Serenity.\
There is no client limit, we can have as many bots per software application as our heart desires. However, this is not recommended as it can be complicated to manage, consider one bot per project.

## Construction

[Client]: https://docs.rs/serenity/0.6/serenity/client/struct.Client.html
