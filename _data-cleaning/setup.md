---
title: "Before you Begin"
excerpt: "Making sure you have the right tools"
toc: true
toc_sticky: true
author_profile: false
layout: single
permalink: /setup/

---

## Hardware

Working with large sequence datasets or on genomes requires some computational power. Most laptops or even desktop computers with their configurations maxed will not be able to handle the majority of genome bioinformatics. This is why most biology departments that are serious about incorporating bioinformatics into their program have access to a school computer cluster or purchase computational time on AWS. However, if you are freelance bioinformatician like me, neither of those are good options. My secret is that I realized servers have been around for awhile now and data centers replace them every so many years which means someone somewhere is surplusing them for cheap. For example [Newegg](https://www.newegg.com/p/pl?N=100852105%204016%20600031341) has a lot and they are quite cheap. Cheaper than buying a new shinny workstation or laptop. 

So I bought a barebones dell poweredge r910 for a few hundred and then wandered over to ebay where I bought four 10 core cpus for 20 dollars, sixteen 600gb hard drives for 10 dollars per a hard drive, and 362gb of ram for less than a dollar per a 1g of ram. Let me tell you, this beast purrrrrs and also heats my apartment in the winter. For awhile the server was sitting on top of a dressor, but I upgraded to a rack eventually and also bought a refurbished UPS ebay to smooth out power flucuations. Total cost? probably sitting at close to $2k at this point, but I have a machine that can do genome bioinformatics for a fraction of the cost of what most people pay.  

## Software

The majority of bioinformatics tools that are readily available are intended to be used in a linux environment using a command line terminal such as bash. There is no way to get around that. Some companies have developed nice graphical user interfaces, but those tools are expensive and also lack the flexibility you will have when working from the command line.

Learning how to use linux and navigating the commandline is a very steep learning curve. But thankfully there is a wealth of information and tutorials available on the internet. The best skill you will develop while learning and doing bioinformatics is how to search for information on the internet. No joke - you will spend copius amounts of time rummaging around for answers to why something isn't working or how to do such and such in python.

In case you are wondering: I use [ubuntu desktop](https://ubuntu.com/download/desktop) installed on my server. It's easy to install and feels similar enough to using windows or macOS that it isn't too painful to learn. 

Once you have ubuntu set up, install some flavor of [conda](https://www.anaconda.com/) and [docker/singularity](https://www.docker.com/). Conda makes life so easy when installing and managing bioinformatics tools. There is a GUI for conda, but I suggest you run it from the command line. 

Additionally, you will want to keep track of everything you do, what commands you use and what options you tweaked etc. A great text file editor for that is called [sublime](https://www.sublimetext.com/). 

Once you have those tools installed you are basically ready to go! Ready to get your hands dirty learning how to use the command line that is. Typing commands into the command terminal is how I spend most of my time interfacing with bioinformatic tools. You will need to be proficient in the basics of bash. There are numerous introduction tutorials scattered throughout the internet. I don't know which is a good one, but for now I will drop this [link](https://www.javatpoint.com/bash-introduction) to one that looked promising at a glance. 