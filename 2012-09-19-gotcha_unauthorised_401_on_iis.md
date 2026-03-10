---
title: Gotcha – Unauthorised 401 on IIS
slug: gotcha_unauthorised_401_on_iis
date_published: 2012-09-19T11:56:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Windows Server, IIS
---

One of the most common issues i see developers having with running websites on IIS7 – 8 is permissions. When IIS7 was made available it introduced the concept of an ‘Application Pool Identity’, which effectively means that [the host process for your site in IIS is run under the context of a Windows account created specifically for the application pool.](http://blogs.iis.net/webdevelopertips/archive/2009/10/02/tip-98-did-you-know-the-default-application-pool-identity-in-iis-7-5-windows-7-changed-from-networkservice-to-apppoolidentity.aspx)

When setting up some sites on a brand new Windows Server recently i was getting brick walled with the following error…

    HTTP Error 401.3 – Unauthorized
    You do not have permission to view this directory or page because of the access control list (ACL) configuration or encryption settings for this resource on the Web server.
    

I checked the directory permissions, and sure enough the application pool user had the right privileges. I checked the application pool settings, and it was configured to run under ApplicationPoolIdentity. I was stumped.  So I started exploring the authentication feature in IIS, figuring i had missed something, and stumbled upon a setting that i hadn’t seen before, which was how to configure the account used for anonymous access to the website...
![](__GHOST_URL__/content/images/2016/01/001-7.png)

It was set to IUSR, which didn’t have directory permissions on the web site in question.

For a full run down on Application Pools in IIS you should check out [this article](http://learn.iis.net/page.aspx/624/application-pool-identities/) on iis.net
