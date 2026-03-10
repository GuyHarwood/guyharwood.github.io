---
title: xUnit tests with asp.net 5
slug: xunit-tests-with-aspnet5
date_published: 2016-01-20T13:53:53.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: asp.net
---

If you've been playing around with the cross platform asp.net 5 bits (currently RC1) you will no doubt be aware of how difficult it can be to get things playing nicely together.  My first task was to get a test framework configured with command line execution.

To scaffold a class library project (assuming you have the [yeoman asp.net generator](https://www.npmjs.com/package/generator-aspnet) installed), perform the following steps...

- open terminal and navigate to your dev folder
- run yo aspnet
- choose Class Library as the project type
- cd to the project root directory
- hold on a moment...

At this point you are probably thinking you should run dnu restore but as of the RC1 version of asp.net 5, the project.json file will contain configuration that doesn't work cross platform.  You must remove the net451 reference and replace dotnet5.4 with dnxcore50.  Why? because the initial 2 references are to the full 'windows only' framework, and dnxcore50 is the cross platform one.  Thanks to [James Sturtevant for the fix](http://www.jamessturtevant.com/posts/compiling-aspnet-5-rc1-on-a-mac-or-linux/).

Replace

    
    "frameworks": {
        "net451": { },
        "dotnet5.4": {
        "dependencies": {
        "Microsoft.CSharp": "4.0.1-beta-23516",
        "System.Collections": "4.0.11-beta-23516",
        "System.Linq": "4.0.1-beta-23516",
        "System.Runtime": "4.0.21-beta-23516",
        "System.Threading": "4.0.11-beta-23516"
            }
        }
    }
    

with...

    
    "frameworks": {
        "dnxcore50": {
            "dependencies": {}
        }
    }
    

You could retian the references to CSharp, Linq, Threading etc if you wish, but you don't need them for the purposes of this exercise.

Now you can run dnu restore followed by dnu build.

Time for a test framework

Those clever chaps over at xunit have already been busy adding cross platform support.  All you need to do is install it from the command line...

    dnu install xunit
    dnu install xunit.runner.dnx
    

now to write some tests...

Your project now has a reference to the xUnit library and the dnx based test runner.  To get a working test we need to:

Import the xUnit namespace, and add passing and failing tests...

    
    using Xunit;
    
    namespace dnxDemo
    {
        public class Tests
        {
            [Fact]
            public void Passing()
            {
                Assert.Equal(1,1);
            }
            
            [Fact]
            public void Failing()
            {
                Assert.Equal(1,2);
            }
        }
    }
    

now execute dnu build and you should have a successful build...
![](__GHOST_URL__/content/images/2016/01/001-1.png)

So now we can build, lets execute the tests...

open the project.json file and append the following to create a new command test that will invoke the xUnit test runner...

    "commands": {
        "test":"xunit.runner.dnx -xml testresults.xml"
    }
    

To invoke the command run dnx test and you should get one passing test and one failing test...
![](__GHOST_URL__/content/images/2016/01/002-1.png)

The above code will work on Linux, OSX and Windows, and you can [download the source](https://github.com/GuyHarwood/xunitdnx) from Github.

It's quite liberating to be able to do this on a unix based machine, and welcome the prospect of no longer being tied to windows for future .net platform development.
