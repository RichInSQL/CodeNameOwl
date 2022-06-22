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
draft: true
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
cover:
    image: "/img/hello.png" # image path/url
    alt: "Hello" # alt text
    caption: "Hello" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
---


You should deploy your server so it's always running. We recommend [fly.io](https://fly.io) which makes it incredibly easy and provides a free plan.

## Pre-requisites

### Fly.io Account

Create an account with [fly.io](https://fly.io/app/sign-up). Although you are required to enter payment details, everything we do here will work on the free tier and you won't be charged.

### Git Installed On Your Local Machine

Download git for Windows from here 

    https://git-scm.com/download/win

Select the version that is suitible for your operating system

If you are unsure if you are running 32bit run this command in the command prompt 

    wmic os get OSArchitecture

![](/img/cmd-arch.png)

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

Open up a command prompt and run

    iwr https://fly.io/install.ps1 -useb | iex

Once that is complete run this command

    flyctl auth login

This should open a web browser and ask you to login - you will need to provide credit card details to proceed but actual shouldn't cost much if anything at all.

Now we need to move into the github directory we just created on the C:\ drive, to do that we can use this command

    CD C:\github

![](/img/cmd-3.png)

**Note:** CD means change directory

Now run this command

    git clone `https://github.com/actualbudget/actual-server.git`

This will pull down the latest files for actual-server from the git hub repository to our local machine.

Then we need to move into that folder, to do that use this command

    cd actual-server

Let's check to make sure we are in the correct place

    dir

You should see a list of files in a list one of them being fly.template.toml if you don't see them go back to step 6

Copy fly.template.toml that is in the folder called actual-server within C:\GitHub and copy it back into the same directory with the name fly.toml

    copy fly.template.toml fly.toml

Open the fly.toml file in notepad or a code editor of your choice like Visual Studio Code

    notepad fly.toml

On line 1 change app = "%NAME%" to something of your choosing like app = "Actual" and save the file

Still on the command prompt run

    flyctl launch

You will see a message that says An existing fly.toml file was found for app Actual Budget ? Would you like to copy its configuration to the new app? (y/N)

Type Y and hit enter

It asked me to give my application a name, I just left it blank and it picked one for me. I did this because no matter what I typed it errored.

Select your location using the up down arrow keys when prompted

    Would you like to setup a Postgresql database now? (y/N)

Type N and press enter

The application should begin deploying.

Once complete you should now be able to navigate to actual using the URL provided. To find that open https://fly.io/dashboard in a browser and click the application, it might have a really random name

![](/img/fly-dash.png)

Once you are in there, you should see hostname section right at the top and under that is a URL this is the URL to your deployment of actual..

Whenever you want to update Actual, update the versions of @actual-app/api and @actual-app/web in package.json and run 

    flyctl deploy.

**Note:** if you don't want to use fly, we still provide a Dockerfile to build the app so it should work anywhere that can compile a docker image.
