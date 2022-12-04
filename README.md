# homebridge-google-nest-sdm-thermostats

A Homebridge plugin for Google Nest Thermostats that uses the (**PAID**) [Google Smart Device Management API](https://developers.google.com/nest/device-access).

It **ONLY** supports Thermostats.

It **does not** support temperature sensors, motion/occupancy sensors, or modifying the home/away state.

This was forked from [homebridge-google-nest-sdm](https://github.com/potmat/homebridge-google-nest-sdm) which supports all device types available via the API. I wanted to make minor changes to thermostat behavior for my use and don't wish to maintain the code for other device types. Use the original plugin if you have other device types you'd like to bridge or don't care for my changes.

If your devices are available via the Nest app (as opposed to the Google Home app); you should likely use [homebridge-nest](https://github.com/chrisjshull/homebridge-nest); it is full featured, free and significantly easier to setup.

**Please read the [FAQ](https://github.com/hn-git/homebridge-google-nest-sdm-thermostats#faq) before creating an issue.**

# Disclaimer

This package is not affiliated with, provided, endorsed, or supported by Google in any way. It is intended for personal, non-commercial use only.

Please review the [Google Smart Device Management Terms of Service](https://developers.google.com/nest/device-access/tos) to ensure that your usage of this package is not in violation.

# Installation

I recommend you use the homebridge web interface to install the plugin and configure the required and optional settings.

# Where do the config values come from?

Follow the getting started guide here: https://developers.google.com/nest/device-access/get-started  Please mind the section below.

**"ONE IMPORTANT DIFFERENCE"** 

In step two "Authorize an Account" in the "Link your account" section, step 1, you are instructed to "open the following link in a web browser":

https://nestservices.google.com/partnerconnections/project-id/auth?redirect_uri=https://www.google.com&access_type=offline&prompt=consent&client_id=oauth2-client-id&response_type=code&scope=https://www.googleapis.com/auth/sdm.service

** DO NOT USE THIS URL! **

You should instead use this URL:

https://nestservices.google.com/partnerconnections/project-id/auth?redirect_uri=https://www.google.com&access_type=offline&prompt=consent&client_id=oauth2-client-id&response_type=code&scope=https://www.googleapis.com/auth/sdm.service+https://www.googleapis.com/auth/pubsub

Note the "+https://www.googleapis.com/auth/pubsub" on the end.  This is so you will have access to events.

## Config Values

**clientId** and **clientSecret** come from this step: https://developers.google.com/nest/device-access/get-started#set_up_google_cloud_platform.  **clientId** should look something like "780816631155-gbvyo1o7r2pn95qc4ei9d61io4uh48hl.apps.googleusercontent.com".  
**clientSecret** will be a random string of letters, numbers, and dashes.

**projectId** comes from this step: https://developers.google.com/nest/device-access/get-started#create_a_device_access_project

**refreshToken** comes from this step: https://developers.google.com/nest/device-access/authorize#get_an_access_token

**subscriptionID** comes from this step: https://developers.google.com/nest/device-access/subscribe-to-events#create_a_pull_subscription. It should look like "projects/your-gcp-project-id/subscriptions/your-subscription-id".

**gcpProjectId** is optional. It is the ID of the Google Cloud Platform project you created when getting the **clientId** and **clientSecret**. If you are having trouble subsribing to events try populating this field.

**isEcoSwitchDisabled**  - (optional) by default a switch will be created within the thermostat accessory to manually toggle eco-mode; use this checkbox to disable the switch and instead use a custom characteristic that is not visible in the default home app.

# FAQ
**Q**: When the plugin starts I get some message about ```Plugin initialization failed, there was a failure with event subscription```.  Why?

**A**: As the error message tells you, make sure you mind the ["ONE IMPORTANT DIFFERENCE"](https://github.com/hn-git/homebridge-google-nest-sdm-thermostatswhere-do-the-config-values-come-from) when setting up your config values.  Try using the **gcpProjectId** config value if you continue to have problems.

**Q**: Do I really have to pay $5 to use the API?

**A**: Yup.

**Q**: Isn't there already a Nest plugin for Homebridge that does more stuff than this?

**A**: Yup.

**Q**: I just added a Nest device to my account, but it's not showing up in Home. Why?

**A**: You need to visit the ["ONE IMPORTANT DIFFERENCE"](https://github.com/hn-git/homebridge-google-nest-sdm-thermostats#where-do-the-config-values-come-from) URL again.  Here you will choose which Nest devices to authorize, you should see your new device here.  After you finish the process and get a new refresh token restart Homebridge, your device should now be visible.


