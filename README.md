MySql Server plugin for Dokku
------------------------

Project: https://github.com/progrium/dokku

This is a fork of the https://github.com/Kloadut/dokku-md-plugin that creates mysql databases on a standalone server or cluster. This makes it different from most dukku db plugins that create a dokku container for each database.

That seems like way to much overhead to me :)

Installation
------------

Install the plugin
```
cd /var/lib/dokku/plugins
git clone https://github.com/micwallace/dokku-mysql-server-plugin mysql-server
dokku plugins-install
```

Then replace the master config file with your database credentials. 
```
vi /home/dokku/.mysql-configs/master
```

NOTE: The user specified must have root privilleges, and if mysql is running on a remote host, must be allowed to login remotely from dokku's IP address.

Commands
--------
```
$ dokku help
     mysqldb:create <app>      Create a MySql container
     mysqldb:delete <app>      Delete specified MySql container
     mysqldb:info <app>        Display database informations
     mysqldb:link <app> <db>   Link app to an existing database
     mysqldb:console <app>     Open mysql-console to MySql database
     mysqldb:dump <app> <file> Dump default db database into file <file> is optional. 
```

Info
--------
This plugin adds following envvars to your project automatically:

* DATABASE_URL
* DB_HOST
* DB_PORT
* DB_NAME
* DB_USER
* DB_PASSWORD

Simple usage
------------

Create a new DB:
```
$ dokku mysqldb:create foo            # Server side
$ ssh dokku@server mysqldb:create foo # Client side

-----> MySQL Database created for: foo

       Host: 172.16.0.104
       User: 'foo'
       Password: 'RDSBYlUrOYMtndKb'
       Database: 'foo'
       Public port: 3306
```

Deploy your app with the same name (client side):
```
$ git remote add dokku git@server:foo
$ git push dokku master
Counting objects: 155, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (70/70), done.
Writing objects: 100% (155/155), 22.44 KiB | 0 bytes/s, done.
Total 155 (delta 92), reused 131 (delta 80)
remote: -----> Building foo ...
remote:        Ruby/Rack app detected
remote: -----> Using Ruby version: ruby-2.0.0

... blah blah blah ...

remote: -----> Deploying foo ...
remote: 
remote: -----> App foo linked to foo database
remote:        DATABASE_URL=mysql://root:RDSBYlUrOYMtndKb@172.16.0.104/foo
remote: 
remote: -----> Deploy complete!
remote: -----> Cleaning up ...
remote: -----> Cleanup complete!
remote: =====> Application deployed:
remote:        http://foo.server
```


Advanced usage
--------------

Inititalize the database with SQL statements:
```
cat init.sql | dokku mysqldb:create foo
```

Deleting databases:
```
dokku mysqldb:delete foo
```

Linking an app to a specific database:
```
dokku mysqldb:link foo bar
```

Database informations:
```
dokku mysqldb:info foo
```

Login to mariadb console
```
dokku mysqldb:console
```

Import to existing database
```
dokku mysqldb:console < import.sql
```
