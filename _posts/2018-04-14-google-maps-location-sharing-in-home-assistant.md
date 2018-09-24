---
title: "Configuration Log: Setting up Google Maps Location Sharing in Home Assistant"
author: Jon Griffith
---

One of the latest updates with the most recent iteration of Home Assistant is the Google Maps Location Sharing component.  I'm a Google user and I use G-Suite, so I'm going to give the setup-instructions a go to see if I'm able to make it work.

#### 4:05PM

Instructions per [this page](https://www.home-assistant.io/components/device_tracker.google_maps/):  You first need to create an additional google account... ???  Not sure if this make sense to me.  Any time instructions tell me to do something this simple, I question why it needs to be done, and 9 times out of 10, there's no explanation as to the reason why.

So, I'm going to skip this with a few reservations about the potential problems I may have.

The component configuration example looks like this:

    # Example configuration.yaml entry
    device_tracker:
      platform: google_maps
      username: example@gmail.com
      password: password

My device trackers are included using `!include_dir_merge_list` so the configuration is going to look a bit different.

## My Configuration Entry

    # Example configuration.yaml entry
    device_tracker: !include_dir_merge_list config/device_trackers/

A file named 'google_maps.yaml' which sits in the /config/device_trackers/ folder will have this in it:

    - platform: google_maps
      username: !secret google_maps_username
      password: !secret google_maps_password

The `!secret` parameter pulls data from a hidden file that holds credentials for system-wide functions.

The "-" before platform is needed because the device trackers that I have defined in my system are in various files and the include method I used was the `!include_dir_merge_list` method, which expects there to be multiple entries to the device trackers.  Platform is just another item in the list of defined trackers.

Before I even get started, I'm anticipating troubles due to 2-factor authentication, but we'll see.

## 4:25PM

- Checked Config: Valid
- Reboot

## 6:02PM

- Noticed that upon reboot there was a confirmation email sent from my google mail account confirming whether or not it was me that was attempting to login.  Since I missed it in the required time, I was unable to confirm.

So, I checked the error log and saw this:

~~~~ ruby
Sat Apr 14 2018 18:01:28 GMT-0700 (MST)
Caught exception  
Traceback (most recent call last):  
  File "/usr/lib/python3.6/site-packages/locationsharinglib/locationsharinglib.py", line 284, in _get_data
    people = [Person(info) for info in output[0]]
TypeError: 'NoneType' object is not iterable
~~~~

- Rebooted for a 2nd time
- No success.  Rebooted a 3rd time.

Ran into the following error:

```File "/usr/lib/python3.6/site-packages/locationsharinglib/locationsharinglib.py", line 209, in _validate_cookie
    raise InvalidCookies(message)
locationsharinglib.locationsharinglibexceptions.InvalidCookies: The cookies provided do not provide a valid session.Please authenticate normally and save a valid session again
```

- Discovered cookie file in root of Home Assistant that was created.  Deleted this file and rebooted to see if I could get the login authentication to prompt me again.
- Upon restart, Google prompted me on my mobile device to verify that it was me signing in.  I accepted the connection.
- Still receiving errors.
- Reboot one more time.
-
