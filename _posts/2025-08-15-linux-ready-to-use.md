---
title: Linux, ready to use, quicky and easily.
---

## Introduction
It's possible to extend the useful life of an older computer by adding a Linux distribution to that computer. As I recently discovered, it's also a lot easier than I thought it would be.

You likely already know that you can install a Virtual Machine (VM) onto an older computer, then install Linux on that VM. It can be a time-consuming process to figure out which distro to use, finding and downloading the .iso file for the distro, running the install and finally configuring it as you want it.

It can be much easier. There are pre-configured distros that you can download, decompress, and add to a VM without much more work than, well, downloading, decompressing, and adding. We'll get to that in just a moment.

## Why?
Extending the use life of a computer is, at the very least, an act of being a Good Global Citizen. If you can keep a slab of plastic and metal out of the landfill, you're making a small gesture towards something useful.

Here are a few, more specific reasons I had for learning how to install Linux on older machines:
- As a senior-ish contributing developer to an Open Source project, I sometimes ended up mentoring more junior-ish developers who lived in parts of the world where computers, electricy and internet-connectivity aren't as plentiful as they are in the Global North. There were a few occasions where the junior-ish devs needed to squeeze as much computing power out of their available machines, but were limited by the age of their equipment. Often, older machines aren't able to support newer tech, web browsers being a simple example. By setting up VM/Linux machines, we were able to remove some of the hardware obstacles these devs faced.
- As I write this post, I'm in the process of relocating and all of my higher-end equipment is in a box, on a boat, on the ocean. All I have for now is an older MacBook Pro, which just can't keep up with The New Stuff (specifically web browsers that UI for AI, GitHub, and Java SE 17). I needed to get this laptop in good enough shape to keep working, without spending a lot of extra time setting up another OS and then taking on the chore of configuring it once it was installed.

It turns out that it's really simple to get this done. You can download a complete "OS in a box", uncompress it, add it to a VM, and go about your business. The most complicated part is figuring out which "OS in a box" you should work with.

## How?
The solution I stumbled across can be found at [osboxes.org](https://osboxes.org). This site is a warehouse of sorts, for different ready-to-go OS installations.

The steps are generally as follows:
- Figure out which distro you need for your particular hardware situation. In my case, a short chat with Google Gemini determined that Lubuntu 18.04 was a good combination of size, performance and functionality for my aging MacBookPro.
- Install, if you don't already have, a Virtual Machine host. The two that immediately come to mind are VirtualBox VM and VMware; I'm using VirtualBox simply because it's already on my machine.
- Download a file compression utility that is capable of handling 7l-format file compression. For MacOS, this is called [Keka](https://www.keka.io). Other file compression utils will likely corrupt the OS download when you try to decompress it.
- Download the distro you need from [osboxes.org](https://osboxes.org). Once the download is complete, uncompress it with Keka.
- You should now have a ready-to-use OS image that ends with a .vdi exention. This might be different, depending on which distro you downloaded. The point is that you now have a ready-to-go OS.
- Create a new VM and add the new OS to it. We'll use VirtualBox for the example. The process generally goes along these lines:
    - Open the VirtualBox application.
    - Click the "New" button to start the wizard for creating a new virtual machine.
    - Name: Give your new VM a descriptive name, like "Lubuntu-VM".
    - Type: Select "Linux".
    - Version: Choose the correct version of Ubuntu from the dropdown menu (e.g., "Ubuntu 64-bit").
    - Click "Next".
    - On the "Hard disk" screen, select the option to "Use an existing virtual hard disk file".
    - Click the folder icon to browse your file system.
    - Navigate to where you extracted the .vdi file from the compressed archive and select it.
    - Click "Create".  
    - Select your new VM and click "Start".

As you continue to work with your new VM-based environment, you can adjust memory / video / etc. settings to best suit your needs.

Keep in mind that this is not a perfect, magical solution to improving older hardware's performance and capability. But as the saying goes, "A little bit of something is better than a whole lot of nothing.". Any useful improvement is worth pursuing.
