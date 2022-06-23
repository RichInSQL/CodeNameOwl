---
title: "Deploying Actual Budget To Fly.io"
date: 2022-06-13T00:00:00+00:00
# weight: 1
aliases: ["/deploying-actual"]
#tags: [""]
author: "Rich"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: false
hidemeta: true
comments: false
#description: "Desc Text."
canonicalURL: "https://codenameowl.com/actual"
disableHLJS: false # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: false
ShowPostNavLinks: false
#cover:
#    image: "/img/hello.png" # image path/url
#    alt: "Hello" # alt text
#    caption: "Hello" # display caption under cover
#    relative: false # when using page bundles set this to true
#    hidden: true # only hide on current single page
---

So, you have heard about Actual Budget going open source? You want to host a copy of Actual yourself, but have no idea where to start? Lets see if we can help out, in this guide, we are going to use [fly.io](https://fly.io) to host an always on copy of your budget which will be accessible from anywhere - it is incredibly easy and best of all free.

Lets' get started.

## Pre-requisites

There are some things we need to do before we get started though.

### Fly.io Account

Create an account with [fly.io](https://fly.io/app/sign-up). Although you are required to enter payment details, everything we do here will work on the free tier and you won't be charged.

### Flyctl tool installation

Open up Powershell on your local machine and paste the following command into the window 

        iwr https://fly.io/install.ps1 -useb | iex

![](/img/fly-install-1.png)

Flyctl should start installing

![](/img/fly-install-2.png)

Once done you should get a message saying 

    Run flyctl --help to get started

![](/img/fly-install-3.png)

Flyctl is now installed.

### Installing Git On Your Local Machine

Download git for Windows from here 

    https://git-scm.com/download/win

Select the version that is suitable for your operating system

If you are unsure if you are running 32bit run this command in the command prompt 

    wmic os get OSArchitecture

![](/img/cmd-arch.png)

Download the setup file that is right for your system

![](/img/git-download.png)

It should then start downloading

![](/img/git-download-progress.png)

Once it is done, open it up - if asked click yes that you are happy for it to make changes to your device.

![](/img/git-install-1.png)

Copy the settings a they are below and click next, next...

![](/img/git-install-2.png)
![](/img/git-install-3.png)
![](/img/git-install-4.png)
![](/img/git-install-5.png)
![](/img/git-install-6.png)
![](/img/git-install-7.png)
![](/img/git-install-8.png)
![](/img/git-install-9.png)
![](/img/git-install-10.png)
![](/img/git-install-11.png)
![](/img/git-install-12.png)
![](/img/git-install-13.png)
![](/img/git-install-14.png)
![](/img/git-install-15.png)
![](/img/git-install-16.png)

Then git should begin installing 

![](/img/git-install-17.png)

Open up a command prompt and type

    git --version 

To make sure it has installed correctly, if it has, you should see a version number as below

![](/img/git-install-18.png)

That is it, Git is now installed.

## Deploying Actual

I got Actual deployed to [fly.io](https://fly.io) below are the instructions I used - understandably these are for Windows only but should work with a few tweaks for MacOSX & Linux.

Press start (or hit the windows key on your keyboard), type **cmd**

![](/img/windows-start-1.png)

when cmd appears in the search results, **right click** it and run it as **Administrator**

![](/img/windows-start-2.png)

Navigate to the C:\

    cd C:\

![](/img/cmd-1.png)

Create a folder called github.

    mkdir github

![](/img/cmd-2.png)

Once that is complete run this command

    flyctl auth login

![](/img/cmd-9.png)

This should open a web browser and ask you to login - you will need to provide credit card details to proceed but actual shouldn't cost much if anything at all.

![](/img/cmd-10.png)

Now we need to move into the github directory we just created on the C:\ drive, to do that we can use this command

    CD C:\github

![](/img/cmd-3.png)

**Note:** CD means change directory

Now run this command

    git clone https://github.com/actualbudget/actual-server.git

![](/img/git-install-19.png)


This will pull down the latest files for actual-server from the git hub repository to our local machine.

Then we need to move into that folder, to do that use this command

    cd actual-server

![](/img/cmd-4.png)

Let's check to make sure we are in the correct place

    dir

![](/img/cmd-5.png)

You should see a list of files in a list one of them being fly.template.toml if you don't see them go back to step 6

Copy fly.template.toml that is in the folder called actual-server within C:\GitHub and copy it back into the same directory with the name fly.toml

    copy fly.template.toml fly.toml

![](/img/cmd-6.png)

Open the fly.toml file in notepad or a code editor of your choice like Visual Studio Code

    notepad fly.toml

![](/img/cmd-7.png)

On line 1 change app = "%NAME%" to something of your choosing like app = "Actual" and save the file

![](/img/cmd-8.png)

Go back to the command prompt and run

    flyctl launch

You will see a message that says An existing fly.toml file was found for app Actual Budget ? Would you like to copy its configuration to the new app? (y/N)

Type Y and hit enter

![](/img/cmd-11.png)

It asked me to give my application a name, I just left it blank and it picked one for me. I did this because no matter what I typed it errored.

![](/img/cmd-12.png)

Select your location using the up down arrow keys when prompted

![](/img/cmd-13.png)

    Would you like to setup a Postgresql database now? (y/N)

Type N and press enter

![](/img/cmd-14.png)

    Would you like to deploy now? (y/N)

Type Y and press enter

![](/img/cmd-15.png)

The application should begin deploying.

![](/img/cmd-17.png)

If you get a message about windows firewall, click Allow Access

![](/img/cmd-16.png)

When complete you should see something like this

![](/img/cmd-18.png)

## Persisting the data in Fly

When we update Actual, if we don't persist the data it will be erased each time we come to update.

Run the following command 

    flyctl volumes create actual_data --region lhr

This will create a volume in the london region

![](/img/cmd-19.png)

You should then get a message to say it was successful

![](/img/cmd-20.png)

Open up fly.toml in notepad - you can do this from the command line 

    notepad fly.toml

Add the following to the file

    [mounts]
        source="actual_data"
        destination="/data"

If you create a volume with a different name, put that in the source. Save the file.

![](/img/cmd-21.png)

Now from the command prompt run

    flyctl deploy

![](/img/cmd-22.png)

Your application should be re-deployed with the updated configuration

![](/img/cmd-23.png)

If all went well, you should now be able to see your volume from the fly.io dashboard.

![](/img/fly-dash-3.png)

## Configuring Actual

Now everything is setup and configured, you should now be able to navigate to actual using the URL provided by Fly in the dashboard. 

To find that open https://fly.io/dashboard in a browser and click the application you created, in my case **myfirstbudget**, it might have a really random name if you left it blank a few steps ago

![](/img/fly-dash.png)

Once you are in there, you should see hostname section under Application Information - click the link

![](/img/fly-dash-2.png)

    https://myfirstbudget.fly.dev

This will now open Actual so we can start configuring it.

You should then see this screen

![](/img/actual-config-1.png)

Click the Use this domain link

![](/img/actual-config-2.png)

Set a password - remember this, you will need it in the future.

![](/img/actual-config-3.png)

If everything went well you should then be taken to your very first budget.

![](/img/actual-register.png)

Actual is now up and running. Congratulations

## Updating Actual

Whenever you want to update Actual, update the versions of @actual-app/api and @actual-app/web in package.json and run 

    flyctl deploy.
