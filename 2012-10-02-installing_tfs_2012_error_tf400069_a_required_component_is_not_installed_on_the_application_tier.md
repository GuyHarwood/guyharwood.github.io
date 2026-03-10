---
title: "Installing TFS 2012 - Error: TF400069 A required component is not installed on the application tier"
slug: installing_tfs_2012_error_tf400069_a_required_component_is_not_installed_on_the_application_tier
date_published: 2012-10-02T16:00:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Windows Server, TFS
---

If your TFS Application Tier resides on a different instance of windows to your SQL Server, you may come across this error...

![](http://1.bp.blogspot.com/-Ak2VaFoXkV0/UI_ptLhEsII/AAAAAAAAAz0/PZbzqEBN8V0/s1600/TFS+Error+1.jpg)

The issue here is that TFS needs the SQL Server Client Tools installing on the same box, so it can manipulate the data via the Analysis Services API.

![](http://3.bp.blogspot.com/-aXRXMZgkASQ/UI_prT60JwI/AAAAAAAAAzs/knEGWBk6HeY/s1600/sqlInst01.jpg)

This should solve the issue and you can continue with the installation of the TFS Application Tier on your Windows Server.
