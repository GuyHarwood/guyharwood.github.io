---
title: Working with Visual Studio Database Projects
slug: working_with_visual_studio_database_projects
date_published: 2012-10-26T11:45:00.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: Visual Studio, Sql Server, Database Projects
---

Visual Studio keeps a SQL file for each and every object in the database, and it can be cumbersome to have to maintain these manually, especially when working with tables. A more productive process is to have visual studio create a database locally for you, make your modifications in SQL Server Management Studio, and then synchronise these changes directly into Visual Studio.

### Initialising the database project schema

If you haven’t done already, create a new database project, relative to the version of SQL Server you are using – in my case, this is SQL Server 2008…
[![](http://4.bp.blogspot.com/-SSnfe8rQ3d0/UIqSF5X8dnI/AAAAAAAAAw4/7W6L1FJeEfs/s640/1.png)](http://4.bp.blogspot.com/-SSnfe8rQ3d0/UIqSF5X8dnI/AAAAAAAAAw4/7W6L1FJeEfs/s1600/1.png)

If you already have a database schema, you will need to import this into the project. Right click the database project node in solution explorer and choose ‘Import Database Objects and Settings…’![](http://1.bp.blogspot.com/-EKqLDSZ_02E/UIqSHYeF_OI/AAAAAAAAAxA/EIWDjS3dUdU/s1600/2.png)

The ‘Import Database Objects and Settings…’ option can also be accessed from the Project Menu.

Run through the wizard that appears and your existing schema will be imported into your database project, resulting in a sql file for each schema object…![](http://1.bp.blogspot.com/-kz-XN-nSC-c/UIqSH9un6eI/AAAAAAAAAxE/vkYUeSPyXqI/s1600/3.png)

### Creating a local copy of the database

Now that your schema is defined in the database project, you can deploy this to an instance of SQL Server directly from Visual Studio. Double click the properties node in solution explorer and switch to the ‘deploy’ tab, where you can configure the scope of your deployment settings – are they just for you, or something you want to configure for every developer using the project. A typical scenario for ‘My isolated development environment’ would be your local development PC and for ‘My project settings’ a development server used for pre-production testing. These settings are build configuration specific, so you could configure settings that would push to your local test server, a pre-production environment, or even a live server if you or your team have enough confidence in the process.
![](http://4.bp.blogspot.com/-H65lrAk2Yuo/UIqSIcWwmsI/AAAAAAAAAxI/RmXNC8_LWLc/s1600/4.png)

The ‘Deploy action’ should be set to create a deployment script and deploy to the database if you want it all done for you. Alternatively, you can just take the script it creates and run it in SQL Server yourself. If you do this, you must ensure SQLCMD mode is enabled, as the script utilises this SQL Server feature heavily in the created script, and it would fail to run without it set. The target connection must be set to enable the deploy tool to target your server and database.

Setting SQLCMD mode…
![](http://2.bp.blogspot.com/-vYafIoI6uks/UIqSI-SqtBI/AAAAAAAAAxU/bVh5YRz7fTk/s1600/5.png)

### Synchronising changes into your project schema

While it is possible to develop your schema directly in Visual studio by editing the SQL file for each schema object in the text editor, it can be very cumbersome, especially with tables, as the keys and indexes are stored in their own files. A more fluid and familiar process is to make the schema changes within SQL Server Management Studio, and synchronise these back into the database project. To do this you must open the ‘schema view’ window in Visual Studio…
![](__GHOST_URL__/content/images/2016/01/001-6.png)

Select the Top Node (which will have the same title as your database project), and click the ‘Compare Schema’ button…
![](__GHOST_URL__/content/images/2016/01/002-3.png)

This will open the ‘Compare Schema’ dialog, where you specify the source and target schemas. The source is your local database that you have made your modifications to, and the target is the Schema of your database project. Visual Studio automatically sets the source to your database project, so you must click the swap button to change this…
![](__GHOST_URL__/content/images/2016/01/003.png)

Now that your database project is set as the target schema, you must select your local database as the source schema on the left hand side of the dialog. If your database is not in the list, click ‘new connection’ to add it. Once you have confirmed your settings the schema comparison will take place and the results presented to you in a visual studio tab…
![](__GHOST_URL__/content/images/2016/01/004.png)

Clicking on each schema object will focus the Object Definitions window on the specific differences between target and source, enabling you to review each one. Click the ‘write updates’ button to push the schema changes to the target. Inspect the schema in your database project to verify that the schema changes have been propagated correctly.

Its worth noting that the process in Visual Studio 2012 is pretty much the same, with just a few minor differences.
