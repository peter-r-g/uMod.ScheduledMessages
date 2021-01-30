## Features
* Able to temporarily enable/disable the scheduled messages from broadcasting.
* Fully customizable both in and out of the game.
* Rich text support, see [here](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/StyledText.html).
* (Rust only) Customizable avatar image that shows in place of the Rust logo in scheduled messages.
* Developer API for broadcasting scheduled messages whenever you want and knowing when a message has been broadcasted.

## Notes
* If you have no scheduled messages, the plugin will disable the timer that broadcasts the messages. You will have to enable the timer again for it to start broadcasting. See chat commands for how to do this.
* This has only been tested within a Rust dedicated server, although the code should be supported universally. If any problems occur, let me know.

## Chat/Console Commands
All commands go through two primary ones. `scheduledmessages` and `smsg` for short form. Under these commands there are a variety of sub commands you can use.
* add/a - Adds a new scheduled message, requires the scheduledmessages.add permission.
  * Usage: `<scheduledmessages/smsg> <add/a> <message>`
  * Chat Example: /scheduledmessages add This is a test message.
  * Console Example: smsg a This is a test message.
* Remove/r - Removes a scheduled message, requires the scheduledmessages.remove permission.
  * Usage: `<scheduledmessages/smsg> <remove/r> <position>`
  * Chat Example: /scheduledmessages remove 1
  * Console Example: smsg r 1
* Edit/e - Edits a scheduled message, requires the scheduledmessages.edit permission.
  * Usage: `<scheduledmessages/smsg> <edit/e> <position> <message>`
  * Chat Example: /scheduledmessages edit 1 This is a new test message.
  * Console Example: smsg e 1 This is a new test message.
* Show/s - Shows all currently registered messages. This will also display their position which you can use in the remove command. Requires the scheduledmessages.show permission.
  * Usage: `<scheduledmessages/smsg> <show/s>`
  * Chat Example: /scheduledmessages show
  * Console Example: smsg s
* Setavatar/sa - (Only used in Rust) Sets the SteamID64 that is used for the avatar in messages. Requires the scheduledmessages.setavatar permission.
  * Usage: `<scheduledmessages/smsg> <setavatar/sa> <steamid64>`
  * Chat Example: /scheduledmessages setavatar 76561198063494192
  * Console Example: smsg sa 76561198063494192
* Setinterval/si - Sets the interval at which messages are broadcasted (in seconds). Requires the scheduledmessages.setinterval permission.
  * Usage: `<scheduledmessages/smsg> <setinterval/si> <seconds>`
  * Chat Example: /scheduledmessages setinterval 30
  * Console Example: smsg si 30
* On - Turns on the timer for broadcasting messages. Requires the scheduledmessages.on permission.
  * Usage: `<scheduledmessages/smsg> <on>`
  * Chat Example: /scheduledmessages on
  * Console Example: smsg on
* Off - Turns off the timer for broadcasting messages. Requires the scheduledmessages.off permission.
  * Usage: `<scheduledmessages/smsg> <off>`
  * Chat Example: /scheduledmessages off
  * Console Example: smsg off
* Random - Sets whether the order of scheduled messages is randomized or not. Requires the scheduledmessages.random permission.
  * Usage: `<scheduledmessages/smsg> <random> <on/off>`
  * Chat Example: /scheduledmessages random on
  * Console Example: smsg random off

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

## Localization
```json
{
  "MissingPermission": "You do not have permission to use the '{0}' command!",
  "ScheduledMessagesAdded": "The message '{0}' has been added!",
  "ScheduledMessagesRemoved": "The message at position {0} has been removed!",
  "ScheduledMessagesShow": "These are the current messages:\n{0}",
  "ScheduledMessagesAvatarChanged": "Scheduled messages avatar has been changed to {0}!",
  "ScheduledMessagesIntervalChanged": "Scheduled messages interval has been changed to {0} seconds!",
  "ScheduledMessagesOn": "Scheduled messages have been enabled!",
  "ScheduledMessagesAlreadyOn": "Scheduled messages are already enabled!",
  "ScheduledMessagesOff": "Scheduled messages have been disabled!",
  "ScheduledMessagesAlreadyOff": "Scheduled messages are already disabled!",
  "ScheduledMessagesOff2": "Scheduled messages have been disabled due to no messages being registered.",
  "ScheduledMessagesRandomOn": "Scheduled messages will now be in a random order.",
  "ScheduledMessagesRandomAlreadyOn": "Scheduled messages are already in random order!",
  "ScheduledMessagesRandomOff": "Scheduled messages will now be shown in order.",
  "ScheduledMessagesRandomAlreadyOff": "Scheduled messages are already in order!",
  "ScheduledMessagesHelp": "Schedules messages are currently {0}. These are the commands available:\n{1}",
  "ScheduledMessagesAddUsage": "Usage: <scheduledmessages/smsg> <add/a> <message>",
  "ScheduledMessagesRemoveUsage": "Usage: <scheduledmessages/smsg> <remove/r> <position>",
  "ScheduledMessagesShowUsage": "Usage: <scheduledmessages/smsg> <show/s>",
  "ScheduledMessagesSetAvatarUsage": "Usage: <scheduledmessages/smsg> <setavatar/sa> <steamid64>",
  "ScheduledMessagesSetIntervalUsage": "Usage: <scheduledmessages/smsg> <setinterval/si> <seconds>",
  "ScheduledMessagesOnUsage": "Usage: <scheduledmessages/smsg> <on>",
  "ScheduledMessagesOffUsage": "Usage: <scheduledmessages/smsg> <off>",
  "ScheduledMessagesRandomUsage": "Usage: <scheduledmessages/smsg> <random> <on/off>",
  "on": "on",
  "off": "off"
}
```

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
