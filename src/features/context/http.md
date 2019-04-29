## Context

### Required Knowledge
Traits, `AsRef`, HashMap, crates, newtype-pattern, orphan-rule

### HTTP

### CacheRwLock
While a context has an `Http`-field it also has a mysteriously named field called `CacheRwLock`.
To keep it simple: It allows us to indirectly implement `AsRef` for `Arc<RwLock<Cache>>`.

If you want a bit more in-depth explanation: The orphan-rule dictates that traits can only be implemented for stdlib/core and your own crate only.
`CacheRwLock` is a newtype wrapping around `Arc<Rwlock<Cache>>` but also dereferencing to it via the `DerefMut`-trait and therefore keeping the same functionality. One could view it as a _neworphantype_, as it, primarily, satisfies the orpan-rule by creating a newtype with the same API as its underlying type.

### Passing Cache and HTTP around
Some functions may require you to pass `AsRef<Http>` or `AsRef<CacheRwLock>`, these functions will use the cache or HTTP-client to perform their operations.\
The reason these functions accept `AsRef`, instead of the actual type, is to accept `Context` as well.\
On the one side, `AsRef<Http>` is implemented for `Http`, so passing `&context.http` would succeed, but instead we can simplify this to `&context`.
On the other side, the same applies to `AsRef<CacheRwLock>` and `Cache`/`Context`: Instead of passing `&context.cache`, we simply pass `&context`.
