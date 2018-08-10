# Welcome to a small documentation for discord.js
 For further knowledge of the library please go to the official docs.

-------------------------------------------------------------------------------------------------------

 A common mistake made by quite a lot of people is forgetting to add your library.

```
const Discord = require('discord.js');
```

 You may also want to add your token and prefix into a file.
Put these at the top of your file.

```
const config = require("./config.json");
const PREFIX = config.prefix;
```

 Put this at the very bottom of your index.js/bot.js file.

```
bot.login(config.token);
```

Inside the config.JSON file you'll want to add the following.

```
{
  "prefix": "<YOUR PREFIX>",
  "token": "<YOUR TOKEN>"
}
```

----------------------------------------------------------------------------------------------------------

On bot startup people like to have an okay message to suggest the bot is online

```
bot.on("ready", function() {
  console.log("This bot is online");
});
```

You can also add more information to this startup message such as:

> process.version | this will show the current node version.

> discord.version | this will show the current discord.js version.

----------------------------------------------------------------------------------------------------------

The next thing you may want to do is add a command handler as its the most efficient way to run commands.

This type of command handler will allow you to have aliases for all commands.

With this you'll also need to create a folder inside your bot file calls "commands"

Inside this "commands" folder you'll be puting your commands.

```
const fs = require("fs");
bot.commands = new Discord.Collection();
bot.aliases = new Discord.Collection();

fs.readdir("./commands/", (err, files) => {
    
     if(err) console.log(err);
    
      let jsfile = files.filter(f => f.split(".").pop() === "js")
      if(jsfile.length <= 0){
        console.log("Couldn't find commands.");
        return;
      }
    
      jsfile.forEach((f, i) => {
        let props = require(`./commands/${f}`);
        bot.commands.set(props.help.name, props);
        props.conf.aliases.forEach(alias => {
            bot.aliases.set(alias, props.help.name);
        });
        
      });
    }); 

So now we obviously need a way to call the commands when they're commented on in the discord chat.

    bot.on("message", async message => {
      if(message.author.bot || message.channel.type === "dm") return; // add this to prevent dm spam and the bot calling commands.
    
      let messageArray = message.content.split(" ");
      let cmd = messageArray[0];
      let args = messageArray.slice(1);
    
      if(.message.content.startsWith(PREFIX)) return; //your prefix should be define at the top of the file.
      let commandfile = bot.commands.get(cmd.slice(PREFIX.length)) || bot.commands.get(bot.aliases.get(cmd.slice(PREFIX.length)));
      if(commandfile) commandfile.run(bot,message,args);

    });
```

----------------------------------------------------------------------------------------------------------

so now we have create a command handler but what do you put in the command files?
the following code will need to go in every single command file or it'll return null.
```
module.exports.run = async (bot, message, args) => { // This is the brackets in which the command goes in

// This will be where the code goes for your command.
} // end bracket to finish the command off, please for the love of god dont forget this.

module.exports.conf = {
    aliases: ['<ALIAS COMMAND>', '<ANOTHER ALIAS>']
};

// ADD DESCRIPTION AND SUCH
module.exports.help = {
    name: "<COMMAND NAME>" // Don't forget to change this in every file otherwise it'll screw up.

}
```
----------------------------------------------------------------------------------------------------------

// There is also a way to put commands in via using a switch. Although I don't recommend it, it's a valid way to create commands.

```
 bot.on("message", function(message) {
   if (message.author.bot) return;

   if (.message.content.startsWith(PREFIX)) return;

   var args = message.content.substring(PREFIX.length).split(" ");

 switch (args[0].toLocaleLowerCase()) {

      case "COMMANDNAME": // and you can add aliases by doing another:  case "ALIAS":

      // command code goes here
      
      break; //this will end the command right here. Don't forget this or you'll screw up your commands.

      case "COMMANDNAME": // and you can add aliases by doing another:  case "ALIAS":

      // command code goes here
      
      break; //this will end the command right here. Don't forget this or you'll screw up your commands.

   default: //this will run if someone uses the prefix but the command isnt above.
     return; //this will just return it whenever someone tries to run a command that doesnt exist.
       break; // returns the code and stops the command.
 }
 });
  ```

----------------------------------------------------------------------------------------------------------

// So now lets add a Game or Activity/Precence to the bot. ( setGame is now deprecated so use Activity or Precence )

```

bot.on("ready", () => {
    bot.user.setPresence({ game: { name: "<GAME>", type: 0 } });
})

bot.on("ready", async () => {
  bot.user.setActivity("+help", {type: "PLAYING"});
})

```
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
----------------------------------------------------------------------------------------------------------
