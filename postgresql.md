---
title: PostgreSQL Cheat
---

Open source.

Default port: 5432

## Get started

<http://stackoverflow.com/questions/1471571/how-to-configure-postgresql-for-the-first-time>

First you must have an OS user (not a database user) named `postgres`.

This OS user usually is created when PostgreSQL is installed on Ubuntu via `apt-get`.

The first login can only be done by the `postgres` user, so you must do:

    sudo -u postgres psql template1

Where `template1` is the name of a database which always exists on new installations. If not given, login may fail, since it tries to use a default database with the same name as the username `postgres`.

This only works if the authentication method for `postgres` is `peer`.

`peer` means that if the OS user named `XXX` is logged in, then he can automatically login on the database if `XXX` exists on the database, without having a database password.

The other major method of authentication is `md5`, which can work for users that don't exist in the OS and requires a password. This method is safer, but you will have to type more every time.

This can be set under the following configuration file:

    sudo vim /etc/postgresql/9.1/main/pg_hba.conf

Your login method is probably being controlled by the user `all` line in the default config file.

Don't forget to restart PG after you have edited the configuration file:

    sudo service postgresql restart

Using the `postgres` PostgreSQL user you can create new users as:

    sudo -u postgres createuser -deElPrs <my_username>

## Role

In PostgreSQL, an user is commonly called a *role*.

## Slash commands

### h

Get help:

    \h

### USE

### c

Connect to a database.

    \c database_name

### SHOW TABLES

### dt

List all tables on current database:

    \dt

Mnemonic: `Database Tables`.

### DESC

### d

Describe table:

    \d+

## Tools

### pgAdmin

pgAdmin is a popular GUI (not browser based) tool to view and edit Pg tables.

Ubuntu 12.04 install:

    suto apt-get install -y pgadmin3

Run:

    pgadmin3

To view data, select the table, right click then View Data

## Internals

Source code:

    git clone git://git.postgresql.org/git/postgresql.git

<http://www.postgresql.org/docs/9.3/static/git.html>

GitHub mirror: <https://github.com/postgres/postgres>

Coded in C.

## Dead rows

<http://stackoverflow.com/questions/2596670/how-do-you-find-the-row-count-for-all-your-tables-in-postgres>

<https://www.postgresql.org/docs/9.1/static/sql-vacuum.html>

## How many rows are there in each table

<http://stackoverflow.com/questions/2596670/how-do-you-find-the-row-count-for-all-your-tables-in-postgres>

Estimate:

    SELECT schemaname,relname,n_live_tup
        FROM pg_stat_user_tables
        ORDER BY n_live_tup DESC;
