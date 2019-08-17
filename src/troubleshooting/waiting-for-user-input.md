# Waiting for user input

Do you want to send a message to a user and then wait for their response? For example a reaction or message?

Bad news first: Serenity v0.6 is not asynchronous, it uses a threadpool of command-threads that dispatch whenever a command is executed. It can be really fast, but if you block all threads, no more command can be executed. That's why letting a command-thread sleep or perform long taking actions is a bad idea.

It's better to move the workload to another self-made thread. Let's imagine a user runs a command and our bot expects a reply to their initial message.\
A solution for this particular situation can be found in our [12th example]. It will register an event, the command-code ends, and once event is due, the process of the command will be continued in the given function.

## Why is v0.6 not asynchronous?
We are awaiting the stabilisation of async_await and our IO ecosystem to support given asynchronous usage.\
Most importantly, this will be a breaking change and thus won't land as patch-update for v0.6.

[12th example]: https://github.com/serenity-rs/serenity/blob/v0.6.3/examples/12_timing_and_events/src/main.rs

