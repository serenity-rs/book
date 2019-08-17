<link rel="stylesheet" href="../../css/span.css">

# Cargo Build Features

### Required Knowledge
[Cargo features], modules


## About
In this chapter we will take a look at what kind of features [Serenity] has and
what they do.
[Serenity] is quite modular because of its module structure.

Each module can be toggled by setting the respective feature. Here is the list of module related features:
- `builder`
- `gateway`
- `client`
- `cache`
- `http`
- `model`
- `framework`
- `standard_framework`
- `utils`
- `voice`

These two features will handle how `http` and `gateway` will establish their TLS:
- `rustls_backend`
- `native_tls_backend`

Let's have a quick look on this TLS story.

[Serenity]: https://github.com/serenity-rs/serenity
[Cargo]: https://doc.rust-lang.org/cargo/
[Cargo Features]: https://doc.rust-lang.org/cargo/reference/manifest.html#the-features-section
[Context-struct]: ./internal_dependencies/context.md
