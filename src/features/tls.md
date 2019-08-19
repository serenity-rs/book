<link rel="stylesheet" href="../../css/span.css">

# Transport Security Layer (TLS)

The communication with Discord must be encrypted via TLS. Serenity supports two ways to get this job done and we need to pick *one*.
If we forget the pick one, Serenity will spit out a compile-error.

The `rustls_backend` is our new TLS implementation in pure Rust, but it does not
support platforms like POWER9 yet.\
Meanwhile, `native_tls_backend` is our legacy TLS and uses SChannel on Windows, Secure Transport on macOS, and OpenSSL on other platforms.\
OpenSSL is a C-library and may cause some headache if you want to cross-compile it.

The `rustls_backend` is enabled by default, it also allocates less memory. If we want all default features but `native_tls`, we can turn
default features off and use `native_default_features`.

```toml
[dependencies.serenity]
default-features = false
features = ["native_default_features"]
```

<span class="info">
<img hspace="10%" src="../../images/dangerous.png" alt="Logo" width="84px"
style="float: right;margin-right: 25px"/>
If we forget to turn off `default-features` we might hit bedrock. Cargo's features work
additively and will therefore pull in the default features *and*  `native_tls_backend`, ending up with both backends enabled.
<br>
As a conseqeuence, both crates will be built. However if our target system is unable to build
native-tls or Rustls, we will fail building Serenity too.
<br>
Final note: Having both features enabled is uncharted territory and thus undefined behaviour. It's tolerated to allow potential edge cases to compile.
</span>
