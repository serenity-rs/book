# Rate Limits

The concept of a rate limit is imposed by Discord. Each API-interaction has a limited amount of times a bot can perform these within a certain time frame.

Serenity respects these and pre-emptively waits to avoid violations of such.

## I get a high Rate Limit

If you see a message as such:
```
[2019-08-14T19:10:09Z DEBUG serenity::http::ratelimiting] Pre-emptive ratelimit on route ChannelsIdMessagesIdReactionsUserIdType(...) for 50300000ms
```
You might wonder why the rate limit is so high. In this case ensure your system clock is synchronised as that will lead to an inaccurate wait-time.