<div style="display:inline-block;width:100%">
    <img src="../images/logo.png" alt="Logo" width="84px" style="float:left;margin-right:25px;border-radius: 50%;"/>
    <h1>Serenity Discord Library</h1>
</div>

## Introduction

Glad you made it here, let's get started!

This book will teach you everything you need to know about building a Discord bot with [`Serenity`], a library written in [`Rust`]. A fast and safe system programming language with great package manager, Cargo.

The library provides you all of Discord's REST-requests and even an optional bot-framework to quickly get a bot done. It allows you to easily define and configure commands.


## Getting Started
* Library Dependencies
* Internal Concepts
    * Build Features
    * Client and its Context
    * Http
    * Cache
    * Sharding
    * Events
    * Command Framework
    * Voice
    * Other Utility
* Building a Bot
    * Pong
    * Database
    * Voice
* Common Issues
* Glossary


## Why Serenity?

First of all, [`Serenity`] is written in a safe, fast, and modern language: [`Rust`].

[`Rust`] runs without garbage collector, guarantees memory safety, provides easy multithreading without data races, offers a special error-handling without exceptions, and comes with _Cargo_.

Cargo easily helps you starting a new project and manages tons of packages to extend your bot with a database, HTTP-client, file-parsers, ... on a whim.
Ranging from downloading packages, updating them, and compiling your bot, Cargo got you covered.

[`Serenity`] itself offers you:

* **Caching**: Avoid issuing HTTP-calls, compare old with edited messages, and check all the data your bot knows about. The cache is, as many parts, optional.

* **Automated Sharding**: Sharding *just works* but can be done manually as well!

* **Low RAM usage**: [`Serenity`]'s base barely scratches 1.6 MB! Serving up to 3000 of guilds with just 200 MB.

* **All Discord-features**: We support all REST and WebSocket features, including Voice on all platforms.

* **No OpenSSL needed**: As of version 0.6.0, OpenSSL got replaced by a Rust TLS alternative, [`Rustls`], but OpenSSL can still be used in case you want to build for an architecture like Power9.

* **Cross-platform code**: [`Serenity`] in its entirety builds on:

Architecture | `Linux` | `Windows` | `macOS`
---          | ---   |  ---    | ---
x86_64       | ✔     | ✔      | ✔
AArch64      | ✔     | ?      | ?
ARMv6        | ✔     | ?      | ?
ARMv7        | ✔     | ?      | ?
POWER9       | ✔     | ?      | ?

* **Nice Community**: Our Discord-chat is an active bundle of friendly people ready to help you with problems concerning _Rust_ or [`Serenity`].

[`Serenity`]: https://github.com/serenity-rs/serenity
[`Rust`]: https://www.rust-lang.org/
[`Rustls`]: https://github.com/ctz/rustls