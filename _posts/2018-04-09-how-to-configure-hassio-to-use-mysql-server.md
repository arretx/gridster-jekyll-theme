---
author: Jon Griffith
title: How to Configure HASS.IO to use MySQL Server
tags: [hassio,mysql]
category: [Database]
---

By default, Home Assistant stores all of its data using SQLite in your configuration directory.  [See This Article](https://www.home-assistant.io/docs/backend/database/)

Since I operate my instance of HA on a Raspberry Pi3 which has limited storage, I decided to re-direct the data to a MySQL server which I happen to be operating in my house.

Before you continue, the following conditions existed in my network infrastructure, so if you don't have a similar setup, or are unable to adapt to your setup, I just wanted to let you know now so you don't waste your time.

- My instance of HA is HASS.IO running on a Raspberry Pi3 with WIFI enabled.
- All of my network devices are assigned at static IP.
- I already have a completely separate server running Ubuntu 16.04 with **Webmin** and **Virtualmin**, **Apache** and **MySQL**.
- I have ZERO Windows based computer.  My laptop is OSX, my Server is Ubuntu 16.04, and my Pi3 is HASS.IO.
- I have limited knowledge of Linux, and I was able to do this.

## How I Did It
**Note: I have no idea how to preserve the data you've already generated in your current HA setup.  I was willing to ditch the history to make the switch.  This process does not include instructions about migrating data.**

### Create Empty Database

Knowing that MySQL was operable on my server, I went ahead and added an empty database to MySQL through Virtualmin:

1. In Virtualmin, choose the virtual server from your drop-down that you want to add the database to.
2. Click 'Edit Databases'
3. Click 'Create a new database'
4. Give it a name like, **hassio** or **homeassistant** or **bananahammock** and click Create.

Home Assistant will automatically create its tables when you eventually get it connected.  Don't worry about that.

### Create a User for MySQL

Or, if you already know the credentials for the table you just created, use those.  Home Assistant will need a user-name and password for the user that has access to the database you just created.  I knew mine because I used the Script Installer in Virtualmin to install phpMyAdmin which automatically generated a new user and password for my virtual server account.

Don't forget, I am the root admin for my server, so I have total access to my system.  I'm not 100% certain how this works if you don't have full access.

### Allow Remote Access by Home Assistant

1. In Virtualmin, choose the virtual server from your drop-down that you want to work on.
2. Click Edit Database
3. Choose the Remote Hosts tab at the top.
4. Under "Allowed hosts for MySQL" add the IP address of your Home Assistant host machine (Raspberry Pi3 in my case) or wildcard IP (192.168.1.%) for multiple hosts.
5. Save your settings.
6. Restart MySQL (Webmin -> Servers -> MySQL Database Server -> MySQL Server Configuration -> Save and Restart button)

### Forward traffic on your router to port 3306

You'll need to do some research for this one.  I don't know what router you have.

### Test Connection

On the laptop, I discovered that telnet was not installed, so I had to install it.

    brew install telnet

This of course requires that you have Homebrew installed.  You can Google how to do that.

Test the connection:

    telnet 192.168.x.x 3306

You should get a positive response.  If you don't, have a piece of pizza and start Googling cause I don't know what else to do.

### Success

The only thing left to do now is to configure Home Assistant to use your instance of MySQL.

Open up your configuration.yaml file inside of your Home Assistant folder (if you don't know how to edit your yaml files...uhhh, I can't help you...you shouldn't be here yet.)

Add the following to your configuration:

~~~
recorder:
  db_url: mysql://username:password@ip_address/database_name?charset=utf8
~~~

There are other options for 'recorder:' but this is the basic setup.

Save your changes.

## Reboot Home Assistant

When it comes back online, assuming you have no errors, which I didn't, so I don't know where they would be anyway, you will be able to look at your MySQL database and magically, there will be new tables and data will have been written to the tables.

That's it.  Enjoy.
