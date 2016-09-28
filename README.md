#Telegram Proxy Bot 
A simple BITM (Bot-In-The-Middle), for [Telegram](https://telegram.org/) acting as some kind of "proxy". Can use it as "virtual" second account for your purposes without revealing your "actual" identity.

Credits to [Groosha](https://bitbucket.org/master_groosha/telegram-proxy-bot) for the actual version.

Credits to [Mr_Gigabyte](https://github.com/mrgigabyte/proxybot) for additional features.

| What user sees | What admin sees |
| --- | --- |
| ![Screenshot](http://i.imgur.com/te66bIa.png) | ![Screenshot](http://i.imgur.com/g3tEa2c.png) |

 * [ChangeLog!](#changelog)
 * [Prerequisites](#prerequisites)
 * [How to install](#how-to-install)
 * [What's new ?](#whats-new-)
 * [How it works ?](#how-it-works)
    * [Basic Functionality](#basic-functionality)
    * [Viewing Log](#viewing-log)
    * [Setting Start Message](#setting-start-message)
    * [Blocking and Unblocking Feature](#blocking-and-unblocking-feature)
    * [Available and Unavailable Feature](#available-and-unavailable-feature)
 * [Notes and restrictions](#notes-and-restrictions)
 * [Upcoming features](#upcoming-features)
 * [Remember](#remember)
 * [F.A.Q.](#faq)
 * [Contact](#contact)

##ChangeLog!
####Version1.0.0
   * **Major Update**: Release, a lot of inline buttons, webhook version
   * **Bugs in this version**: Hopefully no

####Version0.4.0
   * **Major Update**: Replying behavior and usage changed. [Smart availability feature](#available-and-unavailable-feature).
   * **Bugs in this version**: 
      * All strings and pager buttons are waiting for my attention. Not a bug.
      * Unfortunately, In-Reply-To feature gone. Maybe I will come back to it.

####Version0.3.0
   * **Major Update**: Basic log viewing.
   * **Bugs in this version**: 
     1. To reply back to user, you still have to find a message forwarded from this user, and reply to it. Not a bug, but it bugs me right now.
     2. Log looks unreadable now. Not a bug, needs beautification.
     3. Pager buttons don't look fancy enough for me. Not a bug, too.
     
####Version0.2.0
   * **Major Update**: Start Message editing by `/setstartmessage`
   Start message is the first message user sees, when he/she starts the bot.
   * **Bugs in this version**: Hopefully no

####Version0.1.0
   * **Major Update**: 
     1. All storing data functionality moved to MongoDB. 
     2. Added paging to users list and blocked list.
     3. Simplified user blocking using inline keyboards.
     4. Code refactored and beautified.
   * **Bugs in this version**: Hopefully no
   
 
## Prerequisites
* Python 3 (works only with Python3);
* [pyTelegramBotAPI](https://github.com/eternnoir/pyTelegramBotAPI/) library (with bot 2.0 support);
* [pyMongo](https://pypi.python.org/pypi/pymongo) driver and MongoDB credentials;
* Telegram account;
* Basic Knowledge about coding of course! 
* And the ability to read the manual patiently :D 


## How to install
* Get your own bot's token from [@BotFather](https://telegram.me/botfather);
* Get your MongoDB (If you don't want to install it, try online services such as [mLab](https://mlab.com/))
* Find out your account's unique ID (you can use [ID bot](https://telegram.me/my_id_bot) or just send message via Curl or something else and get `message.chat.id` from response JSON);
* Fill in the necessary variables in `config.py`;
* Start bot: `bash launch.sh`


## What's new ???
* Webhook version
* Removed all commands except `/start`. All features are now accessible using inline buttons.
* You can now interactively set messages for users. Removed commands like: `/set*message` and `/view*message` 
* Added **Reply** button to _usercard_. Now it is the only way to reply back to user. Forwards are gone forever.
* [Smart availability](#available-and-unavailable-feature).
* Due to smart availability: removed commands `/available` or `/unavailable`.
* You can now examine full message history with one user. Just use **Show Log** button in _usercard_.
* You can now set Start Message the same way you do with the Block Message and Unavailable Message. New commands are `/setstartmessage` and `/viewstartmessage`
* Bot sends _usercard_ with each user's message (which increases the number of messages, i know. I hope, I'll get to it later). _Usercard_ shows all info about user. Ream more about blocking and unblocking [here](#blocking-and-unblocking-feature).


## How it works
### Basic Functionality
The idea of this bot is pretty simple: you just place the bot between you and the one you want to chat with. The upside is that no one will find out your unique chat id or some other info (nickname, first name or avatar, for example). They won't also know your last seen time. However, the downside is that you can't initiate chat with someone (Because you're writing from bot and bots can't start chats to prevent spam), so you'll have to ask people to write to your bot first. 

![A simple scheme of interaction](https://habrastorage.org/files/4a2/d19/753/4a2d19753eb34073bfda0b872bf228b3.png)


### Viewing Log
I've added **Show log** button to _usercard_ which turns _usercard_ into list of messages. You can read text messages right in the list. Non-text messages, such as stickers, files, photos, voice, location, etc are converted into command, which will forward you specified message.

So, to view log:

1. Find user, which log do you want to see, in list of users or in list of blocked
2. Request his _usercard_ by pressing user's inline button.
3. Tap **Show log** button

To view non-text messages tap on `/msgABCDEF` commands which would be placeholders for non-text messages in the list.

To reply back to user, press **Reply** button.

### Blocking and Unblocking Feature
It was messy and complicated for me in Mr_Gigabyte's implementation, so I've fully rewrited it.

There is a message which I call _usercard_. It has all information about user and an inline keyboard with only one button, which allows to block or unblock user. _Usercard_ can be requested by inline buttons, which could be found in all users list and blocked user list.

Data storage moved from couple text files to MongoDB, which will help us to add functionality in future.

### Available and Unavailable Feature
There can be at times when you as an admin are unavailable or don't temporarily have access to the bot, but you at the same time want to notify all the users about your unavailability just like the way we have on answering machines ? 

> "Joshua is Unavailable! Kindly leave your message after the beep.........."

Keeping this in mind here's the **unavailable** and **available** feature!

Your available status is managed automatically. I did this because I always forget to set it back.

Status is set to **available** each time admin replies to user. After **1 hour** of inactivity it resets back to **unavailable**. 
The expiration time is adjustable in config file.

If your status is set to **unavailable** then the bot will send the message to the admin (as usually) and notify the user about the unavailability **( by sending an unavailable message)**


## Notes and restrictions
1. Message formatting (both Markdown and HTML) is not supported. 
2. You(Admins) should always press **Reply** button to specify user, who will recieve your reply. 
3. Database is needed to store users' statuses and log messages.
4. Supported message types in reply: `text`, `sticker`, `photo`, `video`, `audio`, `voice`, `document`, `location`.
5. To block a user simply tap **Block** button under his _usercard_. It will change its name to **Unblock** and user will be blocked.
6. This bot only works in the private chats.

## Upcoming Features
* Anti-Spam Feature, limiting messages sent per-second
* Broadcast feature for admins, they can broadcast a certain message to selected users they want
* Markdown support
* Group chats support
* Media support for saved messages

## Remember!
I understand, that "proxy" bots can be used to prevent spammers from being reported, so if you encounter such bots that are used to do "bad" things, feel free to report them: [abuse@telegram.org](mailto:abuse@telegram.org)

## F.A.Q
#### 1. Will this bot work in groups/supergroups/channels?
For the time being this bot just works in private chats.

#### 2. Can I use Emojis in my saved messages (start, block and unavailable)?
Yes! You can use **ONLY** emojis or text in your saved messages, you cannot save stickers/gifs or any other media. Hope that links could help in your particular case.

## Contact
You can contact me via my [Proxy Bot](https://telegram.me/phash_bot)*[]:

**PS: Let me know if you need a new feature/tweak in this bot, please don't hesitate to text me :)**
