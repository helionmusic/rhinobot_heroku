# This repo is a version of RhinoBot ready for deployment on Heroku

The original RhinoBot is available [here](https://github.com/Just-Some-Bots/MusicBot).

This is to host the Discord music bot onto your own free Heroku cloud account.

### Instructions to get this up and working:
I'm assuming you don't really know much about how to get this stuff set up so this is as basic of a tutorial as I think is necessary.  
I'm also assuming you're on Windows and are willing to set this up through the Heroku CLI. Although everything should work the same regardless of OS (except the PATH variables stuff if necessary).  
This is the method I personally used to get it up and running.

**Instructions:**

First of all: [Create a Discord Bot account](https://github.com/Just-Some-Bots/MusicBot/wiki/FAQ#how-do-i-create-a-bot-account)  
Don't forget to [add it to your Discord server](https://github.com/Just-Some-Bots/MusicBot/wiki/FAQ#how-do-i-add-my-bot-account-to-a-server)  
  
Clone this repository. I've downloaded it on my side to get everything setup and uploaded through the Heroku CLI


Now, edit the `options.ini` file inside the `config` folder to set your bot's necessary configuration.  
If you are unsure how to do this step, refer to the RhinoBot FAQ [here](https://github.com/Just-Some-Bots/MusicBot/wiki/FAQ)  
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
* ``heroku buildpacks:add https://github.com/Crazycatz00/heroku-buildpack-libopus.git``  
* ``heroku buildpacks:add https://github.com/jonathanong/heroku-buildpack-ffmpeg-latest.git``  
* ``heroku buildpacks:add https://github.com/guilherme-otran/heroku-buildpack-ffprobe.git``  

Before continuing, don't close the command prompt and make sure the project's root folder contains the following 3 files with the same contents  
 **requirements.txt**  
```
discord.py[voice]
aiohttp
websockets
chardet
youtube_dl
pip
cffi==1.6.0
opuslib
```

**runtime.txt**  
```
python-3.6.0
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

Enjoy your bot.




# _____________________________________ 
# Below is the original Rhinobot's README.md 

# RhinoBot: The music bot for Discord.

MusicBot is a Discord music bot written in [Python](https://www.python.org "Python homepage"). It plays requested songs and if the queue becomes empty it will play through a list of existing songs.

### How do I set it up?

[CLICK HERE](https://github.com/SexualRhinoceros/MusicBot/wiki) to find the guide that suites your operating system.

### Commands

Commands are listed [here](https://github.com/SexualRhinoceros/MusicBot/wiki/Commands "Commands list").

### Configuration

The main configuration file is `config/options.ini`, but is not included.  Simply make a copy of `example_options.ini` and rename to `options.ini`.  See `example_options.ini` for more information on how to configure it.

[CLICK HERE](https://github.com/SexualRhinoceros/MusicBot/wiki/Configuration) for more details.

### Great, now how do I use it?
Download the bot, set the dependencies up, then run `runbot.bat`! (or `run.sh` on mac/linux)  Read the tutorial if you don't know what to do.

If you have any errors, read the FAQ. If that didn't help, you can ask for assistance on the discord help server. Is is recommended to take screenshots so the developers can see errors.

[Rhino Help Server](http://discord.me/rhinohelp "Discord link")

### FAQ

Some frequently asked questions are listed on the wiki [here](https://github.com/SexualRhinoceros/MusicBot/wiki/FAQ "Wiki").
