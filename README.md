# Deako HASS integration

Component to integrate with Deako using Deako's pydeako library

Deako was so close to getting this pushed as an official integration: https://github.com/home-assistant/core/pull/102190

This repo just takes those changes, applies a minor tweak, and now can be used as an "official" integration

**This component will set up the following platforms.**

Platform | Description
-- | --
`Lights` | Control your lights

## Installation

1. Using the tool of choice open the directory (folder) for your HA configuration (where you find `configuration.yaml`).
2. If you do not have a `custom_components` directory (folder) there, you need to create it.
3. In the `custom_components` directory (folder) create a new folder called `deako`.
4. Download _all_ the files from the `custom_components/deako/` directory (folder) in this repository.
5. Place the files you downloaded in the new directory (folder) you created.
6. Restart Home Assistant
7. In the HA UI go to "Settings" -> "Devices & services". If all went well, you should get a notification that new devices are available.
8. If not try adding the integration manually. Click "+ Add Integration" and search for "Deako"

## Configuration is done in the UI

Find an IP address of a deako device that is on your wifi, and input that into the configuration popup
