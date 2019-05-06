<link rel="stylesheet" href="../../../css/span.css">

# Why should I switch to v0.6

* Rustls (default) and new OpenSSL-support.
* Proc-Macro Command Framework.
* Nested Groups.
* Unicode-Support for Commands.
* No more globale HTTP/Cache.
* Slow Mode, News Channel, ...
* Voice on Windows

The 0.6-series brings a lot of new changes, ranging from a brand new
framework and many new internal dependencies. From now on, we are using Rustls
by default instead of OpenSSL. Especially if you suffered from the outdated
OpenSSL-struggle, you may take the opportunity and upgrade. Rustls supports most
targets, if you need some exotic platform, like POWER9, 0.6 supports
a fresh OpenSSL.\
One outstanding improvement is the new framework, instead of using
macros to define your commands and then specifying its options on the
configuration, we annotate functions with procedural macros.

```rust,
#[command]
#[aliases("kitty", "neko")]
#[bucket = "emoji"]
#[required_permissions("ADMINISTRATOR")]
fn cat(ctx: &mut Context, msg: &Message) -> CommandResult {
    if let Err(why) = msg.channel_id.say(&ctx, ":cat:") {
        println!("Error sending message: {:?}", why);
    }

    Ok(())
}
```
<span class="caption">Listing 1.1: Proc-macro Command Example</span>

Maybe your sharp eyes have found the next big change already: We no longer
have a global HTTP-client or Cache. Take a look at the `say`-method, its first
argument is `&ctx`, which is just a usual `Context`, the method will take the
underlying `Http`-struct and use that.\
So yes, starting with 0.6, we are
responsible for keeping the Cache and HTTP-client around.
This gives you the freedom of having as many bots in one application as wanted.
