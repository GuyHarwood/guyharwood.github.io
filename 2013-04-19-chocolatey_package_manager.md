---
title: Chocolatey Package Manager for Windows
slug: chocolatey_package_manager
date_published: 2013-04-19T12:16:00.000Z
date_updated: 2018-08-20T15:48:30.000Z
tags: Automation
---

Setting up a fresh dev box with all your favourite tools is something that every developer should automate.  This is fairly straightforward on OSX and Linux thanks to the great package management ecosystem, but support for this on Windows has historically been poor, until some genius called [Rob Reynolds](http://ferventcoder.com/) came up with the idea of [Chocolatey](https://chocolatey.org/) – a package manager for Windows.  It runs at the command line and accepts various commands that automate the management of software installs.

While you can do these ‘one by one’ there is support for batch installs.  For a batch install you create a package config file containing the program package ids from the Chocolatey Gallery that you want included, and pass this to Chocolatey to consume.  I created one for all the tools i use on a regular basis.  I can then pass this as an argument to Chocolatey and it will install everything in the list, one by one…

    <?xml version="1.0" encoding="utf-8"?> 
    <packages> 
        <package id="notepadplusplus" /> 
        <package id="fiddler" /> 
        <package id="GoogleChrome" /> 
        <package id="git" /> 
        <package id="TortoiseGit" /> 
        <package id="reflector" /> 
        <package id="winrar" /> 
        <package id="VirtualCloneDrive" /> 
        <package id="rdcman" /> 
        <package id="greenshot" /> 
        <package id="everything" /> 
        <package id="sysinternals" /> 
        <package id="vlc" /> 
        <package id="sublimetext2" /> 
        <package id="filezilla" /> 
        <package id="paint.net" /> 
        <package id="expresso" /> 
        <package id="lockhunter" /> 
        <package id="linqpad4" /> 
        <package id="dotPeek" /> 
        <package id="winmerge" /> 
        <package id="diffmerge" /> 
        <package id="NugetPackageExplorer" /> 
        <package id="firefox" /> 
        <package id="opera" /> 
        <package id="safari" /> 
    </packages> 
    

Open a command line, with administrator priveliges and type cinst packages.config

Simple installer automation
