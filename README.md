# This repo is a *non-official* version of MusicBot ready for deployment on Heroku

Just a quick note: ***This is not in any way official***. This is just a guide on how to get the bot hosted on Heroku's free cloud service. There might be much better services out there but this works for me and so this repo is showing you how to go about setting it up yourself.

**As this bot is modified from the original version, do not expect any support for it on their official Discord server. They will NOT help you. You may open an issue here if you think it's a Heroku related issue and we can try to help but please try updating the dependencies first using the instructions given near the end of this readme to see if it fixes your problem.**

The original MusicBot is available [here](https://github.com/Just-Some-Bots/MusicBot).

This is to host the Discord music bot onto your own free Heroku cloud account.

This branch is based on MusicBot release-260819. Previous releases of the bot can be found in the [releases page](https://github.com/helionmusic/rhinobot_heroku/releases).

### Instructions to get this up and working:
**Your bot will be shut down by Heroku on the last week of each month. If you're ok with this, you can follow the tutorial below. Otherwise you will need to add a credit card to have enough hours for a whole month (provided you don't have any other applications on your Heroku account)**

**If you have a credit card you can verify with we recommend you make a free AWS account instead and run the bot on there using the official MusicBot instructions. At the end of the day it's up to you whether you want to use Heroku or not.**

There is also a possibility your bot will face 429 errors, [more information here](https://github.com/helionmusic/rhinobot_heroku/issues/31)

I'm assuming you don't really know much about how to get this stuff set up so this is as basic of a tutorial as I think is necessary.  
I'm also assuming you're on Windows and are willing to set this up through the Heroku CLI. Although everything should work the same regardless of OS (except the PATH variables stuff if necessary).  
This is the method I personally used to get it up and running.

**Instructions:**

First of all: [Create a Discord Bot account](https://discordapp.com/developers)  
Don't forget to add it to your Discord server using this URL:

https://discordapp.com/api/oauth2/authorize?client_id=INSERTHERE&permissions=775285841&scope=bot

Replace `INSERTHERE` in the URL with the bot client ID found in the Discord developer portal.
  
Clone this repository by typing `git clone https://github.com/helionmusic/rhinobot_heroku` in a Git window. If you want an earlier version of the bot for whatever reason, check out the [releases page](https://github.com/helionmusic/rhinobot_heroku/releases).


Now, edit the `options.ini` file inside the `config` folder to set your bot's necessary configuration.  
If you are unsure how to do this step, refer to the MusicBot configuration guide [here](https://just-some-bots.github.io/MusicBot/using/configuration/)  
Edit the `options.ini` and the `permissions.ini` to your liking.  
Remember however to leave the `SaveVideos = no` in your options otherwise you will quickly run out of space on the Heroku server.

**NEVER MAKE YOUR BOT TOKEN PUBLIC AS ANYONE CAN USE THE TOKEN TO "HIJACK" THE BOT. You can regenerate the bot token via the Discord developer portal if you accidentally made it public anywhere.**


Now create a free Heroku account [here](https://www.heroku.com/) if you haven't already

Once created and confirmed, go to your [dashboard](https://dashboard.heroku.com/apps)  
Click on `New` and select `Create new app`  
Give it a name and select your server location from the runtime selection dropdown

Download and install Heroku CLI [from here](https://devcenter.heroku.com/articles/heroku-cli)  
If you're on windows and the setup fails to correctly set the PATH variable, do the following:  
* Open your Control Panel and head to System  
* Select `Advanced system settings`, `Advanced` tab, `Environment Variables`  
* Under `System variables`, find the variable named `Path` and click on Edit  
* Add a new variable and paste in the location for the `bin` folder from inside your chosen heroku installation location  
* Apply changes  

To confirm Heroku CLI is installed correctly and the PATH variables have been set correctly, open a command prompt and type `heroku --version`. You should see a valid output if everything is fine

  
If you haven't already, install git (should also be included in the heroku installation)  
To confirm if git is installed correctly and its PATH variables have been set correctly, open a command prompt and type `git --version`. You should see a valid output if everything is fine.  
If not, add the git's `bin` folder to the PATH variables the same way as above  

  
Once both git and heroku cli are installed open the project's root folder in cmd

Now type the following:  
* ``heroku login``  
* Enter your credentials  
* ``git init``  
* ``heroku git:remote -a heroku_application_name`` Obviously replace heroku_application_name with the exact name you've given your heroku app when creating it  
* ``heroku buildpacks:set heroku/python``  
* ``heroku buildpacks:add https://github.com/codeinteger6/heroku-buildpack-libopus.git``  
* ``heroku buildpacks:add https://github.com/jonathanong/heroku-buildpack-ffmpeg-latest.git``    

Before continuing, don't close the command prompt and make sure the project's root folder contains the following 3 files with the same contents  
 **requirements.txt**  
```
discord.py[voice]
pip
youtube_dl
colorlog
cffi --only-binary all; 
aiohttp>=3.3.0,<3.6.0
websockets>=6.0,<7.0
chardet
opuslib
pynacl==1.2.1
```

**runtime.txt**  
```
python-3.7.4
```

 **Procfile** (no extension)  
```
worker: python run.py
```
    
Once those are set correctly continue typing the following into command prompt  
* ``git add .``  
* ``git commit -m "Commit message"``  
* ``git push heroku master``  
This will start pushing the project to the Heroku server and will build it instantly afterwards. This might take some time.

Once completed, the build should push without any problems. And the Heroku worker will be set to run the bot when enabled.  
To do this, type ``heroku ps:scale worker=1`` in the command prompt.

That's it. The bot should instantly fire up and should be logged in to your discord server within the next minute.  
If you've set it to auto join a voice channel it should automatically join it.

**Updating dependencies**

This bot relies on several components listed in the [requirements.txt](https://github.com/helionmusic/rhinobot_heroku/blob/master/requirements.txt) file to make it work. If the bot suddenly stops working for any reason, try updating the dependencies.

The important ones are [discord.py](https://github.com/Rapptz/discord.py/releases) and [youtube-dl](https://github.com/ytdl-org/youtube-dl/releases). You would want to check the latest release of these two dependencies and include these latest versions in your requirements.txt in the following format:

```
discord.py[voice]==1.2.3
pip
youtube_dl==2019.08.13
colorlog
cffi --only-binary all; 
aiohttp ~= 3.5.4
websockets>=6.0,<7.0
chardet
opuslib
pynacl==1.2.1
```
Here's an example format given above. They're using the current versions at the time of this writing (discord.py 1.2.3 and youtube-dl 2019.08.13), simply replace these versions with the latest ones found in the releases section of these two dependencies.

Also, check the [requirements.txt file of discord.py](https://github.com/Rapptz/discord.py/blob/master/requirements.txt) and if they've updated the aiohttp and websockets requirements, copy and paste them into the Heroku MusicBot's requirements.txt file.

# _____________________________________
# Contributors

* [HelionMusic](https://github.com/helionmusic)
* [codeinteger6](https://github.com/codeinteger6)
* [SnGmng](https://github.com/SnGmng)



# _____________________________________ 
# Below is the original README.md 

# MusicBot

[![GitHub stars](https://img.shields.io/github/stars/Just-Some-Bots/MusicBot.svg)](https://github.com/Just-Some-Bots/MusicBot/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/Just-Some-Bots/MusicBot.svg)](https://github.com/Just-Some-Bots/MusicBot/network)
[![Python version](https://img.shields.io/badge/python-3.5%2C%203.6%2C%203.7-blue.svg)](https://python.org)
[![Discord](https://discordapp.com/api/guilds/129489631539494912/widget.png?style=shield)](https://discord.gg/bots)

MusicBot is the original Discord music bot written for [Python](https://www.python.org "Python homepage") 3.5+, using the [discord.py](https://github.com/Rapptz/discord.py) library. It plays requested songs from YouTube and other services into a Discord server (or multiple servers). Besides, if the queue becomes empty MusicBot will play through a list of existing songs with configuration. The bot features a permission system allowing owners to restrict commands to certain people. As well as playing songs, MusicBot is capable of streaming live media into a voice channel (experimental).

![Main](https://i.imgur.com/FWcHtcS.png)

## Setup
Setting up the MusicBot is relatively painless - just follow one of the [guides](https://just-some-bots.github.io/MusicBot/). After that, configure the bot to ensure its connection to Discord.

The main configuration file is `config/options.ini`, but it is not included by default. Simply make a copy of `example_options.ini` and rename it to `options.ini`. See `example_options.ini` for more information about configurations.

### Commands

There are many commands that can be used with the bot. Most notably, the `play <url>` command (preceded by your command prefix) will download, process, and play a song from YouTube or a similar site. A full list of commands is available [here](https://just-some-bots.github.io/MusicBot/using/commands/ "Commands").

### Further reading

* [Support Discord server](https://discord.gg/bots)
* [Project license](LICENSE)
