---
title:  "GSOC: Community Bonding"
date:   2021-06-07 15:53:47 +0530
author: "swaptr"
description: "This blog post elaborates my experience during the community bonding period of the Google Summer of Code 2021 at KDE Community."
tags: [gsoc, kde]
---
![gsoc banner](https://raw.githubusercontent.com/swaptr/site/master/static/images/gsoc.png "lndart.cln")

Hello everyone!
So what a way to start the community bonding period. Kasts just got it's new logo, isn't that great!. The svg render was facing some problems in the beginning but finally we no longer share the Alligator logo, we got here, yay! 
 
![kasts](https://raw.githubusercontent.com/swaptr/site/master/static/images/kasts.svg "kasts")  

So, Community Bonding, Now for people who just had a poof! moment and who don't quite have the idea of how things work in GSoC, we are supposed to provide our communication details in our proposals and that includes your IRC nick, phone number, email, among other things. During the community bonding period the idea is to have friendly interactions with the team and the mentors in our IRC's and chatrooms usually discussing the app with a few memes or jokes here and there.  
So KDE for the most part uses Matrix as the official chat system for communication.These matrix rooms can be bridged to other protocols such as the IRC or Telegram which further helps in communication. Since the major discussions related to Kasts was previously done on the Plasma Mobile room, we decided it would be better if we better move to a new one.  
So in its essence another focus of the Community Bonding is to basically get started with your project, and that could be in any way whether it be building on your proposal, incorporating new ideas, or even reset your deadlines and planning new features and building up on it. Since we had the Alligator/Kasts split right off, I had to re-visit all my deadlines and project goals. A large part of my proposal was to work on the frontend of the app and KDE has a very cool way of approaching this. We have a team of visual designers and developers who work on keeping Plasma in a great shape and visually refreshing as always. The VDG as it is called, can be summoned at will on some project or issue, and will provide you with the details you probably would have overlooked, even more so in the case of convergent applications where you are bound to make more mistakes since now you have to look for developing for both desktop and mobile devices.  
My work in GSoC requires me to interact with the VDG as well, so on my mentor's advice I contacted the VDG and introduced my project there as well with some screenshots and mockups from my proposal.  
The very next day, I was introduced to the first bug. It was relatively  easier to spot and reproduce. Now to understand the bug, we will have to understand how things work in Kasts' core RSS/Atom parser API, [<strong>Syndication</strong>](https://github.com/KDE/syndication). On an abstract level you have what is called a <strong>Feed</strong>. Feeds are basically an ordered list of articles along with some metadata such as the title, homepage, etc. Feeds have this list of <strong>Entries</strong>, which can be understood as an individual article for the feed and in the case of podcasts this is what will provide us with our media files, called <strong>Enclosures</strong>. Seems pretty clear, doesn't it? On the most basic level, all we have to do is to fetch these enclosures and with the metadata and to populate lists of these Feeds.  
Now coming back to the bug I was talking about earlier, Removing a feed is handled by the DataManager::removeFeed(), which in turn iterates over the individual entries and in turn would remove them. The DataManager::removeFeed() would return a Segmentation Fault when removing a feed that has one of its enclosure currently playing. The cause for this segfault would be to delete the enclosures and object entries and not checking first whether it would be a currently playing entry or not. Pretty basic stuff, but I was able to assimilate it clearly only after Bart explained it for me. He is really good at explaining things and to an excellent detail. While that may have been a pretty basic fix, we ran into another issue that would take up our next few days. I was able to put together a small fix for this only after Bart finished his work of changing AudioManager class' ownership back to C++.
For the next few days I would keep staring the giant GStreamer codebase, A large part of which would be to successfully build any QML app that is able to stream a media file from the internet. I would also mockup a QML app that is able to stream media using the VLC-qt library. For the time being we had to go with GStreamer for streaming support, for now.  
Moving from the backend to the frontend, Kasts had a very basic look for the desktop app. we had a very simple bottom-mounted media bar which when you look at it, is very mobile-ish. We had to change that. Bart and I decided to check what options we had and with some imputs I started crafting a newer and fresher top-mounted media bar that would enclose the media details and controls. At first I started creating custom components taking inspirations from another gorgeous KDE app, <strong>Elisa</strong>. The HeaderBar did come out fine but on a closer look, it looked very similar to one of the header's Bart derieved earlier which I felt could be reused here, so I did. As this would be a relatively bigger visual change, I had to consult the VDG who advised me about some intricacies related to it. One of the developers from the VDG, Matthis decided to jump the wagon and started preparing new mockups and I would end up framing a new header, which we all felt was much refreshing and cleaner. (The old mockup still rests on one of the branches on my fork which I check-out every then and now :P)  
With the desktop app in a better state (and I having something conclusive to show :P) we could safely move to discussing the hotter items in the list, that is the Content Discovery and background update of tasks. We had several discussions on which API's to use, consider there were a handful such as gPodder, iTunes, feeddirectory.org, etc. I prepared some mockups and presented them to VDG and to other developers.  
Later that week Kasts was moved to the kdereview, which means the maintainers would have to present it to the KDE Community for minimum standard check which is great because now we will know exactly what the app may be missing, or thing we have overlooked and the features we need to add.