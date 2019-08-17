<link rel="stylesheet" href="../../css/span.css">

# Context

### Quick overview:
* Gives Cache and HTTP-client access.
* Can store your custom services, e.g. a database.
* Can communicate with current shard.
* Is passed to every section of Serenity.

## About
The context is a very import part of Serenity, but what exactly is it?


```rust
pub struct Context {
    pub data: Arc<RwLock<ShareMap>>,

    pub shard: ShardMessenger,

    pub shard_id: u64,

    #[cfg(feature = "cache")]
    pub cache: CacheRwLock,

    #[cfg(feature = "http")]
    pub http: Arc<Http>,
}
```
<span class="caption">Listing 2.3.1: Context's definition.</span>

This is a lot to chew at once, that's why the next chapters will explain every
field in-depth. The general idea is to provide one struct containing useful items around Serenity. One functionality is to issue a REST-request to Discord via `http`, or
fishing for messages in the `cache`.
