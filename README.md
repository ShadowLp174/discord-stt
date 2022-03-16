# Discord.js Speech Transcriber

This transcriber was made for speech to text in discord voice calls. It uses wit.ai.

## Preparation

To use the tool you need

- to know how to [set up a Discord bot](https://discordjs.guide/) that can interact with the [voice channels](https://discordjs.guide/voice/) created using Node.JS
- a free [API key for wit.ai](https://wit.ai)

## Installation

Simply run
```
npm install discord-text-to-speech
```
Happy coding!

## Usage

First, you need to import the module and create a Transcriber instance with your API key as shown here:

``` JavaScript
const Transcriber = require("discord-speech-to-text");

const transcriber = new Transcriber(WITAIKEY);
```

Next you have to join the voice channel with you Discord bot and apply the transcriber when someone says something.

``` JavaScript
let channel = interaction.member.guild.channels.cache.get(interaction.member.voice.channel.id);
const connection = joinVoiceChannel({
  channelId: channel.id,
  guildId: channel.guild.id,
  adapterCreator: channel.guild.voiceAdapterCreator,
  selfDeaf: false,
  selfMute: false
});
connection.receiver.speaking.on("start", (userId) => {
  transcriber.listen(connection.receiver, userId, client.users.cache.get(userId)).then((data) => {
    if (!data.transcript.text) return;
    let text = data.transcript.text;
    let user = data.user;
  });
});
```

Have Fun!

## API

### `.listen(AudioReceiver, userId, user)`

#### Parameter:

- AudioReceiver type [`VoiceReceiver`](https://discord.js.org/#/docs/voice/stable/class/VoiceReceiver)
- userId type `String`; Id of the user who has to be listened to
- user type `User`; Userobject of the user

#### Returns:

- Promise resolving into Object:

``` JavaScript
{
  transcript: {
    text: "text" // other values by wit.ai like natural language-processing values are stored here too
  },
  user: <User>
}
```
