---
layout: post
title: TCM Security PSAA Review
date: 2026-05-25
---

For the past ~11 months, I was studying for the TCM Security [Practical SOC Analyst Associate (PSAA)](https://certifications.tcm-sec.com/psaa/) certification. This is an entry-level SOC analyst cert that covers security fundamentals like phishing analysis, network traffic analysis, endpoint log investigation, searching a SIEM, threat intelligence sources, basic digital forensics, and the phases of incident response.

In this post, I'm going to break down my thoughts on the course and exam, which I passed on my first try (hoorah!). TL;DR: It was a worthwhile learning experience (big respect to [Andrew Prince](https://forensicate.net/)); however, as always, there's room for improvement. I hope that if Andrew/TCM sees this, they take the feedback on board if doing modifications and for future courses.

## My Background

To provide some context about the exam difficulty and existing knowledge, I'm going to quickly explain my background:

- **Secondary school**: studied computer science and did some projects related to basic malware analysis.
- **University**: psychology BSc and cyber security MSc. Some programming projects related to cryptography in my free time.
- **Graduate scheme**: blue team job placements in multiple teams for just over 2 years, covering topics like access management, vulnerability management, threat modelling, and SOC analyst/threat intelligence type work. Did the [CompTIA Security+](https://www.comptia.org/en-us/certifications/security/) certification.
- **Current role**: ~6 months at the time of the exam within a SOC analyst/threat hunting role, but my employer doesn't use the tools in this course.
- **Outside of work**: this is my first practical certification. I'm still doing some programming. I've never been a CTF guy or particularly into red teaming. Prior attempts at TryHackMe/HTB Academy training have failed due to their text-heavy nature, and I don't enjoy reading non-fiction books. However, I do follow security news/blogs/podcasts via a couple of sources and watch some YouTube.

## The Course

What worked well:

- Much more engaging than platforms like TryHackMe/HTB Academy (if you prefer videos).
- Emphasised methodology.
- Coverage of popular and free/accessible tools.
- Logical topics covered (for the most part) given the length restriction.
- Fun practical challenges.
- Additional practice sources listed.
- Geared towards Windows but with some Linux coverage, matching most enterprise environments.

What could be improved:

- Surface-level coverage of some topics. This is partly because the course is already long, and certain activities are unlikely to be performed by SOC analysts. Here are some additional topic ideas:
	- How to write ticket notes/reports, which ties into the exam.
	- Best practice incident response actions (in order/ranked) for different types of attack. More detail was needed here.
	- How to determine the quality and relevance of threat intelligence, and how to make threat intelligence actionable.
	- How to use AI in the SOC.
	- How malicious PDFs/Office documents work and what default protection stops (e.g., web browser sandboxing and Protected View in Microsoft Office).
	- More detail on how attackers evade detection/prevention, especially for initial access (e.g., email delivery, MFA bypasses, etc).
	- Memory forensics (e.g., Volatility 3).
- Limited coverage of the most popular techniques today (e.g., LOLBins/Fix* attacks, DLL sideloading, RMM software, malicious packages/plugins/browser extensions, BYOVD, etc). This is somewhat because the course is a few years old now. There was a focus on web application attacks when more people will probably encounter endpoint/identity attacks.
- More discussion about the impact of noise, false positives, and legitimate-looking activity (e.g., signed/trusted processes doing DLL sideloading) on analyst judgements. The course was largely focused on well-detected malware, which isn't always realistic.
- More practical challenges (e.g., multiple per tool). Some sections lacked them completely (e.g., digital forensics and incident response).
- The quizzes were too short and easy.
- Odd ordering of information, like spread across multiple videos when it would make more sense in the same video.
- Less time spent explaining obvious things/unnecessary examples (at least to me). There was repetition at points.
- Slower talking speed because I had to pause a lot to take notes. It's easy to speed up videos but difficult to slow them down without the quality dropping. Most of my notes were from what was said.
- Cloud labs/VMs for people who don't have the right hardware. However, I think local makes sense to avoid hosting costs and so people end up with a homelab.

## The Exam

### Exam Prep

To prepare for the exam, I first established the most likely topics to come up as a way to prioritise. I then created a list of things to do, separated into required and optional activities.

Next, I reread my notes and created cheat sheets of commands/steps, starting with the least likely topic. Afterwards, I moved on to doing practical challenges on platforms like BTLO, LetsDefend, TryHackMe, Splunk BOTS, and HTB Sherlocks. I also watched some TCM livestreams, padded out certain notes based on reading/AI, reran tools in the course VM, and continued to update a list of bookmarks for web-based tools and threat intel/sample sources.

The last thing I did near the exam was take notes on people's exam [experiences](https://www.cyberspeaklabs.com/post/getting-done-with-psaa-by-tcm-security)/[advice](https://tcm-sec.com/pass-the-psaa-exam/) and reread my cheat sheets, improving the formatting.

I ended up doing most of the required activities but only a few of the optional ones. Probably 1/3 to 1/2 of the total ideas I wrote down never got done. I could've done a lot more labs and further reading. The main platform I used was BTLO. In terms of the pros/cons of each major platform for practice/extra learning:

- All the BTLO challenges are free, you use your own tools, and you can search/filter what the challenges relate to. However, some challenges are better than others. Some of the provided logs were in a format that limited tool choice.
- LetsDefend has a very restrictive free plan and forces you to use specific tools because you remote onto an offline machine. The tools generally don't match the TCM course. I only tried a few challenges rather than any learning material so can't comment on that.
- TryHackMe is a [questionable](https://youtu.be/s1TNS1wN920) company based on employee reviews and their job descriptions. Unfortunately, they have a large catalogue and more readable formatting in my opinion than HTB Academy. I already had a subscription and found the free preview of their SOC Simulator, Threat Hunting Simulator, and Tabletop Exercises beneficial. I didn't really use anything else due to time, but I'm sure there are some challenges worth doing (e.g., related to Splunk).
- Splunk BOTS is obviously specific to Splunk practice but saves you from having to set it up locally. `Getting Started with Splunk for Security` was a good refresher that touched on a few things not mentioned in the course. I expect the other walkthroughs are also worthwhile, but it would take a long time to do all of them.
- HTB Sherlocks allow you to use your own tools but are more difficult/time consuming than BTLO challenges, with some requiring a subscription. Their UI is also terrible, as you have to open a Sherlock to find out what the topic is about (e.g., searching 'network' or 'pcap' won't show challenges related to packet captures, and the names are generally meaningless).
- HTB Academy gets a lot of praise online related to their certs, but when I used it over a year ago, the content I read felt disorganised, contained mistakes, and was difficult to read due to their website formatting. There were also fewer labs than I expected, and it's a bit annoying having a separate subscription. TryHackMe content also contains mistakes, may not be the latest information, VMs are slower/less stable, and the certs/company seem worse, but the content is visually much more readable. In any case, these are probably more of a hindrance for the PSAA because there's a risk of going off on a tangent rather than focusing on course material.
- CyberDefenders has free and paid labs, with some course overlap (e.g., Wireshark). However, a lot of them sound quite long, so I never tried any of these.

If I could go back, I'd do more local practice using resources like those mentioned in the course. They're the most relevant to the exam and promote an investigative mindset instead of merely answering questions. I also wasted a fair bit of time just trying to set up/open things in tools I wanted to use for the platforms above.

### Practical Days 1-2

I opted to do the exam from within my Ubuntu course VM so I didn't have to install OpenVPN on my host to connect to the cloud lab environment. This worked well because I could suspend the VM during breaks and reconnect seamlessly by unsuspending.

I spent 1.5 days on the practical tasks, taking notes in Markdown using [Obsidian](https://obsidian.md/). I did full-screen screenshots for the most part to save time. I was working ~8:15 AM - 7:15 PM (with long breaks), although the start was delayed by having to read the instructions. This was not at a rushing pace, like I was double-checking things. On reflection, there were more things I should've looked at for some tickets. My methodology had room for improvement.

The difficulty was somewhere between low-moderate, like the TCM website says. It was not as tricky as investigating in an enterprise environment (real environments are noisy and weird), but there were a few things that threw me off/that I almost missed. The fact that it was my first practical exam (and first exam in a while) made a big difference. Completing the SOC 101 course is sufficient to pass, but further practice is beneficial, especially to develop a consistent/structured methodology. Research during the exam is also essential.

The environment was more CTF-style than I expected, and you couldn't use whatever tools you wanted, with some tools having to be used on your host. However, my main gripe would be that the clipboard shortcut didn't always work, so I had to click around to get the sidebar to close when transferring details for notes. Otherwise, the cloud VM was stable and performant enough.

### Report Days 3-4

I spent about 2.5 days writing the report, working from ~8:15 AM - 9:30 PM on the first full day and ~8:15 AM - 11 PM on the second (with long breaks). This is obviously going to be very individual, and I'm slow at writing in general because I keep rereading/tweaking bits. My final PDF report was 105 pages, but this was mostly from screenshots. It was written in Markdown, which saved a lot of time with the screenshot handling. Given more time, there were tweaks I would've made, but I was happy with it on the whole.

The report template was somewhat useful, but the instructions could've been clearer. There was repetition and contradictory statements, so I worked based on my interpretation. Another thing is that you lose access to the instructions/report template once the practical time limit is reached, unless you download them beforehand. However, the disclaimer almost suggested you shouldn't download the instructions. Ambiguity is not what you want from an exam, and I wish more information was provided beforehand within the course.

### The Result

I received my result and certificate after 6 days, which is pretty impressive. However, it's worth saying that the certificate email ended up in spam and contained formatting mistakes that ironically made it look a bit like bad phishing. Additionally, there was no score or exam feedback, so you have no idea how well you did or where you need to improve.

## Advice

- Don't take the full year to do the course. I was doing it one day a week at most and quite irregularly until a month or two before the exam, when I scheduled exam prep in a bit more. My voucher nearly expired, and it meant more material had to be recapped.
- Take thorough notes whilst doing the course, including screenshots and commands that can be copied and pasted.
- Condense your notes into cheat sheets of commands/step-by-step instructions because full notes aren't easy to read in an exam.
- Write down incident response actions and security controls that apply to different types of incidents. This saves you some time with the report, although they'll need to be made more specific.
- Think about the topics that are likely to come up in a practical exam. Do additional practice for these locally in your VM using the course-suggested resources and/or the platforms discussed above. If you can do the course challenges and BTLO difficulty challenges, you can pass the exam. Don't spend time going way off the course content; you can learn extra stuff after completing the exam. Also, don't avoid your weakest topics.
- Read the instructions thoroughly at the beginning, and download a copy so you don't lose access after the practical days.
- Try to be methodical when investigating, and trust your instincts. You basically want a checklist of things to look at, and it's safest to screenshot everything and copy/paste important details like timestamps and tool commands/outputs into your notes in chronological order.
- Complete a ticket before moving on or ending the day so you don't lose track of the scenario/where you got to. I'd also recommend a separate note file per ticket to avoid confusion.
- Get started on the report early if you can. Lots of time is required to find and move information from notes into the correct report section. Markdown is much better for screenshots and command snippets than Microsoft Office.
- Every time you have a break, back up your notes/report drafts locally and to the cloud.
- You can only submit your report once, and you can also terminate the practical lab early, so be careful on the exam website.

## Verdict

Despite not being the most relevant to my workplace, I'm happy that I completed the certification. Video-based learning is much more my style than text, which is unfortunately the norm for most training/certifications. I came across and practised using new tools, have notes/cheat sheets that I can refer to in the future, and did a hands-on exam rather than multiple-choice questions. The price, free retake, and lack of proctoring/cert expiry further justify doing it and supporting TCM Security.
