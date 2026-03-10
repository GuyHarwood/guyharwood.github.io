---
title: "SQL71501: User has an unresolved reference to Login in Visual Studio 2012 Database Project"
slug: sql71501_user_has_an_unresolved_reference_to_login_in_visual_studio_2012_database_project
date_published: 2012-12-13T12:20:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Visual Studio, Sql Server
---

If you create application specific logins (which you should) then you are going to come across this error when trying to build your solution.

**UPDATE: 15th May 2013 - Jim commented a fix for this -** "I have resolved this, it's quite simple to add the logins. You have to select include 'Non-Application-scoped' object types in the options when you do a schema compare.  You can then just import the logins into your regular project, and the references are sorted."

Thanks to Jim.

Unlike vs2010, Vs2012 does not have a database server project type, only database project.  This is a bit of a missing link in Vs2012, as with 2010 you could simply create a server project, and reference the server project from your database project, ensuring that the reference to the server level login was valid.

This has been [raised as a bug on connect](http://connect.microsoft.com/VisualStudio/feedback/details/766789/sql-server-database-project-in-vs-2012) but no fix is available at the time of writing and having just recently installed vs2012 update 1 it may not be remedied for some time.  A simple workaround is to add another database project to your solution, and make its target database the master database.  When it imports the master database objects it will include the ‘unresovlved reference to Login’ that the build process was complaining about.
