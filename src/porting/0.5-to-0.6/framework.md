<link rel="stylesheet" href="../../../css/span.css">

# Let's start porting!

We will take a look at the framework and all its lovely bits!

First and foremost, Serenity utilises procedural macros hence the documentation for the framework shifted to its own crate: [command_attr]. This is a Rust limitation, as procedural macros cannot reside in the same crate.

## Command Macro

You may have commands defined like this:

```rust
command!(cat(_ctx, msg, _args) {
    if let Err(why) = msg.channel_id.say(":cat:") {
        println!("Error sending message: {:?}", why);
    }
});
```
<span class="caption">Listing 1: An old framework command.</span>

And they might have a configuration as here:

```rust
.group("Emoji", |g| g
    .prefixes(vec!["emoji", "em"])
    .desc("A group with commands providing an emoji as response.")
    .default_cmd(bird)
    .command("cat", |c| c
        .desc("Sends an emoji with a cat.")
        .batch_known_as(vec!["kitty", "neko"])
        .bucket("emoji")
        .cmd(cat)
        .required_permissions(Permissions::ADMINISTRATOR))
    .command("dog", |c| c
        .desc("Sends an emoji with a dog.")
        .bucket("emoji")
.cmd(dog)))
```
<span class="caption">Listing 2: An old framework configuration.</span>

Let's get rid of all this old cruft. Starting with your command-macro:

```rust
// To get these macros:
use serenity::framework::standard::macros::{command, group};

group!({
    name: "general",
    options: {},
    commands: [cat]
});

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
<span class="caption">Listing 3: A group-macro and its command.</span>

In v0.6, we will no longer register groups and commands on the
framework, but we register groups only. These groups will reference the commands. In v0.5, Serenity silently added one group containing all commands, the "ungrouped"-group, while v0.6 gives you the full control to do this yourself.

The `group!`-macro creates a static group for us, to be precise, it creates an uppercase identifier: `{name}_GROUP`.
Hereby note `{name}` is a placeholder  that will transform into the specified name via `name: "general"`, finally giving us `GENERAL_GROUP`.

Adding this group to the framework is done by calling `group(&GENERAL_GROUP)`:

```rust
StandardFramework::new()
    .configure(|c| c
        .with_whitespace(true)
        .on_mention(Some(bot_id))
        .prefix("~")
    .group(&GENERAL_GROUP)
);
```
<span class="caption">Listing 4: A framework configuring a group.</span>

Nonetheless, this might impose a problem: If the group's name is outside of the allowed character range for Rust identifiers, what are we going to do?\

TODO

## Group Options

As in listing 3, we define groups via the `group!`-command. The macro allows us to specify a group's settings.

```rust
group!({
    name: "emoji",
    options: {
        prefixes: ["emoji", "em"],
        description: "A group with commands providing an emoji as response.",
        default_command: bird,
    },
    commands: [cat],
    sub_groups: [],
});
```
<span class="caption">Listing 5: A group with some options.</span>

The first option is `prefixes`, it sets valid prefixes before a command. In 1.5 we could use `emoji cat` or `em cat` and both will be accepted.

There is a great variety of possible options exceeding the scope for this chaper, for further information read the [group command appendix].

## Help Command

0.6's help-system works a bit differently than before. To declare a
help-command, we need to annotate an extra step function with `#[help]`,
just like we did with `#[command]`.

First we will create a normal function that calls `with_embeds` or `plain`
underneath. You can imagine it as a step before the actual dispatch happens the
function. We need to do this because we cannot annotate the actual help-function
via attributes as they are already defined within Serenity.
A positive side-effect is the power to clone and mutate `HelpOptions` before we call the help-functionality.
We could randomise a field or let it depend on `Context`'s data.

The performant alternative to this would be to define our own `plain` or `with_embeds` and
mutate `HelpOptions` in them. This saves us a clone but forces us to maintain
the help-function from now on.

