---
layout: post
title:      "rails_university web application"
date:       2019-04-18 03:01:02 -0400
permalink:  rails_university_web_application
---


I decided to write the rails_university web application with a Rails backend as a generalized web application for university departments. Although this project will eventually evolve into a generalized Rails API for the backend of such a web application which will allow the development of various UIs (desktop, mobile, etc.) to easily be built on top of it, currently it contains the views implemented in Rails. Eventually, I hope to fork this project and inlcude documentation on how to build a Docker image of it and run it as a container using NGINX as the web application, PostgreSQL as the database, the Puma application server and Rails. 

The underlying database which has been chosen for this project is PostgreSQL due its being open-source and highly robust for high traffic volume. This `README` should provide basic information on how to get this application up and running so that one can focus on modifying it according to ones institution's needs. I hope this blog will serve to help others who are trying to understand how to get a Rails app up and running from scratch using PostgreSQL.

## Dependencies 
The installation of PostgreSQL is highly dependent on the OS one chooses as is RVM. Once these dependencies are met, please skip to the set up section for detail on how correctly set up and configure PostgreSQL and RVM.

- [PostgreSQL 11](https://www.postgresql.org/download/ "PostgreSQL 11")
- [PostgreSQL 11 Installation](https://wiki.postgresql.org/wiki/Apt "PostgreSQL 11 Installation")
- [RVM](https://rvm.io/ "RVM")
- [Ruby 2.6.2](https://www.ruby-lang.org/en/ "Ruby 2.6.2")
- [Rails 5.2.3](https://github.com/rails/rails/tree/v5.2.3 "Rails 5.2.3")

## Set up
### Use RVM to install Ruby 2.6.2.

```bash
rvm install 2.6.2

# if you wish to use Ruby 2.6.2 by default, run:
rvm --default use 2.6.2
```

### Install necessary Ruby gems (including Rails)

```bash
# update your gems:
gem update --system

# if you want to save disk space, run:
echo "gem: --no-ri --no-rdoc" >> $HOME/.gemrc

gem install rails -v 5.2.3
gem install bundler pg pry rspec

# command used to set up files for Rails to use PostgreSQL (don't run):
# rails new rails_university --database=postgresql
```

### Create user in PostgreSQL 11  for rails_university

```bash
# create a superuser (name is your choice, but you have to use it in your config/database.yml:
sudo -u postgres createuser -s university 

# drop into PostgreSQL and change your password securely:
sudo -u postgres psql

postgres=# \password university
```
### Aside - Useful commands if one drops into PostgreSQL directly:

```sql
-- list all databases:
\l

-- list all roles;
\du

-- drop database:
drop database database_name;

-- drop role:
drop role database_role;
```

### Install gems from Rails Gemfile:

```bash
bundle install
```

### Configure the config/database.yml file:
If you are running PostgreSQL locally, make sure to inlcude the `host: localhost` statement.
Else, you will not be able to run your setup or migrations.
NOTA BENE: DO NOT keep your username, password, or even the name of the database in your version control.
If you do not trust yourself with this, run `echo "config/database.yml" >> .gitignore` before your next commit.

```yml
# vim config/database.yml

default: &default
  adapter: postgresql
  encoding: unicode
  host: localhost
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: rails_university_development
  username: university
  password: FILL_IN_YOUR_PASSWORD_FOR_university_HERE

test:
  <<: *default
  database: rails_university_test
  username: university
  password: FILL_IN_YOUR_PASSWORD_FOR_university_HERE
```

### Create your databases through Rails using your PostgreSQL user for the app:

```bash
# create the databases based on config/database.yml
rake db:setup

# complete your migrations
rake db:migrate
```

### Set up is complete!

You now have a working Ruby on Rails application set up with your database. If you get any errors about fileutils and ENV variables already being set, you most likely just have an old version of fileutils installed in your gems. To solve this, run:

```bash
gem uninstall fileutils
gem update fileutils --default
```

### Footnotes for PostgreSQL

```bash
# if you want to verify the permissions that have been granted to the PostgreSQL user
sudo -u postgres psql
postgres=# \du

# if you want to verify in PostgreSQl that the databases have been created:
sudo -u postgres psql
postgres=# \l
```

### Later migrating your database

Please heed the warning in `db/schema.rb` when trying to recreate your database on a new machine:

```ruby
# This file is auto-generated from the current state of the database. Instead
# of editing this file, please use the migrations feature of Active Record to
# incrementally modify your database, and then regenerate this schema definition.
#
# Note that this schema.rb definition is the authoritative source for your
# database schema. If you need to create the application database on another
# system, you should be using db:schema:load, not running all the migrations
# from scratch. The latter is a flawed and unsustainable approach (the more migrations
# you'll amass, the slower it'll run and the greater likelihood for issues).
#
# It's strongly recommended that you check this file into your version control system.
```

## How to use

### Move `config/database.yml.bak`
Assuming you followed the steps in the above set up exactly, you can follow the subsequent steps to get running

```bash
mv config/database.yml.bak config/database.yml
```

### Read about encrypted credentials in Rails 5.2

- [Rails Encrypted](https://www.engineyard.com/blog/rails-encrypted-credentials-on-rails-5.2 "Rails Encrypted Credentials")


## Additional Useful Links

- [Rails and Postgresql](https://medium.com/@frouster/ruby-on-rails-postgresql-f1b037924bdf "Rails & PostgreSQL with Useful Commands")
- [PostgreSQL 11 Documentation](https://www.postgresql.org/docs/11/index.html "PostgreSQL 11 Documentation")
- [PostgreSQL 11 Client Applications](https://www.postgresql.org/docs/11/reference-client.html "PostgreSQL 11 Client Applications")
- [Dockerize PostgreSQL](https://docs.docker.com/engine/examples/postgresql_service/ "Dockerize PostgreSQL")
- [Secure TCP/IP Connections with SSH Tunnels](https://www.postgresql.org/docs/11/ssh-tunnels.html "Secure TCP/IP Connections with SSH Tunnels")
- [PostgreSQL Security](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps "PostgreSQL Security (dated, but useful)")
