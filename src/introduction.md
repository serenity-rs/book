<link rel="stylesheet" href="../css/span.css">

<div style="display:inline-block;width:100%">
    <img src="../images/logo.png" alt="Logo" width="84px" style="float:left;margin-right:25px;border-radius: 50%;"/>
    <h1>Serenity Discord Library</h1>
</div>

## Introduction

Glad you made it here, let's get started!

This book will teach you everything you need to know about building a Discord
bot with [Serenity], a library written in [Rust]. A fast and safe systems
programming language with a great package manager, Cargo.

The library provides you all of Discord's REST-requests and even an optional
bot-framework to quickly get a bot done. It allows you to easily define and
configure commands.

Last but not least, this book will expect you to know certain Rust fundamentals.
Therefore, if you want to learn Rust, check out [the official book].
Nonetheless, we will reference what's *good to know* at the beginning of each
chapter.

## Why Serenity?

First of all, [Serenity] is written in a safe, fast,
and modern language: [Rust].

[Rust] runs without a garbage collector, guarantees memory safety,
provides easy multithreading without data races,
offers a special error-handling system without exceptions, and comes
with _Cargo_.

Cargo easily helps you start a new project and manages tons of packages
to extend your bot with a database, HTTP-client, file-parsers, ... on a whim.
Ranging from downloading packages, updating them, and compiling your bot,
Cargo got you covered.

[Serenity] itself offers you:

* **Caching**: Avoid issuing HTTP-calls, compare old with edited messages,
and check all the data your bot knows about. The cache is, as many parts,
optional.

* **Automated Sharding**: Sharding *just works* but can be done manually as
well!

* **Low RAM usage**: [Serenity]'s base barely scratches 1.6 MB with some bots
 serving 3000 guilds with just 200 MB.

* **All Discord-features**: We support all REST and WebSocket features,
including Voice on all platforms.

* **No OpenSSL needed**: As of version 0.6.0, OpenSSL got replaced by a
Rust TLS alternative, [Rustls], but OpenSSL can still be used in case you
 want to build for an architecture like POWER9.

* **Cross-platform code**: [Serenity] in its entirety builds minimum on:

Architecture | `Linux`                              | `Windows` | `macOS` | `FreeBSD`
---          | ---                                  |  ---    | --- | ---
x86_64       | ✔                                    | ✔      | ✔ | ✔
AArch64      | ✔                                    | ?      | ? | ?
ARMv6        | ✔                                    | ?      | ? | ?
ARMv7        | ✔                                    | ?      | ? | ?
POWER9       | ✔<sup id="footnote_id_power9">[1](#footnote_power9)</sup> | ?      | ? | ?
Android      | Termux

* **Nice Community**: Our [Discord-chat] is an active bundle of friendly
people ready to help you with problems concerning [Rust] or [Serenity].

<b id="footnote_power9">1</b> POWER9 currently only works with the `native_tls`
feature enabled because `rustls` depends on `ring`
which does not support POWER9. (See [ring/#389])[↩](#footnote_id_power9)

## Ferris, your helper

Throughout the awaiting best-seller book hides Ferris. A crusty crab providing
you with top-notch information. Every Ferris symbolises a certain meaning:

<span class="block">
<img hspace="10%" src="../images/curious.png" alt="Logo" width="84px"
style="float: left;margin-right: 25px"/>
Introduces insightful bonus information for curious
readers.
</span>

<span class="block">
<img hspace="10%" src="../images/dangerous.png" alt="Logo" width="84px"
style="float: left;margin-right: 25px"/>
Warns us about potential issues; always
recommended to read and follow to prevent nasty surprises.
</span>

[Serenity]: https://github.com/serenity-rs/serenity
[Rust]: https://www.rust-lang.org/
[Rustls]: https://github.com/ctz/rustls
[the official book]: https://doc.rust-lang.org/book/
[ring/#389]: https://github.com/briansmith/ring/issues/389
[Discord-chat]: https://discord.gg/eHpnFrm
