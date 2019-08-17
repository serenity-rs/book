<link rel="stylesheet" href="../../../css/span.css">

# Why should I switch to v0.6

* Rustls (default) and fixed OpenSSL-support
* Less RAM usage
* Proc-Macro Command Framework
* Nested Groups
* Unicode-Support for Commands
* No more global HTTP/Cache
* Voice on Windows
* Slow Mode, News Channel, Guild Boost-fields, ...

The 0.6-series brings a lot of new changes, ranging from a brand new
framework to many new internal dependencies. From now on we are using Rustls
by default instead of OpenSSL.\
Especially if you suffered from the outdated
OpenSSL-struggle, you may take the opportunity and upgrade. Rustls supports most
targets, if you need some exotic platform, like POWER9, 0.6 supports
a fresh OpenSSL too.\
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
<span class="caption">Listing 1: Proc-macro Command Example</span>

The book will guide you through the process.
