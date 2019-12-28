---
layout: post
title: Steps to share a folder between MacOS and Guest VirtualBox Linux VM
categories: Development-Enviornment 
---

Welcome to my first post! 

The VirtualBox console has a fairly simple UI where you can add a host folder to share with your VM. Unfortunately for me the sharing folder through VirtualBox console has never worked. Eventually, I ended up spending close to 3 hours; browsing through various tutorials multiple times to achieve this. I hope this post helps you if you’re going through the same struggle.

Before we jump into the steps I would like to highlight why in the first place I wanted to do this.

I work on Linux platform but I prefer Mac as my development machine. In my case the VM basically acts like my build machine. 

This post may help you if you’re one of those guys who is just getting started with Linux programming but don’t know how to develop for Linux on Mac. 

I’m assuming you already have your VM created. I’m gonna use an Ubuntu Guest VM. First we'll go through the steps on Mac and then on the VM.

Alright, lets begin! 

# MacOS

Execute the following command on terminal.

```sh
~ vboxmanage list vms
```

From the output, get the UUID/Name of the VM to share the folder with. Something like da3de34e-8f69-49af-bebb-be3227526092.

Then execute

```sh
~ vboxmanage sharedfolder add <UUID> --name "dev" --hostpath <folder_path> --automount
```

* UUID is the unique identifier the VM that we just noted.
* "dev" is the name of the vbox share folder device that will be created. You can use any name here.
* folder_path is absolute path of directory you want to share


# Linux VM

Execute the following command on terminal.

```sh
apt-get update
apt-get install build-essential linux-headers-$(uname -r) virtualbox-guest-dkms virtualbox-guest-additions-iso virtualbox-guest-x11
adduser vboxsf
reboot
```
Once the system is back online. Lets add the guest additions.

```sh
mount -o loop /usr/share/virtualbox/VBoxGuestAdditions.iso /mnt
./mnt/VBoxLinuxAdditions.run
umount /mnt
reboot
```

We're almostcthere. We now mount the "dev" device which we created on mac on destination directory using regular mount command.

```sh
mount -t vboxsf -o uid=$UID,gid=$(id -g) dev <dest_dir>
```

Mission Accomplished! 

I hope you liked my first post. 

Nilesh Somani
