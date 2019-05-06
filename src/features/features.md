## Cargo Build Features

### Required Knowledge
[Cargo features], modules


### About
In this chapter we will take a look at what kind of features [Serenity] has and what they do.
[Serenity] is quite modular, each module represents a feature and you decide if you really want to compile it.

Also yes, turning a feature off reduces stops it from being compiled! Thus less time for preparing tea.

To start things off, here is an overview of Serenity's features:
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

Seems to be a lot, huh?
All of them but `voice` are enabled by default.
The next chapters will discuss each of these modules!

[Serenity]: https://github.com/serenity-rs/serenity
[Cargo]: https://doc.rust-lang.org/cargo/
[Cargo Features]: https://doc.rust-lang.org/cargo/reference/manifest.html#the-features-section
[Context-struct]: ./internal_dependencies/context.md
