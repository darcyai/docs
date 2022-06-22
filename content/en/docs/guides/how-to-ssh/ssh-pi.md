---
title: "Keyless remote SSH- Raspberry Pi"
linkTitle: "Raspberry Pi SSH"
weight: 200
---
## Raspberry Pi are awesome

I will begin by reaffirming my love for Raspberry Pi boards. If RPi boards were living creatures, I would personally operate what could only be called a Raspberry Pi open space preserve. I have many healthy units of all historical models happily running.

>One of the things that I enjoy the most about Pi boards is that they all operate pretty much the same.

So if I know how to do something with a Raspberry Pi Zero W, then I can do the same thing with a Raspberry Pi 4. Therefore, what I’m going to do in this article should work with all RPi models.

If you’ve read my other article about keyless SSH for the Nvidia Jetson Nano, then you can guess what I’m about to say…
The problem with using Raspberry Pi boards anywhere other than your own home or office network is that you probably don’t have remote network access. And you probably shouldn’t, either. Asking a customer to provide VPN access to their facility network so you can manage the solution you sold them is not safe and not very professional. Also they won’t do it most of the time.

>And now that I think about it, I actually have this problem even when my Pi is on my home network. Because as soon as I leave my home, I cannot access it.

Now you contrarians may say “What about VNC!? It comes with my RPi”. I hardly ever need a remote desktop to perform the work I am doing, which is exclusively in the command line. I would argue most others are like me. I could use the command line in a remote desktop environment, but often times the connection is spotty at best with a noticeable lag. Not only that, VNC requires a server hosted elsewhere, adding to lag and potentially a cost if I am using it professionally. Paying for a laggy GUI, when I don’t need a GUI? No thank you!

What I need is a simple way to get into my RPi boards when I’m not sitting next to them. I also need to be able to get into them from anywhere and on any device. I’m out of luck if I don’t have my specific laptop with me and I need to get in to a board to fix something.

The tech for doing this is called “keyless SSH” and it’s a great way for enabling your own remote access and also opening access for other team members. The keyless part is that you don’t need a user account and password and you don’t need a pre-installed SSH key, either. You get access when you need it. That’s all. It’s elegant!

Normally, to have this type of access you would need a special network setup such as a VPN. Or you would need some special SSH configuration and a hosted server to provide the access passageway. Those options have severe security implications and they take effort to set up and the effort doesn’t scale well.

>Keyless SSH functionality is awesome but it isn’t just laying around. Someone has to provide it.

You can get it easily with the Darcy Cloud! I’m gonna spend the rest of this article walking through how to get it set up. You should expect to spend about 15 minutes if your Raspberry Pi is not yet SSH enabled and less than that if it’s already setup.

After you get your RPi setup on the Darcy Cloud you will be able to get into it from anywhere at the click of a button or with a single command. You’ll also get device monitoring and management and a huge amount of edge application functionality, but that’s a topic for another post!

## Get your Raspberry Pi ready

To get started, you just need to be able to run a command on your Raspberry Pi. You can do that via the command line UI if you are using the desktop version of Raspberry Pi OS or you can do it via a local SSH session if you are using headless Raspberry Pi OS. Either way, you just need an operating system and your Pi needs to have Internet access.

If you haven’t set up your OS yet, follow the standard setup instructions:

[Official Raspberry Pi getting started guide](https://www.raspberrypi.com/documentation/computers/getting-started.html)

And once you have the OS running, you need to enable SSH on your RPi. Here’s a straightforward guide to doing that via several methods:

[Enabling SSH on a Raspberry Pi](https://linuxize.com/post/how-to-enable-ssh-on-raspberry-pi/)

Now that you have SSH enabled and a ready command line prompt, the next steps will fly by!

## Create a Darcy Cloud account

Get yourself a Darcy Cloud account at https://cloud.darcy.ai. Pick a username. Then user either email, a Google account, or a Github account for your identity.

FIXME image 1

You’ll get a starting project. Yay! You can rename it to anything you want later. For now, let’s keep moving and get your Raspberry Pi board added.

FIXME image 2

## Add your Raspberry Pi as an edge node

You should be sitting at a command line prompt on your Raspberry Pi. Now we are going to run an installation command. The Darcy Cloud does its magic using a piece of software called an “agent” that will do what you need on the board when you are remote.

FIXME image 3

In your Darcy Cloud account, click the “+ADD NODE” button.

FIXME image 4

You’ll see a dialog box with a command in it. Copy the command text or just click on the “copy” button.

FIXME image 5

## Paste the agent installation command to your Raspberry Pi

Now paste the command that you copied into your Raspberry Pi prompt. The installation process will run for a few minutes and should look something like this:

FIXME image 6

## Enjoy your Raspberry Pi in the Darcy Cloud

You’re pretty much done. You should see your Raspberry Pi listed in your Darcy Cloud account. You’ll notice that some handy information about the board is available. You can click on it and get more details.

FIXME image 7

You can also access the board from anywhere using that nice little “SSH” button in blue. It will open a new browser tab with an SSH session. Your Raspberry Pi does not need any special network configuration. It just needs an Internet connection from wherever it sits.

>This feature is amazing and I literally use it every day.

Now you can stop worrying about how you are going to work with your RPi boards when you’re not on the same local network. And you can get back to worrying why your code isn’t doing what you thought it would do (my frequent challenge) or thinking up where you’re going to put that Pi now that you have location freedom!