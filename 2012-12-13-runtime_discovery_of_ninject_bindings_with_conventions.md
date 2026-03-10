---
title: Runtime discovery of Ninject Bindings with Conventions
slug: runtime_discovery_of_ninject_bindings_with_conventions
date_published: 2012-12-13T13:20:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Dependency Injection, Ninject
---

Ninject can figure out a lot of your interface implementation bindings with a few hints on where to look using the conventions extension.

Typing out lots of explicit bindings for Ninject almost takes on the same kind of monotony that comes with declarative bindings in XML (think old style structure map bindings!).  Lets be clear – a serious amount of care and consideration should go into your bindings, especially if you build specific binding profiles for certain configurations (think traditional client – server vs distributed).  But there are times, especially during proof of concept and product incubation cycles where it would be nice if Ninject took the hint and worked the bindings out on its own, and it can.

Ninject has some [great extensions](http://www.ninject.org/extensions.html), one of which is convention based bindings.  What it allows you to do is make suggestions on how to bind concrete implementations to interface declarations using conventions that you define.  For example, lets say i wanted Ninject to scan a specific set of assemblies, and bind implementations based upon the naming convention…

    kernel.Bind(x => x.FromAssembliesMatching("Company.Product.Namespace.*")
            .SelectAllClasses()
            .BindDefaultInterface()
            .Configure(b => b.InRequestScope()));
    

If i have an interface called IContactService, Ninjects first choice for an implementation would be ContactService.
