---
title: Upgrading Jenkins on Windows
slug: upgrading-jenkins-on-windows
date_published: 2016-05-25T16:00:42.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: jenkins, CI, Automation
---

This procedure is the same for other operating systems, yet many windows users end up looking for an msi installer, mistakenly thinking that it should be updated in the same way as a traditional windows application.

Jenkins is a java application, and is distributed in a war archive file called jenkins.war.  This archive typically extracts to c:\program files (x86)\jenkins\war\ when the jenkins windows service is invoked for the first time.

When it comes to upgrading Jenkins on Windows the process is very straightforward...

- Stop the Jenkins windows service
- Rename war directory to war.bak
- Rename jenkins.war to jenkins.war.bak
- Copy new jenkins.war into jenkins directory
- Start Jenkins windows service
- Observe creation of war directory which will now contain contents of jenkins.war archive
- Navigate to jenkins home page, at which point you will be informed that Jenkins is preparing your installation
- Once complete, test your existing jobs to verify compatability
- Navigate to 'Manage Jenkins' via the menu and Action any notifications displayed - plugins, data format conversions etc.
- Delete jenkins.war.bak file and war.bak folder
