---
title: Configuring MongoDB and C# Client for TLS / SSL
slug: configuring-mongodb-and-c-client-for-tls-ssl
date_published: 2016-11-29T16:24:12.000Z
date_updated: 2018-08-20T15:48:31.000Z
---

MongoDB supports secure data transmission but requires some configuration on both the client and the server before it will work correctly.  This article covers the server configuration (can be applied to a mongo instance on any OS) and the [C# client](https://www.nuget.org/packages/MongoDB.Driver/2.3.0) currently at version 2.3.0.  A self signed certificate is used for development purposes.

This includes the following steps...

- Create a Self Signed Certificate with OpenSSL
- Update the MongoDB configuration file
- Configure the C# Client

## Create a self signed certificate with OpenSSL

Make sure you have [OpenSSL](https://www.openssl.org/) installed, and enter the following commands into your favourite terminal / command prompt...

    openssl genrsa -des3 -out server.key 2048
    openssl rsa -in server.key -out server.key
    openssl req -sha256 -new -key server.key -out server.csr -subj '/CN=localhost'
    openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
    

When prompted, enter a password for the certificate.  You will need this in the next step.

Mongo needs a pem file, which is a container format that simply contains the certificate and the private key in the same file.  You can generate this in bash with the following command...

    cat server.crt server.key > cert.pem
    

If you are running windows and haven't installed bash just create the cert.pem file, paste in the contents of server.crt, then the contents of server.key.

Copy the cert.pem file to the Mongo directory

## Update the MongoDB Config file with SSL details

Open your [MongoDB configuration file](https://docs.mongodb.com/v3.2/reference/configuration-options/) and add the **net** section if it does not already exist.  It's worth noting that the file is in YAML format, so you must use spaces for indentation, not tabs.

    net:
        bindIp: localhost
        ssl:
            mode: requireSSL
            PEMKeyFile: /Mongo/cert.pem
            PEMKeyPassword: <certificate password created in previous step>
            allowInvalidCertificates: true
            allowInvalidHostnames: true
    

## Start mongod

Now you have configured the certificate, start mongod.  Any problems with the configuration file will be written to the [log target specified in the configuration file](https://docs.mongodb.com/v3.2/reference/configuration-options/#core-options)

A quick and easy way to check client connectivity is to run the mongo executable, making sure you specify the parameters which instruct it to ignore the self signed certificate....

    mongo --ssl --sslAllowInvalidHostnames --sslAllowInvalidCertificates
    

Any errors should be sent to standard out and, as above, to the mongod log file.

## Configure C# Client connection

Configuring the client connection to handle SSL with a self signed certificate is fairly straightforward, you just need to instantiate an SSL settings instance and override certificate validation...

    var connectionString = ConfigurationManager.ConnectionStrings["MongoDb"].ConnectionString;
    
    var url = new MongoUrl(connectionString);
    var clientSettings = MongoClientSettings.FromUrl(url);
    clientSettings.SslSettings = new SslSettings();
    clientSettings.SslSettings.CheckCertificateRevocation = false;
    clientSettings.UseSsl = true;
    clientSettings.VerifySslCertificate = false;
    
