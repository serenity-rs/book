<link rel="stylesheet" href="../../../css/span.css">

# Data, the ShareMap

### Good to know:
[`Arc`], [`RwLock`], [`Traits`], [`HashMap`]

### About
The first field is the `data`-field, wrapping a [`ShareMap`] inside an
atomic reference counter ([`Arc`]) and a read-write lock ([`RwLock`]).

Serenity provides a `Context` to every possible place you can imagine, we will
always be able to access its fields. You may or may not wonder:
"Isn't this a great way to share my own types across Serenity?", and yes,
you are right! The `data`-field can store everything your wildest dreams desire:
A database-connection, a command-counter, chat-game-session-handler,
weather-API, ..., and so many more wonderful things!
There is no limit to this magic!

### Use
Under the hood, [`ShareMap`] is actually a [`TypeMap`] with set generics,
saving us from to doing the work. A [`TypeMap`] is a special kind of [`HashMap`]
where keys can be *different* types, thus allowing multiple types of
keys instead of just one.

<span class="info">
<img hspace="10%" src="../../../images/curious.png" alt="Logo" width="84px"
style="float: right;margin-right: 25px"/>
If you did not work with [`HashMap`]s that much yet or the *multiple keys*
caused more confusion than it solved, then let's get practical for a second.<br>
As they say, *a [`HashMap`] is worth a thousand words*: `HashMap<String, u64>`
is a [`HashMap`] that accepts `String`s as keys to access `u64`s.
Hence `String`s being our only allowed keys to grab `u64`s.<br>
In contrast, [`ShareMap`] enables us to use Rust-types as keys instead of just
`String`s to access different types instead of `u64`s only.
</span>

In order to turn a type into a key, we need to implement the
[`TypeMapKey`]-trait for it.

```rust,ignore
1  struct CommandCounterKey;
2
3  impl TypeMapKey for CommandCounterKey {
4      type Value = HashMap<String, u64>;
5  }
```
<span class="caption">Listing 1.2: Implementing `CommandCounterKey`.</span>

In this example, we define a `CommandCounterKey`-type and by implementing
[`TypeMapKey`]-trait for [`ShareMap`].
Now we need to tell [`ShareMap`] what type we want to access,
luckily line 4 helps us: We define a type-alias for `HashMap<String, u64>`
named `Value`.
This is an associated type needed by the [`Key`]-trait.

<span class="info">
<img hspace="10%" src="../../../images/curious.png" alt="Logo" width="84px"
style="float: right;margin-right: 25px"/>
If you really are curious what happens behind the curtains, looking at
[`Key`]'s source might be worth it or reading about
[associated types here](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#specifying-placeholder-types-in-trait-definitions-with-associated-types "associated types").
</span>

Now let's see how we can construct a [`ShareMap`] and put our key-implementation
to use!

```rust,ignore
1  let mut client = Client::new(&token, Handler).expect("Err creating client");
2
3  let mut data = client.data.write();
4  data.insert::<CommandCounterKey>(HashMap::default());
```
<span class="caption">Listing 1.3: Inserting value for `CommandCounterKey`.</span>

This is pretty straightforward. The [`Client`] will dispatch [`Context`] to all
the different places. So all we need to do is inserting our map-value and
somehow specify the key.\
Well, not so fast! As we can see in line 3, we are calling `write` on `data`
and bind it to a mutable variable. The reason is simple: Serenity is
multithreaded and this [`RwLock`] ensures that only one thread can mutate
our [`ShareMap`] at once.\
There is one last thing left: Putting our inserted item to use. Well, we can do
this in every place inside Serenity, we will always find a [`Context`] being
passed to us.\

```rust,ignore
1  .before(|context, msg, command_name| {
2
3     let mut data = context.data.write();
4     let counter = data.get_mut::<CommandCounterKey>()
5          .expect("Expected CommandCounter in ShareMap.");
6     let entry = counter.entry(command_name.to_string()).or_insert(0);
7     *entry += 1;
8
9     true
10 })
```
<span class="caption">Listing 1.4: Using [`ShareMap`] before a command gets
executed.</span>

Here we access the [`ShareMap`] with our `CommandCounterKey` and bind the
value on `counter` in line 4.\
Furthermore, the line shows us that we need to pass the key-type as
generic-parametre on `get_mut`: `data.get_mut::<CommandCounterKey>()`.
You might notice the `expect` in line 5, yes, this will panic if you forgot
to add a value for `CommandCounterKey`, to prevent this make sure you did as
1.3 has shown.\
In line 6 we simply check if a command name has been used yet
and if not simply add it entry with the counter set to 0, as line 7 will
increment this counter already. In case you are wondering about line 9,
returning `true` continues the dispatch-process.

### Summary
To sum things up, [`ShareMap`] is *the* way to share your own dependencies
across Serenity.\
Initially, before starting the bot, we insert values just like in 1.3 and access
them wherever we want as shown in 1.4.\
Last but not least, it's important to keep the lock on the [`ShareMap`] as
short as possible, so that other executing threads do not need to wait too long.


[`Arc`]: https://doc.rust-lang.org/std/sync/struct.Arc.html
[`RwLock`]: https://doc.rust-lang.org/std/sync/struct.RwLock.html
[`Traits`]: https://doc.rust-lang.org/book/ch10-02-traits.html
[`TypeMap`]: https://docs.rs/typemap/0.3.3/typemap/struct.TypeMap.html
[`TypeMapKey`]: https://docs.rs/serenity/0.6.0-rc.0/serenity/prelude/trait.TypeMapKey.html
[`Key`]: https://docs.rs/typemap/0.3.3/typemap/trait.Key.html
[`HashMap`]: https://doc.rust-lang.org/std/collections/struct.HashMap.html
[`ShareMap`]: https://docs.rs/serenity/0.6.0-rc.0/serenity/prelude/type.ShareMap.html
