# discord-server-backup
to have a file of your server
READ THE UPDATED README.md
YOU CAN USE MY REGEX TO BLOCK ALL WORDS AND MAKE SERVER UNABLE TO TEXT BY PUTING IT IN BLOCKED NAMES FOR OTHER USERS AND EVEN MAKE PEOPLE LEAVE BY DOING THE SERVER ONBOARDING AND MAKING IT SO THEY HAVE TO TEXT IN A CHANNEL THEN HAVING TO DM YOU TO GET A SPECIAL ROLE HERE IS THE REGEX

///////////////////NOTE THIS IS MY REGEX FOR DISCORD

^.*([A-Za-z0-9]+( [A-Za-z0-9]+)+).*[A-Za-z]+.*$
^<#(?<id>\d{17,20})>$
(?<subdomain>\w+)\.?(?<hostname>dis(?:cord)?(?:app|merch|status)?)\.(?<tld>com|g(?:d|g|ift)|(?:de(?:sign|v))|media|new|store|net)
[a4]?+\s*[b8]+\s*c+\s*d+\s*[e3]?+\s*f+\s*[g9]+\s*h+\s*[i1l]?+\s*j+\s*k+\s*[l1i]+\s*(m|nn|rn)+\s*n+\s*[o0]?+\s*p+\s*q+\s*r+\s*[s5]+\s*[t7]+\s*[uv]?+\s*v+\s*(w|vv|uu)+\s*x+\s*y+\s*z+\s*0+\s*9+\s*8+\s*7+\s*6+\s*5+\s*4+\s*3+\s*2+\s*1+
^https?:\/\/
^<@&(?<id>\d{17,20})>$
^<@!?(?<id>\d{17,20})>$
^wss?:\/\/
(?<url>^https:\/\/(?:(?:canary|ptb).)?discord(?:app)?.com\/api(?:\/v\d+)?\/webhooks\/(?<id>\d+)\/(?<token>[\w-]+)\/?$)
[^\f\n\r\t\v\u0020\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]

///////////////////NOTE THIS IS A .js THE NEXT WILL BE .json

const { Client, GatewayIntentBits, MessageEmbed } = require('discord.js');
const fs = require('fs');
const { CLIENT_RENEG_LIMIT } = require('tls');
require('dotenv').config();
const axios = require('url');
const PORT = process.env.PORT || 1500;
const express = require('express');
const { url } = require('inspector');

const app = express();
const port = 1500;

app.get('/api/auth/discord/redirect', async (req, res) => {
    const { code } = req.query;

    if (code) {
        const formData = new url.URLSearchParams({
            client_id: process.env.ClientID,
            client_secret: process.env.ClientSecret,
            grant_type: 'authorization_code',
            code: code.toString(),
            redirect_uri: 'http://http://localhost:1500/api/auth/discord/redirect'
        });

        const output = await axios.post('https://discord.com/api//v10/oauth2/token',
        formData, {
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded',

            },    
        });

        if (output.data) {
            const { access_token, token_type } = output.data;
            const userinfo = await axios.get('https://discord.com/api/v10/users/@me', {   
            headers: { 'Authorization':`Bearer ${access}`,
            },
        });

        console.log(output.data, userinfo.data);
    }    
    }        
});

app.listen(port, () => {console.log(`Running on ${port}`)});
    
// Load the configuration file
const config = JSON.parse(fs.readFileSync('./config.json', 'utf-8'));

const client = new Client({ intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent] });

client.once('ready', () => {
    console.log(`Logged in as ${client.user.tag}!`);
});

client.on('messageCreate', message => {
    if (!message.content.startsWith(config.prefix) || message.author.bot) return;

    const args = message.content.slice(config.prefix.length).trim().split(/ +/);
    const command = args.shift().toLowerCase();

    if (command === 'ping') {
        message.channel.send('Pong!');
    }

    if (command === 'botinfo') {
        const botInfo = {
            username: client.user.username,
            id: client.user.id,
            createdAt: client.user.createdAt,
            guilds: client.guilds.cache.map(guild => guild.name)
        };
        message.channel.send(`\`\`\`json\n${JSON.stringify(botInfo, null, 2)}\n\`\`\``);
    }

    if (command === 'serversettings') {
        const guild = message.guild;
        const serverSettings = {
            name: guild.name,
            id: guild.id,
            memberCount: guild.memberCount,
            roles: guild.roles.cache.map(role => role.name),
            channels: guild.channels.cache.map(channel => channel.name)
        };
        message.channel.send(`\`\`\`json\n${JSON.stringify(serverSettings, null, 2)}\n\`\`\``);
    }
});

CLIENT_RENEG_LIMIT
client.login(process.env.TOKEN);

///////////////////NOTE THIS IS A .json

{
    "name": "oauth",
    "version": "1.0.0",
    "description": "",
    "main": "src/index.js",
    "scripts": {
    "test": "node src/index.js"
    },
    "author": "soluzka",
    "license": "ISC",
    "dependencies": {
    "axios": "^1.4.0",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "prefix": "!"
    }
}

///////////////////CREATE A .env https://replit.com/ FOR THE .env USE TOKEN= NO SPACES WITH BOT TOKEN AFTER = AND ClientID= AND ClientSecret= AND USE THIS AS YOUR REDIRECT http://localhost:1500/api/auth/discord/redirect WITH THE PORT=1500 ALL IN THE .env
