---
title: "Build Warning - MSB3247: Found conflicts between different versions of the same dependent assembly"
slug: build_warning_msb3247_found_conflicts_between_different_versions_of_the_same_dependent_assembly
date_published: 2012-12-18T13:40:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: TFS
---

    Build Warning - MSB3247: Found conflicts between different versions of the same dependent assembly
    

I kept seeing this in the output window when building a solution, but all it would show me when i clicked on the warning was the Microsoft.Common.targets file where the rule was defined.

In order to get to the root of the issue i had to enable Detailed build output in visual studio, which you can do from the Tools –> Options dialog…

![](__GHOST_URL__/content/images/2016/01/001-5.png)

I then cleaned the solution and did a rebuild, which led to a 115000 line build output!  Copying this into notepad++ and searching for the warning code (MSB3247) revealed some detail that would help me track it down…

![](__GHOST_URL__/content/images/2016/01/002-2.png)

Walking up the file led me to the project in question, which was an MVC4 web project.  Having recently added a WebApi controller to this project, it started to become clear what MsBuild was complaining about.

We had references to System.Net.Http.Formatting and System.Net.Http.  System.Net.Http relies upon version 2.0.0.0 of System.**Web**.Http, while System.Net.Http.Formatting uses version 4.0.0.0 of System.**Web**.Http (confused yet?).

I came across [a post on Connect](http://connect.microsoft.com/VisualStudio/feedback/details/733213/warning-generated-for-default-asp-net-mvc-4-project) that suggested a workaround of explicitly defining the version of System.Net.Http to use by hand editing the project file.  Ok, it worked, but then i found a [better solution on Stack Overflow](http://stackoverflow.com/a/9950089/137474) which was to use [AsmSpy, a nifty little tool written by Mike Hadlow](http://mikehadlow.blogspot.co.uk/2011/02/asmspy-little-tool-to-help-fix-assembly.html) for this exact scenario.

2 more tools in the arsenal, i learned something today.
