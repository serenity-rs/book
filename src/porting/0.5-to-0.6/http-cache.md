<link rel="stylesheet" href="../../../css/span.css">

# Http/Cache
You might have read that we longer have a global HTTP-client
or cache. This is a fundamental design change in Serenity.\
Functions used to magically pull in the global HTTP-client and looked like this:

```rust
let _ = message.channel_id.say("Ferris!");
```
<span class="caption">Listing 1: Methods using global HTTP.</span>

Whilst very comfortable, we came to the conclusion to free our design from singletons. Hence, every method that once used to utilise either
the cache or the HTTP-client will from now on expect the resources to be
passed.

The methods will expect one of the following as first argument: `impl AsRef<Http>`, `impl AsRef<CacheRwLock>`, or `impl CacheHttp`.

## `impl AsRef<Http>`
```rust
    #[cfg(feature = "http")]
    #[inline]
    pub fn create_permission(&self, http: impl AsRef<Http>, target: &PermissionOverwrite) -> Result<()> {
        self.id.create_permission(&http, target)
    }
```
<span class="caption">Listing 2: `ChannelCategory`'s method requiring an HTTP-client.</span>

A type able turning itself into a reference to an `Http`.
Types qualifying are `Arc<Http>`, being the `http`-field on `Context` and `Context` itself.

## `impl AsRef<CacheRwLock>`
We cannot not implement `AsRef` for `RwLock<Cache>`. `CacheRwLock` automatically
dereferences into `Cache`, offering the same usability as with working on the actual
type.

## `impl CacheHttp`
Some methods can utilise the cache if enabled. This type allows to express both types may be required. The trait is implemented for `Context`, but also for `(CacheRwLock, Http)`. The tuple allows us to use pass the combination of both outside places with a passed `Context`.
You may wonder, why is this type not `AsRef<CacheHttp>`?
It primarily exists to express `CacheRwLock, Http` outside of the scope of Serenity functions, however the tuple could not turn into a reference.