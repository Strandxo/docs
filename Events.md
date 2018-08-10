# Events
----------------------------------------------------------------------------------------------------------

// Now lets add a way for the bot to detect a user join and by doing this we'll be able to send a message and add a role.

```
bot.on("guildMemberAdd", member => { // guildMemberAdd runs whenever a new member joins the guild, this is an event.
    console.log("member.user.username + " has joined " + member.guild.name + ".") // writes new user into console
    member.guild.channels.find("name", "general").send(member.toString() + " Welcome to " + member.guild.name + ", please check out the server rules before messaging.")
    var joinrole = member.guild.roles.find('name', '<ROLE NAME>'); //this will find the role in the guild
    member.addRole(joinrole) // this will add the role to the user
});
```
----------------------------------------------------------------------------------------------------------

// Now since we have a join message. Why not add a leave message, this will display a member when they leave the guild.

```
bot.on("guildMemberRemove", function(member) { // guildMemberRemove runs whenever a member leaves the guild, this is also an event.
    console.log("member.user.username + " has left " + member.guild.name + ".")
    let sChannel = member.guild.channels.find("name", "general")
    sChannel.send(member.user + " has left " + member.guild.name + ".")

});
```
----------------------------------------------------------------------------------------------------------

// Whilst we're here I might aswell throw in some other events.
// You'll need to replace '<Logs Channel>' with your guilds LogsChannel.

```
bot.on("channelCreate", async channel => {
    console.log(`${channel.name} has been created`)
    let sChannel = channel.guild.channels.find(`name`, "<Logs Channel>"); //this is defining what sChannel is.
    sChannel.send(`${channel} has been created.`) //this will send a message whereever you've define as a the logs channel
});

bot.on("channelDelete", async channel => {
    console.log(`${channel.name} has been deleted`)
    let sChannel = channel.guild.channels.find(`name`, "<Logs Channel>"); //this is defining what sChannel is.
    sChannel.send(`${channel.name} has been deleted.`) //this will send a message whereever you've define as a the logs channel
});

bot.on("roleCreate", async role => {
    console.log(`${role.name} has been created`)
    let sChannel = channel.guild.channels.find(`name`, "<Logs Channel>"); //this is defining what sChannel is.
    sChannel.send(`${role} has been created.`) //this will send a message whereever you've define as a the logs channel
});

bot.on("roleDelete", async role => {
    console.log(`${role.name} has been deleted`)
    let sChannel = channel.guild.channels.find(`name`, "<Logs Channel>"); //this is defining what sChannel is.
    sChannel.send(`${role.name} has been deleted.`) //this will send a message whereever you've define as a the logs channel
});
```
