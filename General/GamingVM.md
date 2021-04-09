# How To Making A Gaming VM
We've all been there. Well, at least all us really big tech nerds. You want to use Linux as your primary Operating System, but you CAN'T because you still need to use development tools and games that only will run on Windows. You would ditch Windows at the first sight if everything wasn't so dependent on it... But you don't want to run Windows in a traditional virtual machine since it is soooooo slow. Also, you can't feed it your GPU which is a big problem.

## Introduction
Hello, this is Gota7, and today I'm going to show you how to make your own gaming virtual machine! Now let's go over some pre-requisites and general info about my machine:
* You need at least two GPUs, or a GPU and a CPU with integrated graphics (basically two things that have the ability to process graphics). I have a 1070GTX Ti and an AMD Ryzen 3200G with VEGA 8 graphics so I am good. If you don't, you can do a single passthrough, but it's more work and I won't cover it in this guide.
* I'm using XUbuntu 20.10, but any Debian-based distribution should work here. You may need to tweak stuff for yours, but it should be mostly the same. If you are using Windows or Mac OS, then you shouldn't be following this tutorial.
* You should probably have a decent amount of RAM, I'd recommend 16 GB at least. I currently have 32 GB installed.
* Having multiple storage drives is nice. I gave Windows its own dedicated SSD, but it really doesn't matter how you do it.
* What we will be doing is running Windows on KVM or Kernel Virtual Machine, which is just about as close to running Windows on the bare metal can get. We are also telling Linux *no, you can not use our GPU*, as we are reserving it for Windows. I highly recommend doing this from a fresh Linux install.

## BIOS Settings

## Install Dependencies

## VFIO Setup

## Making The VM

## Stop NVidia's Error

## Installing Windows

## Sharing Mouse And Keyboard

## Low-Latency Sound

## Make Anti-Cheat Happy
For some of those multiplayer games, anti-cheat does not like it if you are using a KVM. However, SomeOrdinaryGamer on YouTube shows how to beat this protection: [Youtube Link](https://www.youtube.com/watch?v=L1JCCdo1bG4&t=868s). Do note that he mentions that you should be on a fairly new version of the 5.11 Linux Kernel at least to have this work without issues.