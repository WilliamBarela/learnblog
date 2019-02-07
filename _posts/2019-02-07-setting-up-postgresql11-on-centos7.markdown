---
layout: post
title:  "Setting Up PostgreSQL 11 on centos7"
date:   2019-02-07 04:30:26 -0400
---

Now that we are coming into the Spring time and the winter chill has left for the most part, I realized I needed to crack my knuckles, dust off my blog, and get some valuable information out to the community.

In the process of setting up an environment for developing a multi-user, web application, I decided to use PostgreSQL 11 as my database due to the traffic which we are projecting to get from it.
In trying to keep the good practice of having separations of concerns for data and the OS, I decided to change the default location of the data_directory of Postgres. However, search for a tutorial on this topic, I found none which were current.
Going to **[DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-move-a-postgresql-data-directory-to-a-new-location-on-ubuntu-16-04)**, the only tutorial that I could find on the topic was for Ubuntu 16.04 and for PostgreSQL 9.5.
After attempting to modify the solution to my environment and PostgreSQL 11, I quickly realized that much had changed between 9.5 and 11. Further investigation of a sytem that I had with version 10 also proved to me that there have been some major changes in file structure of PostgreSQL in this latest version.

So, I am writing this blog post to save you the trouble of having to search /var/lib trying to piece patch what now goes where. Hopefully this will help you.
Additionally, I will give a short introduction on where to get the RPM and how to install PostgresSQL.


# Installing PostgreSQL 11:
First all you need to go to the [PostgreSQL download page](https://www.postgresql.org/download/linux/redhat/) and select the RPM for your environment and version of choice. Based on my selection of my OS being Centos 7, the architecture being x86_64 (use `uname -a` if you are unsure), and wanting PostgreSQL 11, this is the instructions which are given.
By the time you read this, Postgres may have already updated to a new minor version, so the first line I code in the following has to be modfied:

```
$ #! /etc/bash
$ # sudo yum install https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-centos11-11-2.noarch.rpm     # replace this line
$ sudo yum install postgresql11
$ sudo yum install postgresql11-server
$ sudo yum install pgadmin4    # optional GUI admin interface

$ # Once you have selected y to the above and they are install, you need to setup your database.
$ # The following also allows you to have Postgres start on startup (useful in case you reboot).
$ # The last command starts the service.
$ sudo postgresql-setup initdb
$ sudo systemctl enable postgresql.service
$ sudo systemctl start postgresql.service
```

# Moving PostgreSQL Data Directory:

Okay, assuming everything went as expected for the install, this where the changes are.
In previous versions of PostgreSQL, the data_directory was located in `/var/lib/postgresql/XX/main`. 
But on CentOS 7, you will see that they are now in `/var/lib/pgsql/11/data`. You can verify this by running psql with the postgres user:

```
$ sudo -u postgres psql
```

And then running the following query:

```
postgres=# SHOW data_directory;
postgres=# \q    -- how to quit psql
```

The output of this is:

```
postgres=# show data_directory;
     data_directory
------------------------
 /var/lib/pgsql/11/data
(1 row)
```

So, now you need to shutdown the PostgreSQL service. One change here is that now the version has to be included in the service name:

```
$ sudo systemctl stop postgresql-11
```

Verify that it has indeed been shutdown:

```
sudo systemctl status postgresql-11
```

...to be continued.


