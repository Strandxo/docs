# Welcome to a small documentation for discord.js
 For further knowledge of the library please go to the official docs.

-------------------------------------------------------------------------------------------------------

 A common mistake made by quite a lot of people is forgetting to add your library.

```
const Discord = require('discord.js');
```

 Another common mistake made by quite a lot of people is forgetting to define "bot" or "client"

```
const bot = new Discord.Client();
```
or
```
const client = new Discord.Client();
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
    
      if(!message.content.startsWith(PREFIX)) return; //your prefix should be define at the top of the file.
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

There is also a way to put commands in via using a switch. Although I don't recommend it, it's a valid way to create commands.
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
