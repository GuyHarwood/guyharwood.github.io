---
title: Custom HttpHandler returns 404 after migrating to Vs2012
slug: custom_http_handler_returns_404_after_migrating_to_vs_2012
date_published: 2012-10-29T18:00:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Visual Studio, asp.net, Configuration
---

After migrating an asp.net web forms app to visual studio 2012 we started to observe some odd behaviour.

The custom http handler used across the site to render images was returning a 404. The framework version had not been changed, nor had any configuration been updated. I decided to do a diff on the web.config from before and after the solution upgrade and sure enough, there it was. The resourceType attribute of the handler element had been switched from ‘unSpecified’ to ‘file’.  You can read more about handler configuration and the resourceType attribute on [the iis.net documentation page.](http://www.iis.net/configreference/system.webserver/handlers/add)
