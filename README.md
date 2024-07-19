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

## Reload Deako Automation to keep it consistent

Setup this automation. Obviously change the light.<name> to the actual device name for each trigger. It checking to see if the state goes to `unavailable` or `unknown` for 2 minutes.
The other trigger is every hour and 40 seconds (40 seconds is to avoid conflicts with time based automations). Deako will consistently lose connection and the integration becomes useless.

```
alias: Reload Deako on failure
description: ""
trigger:
  - platform: state
    entity_id:
      - light.<name>
      - light.<name>
    to: unavailable
    for:
      hours: 0
      minutes: 2
      seconds: 0
    id: unavailable
  - platform: state
    entity_id:
      - light.<name>
      - light.<name>
    to: unknown
    for:
      hours: 0
      minutes: 2
      seconds: 0
    id: unknown
  - platform: time_pattern
    seconds: "40"
    hours: /1
condition: []
action:
  - service: rest_command.reload_deako
    alias: Reload Deako
    data: {}
    response_variable: deako_response
  - if:
      - condition: template
        value_template: "{{ deako_response['status'] != 200 }}"
    then:
      - service: notify.notify
        data:
          title: Could not reload Deako
          message: "HTTP code: {{ deako_response }}"
mode: single
```

## Reload Deako integration REST configuration

### To get your entry ID of the integration:

1. Navigate to the Deako integration. Open Developer Tools (F12 or right click 'Inspect')
2. Click the 3 dots and select `Reload`
3. Developer screen will show a request to the API endpoint you need

### To get a long-lived access token

1. Navigate to your user settings (that has permissions to reload integrations)
2. Select `Security`
3. Scroll down to `Long-lived access tokens` and click `CREATE TOKEN`

```
rest_command:
  reload_deako:
    url: http://homeassistant:8123/api/config/config_entries/entry/<entry ID>/reload
    method: POST
    headers:
      authorization: 'Bearer <token>'
      content-type: 'application/json'
```
