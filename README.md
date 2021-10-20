# smarthome-security.telus.com plugin for Homebridge using Node.js

smarthome-security.telus.com plugin for [Homebridge](https://github.com/nfarina/homebridge) using the [node-alarm-dot-com](https://github.com/node-alarm-dot-com/node-alarm-dot-com) interface.

This is a drop-in replacement for the alarm dot com plugin which didn't seem to work for Telus Smarthome Security.
This is also a drop-in replacement for Bryan Bartow's homebridge-alarm.com and John Hurliman's FrontPoint plugin for Homebridge with more functionality and the removal of any external dependencies (e.g., :no_entry_sign: WrapAPI :tada: ).

# Supported Features

 * Querying panels
   * Arming
   * Disarming
 * Sensors (due to lag and capabilities in smarthome-security.telus.com's web API, these features are partially supported)
   * Contact sensor states
   * Water leak sensor states
   * Motion sensor states
 * Lights
   * On/Off switch
   * Dimmer switch
 * Locks
   * Lock/Unlock switch
 * Garage Doors
   * Open/Close switch

# Installation

# 1. Install homebridge: `npm install -g homebridge`
# 2. Install this plugin: `npm install -g prsng/homebridge-node-telussecurity-dot-com`
# 3. Update your configuration file (see below).

# Configuration

### Sample config.json:


```json
{
    "platform": "Telussecuritydotcom",
    "name": "Security System",
    "username": "<ENTER YOUR TELUS USERNAME>",
    "password": "<ENTER YOUR TELUS PASSWORD>",
    "armingModes": {
        "away": {
            "noEntryDelay": false,
            "silentArming": false
        },
        "night": {
            "noEntryDelay": false,
            "silentArming": false
        },
        "stay": {
            "noEntryDelay": false,
            "silentArming": false
        }
    },
    "ignoredDevices": [
        "96922426-1",
        "96922426-4"
    ]
}
```
### Fields:

* "platform": Must always be "Telussecuritydotcom" (required)
* "name": Can be anything (required)
* "username": Telus login username, same as app (required)
* "password": Telus login password, same as app (required)
* "armingModes": Object of objects with arming mode options of boolean choices (**WARNING:** the Smarthome Security webAPI does not support setting silent arming to true and this feature does not work at this time)
* "authTimeoutMinutes": Timeout to Re-Authenticate session (**WARNING:** choosing a time less than 10 minutes could possibly ban/disable your account from Telus Smarthome Security)
* "pollTimeoutSeconds": Device polling interval (**WARNING:** choosing a time less than 60 seconds could possibly ban/disable your account from Telus Smarthome Security)
* <details><summary>"logLevel": Adjust what gets reported in the logs <strong>(click to expand)</strong></summary><ul><li>0 = NO LOG ENTRIES</li><li>1 = ONLY ERRORS</li><li>2 = ONLY WARNINGS and ERRORS</li><li><strong>3 = GENERAL NOTICES, ERRORS and WARNINGS (default)</strong></li><li>4 = VERBOSE (everything including development output, this also generates a file `ADC-SystemStates.json` with the payload details from Telus Smarthome Security in the same folder as the Homebridge config.json file)</li></ul></details>
* "ignoredDevices": An array of IDs for Telus Smarthome Security accessories you wish to hide in Homekit

# Troubleshooting

Before assuming that something is wrong with the plugin, please review the [issues on this project's github repository](https://github.com/prsng/homebridge-node-telussecurity-dot-com/issues?utf8=%E2%9C%93&q=sort%3Aupdated-desc+) to see if there's already a similar issue reported where a solution has been proposed or the outcome is expected due to limitations with the Telus Smarthome security web API.

### Migrating from Bryan Bartow's homebridge-alarm.com or alarm-dot-com plugin

If you are replacing the Bryan Bartow's Homebridge plugin with this implementation, you may be required to delete the `~/.homebridge/accessories/cachedAccessories` file for the new platform to show up with the new panel, accessories and devices.

**WARNING:** If you delete the contents of the `~/.homebridge/persist` folder, your Homebridge and devices will become unresponsive and you will have to entirely re-pair the Homebridge bridge (remove and re-scan the QR-code for Homebridge and set up all of your accessories/devices again).

### Logging

The default setting for log entries is set to report critical errors, warnings about devices and notices about connecting to the Telus Smarthome security account. Once you feel that your security system devices are being represented in HomeKit correctly you can choose to reduce the amount of information being output to the logs to save space or remove cruft while troubleshooting other Homebridge plugins.

To modify the log behaviour, add the "logLevel" field to the Telussecuritydotcom platform block in the Homebridge configuration file. The following example illustrates that we only want critical errors to be reported in the log.
#### Sample config.json with "logLevel" setting:
```json
{
    "platform": "Telussecuritydotcom",
    "name": "Security System",
    "username": "<ENTER YOUR Telus USERNAME>",
    "password": "<ENTER YOUR Telus PASSWORD>",
    "armingModes": {
        "away": {
            "noEntryDelay": false,
            "silentArming": false
        },
        "night": {
            "noEntryDelay": false,
            "silentArming": false
        },
        "stay": {
            "noEntryDelay": false,
            "silentArming": false
        }
    },
    "logLevel": 1
}
```

### Ignoring Devices

Accessories that you wish to hide in Homekit (e.g., fobs) can be identified by finding the Serial Number in the settings of the accessory in the Apple Home app, or alternatively in your output log (log level 3 or higher) when Homebridge starts up. If the accessories still exist in Homekit, please make sure that you have typed the serial number exactly. If they still continue to be displayed (or vice-versa they still don't show up after un-ignoring them), then you may be required to delete the `~/.homebridge/accessories/cachedAccessories` file as they may still be stored in the cache within Homebridge.

# Credits

Forked from John Hurliman's FrontPoint* plugin for Homebridge<small>[↗](https://github.com/jhurliman/homebridge-frontpoint)</small> to replace the branding and code namespace from FrontPoint to Telus Smarthome Security.

<small>*FrontPoint is simply a rebranded service provider for Telus Smarthome Security, but FrontPoint is not needed for this plugin to work.</small>

