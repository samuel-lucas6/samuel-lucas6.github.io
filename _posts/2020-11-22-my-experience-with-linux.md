---
layout: post
title:  "My experience with Linux"
date:   2020-11-22
---

I've used most versions of Windows. I started out on Windows 95, upgraded to 98, skipped 2000, used XP for a long time, had Vista for a short time, ran Windows 7 for a very long time, switched to Windows 10, tried Windows 8.1, and then went back to Windows 10.

I've installed Windows 7 and Windows 10 far more times than the average person since I've had lots of virtual machines. I also typically reinstall Windows after a while to give the system some new life. In other words, I have had a decent amount of experience running Windows.

This year I became more invested in privacy and free and open source software (FOSS). Windows 10 is a privacy nightmare due to all the data collection that can't be turned off, and it's also a proprietary operating system (OS), meaning the source code isn't publicly shared. So I decided to switch to Linux as my main OS.

I've tried Linux before but not properly. After watching Mr Robot, I installed Kali Linux alongside Windows. I actually did do a little on there because I was looking at an online ethical hacking course at the time, but I never even considered not using Windows. I also tested Ubuntu once, but that was out of curiosity. In sum, I lack Linux experience. I'm a noob.

My first stop was Manjaro. Manjaro is recommended by a lot of people online, most notably [Linus Tech Tips](https://www.youtube.com/watch?v=Co6FePZoNgE). I wanted a 'gaming distro' because I play games on my PC, and I went for the KDE Plasma version after hearing good things about KDE. In retrospect, both of these choices were bad moves.

I don't remember having any issues with the installation, but it didn't take long for problems to arise. It has been eight or so months since I used Manjaro, but I remember that the system froze randomly, I had issues with pacman, there were graphical issues, setting up the ordering of multiple monitors was glitched, I had a problem with 144Hz, and KDE was glitchy (e.g. the GUI became completely unresponsive). Not exactly a smooth experience compared to Windows, which was working fine. I can't recall experiencing any serious issues (e.g. frequent crashing/freezing) on Windows in the last two years.

Manjaro is often recommended as a 'beginners distro' on Reddit despite it being Arch based. Frankly, it shouldn't be recommended at all since it sits in a category of being worse than Arch whilst being less user friendly and stable than Ubuntu based distros like Linux Mint. Not to mention some of the [lazy practices](https://www.reddit.com/r/linux/comments/31yayt/manjaro_forgot_to_upgrade_their_ssl_certificate/) and [scummy attitudes](https://web.archive.org/web/20201008195237/https://forum.manjaro.org/t/what-is-wrong-i-am-not-to-blame/30565) of the Manjaro developers. Oh how I love the Linux community.

After I ditched Manjaro, I went back to Windows 10 for a while because stability is great. I eventually decided to try Pop!_OS since it's also recommended as a gaming and beginner distro, with lots of people claiming that it's a better version of Ubuntu.

Pop!_OS does look great out of the box, but it was annoying having no minimise button and not being able to click on icons on the panel to minimise them by default. Unfortunately, I have experienced my fair share of issues and have since switched distro.

The first issue I ran into was not being able to access Windows 10 on my other drive. Installing Pop!_OS hid Windows 10 from the BIOS boot options, and there was no option to boot to Windows when booting Pop!_OS. I somehow managed to resolve this issue after running some tools and reinstalling Pop!_OS, but I still had to use the BIOS to boot into Windows 10 because I never configured the EFI partition. Ideally, this isn't something that should have to be done as a beginner to dual boot.

The main issue I had with Pop!_OS was freezing. Sometimes the OS froze 3-10 times a day. Each time, I was forced to hard reset, which probably corrupted my VeraCrypt drive since it wasn't being dismounted properly. I luckily didn't lose any work as a result of this freezing.

Freezing/crashing is one of the most severe issues you can experience, and the Pop!_OS team doesn't know what's causing the problem at the time of writing. This is an [issue](https://github.com/pop-os/pop/issues/1172) that other users are facing as well, although perhaps not for the same reason. I hope they manage to fix it down the line because Pop!_OS has potential to be the best Ubuntu based distro, but there are obviously stability problems based on my issues and the bugs being reported on GitHub and Reddit.

Another issue I experienced was horrible lag when deleting VirtualBox snapshots. Everything just stopped working for minutes at a time besides being able to move the cursor, and deleting a snapshot took longer than it should have. Other issues I've experienced include games freezing the OS, not being able to interact with the panel, the Files application crashing whilst trying to search, the screenshots tool not always working with full screen windows (e.g. videos), the night light not turning on/off, and Pop Shop updates never going away.

I know some of these are minor issues and not necessarily Pop!_OS issues, but they're still bugs that wouldn't exist in an ideal world. The freezing and lag were quite serious problems. It's sad that I've experienced so much freezing on the Linux distros I've tried so far.

I decided I needed to switch from Pop!_OS in an attempt to find something more stable. However, I'm not a fan of most of the distros based on reported stability issues, their default appearance, application support, etc. Windows 10 has problems, but it also does some things right. There are lots of Linux distros, but it seems like most of them [aren't great](https://www.slant.co/options/2689/alternatives/~ubuntu-alternatives).

Most Linux distros for desktop don't seem as stable or polished as Windows and macOS despite lots of Linux users going on about how stable Linux is. Some of them should also be more beginner friendly and pleasant out of the box rather than requiring customisation. Then there's the issue of the often toxic and unhelpful community who blames you for experiencing a problem and brags about how superior they are because they use Linux. It makes you wonder why bother leaving Windows or macOS at all.

### Update #1

I've switched to Ubuntu 20.10 now, but that could have gone smoother. I tried to install 20.04, but the installer crashed 4 times. I tried Etcher, Rufus, and two different USB sticks. I also tried installing Linux Mint twice but encountered the same installer crashes (two separate errors). Installing the minimal version of 20.10 worked though. It only took me half a day to install Ubuntu. What a great use of my time.

So far I've experienced one serious freeze that forced me to restart and severe lag on 4 or 5 occasions for seconds to minutes when copying files. I'm talking about not being able to interact with programs, sometimes not being able to even move the cursor. The same thing happens when deleting VirtualBox snapshots (like I mentioned about Pop!_OS). God knows what's up with Ubuntu. It looks like it's more than just a Pop!_OS issue.

With that said, Ubuntu is proving to be more stable than Pop!_OS, but there's obviously room for improvement. Things have been better since I selected the latest GPU driver and upgraded my kernel to 5.9, but I haven't tried copying lots of files or deleting a VirtualBox snapshot yet. I still can't get any Steam games to run at the moment on NTFS or ext4. The claim that Linux is just as good as or better than Windows for gaming is bogus. There are lots of hurdles you need to go through, the stability of games is worse, and fewer games are supported.

I want to like Linux, but it hates me. Linux wants me to go back to Windows, and I expect the r/Linux and r/Linux Gaming communities do too because I complain about Linux far too much. Linux is perfect and Windows 10 is trash remember. However, I'm going to keep using Linux as my main OS for the time being because I want to support open source software and get some privacy back. However, I won't be removing Windows 10 because it's the perfect backup system for gaming and when Linux decides to break. Who knows whether I'll end up returning to Windows completely; PC gaming, Visual Studio, and Microsoft Office are all great reasons to use Windows rather than Linux.

### Update #2

It's been a few weeks, and I'm unfortunately still experiencing some freezing on Ubuntu. I've tried several different versions of the kernel and am now using 5.9.12-xanmod1. Changing the kernel doesn't seem to have done anything. The system still sometimes freezes when creating or deleting VirtualBox snapshots and copying files on the system drive. Sometimes my left monitor goes completely purple for some reason. SPSS also often causes the whole system to glitch out after doing analyses, but that might just be SPSS being crap. I might end up switching to Solus over the Christmas holiday because it's the only alternative I like the look of.

### Update #3

I did look at Solus, but I couldn't even run the installer because my graphics card wasn't supported. I've ended up going back to Windows 10 full time and just used some privacy tools and a firewall to try and tackle the telemetry issues. Dual booting honestly isn't worth the hassle in my opinion, and Linux simply hasn't been stable enough for me. Furthermore, the program support isn't there. It's all well and good saying 'use FOSS alternatives', but some of them aren't as good. For example, I certainly can't live with VSCode/VSCodium instead of Visual Studio. Don't even get me started with gaming on Linux.