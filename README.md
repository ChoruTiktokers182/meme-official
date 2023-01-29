# meme-official
This is api in random meme 
```
https://api-official-choru-tiktokers.ohio-final-boss542.repl.co/meme
```

https://img.shields.io/badge/UNOFFICIAL-MEME-green

```
{
  author: 'Choru TikTokers',
  api: 'https://API-OFFICIAL-CHORU-TIKTOKERS.ohio-final-boss542.repl.co/meme',
  upvotes: '6925',
  upvoteRatio: '0.897',
  image: 'https://i.redd.it/c5a2wecl3yea1.jpg',
  title: 'me_irl',
  id: '10nrde0',
  createdUtc: '1674944652',
  nsfw: 'false',
  spoiler: 'true',
  score: '320',
  user: 'MimirHinnVitru',
  preview: 'https://preview.redd.it/c5a2wecl3yea1.jpg?width=108&crop=smart&auto=webp&v=enabled&s=c5f43e9f3a510fff0cd706ae06bd5b35ea0f0523,https://preview.redd.it/c5a2wecl3yea1.jpg?width=216&crop=smart&auto=webp&v=enabled&s=71b3824f9fe30f341a96566d2f2842266e93ade5,https://preview.redd.it/c5a2wecl3yea1.jpg?width=320&crop=smart&auto=webp&v=enabled&s=f7fe1e6cb230229ad9c5b037bacc3670f9abd522,https://preview.redd.it/c5a2wecl3yea1.jpg?width=640&crop=smart&auto=webp&v=enabled&s=8eeb978cc2ebe09feb5d8c1d6a91712ed03f0699,https://preview.redd.it/c5a2wecl3yea1.jpg?width=960&crop=smart&auto=webp&v=enabled&s=66928e24ddd2a39fc5670a8750d86c25e0770db1,https://preview.redd.it/c5a2wecl3yea1.jpg?width=1080&crop=smart&auto=webp&v=enabled&s=b8f9448fe18a90d117f5e3193d38122dcb41e282'
}
```
I'll give you all a command in the miraiproject
```
//Do not change the credit of the owner in the cmd because I will have trouble with that code
const axios = require('axios');
const fs = require("fs-extra");
const request = require("request");

module.exports.config = {
    name: "meme",
    version: "30.0.0",
    hasPermision: 0,
    credit: "CHORU_TIKTOKERS",
    description: "photo MEME",
    commandCategory: "RANDOM MEMES",
    cooldowns: 0,
};

module.exports.run = async function({api, event, args, utils, Users, Threads}) {
    try {
        let {threadID, senderID, messageID} = event;
        const res = await axios.get(`https://api-official-choru-tiktokers.ohio-final-boss542.repl.co/meme`);
        console.log(res.data);
        var data = res.data;
        // check if data.image is defined before making request
        if(!data.image) {
            return api.sendMessage("Error: Image URL is undefined", event.threadID);
        }

        let callback = function() {
            return api.sendMessage({
                body:`Title: ${data.title}\nId: ${data.id}\ncreatedUtc: ${data.createdUtc}\nspoiler: ${data.spoiler}\nPost by: ${data.user}\nupvotes: ${data.upvotes}\nupvoteRatio: ${data.upvoteRatio}\nnsfw ${data.nsfw}\nscore: ${data.score}\n\napi: ${data.api}\nauthor: ${data.author}`,
                attachment: fs.createReadStream(__dirname + `/cache/photo.png`)
            }, event.threadID, () => fs.unlinkSync(__dirname + `/cache/photo.png`), event.messageID);
        };
        
        return request(encodeURI(data.image)).pipe(fs.createWriteStream(__dirname + `/cache/photo.png`)).on("close", callback);
    } catch (err) {
        console.log(err)
        return api.sendMessage("Error: " + err, event.threadID)
    }
}
```
There is another Command on discord 
```
const Discord = require('discord.js');
const client = new Discord.Client();

client.on('ready', () => {
    console.log(`Logged in as ${client.user.tag}!`);
});

client.on('message', message => {
    if (message.content === '!meme') {
        // Make the API call and handle the response as before
        axios.get('https://api-official-choru-tiktokers.ohio-final-boss542.repl.co/meme')
            .then(res => {
                // Use message.channel.send() instead of api.sendMessage()
                message.channel.send(`Title: ${res.data.title}\nId: ${res.data.id}\ncreatedUtc: ${res.data.createdUtc}\nspoiler: ${res.data.spoiler}\nPost by: ${res.data.user}\nupvotes: ${res.data.upvotes}\nupvoteRatio: ${res.data.upvoteRatio}\nnsfw ${res.data.nsfw}\nscore: ${res.data.score}\n\napi: ${res.data.api}\nauthor: ${res.data.author}`, {
                    files: [res.data.image]
                });
            })
            .catch(err => {
                console.log(err);
                message.channel.send('Error: ' + err);
            });
    }
});

client.login('your-bot-token-goes-here');
```
