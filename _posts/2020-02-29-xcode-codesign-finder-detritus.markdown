---
layout: post
title:  "Xcode CodeSign \"resource fork, Finder information, or similar detritus not allowed\""
categories: programming iOS 
tags: xcode codesign iOS 
---

I really enjoy using Xcode and the whole Apple development ecosystem overall, but I must say it does have its funny ways sometimes (laughs in pain).

One of the iOS projects I'm working on started failing on my machine out of the blue when I tried to execute **Product -> Run** for a device. 
Xcode failed during the **Sign** step mentioning `resource fork, Finder information, or similar detritus not allowed` when trying to sign one of our extension bundles. I'm not sure how or why this happened to be honest, but it also impacted other team members.

 From all the information out there, this [official Apple post](https://developer.apple.com/library/archive/qa/qa1940/_index.html) seems to explain what is happening.

 > Code signing no longer allows any file in an app bundle to have an extended attribute containing a resource fork or Finder info.

 However the extended attributes were added to those files in particular I don't know, but following the instructions to call `xattrs -cr` on the failing bundle did work.
 
 Since this app has many bundles (apps and extensions), I had to fix each one individually, which is not great, and because it seems the problem can happen at any other time (e.g. if you browse the DerivedData folder) I decided to go for the nuclear option: **add a Build Phase to the main app target that will clear the whole DerivedData recursively.**
 This has the added benefit that all other team members don't have to worry about it. 

![Clear Mac Xattrs](/assets/xcode-xattrs-build-phase.png)

Thanks Apple.