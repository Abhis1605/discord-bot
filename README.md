# 🧠 Project Title

Discord Ping and Auto-Reply Bot

# 📌 Overview

This project is a lightweight Discord bot built with Node.js and discord.js that handles both message-based and slash-command interactions.

It solves the common starter problem of setting up Discord event handling quickly by providing:
- A message listener that replies to user messages
- A basic create-trigger flow for parsing user input
- A slash command registration script using Discord REST APIs

This project is best for:
- Beginners learning Discord bot development
- Developers needing a quick bot starter template
- Teams prototyping bot interactions before building advanced features

Assumption:
- The repository currently hardcodes bot credentials in source files. This README assumes you will move these values to environment variables for secure usage.

# 🚀 Features

- Event-driven message handling via Discord gateway intents
- Automatic bot-message guard to prevent bot loops
- Keyword-based message trigger using create prefix
- Fallback auto-reply for general user messages
- Slash command registration through Discord REST API
- Simple ping slash command scaffold ready for expansion

# 🛠 Tech Stack

- Language: JavaScript (ES Modules)
- Runtime: Node.js
- Library: discord.js (v14)
- API Layer: Discord REST API via discord.js REST client
- Package Manager: npm

# 📂 Project Structure

- [index.js](index.js): Main bot runtime. Creates client, listens to events, replies to messages, and logs in.
- [command.js](command.js): One-time command deployment script for global slash commands.
- [package.json](package.json): Project metadata, scripts, and dependencies.
- [package-lock.json](package-lock.json): Dependency lockfile for reproducible installs.

# ⚙️ Installation & Setup

1. Clone the repository.
2. Install dependencies.
3. Create a Discord application and bot in the Discord Developer Portal.
4. Collect required values:
- Bot token
- Application (client) ID
5. Add environment variables in a .env file.
6. Run slash command registration.
7. Start the bot.

~~~bash
npm install
~~~

Create a .env file in the project root:

~~~env
DISCORD_BOT_TOKEN=your_bot_token_here
DISCORD_APP_ID=your_application_id_here
~~~

Recommended secure updates before running:
- Replace hardcoded token in [index.js](index.js) with process.env.DISCORD_BOT_TOKEN
- Replace hardcoded token and app ID in [command.js](command.js) with process.env.DISCORD_BOT_TOKEN and process.env.DISCORD_APP_ID

Run command deployment:

~~~bash
node command.js
~~~

Start the bot:

~~~bash
npm start
~~~

# 💡 Usage

After startup:

- Send any normal message in a server where the bot is present. The bot replies with Hi From Bot.
- Send a message starting with create, for example create https://example.com. The bot replies with a generation message.
- Use the slash command /ping after command registration. The bot responds with Pong!!.

# 🧩 Key Implementation Insights

1. Gateway intents define what events your bot can read

The client is initialized with Guilds, GuildMessages, and MessageContent intents. Without these, message-based features would not work.

~~~js
const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMessages,
    GatewayIntentBits.MessageContent,
  ],
});
~~~

2. Bot-loop prevention in message handlers

The first condition in the messageCreate listener exits early for bot-authored messages, avoiding recursive bot responses.

~~~js
client.on('messageCreate', (message) => {
  if (message.author.bot) return;
  // further processing
});
~~~

3. Prefix-style parsing for create workflow

The create branch uses string splitting to extract user input after the keyword. This is a basic parser that can be evolved into full command argument validation.

~~~js
if (message.content.startsWith('create')) {
  const url = message.content.split('create')[1];
  return message.reply({ content: 'Generating Short ID for' + url });
}
~~~

4. Global slash command deployment with REST

The command deployment script uses REST.put with Routes.applicationCommands to register slash commands globally.

~~~js
await rest.put(Routes.applicationCommands(APP_ID), { body: commands });
~~~

# 📸 Screenshots / Demo (if applicable)

Suggested additions:
- Bot responding to a normal message
- Bot responding to create input
- Slash command suggestion list with /ping
- Terminal output showing successful command registration

You can place images in an assets folder and reference them in this README.

# 🔐 Environment Variables

Required variables:

- DISCORD_BOT_TOKEN: Bot authentication token from Discord Developer Portal
- DISCORD_APP_ID: Discord application ID used for slash command registration

Optional variables for future growth:

- DISCORD_GUILD_ID: For guild-scoped command deployment during faster testing
- NODE_ENV: Runtime mode such as development or production

# 🧪 Possible Improvements

- Replace hardcoded credentials with dotenv-based secure config
- Add robust command parsing and URL validation for create input
- Support guild-specific command registration for faster command updates in development
- Add structured logging and error boundaries for all event handlers
- Add unit and integration tests for message and interaction logic
- Implement rate limiting and permission checks
- Split logic into modules such as handlers, services, and config

# 🤝 Contributing

1. Fork the repository.
2. Create a feature branch.
3. Make focused changes with clear commit messages.
4. Test your changes locally.
5. Open a pull request with a concise description.

# 📜 License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files, to deal in the software
without restriction, including without limitation the rights to use, copy,
modify, merge, publish, distribute, sublicense, and or sell copies of the
software, and to permit persons to whom the software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the software.

The software is provided as is, without warranty of any kind, express or
implied, including but not limited to the warranties of merchantability,
fitness for a particular purpose and noninfringement. In no event shall the
authors or copyright holders be liable for any claim, damages or other
liability, whether in an action of contract, tort or otherwise, arising from,
out of or in connection with the software or the use or other dealings in the
software.
