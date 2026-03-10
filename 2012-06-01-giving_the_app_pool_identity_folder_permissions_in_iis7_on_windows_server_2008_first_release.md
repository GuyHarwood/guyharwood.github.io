---
title: Giving the App Pool Identity folder permissions in IIS7 on Windows Server 2008 (First Release)
slug: giving_the_app_pool_identity_folder_permissions_in_iis7_on_windows_server_2008_first_release
date_published: 2012-06-01T12:00:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Windows Server, IIS
---

When setting up a new Application Pool in IIS7 the default user (or identity) is ApplicationPoolIdentity which is a specific user account with minimal rights, created specifically for that application pool to run under the context of that user.

![](http://2.bp.blogspot.com/-46_KLgmCHVs/UIqUkcWWXVI/AAAAAAAAAyw/80nFzekozZ8/s1600/appPools1.jpg)

In Windows Server 2008 (the Vanilla version, as this is ‘slightly fixed’ in R2) you will struggle to assign your App Pools user permissions on your website directories (including any web applications that use this application pool!) simply because they do not appear in the user picker that is shown on the security tab of folder properties. It is documented that you should enter “IIS APPPOOL<App Pool Name>” into the object names box in order to assign that user permissions on your website directory.

![](http://4.bp.blogspot.com/-4t2zS9Jwrms/UIqU8hFBWjI/AAAAAAAAAzI/CC3VTCaOTps/s1600/selectUsers.jpg)

Click ‘Check Names’ and the Application Pool User will not be found…

![](http://2.bp.blogspot.com/-yCvFyG4COr0/UIqUlX4fnZI/AAAAAAAAAy4/hL4LnxnVUtA/s1600/notfound3.jpg)

The only way to do this on Vanilla Windows Server 2008 is to use the ICACLS command line utility.  You can then use the Security tab on the folder properties dialog to further modify the permissions.

More information on this subject…

- [Issue and resolution on Serverfault.com](http://serverfault.com/questions/81165/how-to-assign-permissions-to-applicationpoolidentity-account)
- [New in IIS 7 – App Pool Isolation](http://adopenstatic.com/cs/blogs/ken/archive/2008/01/29/15759.aspx)
- [Application Pool Identities on IIS.net](http://learn.iis.net/page.aspx/624/application-pool-identities/)
