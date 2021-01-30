## Features
* Able to temporarily enable/disable the scheduled messages from broadcasting.
* Fully customizable both in and out of the game.
* (Rust only) Customizable avatar image that shows in place of the Rust logo in scheduled messages.
* (Only tested in Rust) Rich text support. See [here](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/StyledText.html)
* Developer API for broadcasting scheduled messages whenever you want and knowing when a message has been broadcasted.

## Notes
* If you have no scheduled messages, the plugin will disable the timer that broadcasts the messages. You will have to enable the timer again for it to start broadcasting. See chat commands for how to do this.
* This has only been tested within a Rust dedicated server, although the code should be supported universally. If any problems occur, let me know.

## Permissions
* scheduledmessages.cmd - Required to use the scheduledmessages/sm commands.
* scheduledmessages.add - Required to use the "add" sub-command.
* scheduledmessages.remove - Required to use the "remove" sub-command.
* scheduledmessages.show - Required to use the "show" sub-command.
* scheduledmessages.setavatar - Required to use the "set avatar" sub-command.
* scheduledmessages.setinterval - Required to use the "set interval" sub-command.
* scheduledmessages.on - Required to use the "on" sub-command.
* scheduledmessages.off - Required to use the "off" sub-command.
* scheduledmessages.random - Required to use the "random" sub-command.

## Configuration
```json  
{
  "Scheduled Messages": [
    "This is a scheduled message, this plugin was originally intended for a community I run. But I thought making it public would serve a better purpose. I hope you find the plugin useful!"
  ],
  "Scheduled Messages Interval": 30.0,
  "Scheduled Messages Avatar ID": 0,
  "Scheduled Messages Randomizer": true
}
```

## Chat/Console Commands
All commands go through three primary ones. `scheduledmessages` and `sm` or `smsg` for short form. Under these commands there are a variety of sub commands you can use.
* add/a - Adds a new scheduled message, requires the scheduledmessages.add permission.
  * Usage: `<scheduledmessages/sm/smsg> <add/a> <message>`
  * Chat Example: /scheduledmessages add This is a test message.
  * Console Example: sm a This is a test message.
* Remove/r - Removes a scheduled message, requires the scheduledmessages.remove permission.
  * Usage: `<scheduledmessages/sm/smsg> <remove/r> <position>`
  * Chat Example: /scheduledmessages remove 0
  * Console Example: sm r 0
* Show/s - Shows all currently registered messages. This will also display their position which you can use in the remove command. Requires the scheduledmessages.show permission.
  * Usage: `<scheduledmessages/sm/smsg> <show/s>`
  * Chat Example: /scheduledmessages show
  * Console Example: sm s
* Setavatar/sa - (Only used in Rust) Sets the SteamID64 that is used for the avatar in messages. Requires the scheduledmessages.setavatar permission.
  * Usage: `<scheduledmessages/sm/smsg> <setavatar/sa> <steamid64>`
  * Chat Example: /scheduledmessages setavatar 76561198063494192
  * Console Example: sm sa 76561198063494192
* Setinterval/si - Sets the interval at which messages are broadcasted (in seconds). Requires the scheduledmessages.setinterval permission.
  * Usage: `<scheduledmessages/sm/smsg> <setinterval/si> <seconds>`
  * Chat Example: /scheduledmessages setinterval 30
  * Console Example: sm si 30
* On - Turns on the timer for broadcasting messages. Requires the scheduledmessages.on permission.
  * Usage: `<scheduledmessages/sm/smsg> <on>`
  * Chat Example: /scheduledmessages on
  * Console Example: sm on
* Off - Turns off the timer for broadcasting messages. Requires the scheduledmessages.off permission.
  * Usage: `<scheduledmessages/sm/smsg> <off>`
  * Chat Example: /scheduledmessages off
  * Console Example: sm off
* Random - Sets whether the order of scheduled messages is randomized or not. Requires the scheduledmessages.random permission.
  * Usage: `<scheduledmessages/sm/smsg> <random> <on/off>`
  * Chat Example: /scheduledmessages random on
  * Console Example: sm random off

## Developers
### Hooks
```C#
void OnScheduledMessageBroadcasted(string message, ulong avatarID)
```
The `message` parameter is the message that was broadcasted to all players. The `avatarID` parameter will only be useful in Rust. That parameter is the SteamID64 of the avatar used in the message.

### Functions
```C#
API_GetScheduledMessagesList() : List<string>
```
Returns the list of currently registered messages.

```C#
API_GetScheduledMessages() : string[]
```
Returns the list of currently registered messages but as an array.

```C#
API_BroadcastScheduledMessage() : void
```
Broadcasts a scheduled message. This internally calls the same function as the scheduled messages timer so this will call the OnScheduledMessageBroadcasted hook.

```C#
API_BroadcastScheduledMessage(int index) : bool
```
Broadcasts a scheduled message where the message is the one at the provided index. (Rust only) This will preserve the configured avatar SteamID64.

```C#
API_BroadcastScheduledMessage(int index, ulong avatarID=0) : bool
```
Broadcasts a scheduled message where the message is the one at the provided index. (Rust only) This will also override the configured avatar SteamID64 with the one provided. If you're running a server that isn't  Rust then this function is equivalent to `API_BroadcastScheduledMessage(int index)`
