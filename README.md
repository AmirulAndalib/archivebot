# Archive-Bot

[![MIT Licence](https://img.shields.io/badge/license-MIT-success.svg)](https://github.com/Nukesor/archivebot/blob/master/LICENSE.md)
[![Paypal](https://github.com/Nukesor/images/blob/master/paypal-donate-blue.svg)](https://www.paypal.me/arnebeer/)
[![Patreon](https://github.com/Nukesor/images/blob/master/patreon-donate-blue.svg)](https://www.patreon.com/nukesor)

A handy bot which enables to download files from telegram chats to your server.

It features a full backup of all files posted in a chat and a continuous backup of incoming new media.
A 7z archive can then be created and downloaded from the Telegram chat with a single command at any time.

For instance, this is great to collect images and videos from the members of your last holiday trip or to simply push backups or interesting files from your telegram chats to your server.

<p align="center">
    <img src="https://raw.githubusercontent.com/Nukesor/images/master/archivebot_example.png">
</p>

To send multiple uncompressed pictures and videos with your phone:

1. Click the share button
1. Select `File`
1. Select Gallery (To send images without compression)

**WARNING:**  
This is a hobby side project of mine. It has been developed for use on Linux, but might work on windows as well.
There might be some bugs, but I didn't find them yet. I'm happy about any PR's, feel free to help out!  

## Features

- Zip all files and post it into the chat with the simple `/zip` command.
- Clear all files from the server with a simple `/clear_history` command.
- Scan the whole chat with `scan_chat` (Bot needs to be logged in as a normal user for this feature).
- Specify your accepted media types.
- Set a custom name for a chat for easier server file management and naming of the zip file.
- Automatic sorting of files by chat and user. `sort_by_user` can be disabled.
- Properly handles forwarded messages (If `sort_by_user` is enabled, the original sender will be used).
- Verbose option for notifying users of duplicates or compressed images.

## Stuff that's not working

- I don't handle all media files yet. Feel free to create a pull request for this.

## Installation and starting

**This bot is developed for Linux.** Windows isn't tested, but it shouldn't be too hard to make it compatible. Feel free to create a PR.

Dependencies:

- `poetry` to setup a virtualenv and run everything conveniently.
    (If you don't want poetry, you need to install the dependencies defined in `pyproject.toml` by hand.)
- [Just](https://github.com/casey/just) for convience.
- `7zip` if you want to use the zip feature
- Sqlite

1. Clone the repository:
    ```sh
    git clone git@github.com:nukesor/archivebot && cd archivebot
    ```

1. Run `just setup` to setup the project
1. Run `just initdb` to initialize the database
1. Run `just run` to create a default config `~/.config/archivebot.toml`
1. Adjust the configuration
1. Start the bot via `just run`

## Configuration

You can choose to run archivebot as a bot with a telegram bot token.
If run as a normal telegram bot, archivebot is unable to scan the whole chat history.
Thereby `/scan_chat` doesn't work as well as the `/zip` command, since normal bots can't upload files larger than 20MB.
Userbots on the other hand can upload files up to 1.5GB.

**If you run the bot as a normal Telegram bot, disable the privacy mode for your bot via the BotFather menu!** Telegram bots can't read group messages by default.

In case you decide to run it as a userbot to access all features, set the userbot flag to `true` and add your phone number to the configuration.
You will receive a login code, which has to be entered on the first start and every time your session expires (which happens pretty much never).

## Commands

In group chats the bot expects a command in combination with its username. (In userbot mode that's your own username)
E.g. `/start@some_bot_name` or `/start@Nukesor`

```txt
/start Start the bot
/stop Stop the bot
/clear_history Clear all files from the server.
/zip Create a zip file of all files on the server
/set_name Set the name for this chat. This also determines the name of the target folder on the server.
/scan_chat Scan the whole chat history for files to back up.
/accept ['document', 'photo'] Specify the accepted media Example: '/accept document photo'
/verbose ['true', 'false'] The bot will complain if there are duplicate files or uncompressed images are sent, whilst not being accepted.
/sort_by_user ['true', 'false'] Incoming files will be sorted by user in the server directory for this chat.
/allow_duplicates ['true', 'false'] Allow to save files with duplicate names.
/info Show current settings.
/help Show this text
```

### Botfather commands

These are the command descriptions formatted for the botfather, in case you want to host your own bot

```txt
start - Start archiving Files for this chat
stop - Stop archiving Files for this chat
clear_history - Clear all files from the server.
zip - Create a zip file of all files on the server.
set_name - Set the name for this chat. This also determines the name of the target folder on the server.
scan_chat - Scan the whole chat history for files to back up.
accept - ['document', 'photo'] Specify the allowed media types. Example: `/accept document photo`
sort_by_user - ['true', 'false'] Incoming files will be sorted by user in the server directory for this chat.
verbose - ['true', 'false'] The bot will complain if there are duplicate files or uncompressed images are sent, whilst not being accepted.
allow_duplicates - ['true', 'false'] Allow to save files with duplicate names.
info - Show current settings.
help - Show the help text.
```
