# Cache

### Quick overview:
* Stores data received from Discord.
* Cache is shared across all shards using RwLock.
* Can be accessed via [Context-struct].
* Serenity may check cache before issuing REST-requests.
* Increases needed memory.

## Details:
The `cache`-feature enables caching. A cache keeps all data received via REST/WebSockets and is shared across all shards. Moreover, the cache will know about all guilds your bot is in, but also about all users, roles, emojis, and other useful data.

If we want to access the cache, we will need a Context-struct.

Now imagine a function would perform a REST-request: Enabling the cache will avert this and your bot will check the cache first! Especially when you have bot-commands that would fetch a lot of guild-data, with the cache enabled,
The exception to this behaviour are functions inside the `http::raw`-module because they explicitly send REST-requests. That means, if you use these functions, the cache won't be checked.

If your bot is running on a low RAM machine you should consider whether this feature gives you enough benefits to reason the potential RAM overhead. A cache will grow and rarely gets rid of its entries.