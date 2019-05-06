<link rel="stylesheet" href="../../../css/span.css">

# Shard Messenger

### Good to know:
Sharding

#### Quick overview:
Enables to handle current gateway-connection

### Reference
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
<span class="caption">Listing 1.1: Context's definition.</span>

### Shard Messenger
Right after `data` comes the `shard`-field, a [ShardMessenger] communicates
with the related shard. To understand what *related* means, we need to grasp the following concept: When Serenity dispatches a command, we do not know on which
shard we are operating on. Luckily [Context] got us covered and provides us
with the needed information. It gives us the [ShardMessenger] in charge of
communicating with the right shard. That means if a we get an event for shard 5,
we would be provided with the messenger for shard 5 as well.

<span class="info">
<img hspace="10%" src="../../../images/curious.png" alt="Logo" width="84px"
style="float: right;margin-right: 25px"/>
If you do not understand what sharding is, this type might seem very confusing.<br>
One single shard can be simplified to a connection to Discord, each connection receives events from Discord.<br>
So, if we have two shards each will receive events for around one half of all
guilds. As a result, if we have 100 shards the guilds will distributed across those 100 shards.<br>
Moreover, Discord requires a maximum amount of 2,500 guilds per shard,
but fear not, Serenity handles this automatically.
</span>


### Shard Id
The third field on [Context], `shard_id`, is a plain number refering to the shard's ID. Thus it actually
just informs us on which shard the current thread is operating on. As the name
[Context] somewhat conveys, it provides us information about the current
circumstances, contextual background, just like a shard number would be.

[ShardMessenger]: https://docs.rs/serenity/0.6.0-rc.1/serenity/client/bridge/gateway/struct.ShardMessenger.html