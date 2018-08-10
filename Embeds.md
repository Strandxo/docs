# Embeds
----------------------------------------------------------------------------------------------------------

// Lets start this new section with Embeds. I will show you two ways of doing embeds however the first one will be the one you should use.

```

             var embed = new Discord.RichEmbed()
             .setColor("RED")
             .setAuthor(`TEXT`, message.guild.iconURL) //This is the top part of the embed.
             .setTitle("TEXT")
             .setDescription("TEXT")
             .setURL(url)
             .addField("HEADER", `SUBHEADER`)
             .addField("HEADER", `SUBHEADER`)
             .addField("HEADER", `SUBHEADER`, true) 
             .addBlankField(true)
             .setThumbnail(message.author.displayAvatarURL) // Displays the users discord icon on the right hand side.
             .setFooter(`TEXT`, message.guild.iconURL)

// One way of sending an embed

			 let sChannel = members.guild.channels.find("name", "<CHANNEL NAME>");
			 if(.sChannel) return message.channel.send("Sorry. It looks like there isnt a ``<CHANNEL NAME>`` channel, this means the information wont be documented")
             sChannel.send(embed)

// Another way to send an embed, extremely load code.

    ```
message.channel.send(embed)
```


// Now this is the other way to send an embed however this is the shamed upon way and its extremely clumky. Still works though.
```

      message.channel.send({embed: {
          color: 1339135, // hex colour code.
          thumbnail: {
              url: (target.displayAvatarURL) // can be an image link or the guild icon also
            },
          fields: [{
                  name: "<TITLE>",
                  value: "<TEXT>" || <STRING>",
                  inline: true,
            },
            {
                  name: "<TITLE>",
                  value: "<TEXT>" || <STRING>",
                  inline: true,
            },
            {
                  name: "<TITLE>",
                  value: "<TEXT>" || <STRING>",
                  inline: false,
            },
      ],
          timestamp: new Date(),
          footer: {
            icon_url: bot.user.displayAvatarURL ,
            text: "<TEXT>",
          },
          author: {
              icon_url: message.guild.iconURL,
              name: message.guild.name,
            }
      }});
```
