<link rel="stylesheet" href="../../../css/span.css">

# Cache and Http

In v0.6 we got rid of the global Cache and Http. They allowed Serenity to
magically perform an HTTP-requests or cache-checks with neither the user knowing
nor them having to pass either to Serenity.

```rust
let _ = message.channel_id.say("Ferris!");
```
<span class="caption">Listing 1: Relying on the global Http.</span>

As a result, we need to pass these types manually to functions that require
either cache or HTTP.\
The functions in question will expect `AsRef<T>`, where `T` is either
`CacheRwLock` or `Http`.
`AsRef<T>` is also implemented for `Context` for both types.

```rust
let _ = message.channel_id.say(&context, "Ferris!");
let _ = message.channel_id.say(&context.http, "Ferris!");
```
<span class="caption">Listing 2: Manually passing HTTP.</span>

This comes with a limitation: Sometimes we are facing situations where do not
have a `Context`. It is recommended to clone `CacheRwLock` or `Arc<Http>` in
this case.

## Functions that need both Http and Cache

When a function must use the HTTP-client, but can also utilise the cache,
designing a simple API becomes trickier.

<span class="info">
<img hspace="10%" src="../../../images/curious.png" alt="Logo" width="84px"
style="float: right;margin-right: 25px"/>
For Serenity's design decision, there were multiple approaches considered. For one is to simply compile different APIs based on features, but features cannot gate function signatures. Another idea was to gate an inner portion of the function to an extra function that will only be invoked and compiled if the cache-feature is active.
<br>
Serenity settled for the trait-argument approach. It will accept a type implementing the `CacheHttp`-trait. This sweety *can* return a cache but must not.
<br>
It won't be able to do so in the first place if the cache-feature is not picked. Furthermore, if we pass `Http` only — which implements `CacheHttp` — it will return `None`.
<br>
This allows the user to decide if they want to run the function with or without cache-check even if they have the cache enabled.
</span>

Some functions will perform a cache-lookup when the `cache`-feature is enabled and therefore avoiding a potential REST-request.\
Such functions will expect a type implementing the `CacheHttp`-trait.

```rust
let _ = message.channel_id.say("Ferris!");
```
<span class="caption">Listing 1.2: A method expecting `CacheHttp`.</span>
