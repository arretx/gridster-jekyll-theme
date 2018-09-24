---
title: "Configuration Log: My First Time Configuring the Z-Wave Z-Stick GEN 5"
author: Jon Griffith
---

#### Saturday April 14th

So I received the GEN5 Z-Stick from Amazon today and as excited as I am to have this thing, I'm confused by the Z-Wave instructions on the Home Assistant site.

- Plugged the USB stick into one of the USB ports on the Raspberry Pi3 and it immediately began flashing multi-colors.
- Added the following to configuration.yaml  

~~~~
zwave:
  usb_path: /dev/ttyACM0
~~~~

- Checked config: Passed.
- Rebooted

Upon reboot, under the Configuration link in the sidebar, a new option appeared labeled "Z-Wave, manage your Z-Wave network."

#### 8:30PM

- Discovered Z-Wave dimmer shipped from Amazon is actually a defective, previously used unit.  Someone pulled a switcheroo.

#### Tuesday April 17th, 11:22PM

I successfully added two devices to the Z-Wave network.  Node 1 is the Aeotec ZStick Gen5.  The other two nodes are two GE dimmers that arrived on Sunday, which I installed.

Configuring the setup was rather simple.  The only initial configuration step that I took was adding the zwave component to the configuration.yaml.

### How to add...

In the config for Zwave, if you've setup your secure key then you can create a secure node.  Call up the service for creating a secure node from this page, then make your switch do something.  Turn it on, dim it up, etc.  Within moments, it should be automatically added to the `entity_registry.yaml` file which you'll now see in the root of your HA folder.  You'll also see a few other files, one being the zwcfg_XXXXX.xml file and the other the zwscene.xml file.  There will also be a hidden file called OZW_Log.txt and in my setup I also have a file called pyozw.sqlite.  Not sure what this one is yet.
