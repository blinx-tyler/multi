Nodekick
========

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-nc-sa/2.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.0/">Creative Commons Attribution-NonCommercial-ShareAlike 2.0 Generic License</a>.

Nodekick is a realtime, multiplayer fighting game built using NodeJS, socket.io, and pixi.js.

This codebase is a cleaned up version of what our team built for Node Knockout 2013. Out of 385 teams, we placed 15th overall and 6th in the "fun/utility" category.

You can play online [here](http://nodekick.jit.su).

#Screenshots and Instruction to deploy your own Nodekick.

##jump

<img src="/ss_jump.png" />

##attack

<img src="/ss_attack.png" />

##kill

<img src="/ss_kill.png" />

##victory

<img src="/ss_victory.png" />

##Run Locally

**Due to the use of websockets (and time sensitive game events over those sockets), this game server will not perform well on a Windows machine (tested on Windows 7).**

Install all the dependencies:

    npm install (you may need to prefix this with sudo if you're on Mac)

Run the app:

    node server.js

Then navigate to `http://localhost:3000` (use chrome or firefox).

#Deploy to Platfrom as a Service

##Signing up, and deploying to Nodejitsu

###Documentation

The documenation was available on the front page (right under the sign up for free button): https://www.nodejitsu.com/getting-started/

Install the Nodejitsu Package

    npm install jitsu -g (you may need to prefix this with sudo if you're on Mac)

Register via the command line:

    jitsu signup (yes you can sign up via the command line)

You'll get a confirmation email with a command to type in:

    jitsu users confirm [username] [confirmation-guid]

If you've already registered, you can login with:

    jitsu login

After you confirm your email, you can login (the `confirm` command should prompt you to log in).

Change the `subdomain` value in `package.json`, to reflect the url you want to deploy to:

    {
      "name": "nodekick",
      [...],
      "subdomain": "nodekick" <--- this value
    }

now deploy:

    jitsu deploy

And your app should be up on Nodejitsu.

##Signing up, and deploying to Heroku

###Documentation

From heroku.com, click Documentation, then click the Getting Started button, then click Node.js from the list of options on the left...which will take you here: https://devcenter.heroku.com/articles/nodejs 

Install Heroku toolbelt from here: https://toolbelt.heroku.com/

Sign up via the website (no credit card required).

Login using the command line tool:

    heroku login

Create your heroku app:

    heroku create

Git deploy your app:

    git push heroku master

Assign a dyno to your app:

    heroku ps:scale web=1

Enable websockets (currently in beta):

    heroku labs:enable websockets

Open the app (same as opening it in the browser):

    heroku open

And your app should be up on Heroku.

##Signing up, and deploying to Clever Cloud

[Clever Cloud](http://www.clever-cloud.com) promote http://nodejs-cloud.com/ and host lots of nodejs with websocket.

###Documentation

All the documentation about Clever Cloud node deploy is here : http://doc.clever-cloud.com/nodejs/

You have nothing to install in your computer.

Sign up via the website (no credit card required).

Login into https://console.clever-cloud.com/ and register your public ssh key as mention here : http://doc.clever-cloud.com/admin-console/ssh-keys/

Create a nodejs application.

![demo create clever cloud app](http://i.imgur.com/kBCD71w.png)

let default settings and create your app. 

The console will show you a git remote repository to push your app. 

![demo git repo clever cloud](http://i.imgur.com/lRzseIt.png)

Go on your clone repository, and add remote :

    git remote add clever <the_repo_url_get_from_console>
    
You have to change the config.js file to listen on 8080 port :

    // just change this value on the file
    production: { port: 8080 }
    
Commit your change

    git commit -am "port production change"

Git deploy your app:

    git push clever master

And your app should be up on Clever Cloud. To get the temporary url for you application, click the `Domains` menu item after selecting your app from the `Apps` dropdown located at the top of the page. The url will be listed there.

##Signing up, and deploying to Azure

*I've deployed Nodekick to Azure's free instance, but the performance of the websockets was really poor. Not worth deploying there. If you've had a different experience, please let me know via an issue or pull request.*

If you really want to try Azure out, refer to these instructions:

###Documentation

From windowsazure.com, click Documentation, click Developer Center, click node.js, then click the Learn More button which will take you here:

http://www.windowsazure.com/en-us/develop/nodejs/tutorials/create-a-website-(mac)/ (if you're on a Mac, looks like the link is contextual)

Install the command line tools from here:

http://www.windowsazure.com/en-us/downloads/#cmd-line-tools (on Windows, be sure to install the cross platform command line interface...not the powershell version)

From the command line, first download your publish settings (this will redirect you to a website):

    azure account download

After the `.publishsettings` file is downloaded, you'll need to import it:

    azure acount import %pathtofile%

Next create the site, with a git backed repository:

    azure site create %uniquesitename% --git

Deploy site:

    git push azure master

List of your websites:

    azure site list

And your app should be up on Azure.

After your app is deployed. Go to the Azure control panel and under the `Configure` tab for your application, enable websockets and save.

#Deploy to Infrastructure as a Service

The deployment instructions thus far have been about deploying to PaaS. You can spin up your own linux box and deploy Nodekick (or any other NodeJS app for that matter). This is ofcourse a bit harder, but you'll have fine grained control over your box as opposed to being constrained to what PaaS provides.

##Signing up and deploying to Amazone EC2

Sign up for Amazone AWS at http://aws.amazon.com/

Take note of any Access and Secret Keys provided during the sign up process, you won't get another chance to look at these.

##AWS CLI Installation

###Automatic

After this, you'll want to install the Amazon CLI, instructions located here: http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html

###Clutch

I found that AWS CLI is open source. So I decided to be build it from source as opposed to installing it.

First I cloned the repo

    git clone https://github.com/aws/aws-cli

The source is written in Python. So you'll have to install that based on whatever OS you are running. Brew, apt-get flavors, and chocolatey all have a means to install Python.

After cloning the repo, I checked out the latest tag (at this point it was `1.2.5`)

    git checkout tag 1.2.5

I then built and installed.

    python setup.py build
    python setup.py install

Looking at the console output of the build showed that the files were copied to `build/scripts-2.7` (this may be different for your OS). Navigating to this directory, I then ran

    python aws

Which yielded results.

Now that we have a successful build, move the scripts to a location of your choosing and expose the directory to `$PATH`.  I chose to put the scripts in `/usr/local/aws-cli/bin`

After moving the scripts over, I added the following line to my `.bashrc`. If you are on windows, you can place the directory in the `C:` drive and manipulate the path variable by right clicking My Computer, Properties, Advanced.

    export PATH="/usr/local/aws-cli/bin:$PATH"

After you have updated your `.bashrc` or the Environment Variables (and have sourced the new file... or restarted your computer), you should be able to run the `aws` command from anywhere.

##Configuring AWS CLI

Based on the documentation located here: http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

>The simplest way to use a configuration file with the AWS CLI is to name it config and place it in a folder named .aws in your home directory. On Windows, that would look like C:\Users\USERNAME\.aws\config. On Linux, OS X, or Unix, that is ~/.aws/config. The AWS CLI will automatically check this location for a configuration file.

Start by creating an empty configuration file.

    mkdir ~/.aws
    touch ~/.aws/config

You'll need to provide your Access Key, Secret Key, Region and Output Format. Here is a sample config file.

    [default]
    aws_access_key_id=REDACTED
    aws_secret_access_key=REDACTED
    region=us-east-1
    output=json

A full region list can be found here: http://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region

You can go to https://console.aws.amazon.com/iam/home?#security_credential to see your security credentials, but the secret access key is only shown **once**. If you didn't write down your secret key, you can create a new one here.
