## 1.1.0 - 2022-09-15

### What's changed?

- Docker users can now use a `json` file to configure the add-on as well.

### 🐛 Bug fixes

- Start-up error when using more than one PSN account @FunkeyFlo (#68)
- docker env "PSN_ACCOUNTS" mandatory @FunkeyFlo (#69)

### ⬆️ Dependency updates

- Bump @reduxjs/toolkit from 1.8.3 to 1.8.5 in /ps5-mqtt @dependabot (#48)
- Bump redux-saga from 1.1.3 to 1.2.1 in /ps5-mqtt @dependabot (#50)
- Bump grommet-icons from 4.7.0 to 4.8.0 in /ps5-mqtt @dependabot (#60)
- Bump async-mqtt from 2.6.2 to 2.6.3 in /ps5-mqtt @dependabot (#65)
- Bump grommet from 2.25.1 to 2.25.3 in /ps5-mqtt @dependabot (#66)
- and much more...

## 1.0.1 - 2022-08-19

### What’s changed

#### 🐛 Bug fixes

- Fix PS4 title activity not being matched to PS5 devices @FunkeyFlo (#29)

## 1.0.0 - 2022-08-11

### What’s changed

### 🚨 New functionality

Matching PSN account activity to consoles is now supported.

The add-on will create a new `sensor.my_playstation_activity` sensor that tells you which app/game is being used on the console! Both entities, `activity` and `power`, will now include the following attributes:
| Attribute | Description |
| -- | -- |
| `Players`  | A list of players active on the device |
| `Activity` / sensor.state | `idle`, `playing` or `none`; indicating the kind of activity on the device |
| `Title Name` | A user-friendly name of the Playstation App / Game currently in use. |
| `Title Image` | A `url` referring to the official box-art. |
| `Title ID` | unique code for the Playstation App / Game currently in use. |

Take a look at the [documentation](https://github.com/FunkeyFlo/ps5-mqtt/blob/main/add-ons/ps5-mqtt-edge/DOCS.md#psn_accounts-optional-multiple) to see how to configure the add on for tracking PSN accounts and how to acquire the required information.

#### Examples

![image](https://user-images.githubusercontent.com/4623715/184224674-97c167f6-44bc-463a-a573-3a47b5eaefc8.png)
![image](https://user-images.githubusercontent.com/4623715/184225211-9be41ffc-7a19-4ab1-9242-7eac7053285d.png)

⚠ The MQTT implementation saw some changes that might cause you to have duplicate `power` entities in HA after upgrading. ⚠

- The advised upgrade path is to remove your old devices from Home Assistant and let the add-on rediscover them for you.
- Alternatively remove the old `power` entity and rename the *new* `power` entity (which will probably be called something like `switch.my_ps5_power_2`) to your old entity's name.

### 🚀 Enhancements

- Psn-presence @FunkeyFlo (#26)

### 📚 Documentation

- Update HA Core Docs @matt8707 (#24)
- Add HA Core Docs @FunkeyFlo (#23)
- Psn-presence @FunkeyFlo (#26)

## 0.7.1 - 2022-08-04

### What’s changed

- 💪 #22: PS4 devices were *unintentionally* being discovered as [@bryeartem](https://github.com/bryeartem) correctly pointed out. He also mentioned that, in his case, the add-on detects state changes in the PS4 quicker than the HA integration. So instead of 'fixing' the issue I decided to add an extra option (`include_ps4_devices`) to the config. Turning the option on will discover PS4 devices as well as PS5's. The option is disabled by default.

## 0.7.0 - 2022-08-03

### What’s changed

- 🔧 #21: fixed failing device state checks when PS5 name contained **non**-alphanumeric or '-' and '_' characters.
- 🔐 improved add-on security (rating) by adding ingress support.
- 💔 removed `ps5_credentials` option from add-on configuration.

## 0.6.4 - 2022-07-30

### What’s changed

- client improvements @FunkeyFlo (#19)
- - added dark/light theme option to client UI 🌗
- 
- 
- 
- 
- 
- - (hopefully) cleared up some of the confusion people were having with the authentication steps by changing the walkthrough text that's displayed in the Authorization dialog.
- 
- 
- 
- 
- 
- 

## 0.6.3

- Switched to images instead of locally built Dockerfiles [#3](https://github.com/FunkeyFlo/ps5-mqtt/issues/3).

### **⚠ IMPORTANT FOR USERS UPGRADING FROM `v0.6.2` ⚠**

Because the add-on switched from locally built Dockerfiles to pre-built images you *have* to **uninstall** and **re-install** the add-on to upgrade! ⚠ Be sure to backup your configuration first because Home Assistant *will not* save it for you!

## 0.6.2

- Added a Web UI to acquire credentials through the Add-on itself.
- Credentials file will now be written to the file `/config/ps5-mqtt/credentials.json`.
- ⚠ **Deprecation Warning**
- - The `ps5_credentials` option is deprecated and will be removed with the next *[minor](https://semver.org/)* release. Check the [docs](/ps5-mqtt/DOCS.md) for migration options.
- 
- 
- 
- 
- 
- 

## 0.5.2

- Fixed issue where MQTT traffic could cause a loop, starving the MQTT broker.

## 0.5.0

- Changed MQTT implementation to...
- - ...reduce amount of messages being sent.
- 
- 
- 
- 
- 
- - ...avoid entities being unavailable after home assistant restart. ([#5](https://github.com/FunkeyFlo/ps5-mqtt/issues/5))
- 
- 
- 
- 
- 
- 
- Re-implemented device discovery
- - Added `device_discovery_interval` option to change the frequency of discovering PS5 devices.
- 
- 
- 
- 
- 
- - Optimizations for discovered devices.
- 
- 
- 
- 
- 
- 
- Various minor tweaks and improvements.

## 0.4.4

- Changed config to ensure mqtt is ready before service starts.

## 0.4.3

- Changed device discovery to only run once during startup to reduce CPU usage. To discover new devices you will have to restart the add-on.
- Added `device_check_interval` option to change the frequency of checking the PS5's status.
- Reduced the frequency of mqtt updates by only sending an update when device state actually changed.
- Added a delay between device registration in Home Assistant and sending the first availability update to solve [issue](https://github.com/FunkeyFlo/ps5-mqtt/issues/1) where after discovery the power entity would remain unavailable.
