---
comments: true
date: 2011-12-20 15:39:28
layout: post
slug: connecting-to-a-remote-db2-instance
title: Connecting to a remote DB2 instance
wordpress_id: 229
tags:
- Categories
---

# Preparing the DB2 Client

## On Windows

After the initial installation, to be able to connect using the command line, just run:
    
    
    db2cmd -i -w db2clpsetcp


## On Unix

Source the db2profile

    
    
    . $HOME/sqllib/db2profile
    


# Accessing a Remote Database

To be able to access a remote database you have to catalog the node and the database.

    
    
    db2 "catalog tcpip node remote remote remotehost server 50000"
    db2 "catalog database testdb as remotedb at node remote authentication server"
    



As an example for our internal DB2 test instance:

    
    
    db2 "catalog tcpip node usphlesx remote usphlesxcab04.pgdev.sap.corp server 50000"
    db2 "catalog database sample as cab04smp at node usphlesx"
    


Once you did that, you can list the open catalogs:

    
    -
    db2 "list node directory"
    db2 "list database directory"
    


To uncatalog the node/database run:

    
    
    db2 "uncatalog database remotedb"
    db2 "uncatalog node remote"
    



##### Further reading
* [IBM](http://publib.boulder.ibm.com/infocenter/db2luw/v9/nav/3_1_2_3)
* [SqlRelay](http://sqlrelay.sourceforge.net/sqlrelay/gettingstarted/db2.html#accessingremote)
