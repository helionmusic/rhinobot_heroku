# This repo is a *non-official* version of MusicBot ready for deployment on Heroku

Just a quick note: ***This is not in any way official***. This is just a guide on how to get the bot hosted on Heroku's free cloud service. There might be much better services out there but this works for me and so this repo is showing you how to go about setting it up yourself.

**As this bot is modified from the original version, do not expect any support for it on their official Discord server. They will NOT help you. You may open an issue here if you think it's a Heroku related issue and we can try to help but please try updating youtube-dl and/or using the alternative ffmpeg buildpack first using the instructions given near the end of this readme to see if it fixes your problem.**

The original MusicBot (previously known as RhinoBot) is available [here](https://github.com/Just-Some-Bots/MusicBot).

This is to host the Discord music bot onto your own free Heroku cloud account.

This branch is based on MusicBot release-120519

### Instructions to get this up and working:
I'm assuming you don't really know much about how to get this stuff set up so this is as basic of a tutorial as I think is necessary.  
I'm also assuming you're on Windows and are willing to set this up through the Heroku CLI. Although everything should work the same regardless of OS (except the PATH variables stuff if necessary).  
This is the method I personally used to get it up and running.

**Instructions:**

First of all: [Create a Discord Bot account](https://discordpy.readthedocs.io/en/rewrite/discord.html)  
Don't forget to [add it to your Discord server](https://discordpy.readthedocs.io/en/rewrite/discord.html#inviting-your-bot)  
  
Clone this repository by typing `git clone https://github.com/helionmusic/rhinobot_heroku` in a Git window. I've downloaded it on my side to get everything setup and uploaded through the Heroku CLI


Now, edit the `options.ini` file inside the `config` folder to set your bot's necessary configuration.  
If you are unsure how to do this step, refer to the MusicBot configuration guide [here](https://just-some-bots.github.io/MusicBot/using/configuration/)  
Edit the `options.ini` and the `permissions.ini` to your liking.  
Remember however to leave the `SaveVideos = no` in your options otherwise you will quickly run out of space on the Heroku server


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
youtube_dl==2019.05.11
colorlog
cffi --only-binary all; 
aiohttp ~= 3.5.4
websockets
chardet
opuslib
pynacl==1.2.1
```

**runtime.txt**  
```
python-3.7.3
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
To do this, go back to the [Heroku dashboard](https://dashboard.heroku.com/) and select your app  
There's a section ``Dyno formation`` on the home page of the app. Click ``Configure Dynos``  
Click the edit icon, enable the worker and confirm.

That's it. The bot should instantly fire up and should be logged in to your discord server within the next minute.  
If you've set it to auto join a voice channel it should automatically join it.

**Updating youtube-dl**

This bot, like many other music bots rely on a component named youtube-dl. Sometimes sources like YouTube may break this and the bot may stop working. If you suddenly get errors when using the bot, updating the component may help.

We try our best to update the repo with the latest youtube-dl version for deployment, but you can easily do it yourself by editing the requirements.txt file.

Use the date of the latest release that is found at https://github.com/ytdl-org/youtube-dl/releases. For example, if the latest youtube-dl version is 2019.05.11, change the youtube-dl date in the requirements.txt file to 2019.05.11. Save the file and push it to Heroku using the instructions given earlier.

**Alternative ffmpeg buildpack**

The ffmpeg buildpack which is given above always uses the latest version upon build, very rarely it may cause issues. You may use this buildpack as a temporary solution if you encounter issues.

https://github.com/kitcast/buildpack-ffmpeg.git

To change the buildpack go to your bot application in Heroku, select settings, remove the current ffmpeg buildpack and add the above one by pasting the URL in the add buildpack dialog box. Make sure it's at the last position of the buildpack list.

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

MusicBot is the original Discord music bot written in [Python](https://www.python.org "Python homepage") 3.5+, using the [discord.py](https://github.com/Rapptz/discord.py) library. It plays requested songs from YouTube and other services into a Discord server (or multiple servers). Besides, if the queue becomes empty MusicBot will play through a list of existing songs with configuration. The bot features a permission system allowing owners to restrict commands to certain people. As well as playing songs, MusicBot is capable of streaming live media into a voice channel (experimental).

![Main](https://i.imgur.com/EZljY52.png)

## Setup
Setting up the MusicBot is relatively painless - just follow one of the [guides](https://just-some-bots.github.io/MusicBot/). After that, configure the bot to ensure its connection to Discord.

The main configuration file is `config/options.ini`, but it is not included by default. Simply make a copy of `example_options.ini` and rename it to `options.ini`. See `example_options.ini` for more information about configurations.

### Commands

There are many commands that can be used with the bot. Most notably, the `play <url>` command (preceded by your command prefix) will download, process, and play a song from YouTube or a similar site. A full list of commands is available [here](https://just-some-bots.github.io/MusicBot/using/commands/ "Commands").

### Further reading

* [Support Discord server](https://discord.gg/bots)
* [Project license](LICENSE)
