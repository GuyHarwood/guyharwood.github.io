---
title: Linux Build Server for Visual Studio Online (VSTS/VSO)
slug: linux-build-server-for-visual-studio-online-vsts-vso
date_published: 2017-11-10T16:02:56.000Z
date_updated: 2018-08-20T15:48:31.000Z
---

Setting up a Linux build server for VSTS is not as straightforward as it first seems.  Below are my notes on setting one up...

## Pre-requisites

Official docs on pre-reqs: [https://github.com/Microsoft/vsts-agent/blob/master/docs/start/envubuntu.md](https://github.com/Microsoft/vsts-agent/blob/master/docs/start/envubuntu.md)

## Install the VSTS agent on Ubuntu

[https://www.visualstudio.com/en-gb/docs/build/actions/agents/v2-linux](https://www.visualstudio.com/en-gb/docs/build/actions/agents/v2-linux)

    ~/$ wget https://github.com/Microsoft/vsts-agent/releases/download/v2.111.1/vsts-agent-osx.10.11-x64-2.111.1.tar.gz
    ~/$ mkdir vstsagent && cd vstsagent
    ~/vstsagent$ tar zxvf ~/vsts-agent-osx.10.11-x64-2.111.1.tar.gz
    

## Service configuration

Running config.sh currently fails with: `Failed to initialize CoreCLR, HRESULT: 0x80131500`

This is due to compatibility issues with the icu library v55 on 16.04.

To install v52 run the following...

    wget http://security.ubuntu.com/ubuntu/pool/main/i/icu/libicu52_52.1-8ubuntu0.2_amd64.deb
    sudo dpkg -i libicu52_52.1-8ubuntu0.2_amd64.deb
    

Now you can continue with the documented steps...

run the configuration tool

    ~/vstsagent$ ./config.sh
    

You will be asked for the following...

Replace **myhost** with your VSTS instance...

*URL:* https://**myhost**.visualstudio.com

*PAT (Personal access token):* can be found in your account security section

The agent should be configured to run as a service...

[https://github.com/Microsoft/vsts-agent/blob/master/docs/start/svcsystemd.md](https://github.com/Microsoft/vsts-agent/blob/master/docs/start/svcsystemd.md)

## Known issues

### PATH updates

If you ever update PATH this will not automatically be reflected in VSO agent metadata.  The workaround is to run run the following...

    # update the vso PATH metadata
    ~/vstsagent/env.sh 
    # restart the vso agent service
    ~/vstsagent/svc.sh stop
    ~/vstsagent/svc.sh start
    
