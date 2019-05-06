<link rel="stylesheet" href="../../../css/span.css">

# Let's start porting!

We will take a look at the framework and all its lovely bits!

## Command Macro

You may have commands looking like this:

```rust
command!(cat(_ctx, msg, _args) {
    if let Err(why) = msg.channel_id.say(":cat:") {
        println!("Error sending message: {:?}", why);
    }
});
```
<span class="caption">Listing 1.1: An old framework command.</span>

And they might have a configuration as in this listing:

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
<span class="caption">Listing 1.2: An old framework configuration.</span>

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
<span class="caption">Listing 1.3: A group-macro and its command.</span>

In 0.6, we will no longer register groups and commands on the
framework, but groups only. However, groups will reference its commands.

From now on, we define a command's options with `#[{option}{value}]`, where
`{option}` will always be the *name*, as in `bucket` or `required_permissions`,
while `{value}` will be required to be `= "emoji"`, for single values,
or `("kitty", "neko")` for multiple ones. Whenever an option could have multiple
values, the latter one applies.

One last step, adding the group to the framework. In order to understand the
following listing, know that the `group!`-macro creates a static group for us.
The naming convention for this static is all uppercase: `{name}_GROUP`.
The code above defines `name: "general"` and the macro turns this into:
`GENERAL_GROUP`. Therefore the following code will do:

```rust
StandardFramework::new()
    .configure(|c| c
        .with_whitespace(true)
        .on_mention(Some(bot_id))
        .prefix("~")
    .group(&GENERAL_GROUP)
);
```
<span class="caption">Listing 1.4: A framework configuring a group.</span>

## Group Options

As in listing 1.3, we define groups via the `group!`-command. That's also the
new place of group's options:

```rust
group!({
    name: "emoji",
    options: {
        prefixes: ["emoji", "em"],
        description: "A group with commands providing an emoji as response.",
        default_command: bird,
    },
    commands: [cat]
});
```
<span class="caption">Listing 1.5: A group with some options.</span>

## Help Command

Need some help for your help?\
To please 0.6, we first need to literally define a function and then dispatch
to the actual help-function, e.g. `with_embeds` or `plain`.

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
<span class="caption">Listing 1.6: Function dispatching to help-function
`with_embeds`.</span>

