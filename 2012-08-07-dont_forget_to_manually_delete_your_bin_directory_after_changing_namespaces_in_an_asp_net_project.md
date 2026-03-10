---
title: Dont forget to manually delete your bin directory after changing namespaces in an asp.net project
slug: dont_forget_to_manually_delete_your_bin_directory_after_changing_namespaces_in_an_asp_net_project
date_published: 2012-08-07T11:58:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Visual Studio, asp.net
---

If you change the default application name on the web tab of the project properties in your asp.net project, and you have previously compiled your project, you may find some strange errors when you next build your project.  This is because the dll names that the build outputs in your bin directory have changed, and when you go to run your application, asp.net sees both sets – the old and the new, and loads them both.

Cleaning your solution will not rectify the issue because as far as asp.net is concerned, they are not an output of the build, so it respects the fact that you may have put them there manually (but you wouldn’t do that in a web application project anyway, right?). Just delete the old dlls (and pdb if its there) and you are good to go. This happened to me in an asp.net mvc project and it complained that my routes already existed.
