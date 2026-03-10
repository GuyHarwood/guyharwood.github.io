---
title: Entity Framework Tests with LocalDB
slug: entity_framework_tests_with_local_db
date_published: 2013-03-30T21:14:00.000Z
date_updated: 2018-08-20T15:48:30.000Z
tags: EntityFramework, UnitTesting
---

Whenever i work on an Entity Framework Database First project i often think back to when i first saw [Ayendes post on using sqlite for an in memory database while unit testing nHibernate](http://ayende.com/blog/3983/nhibernate-unit-testing).  It keeps things simple for testing code that works directly against the data store abstraction.When i came across SQL Server LocalDB i realised a similar setup could be had for lightweight integration testing of code that uses Entity Framework to manipulate a database.

### The Code

I created a base class...

    using System;
    using System.Data.Entity;
    using System.Diagnostics;
    using NUnit.Framework;
    
    namespace EntityFrameworkTests
    {
    	//TODO replace this stub with your own
    	public class MyDataContext : DbContext
    	{}
    
    	[TestFixture]
    	public abstract class DataTest
    	{
    		private readonly Lazy<MyDataContext> _dbContext = new Lazy<MyDataContext>(CreateContext, false);
    		
    		private static MyDataContext CreateContext()
    		{
    			var context = new MyDataContext();
    			
    			if (context.Database.Exists())
    				context.Database.Delete();
    
    			context.Database.Create();
    
    			return context;
    		}
    
    		protected MyDataContext DataContext
    		{
    			get { return _dbContext.Value; }
    		}
    		
    		/// <summary>
    	        /// Inheritors should implement this to include their own test teardown code
    	        /// </summary>
    		protected abstract void OnTearDown();
    
    		[TearDown]
    		public void Destroy()
    		{
    			if (!_dbContext.IsValueCreated)
    				return;
    
    			try
    			{
    				_dbContext.Value.Database.Delete();
    			}
    			catch (Exception ex)
    			{
    				Assert.Inconclusive("Could not delete database, it may need to be removed manually.\n Check connection string for database location detail.\n Error Message:{0}", ex.Message);
    			}
    			finally
    			{
    				if (_dbContext != null)
    					_dbContext.Value.Dispose();
    			}
    			this.OnTearDown();
    		}
    	}
    }
    

You will need to swap out MyDataContext for your own, and also ensure that the project in which it resides has a connection string which connects to your LocalDB - details on the specifics [here](https://www.connectionstrings.com/sqlconnection/localdb-named-instance/).

You will notice there isn't much to it - this is because Entity Frameworks DataContext is capable of creating an instance of the database that it is modelled on - a very useful feature.

### Leaky LINQ

The added bonus of using LocalDB is that it uses the same engine as its big brother version.  So if you use the standard or enterprise edition of SQL Server in production you are getting as close as possible to testing against the real thing with LocalDB.  When using a different engine for in memory testing (such as SQLite, for example) you are vulnerable to leaky abstractions because the database engine underneath could interpret and execute the queries in a very different manner.
