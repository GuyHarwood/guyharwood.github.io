---
title: Manually installing Jenkins Slave as a service on Windows
slug: manually-installing-jenkins-slave-as-a-service-on-windows
date_published: 2016-05-26T08:06:46.000Z
date_updated: 2018-08-20T15:48:31.000Z
tags: jenkins, CI, Automation, Java
---

One option of installing a Jenkins slave on Windows is to install it as a windows service.  This has the added benefit of automatic startup and running under the credentials of a service account if you so wish.

The [documented process](https://wiki.jenkins-ci.org/display/JENKINS/Step+by+step+guide+to+set+up+master+and+slave+machines) suggest invoking java web start from the slave node via a browser, and then letting it automatically install the windows service for you.  This can prove challenging in a corporate environment where permissions can be heavily restricted.

You can work around this, providing you have local administrative privileges, by installing the windows service yourself, using the [sc utility](https://technet.microsoft.com/en-us/library/bb490995.aspx).  The tricky bit is getting hold of the jenkins slave binaries, which can be done one of two ways.  Either by downloading the slave.jar, and invoking it on the slave machine, which in turn unpacks the jenkins slave service binaries into the 'remote root directory' you specified when setting up the slave or simply by invoking the java web start from the orange branded java button on the page.  Providing the dialog opens and offers the 'Install as a service' option it may get as far as copying the files locally, but fail on service installation.

To invoke slave.jar simply copy the command conveniently shown on the slave node page - this will only be shown if you have chosen java web start as your launch method on the slave configuration page.  So, open up a command prompt and paste in the exact command shown on the page, which will have the format...

    java -jar slave.jar -jnlpUrl http://<your-jenkins>:8080/computer/<slave-node-hostname>/slave-agent.jnlp -secret <your-secret>
    

You should end up with a successful connection once the client is up and running, and this will have unpacked the service binaries into the remote root directory specified on the slave node configuration page.

Now that you have the service binaries, simply make the following call to sc (notice the space between the binPath parameter and the actual value, this is required)...

    > sc.exe create Jenkins binPath= "d:\path-to-binaries\jenkins-slave.exe"
    

Now navigate to the slave node page on your master and it should be connected.  Don't forget to change the service start up type to automatic and set the service to run-as a service account of your choice if need be.
