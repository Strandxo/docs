# Status and Presence/Activity
----------------------------------------------------------------------------------------------------------

This is one of two ways in which you can set your bots "game"
```
bot.on("ready", () => {
    bot.user.setPresence({ game: { name: "<GAME>", type: 0 } });
})
```
This is the other way
```
bot.on("ready", async () => {
  bot.user.setActivity("+help", {type: "PLAYING"});
})

```

This is for your bots status.
```
// Set the bot's online/idle/dnd/invisible status
bot.on("ready", () => {
    bon.user.setStatus("online");
});
```