```rust
#[help]
#[individual_command_tip =
"Hello! こんにちは！Hola! Bonjour! 您好!\n\
If you want more information about a specific command, just pass the command as argument."]
#[command_not_found_text = "Could not find: `{}`."]
#[max_levenshtein_distance(3)]
#[indention_prefix = "+"]
#[lacking_permissions = "Hide"]
#[lacking_role = "Nothing"]
#[wrong_channel = "Strike"]
fn my_help(
    context: &mut Context,
    msg: &Message,
    args: Args,
    help_options: &'static HelpOptions,
    groups: &[&'static CommandGroup],
    owners: HashSet<UserId, impl BuildHasher>
) -> CommandResult {
    help_commands::with_embeds(context, msg, args, help_options, groups, owners)
}
```
<span class="caption">Listing 6: Function dispatching to help-function
`with_embeds`.</span>

As with our commands, we define options via attributes. A new addition is
`indention_prefix`, this provides visual aid and will indent a command based
one its nest-level. The level increases for each time we create a sub-group.\
Discord supports no real indenting, not even tabs or leading
whitespaces. If a group has a sub-group, all the sub-group's items will be
prefixed with the symbol we decided to use. Using an empty string will lead to
no prefix.

This is not everything though, the help-command can properly
work with `owners` now too and even incorporate whether a Serenity user-check shall be run or not.

## Dispatch
If a user dispatches a command, it's important to note that Serenity's framework starts with the highest group and then proceeds to the sub-groups. That means, if the command is part of a sub-group, the user will need to pass all requirements of super or parent groups. As a consequence, if the first group requires admin permission, all of its commands and sub-groups with their commands will require the given permission too.\
The help-system copies this behaviour.

## Checks

### Return Values

Checks used to return a bool expressing success or failure.
In spite of that, we never knew why a check failed and this has been improved in v0.6.
Here is an example from v0.5:

```rust
fn admin_check(_: &mut Context, msg: &Message, _: &mut Args, _: &CommandOptions) -> bool {
    if let Some(member) = msg.member() {

        if let Ok(permissions) = member.permissions() {
            return permissions.administrator();
        }
    }

    false
}
```
<span class="caption">Listing 7: Admin permission framework test as v0.5 check.</span>

Note that checking for admin-permission is considered redundant via checks, but they perfectly portray different nuances of potential failure.\
Let's take a look how we would do this in v0.6:

```rust
#[check]
#[name = "Admin"]
#[check_in_help(true)]
#[display_in_help(true)]
fn admin_check(ctx: &mut Context, message: &Message, _: &mut Args, _: &CommandOptions) -> CheckResult {
    if let Some(member) = message.member(&ctx.cache) {

        if let Ok(permissions) = member.permissions(&ctx.cache) {
            return permissions.administrator().into();
        }
    }

    CheckResult::new_user("Please use this command within a guild.")
}
```
<span class="caption">Listing 8: Admin permission framework test as v0.6 check.</span>

This allows us to describe what kind of failure we are dealing with.
There are five ways to return from v0.6 checks.

* A simple bool, a bool will be converted into a `CheckResult` without a message attached, we simply need to call `into()` on it.
* `CheckResult::Success`, to express success.
* `CheckResult::new_log("")`, allowing us to describe a general logging case.
* `CheckResult::new_user("")`, giving us the chance to send tips to the user on failure cause's reasoning.
* `CheckResult::new_unknown()`, in case there is nothing to be said or known about the cause.

Returning `CheckResult::new_unknown()` is also what happens when we call `false.into()`, as there is no information to be given.

### Check Macro

```rust
#[check]
#[name = "Admin"]
#[check_in_help(true)]
#[display_in_help(true)]
```
<span class="caption">Listing 9: Admin check's macros.</span>

Let's process this from top to bottom. First we get to gaze at the procedural macro to mark a function as a check. Make sure that the function's signature matches how Serenity defines the [CheckFunction].

```rust
type CheckFunction = fn(_: &mut Context, _: &Message, _: &mut Args, _: &CommandOptions) -> CheckResult;
```
<span class="caption">Listing 10: Serenity's [CheckFunction].</span>

[command_attr]: https://docs.rs/command_attr/0.1.4/command_attr/
[group command appendix]: ../../appendix/group-options.md
[CheckFunction]: https://docs.rs/serenity/0.6/serenity/framework/standard/type.CheckFunction.html
