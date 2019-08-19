# The Command Framework

### Quick overview:
* A Serenity framework intends to handle commands.
* Since the [Framework] is a trait, you can easily create your own.
* There is a [StandardFramework]-implementation of the trait, providing a solution for many common needs.

## About

First of all, the framework is a trait

The command framework takes received messages and checks if they meet your set requirements. If everything is okay, the framework will dispatch the respective command.

Moreover, the framework is a trait that can be implemented by anyone.
However doing that can be a bit of work, that's why Serenity offers the standard framework!

It entails commands:
* groups
* sub-groups
* a help-system
* customised checks
* required roles/permissions
* user-ratelimiting

The next chapters will dive into each of these aspects.

[Framework]: https://docs.rs/serenity/0.6/serenity/framework/trait.Framework.html
[StandardFramework]: https://docs.rs/serenity/0.6/serenity/framework/standard/struct.StandardFramework.html