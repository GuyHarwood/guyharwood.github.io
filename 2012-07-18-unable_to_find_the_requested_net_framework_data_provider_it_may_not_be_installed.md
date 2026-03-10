---
title: Unable to find the requested .Net Framework Data Provider. It may not be installed
slug: unable_to_find_the_requested_net_framework_data_provider_it_may_not_be_installed
date_published: 2012-07-18T11:59:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Visual Studio, dotnet
---

This usually means that a provider specified in one of your web.config connection strings is not installed on your machine.  In my specific case the offender was this…

    <add name="MyContext" 
        connectionString="Data Source=|DataDirectory|myDatabase.sdf"
        providerName="System.Data.SqlServerCe.4.0"/>
    

I needed to install SQL Server Compact 4.0

The same error will crop up for various other providers also.
