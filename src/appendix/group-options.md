<link rel="stylesheet" href="../../../css/span.css">

# Group Options

## About
In this chapter, we will list all possible options and explain them.

Here is a group macro for reference:

```rust
group!({
    name: "general",
    options: {
        // Options are here:
        owners_only: true,
        only_in: "dm",
        checks: [admin],
    },
    commands: [cat]
});
```
<span class="caption">Listing 1.1: A group macro example.</span>

Group options are defined within the `options`-block and as shown in the listing.

## Legend
**Accepts** describes what inputs are being accepted by the macro.\
**Default** shows what value the option will have if not specified by the user.\
**Effect** elaborates on what the option will do depending on the input.

## only_in
**Accepts**: "dm", "guild"\
**Default**: Permits both.\
**Effect**: Limits a group to either direct messages or guilds.

## owners_only
**Accepts**: true, false\
**Default**: false\
**Effect**: Sets whether a command can be used by bot-owners only. A bot-owner must be specified on the framework-configuration.

## owner_privilege
**Accepts**: true, false
**Default**: false
**Effect**: whether bot-owners bypass all command-checks.

## help_available
**Accepts**: true, false
**Default**: true
**Effect**: whether a command will be listed in
the help-system.

## allowed_roles
**Accepts**: [&str] as in `["ferris-role", "crab-fan"]`,
**Default**: empty
**Effect**: The group will be exclusive to members with at least one allowed role.

## help_available
**Accepts**: true, false
**Default**: true
**Effect**: Whether the group will work in the help-system.

## required_permissions
**Accepts**:
**Default**: empty
