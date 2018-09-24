---
title: How to Install and Configure Shinobi Pro HASS.IO Add-On
author: Jon Griffith
---

## Shinobi Pro

The instructions made available at the [Shinobi Pro Repo](https://github.com/hassio-addons/addon-shinobi) sent me on a wild ride of confusion at first, but I finally figured it out...

## Installation

### 1. Add the Hass.io add-ons repository to your instance of Hass.io.

- Login to Home Assistant and click the Hass.io link in the side-bar.
- Click the ADD-ON STORE link on the top.
- If you haven't already added the Community Hass.io Add-ons, do that by pasting this ```https://github.com/hassio-addons/repository``` into the "Add new repository by URL" field then clicking ADD.
- You should now be able to see Shinobi Pro as one of the available add-ons.

### 2. Install the Add-On

- From from the ADD-ON STORE page in Hass.io, under Community Hass.io Add-ons, click Shinobi Pro.
- Click Install.  Wait.
- Once the install is complete, change the log_level parameter in the Config section to "trace" so you get all of the log feedback you need while you're working through installation.

### 3. Start The Add-On

- Start the Service.  If it doesn't start, make sure you're using a free port by setting and saving a new port number in the Network setting on the Shinobi add-on config page.

NOTE: The default port used for the user interface in Shinobi Pro is 5000.  If you have any other add-ons that are using port 5000, the install will not start.  You can test this by starting the service.  If it doesn't start, check the System Log under the System tab on the Hass.io page of your Home Assistant instance.  You'll see an error related to port 5000.

### 4. Create your Account Login

NOTE: If you intend to use an alternate database engine, such as MySQL, configure your MySQL settings now otherwise what you do next will be abandoned when you switch to MySQL.  The data does not migrate.

- Point a new tab in your browser to your instance of Hass.io `http://192.168.1.XXX:5000/super` or whatever the address is for your system, making sure you're using the right port.

NOTE: Don't use the OPEN WEB UI link for this step.  Make sure you add 'super' to the end of your URL.  I glossed over this step.  Bad idea.

- Using the login information that was added by default in the Config section of the Add-On, login to the Superuser login.  It should be user: `admin@shinobi.video` pwd: `admin`.

- In the Accounts section, click the little green plus sign.
- Enter your e-mail address.
- I left Group Key alone, letting it remain as the e-mail address.
- Create a password and repeat it.
- Click Save.
- Logout (upper right corner)

### 5. Login to Shinobi with your new Account

- Browse to your instance of Shinobi `http://xxx.xxx.xxx.xxx:5000`.
- Login with the new account credentials.

### 6. Customize your config.

I use a separate MySQL server for data, so my config needs to reflect that.  I created a separate database in MySQL and imported that database into one of my virtual server accounts in Virtualmin (I'm using Webmin with Virtualmin on Ubuntu 16.04.)

Shinobi Pro will create tables the first time it connects to MySQL, so all you need is an empty database defined with credentials.

#### Custom Config - If you use MySQL

WARNING: If you modify the database settings in the config AFTER setting up your user information, you will need to repeat that process because the data does not migrate.

If you don't use MySQL, leave the mysql parameters alone.

    {
      "log_level": "info",
      "super_username": "admin@shinobi.video",
      "super_password": "<MAKEUPAPASSWORD>",
      "mysql": true,
      "mysql_host": "192.168.1.xxx", <-- Your MySQL Server Location
      "mysql_username": "USERNAME", <-- Your MySQL Database User Name
      "mysql_password": "PASSWORD", <-- Your MySQL Database Password
      "mysql_database": "DATABASE_NAME", <-- Your MySQL Databse Name
      "mysql_port": MYSQL_PORT,
      "mail_service": "gmail",
      "mail_username": "your_email@gmail.com",
      "mail_password": "your_password",
      "mail_host": "smtp.example.com",
      "mail_port": 587,
      "mail_secure": false,
      "mail_cert_verify": true,
      "ssl": false,
      "certfile": "fullchain.pem",
      "keyfile": "privkey.pem"
    }

I haven't set the mail setting nor the ssl settings yet and won't until I know more about what I'm doing.

- After making your changes, click the Save link, then Restart the service.

### 7. Verify MySQL is working with Shinobi

Your MySQL database should have been completely empty and void of tables prior to starting, but now, if you access your MySQL database (using whatever tool you use) you should see tables added, and data in those tables.

### 8. Login to your Shinobi Dashboard

- Point your browser to `http://yourhassioip:yourportnumber` and login using your new user credentials.

## That's it for now.

I'm currently working through the next steps as I've never worked with CCTV or recording before, and to top it off, it's in this environment, which is still a bit foreign to me.

You can head over to [Shinobi's website](https://shinobi.video/) for documentation and resources.
