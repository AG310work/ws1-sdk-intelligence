---
layout: page
title: Telemetry Privacy Configuration for iOS
hide:
  #- navigation
  - toc
---


The Privacy Configuration, or Config for short, is a JSON payload which can be used to control the reporting of Telemetry Feature Data.
This payload can either be generated manually or pulled from the WS1 SDK's Custom Settings if integrated in the application.

### How to Use

To inject this configuration, use the API `setPrivacyConfiguration`. 

A JSON Configuration block will need to be transformed into an NSDictionary before submitting it.

For more on this API, visit: [setPrivacyConfiguration](ws1intelligence.md/#setprivacyconfigurationprivacyconfig-telemetryfeature)

```objective-c
/// Function to help set privacy configuration for a particular telemetry type.
/// -> This configuration is ideally part of WS1 SDK's custom settings profile payload. However, IntelligenceSDK is agnostic to the configuration delivery mechanism as long
///   as the payload conforms to the required format.
/// -> A sample privacy config is like below -
/// "DEXData": {
///        "Version": 1.0,
///        "BatteryData": {
///            "DisableAll": true,
///            "AttributesToDisable": ["plugged_type", "battery_charging_rate"],
///            "EventsToDisable": []
///        },
///        "DeviceData": {
///            "DisableAll": true,
///            "AttributesToDisable": ["location_latitude", "location_longitude"],
///            "EventsToDisable": []
///        },
///        "NetworkData": {
///            "DisableAll": true,
///            "AttributesToDisable": ["jitter", "latency"],
///            "EventsToDisable": []
///        }
///    }
///
///
/// NOTE -
/// * This config is only supported for the DEX telemetry type currently. The privacy config could help  disable certain attributes / events from being reported.
///
/// - Parameters:
///   - privacyConfig: - A dictionary containing the necessary key value pairs for parsing the privacy config. If the privacy config is
///                     fetched via WS1 UEM SDK, then it is in the JSON format. Apps / SDK's are expected to fetch the value for key "DEXData"
///                     and convert it into a dictionary.
///                   - If the property "DEXData" and its value does not exist, apps are still required to call this API with nil value
///                     for the privacyConfig parameter.
///   - type: Feature for which the privacy configuration needs to be applied. Currently only DEX is supported. Other telemetry type's are ignored.
+ (void)setPrivacyConfiguration:(nullable NSDictionary<NSString *, id> *)privacyConfig forType:(WS1TelemetryType)type;
```

### Privacy Configuration Attributes
The following is a table of the Attributes allowed within the Privacy Configuration JSON format.

#### Configuration JSON Attributes Table
|       **Flag**      | **Description**                                                                     |           **Value**          |                                              **Outcome**                                              |                                                                        **Notes**                                                                       |
|:-------------------:|-------------------------------------------------------------------------------------|:----------------------------:|:-----------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------:|
|       DEXData       |                    JSON container for all DEX granular controls.                    |                              |                                                                                                       |                                     This value of this key (JSON object) is parsed only if CaptureDEXData is true.                                     |
|                     |                                                                                     |            MISSING           | All attributes within the DEX umbrella are reported.                                                  |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | Attributes are selectively reported as per the configured flags.                                      |                                                                                                                                                        |
|       Version       |  Helps version the JSON payload. Could be useful when introducing breaking changes. |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | The payload is considered un-usable.                                                                  |                                                                                                                                                        |
|                     |                                                                                     | AVAILABLE (But not a number) | The payload is considered un-usable.                                                                  |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | Payload is parsed accordingly                                                                         |                                                                                                                                                        |
|     BatteryData     |              JSON container for all `battery` entity granular controls.             |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All `battery` attributes are computed and reported.                                                   |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | `battery` attributes are selectively computed / reported as per the configured flags.                 |                                                                                                                                                        |
|      DeviceData     |              JSON container for all `device` entity granular controls.              |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All `device` attributes are computed and reported.                                                    |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | `device` attributes are selectively computed / reported as per the configured flags.                  |                                                                                                                                                        |
|     NetworkData     |              JSON container for all `network` entity granular controls.             |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All `network` attributes are computed and reported.                                                   |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | `network` attributes are selectively computed / reported as per the configured flags.                 |                                                                                                                                                        |
|      DisableAll     | Used for enabling / disabling `battery`, `network` or `device` entities completely. |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
|                     |                                                                                     |             TRUE             | All attributes are **not** computed and reported for the corresponding entity.                        |                                                                                                                                                        |
|                     |                                                                                     |             FALSE            | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
| AttributesToDisable |               Used for disabling specific attributes within an entity               |                              |                                                                                                       | - Non-Optional mandatory attributes cannot be disabled. They will be ignored if present in the array. - Optional mandatory attributes can be disabled. |
|                     |                                                                                     |            MISSING           | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
|                     |                                                                                     |      AVAILABLE AND EMPTY     | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
|                     |                                                                                     |    AVAILABLE AND NOT EMPTY   | Attributes in the array are disabled from being computed or transmitted for the corresponding entity. |                                                                                                                                                        |
|   EventsToDisable   |                 Used for disabling specific events within an entity                 |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All events are computed and reported for the corresponding entity.                                    |                                                                                                                                                        |
|                     |                                                                                     |      AVAILABLE AND EMPTY     | All events are computed and reported for the corresponding entity.                                    |                                                                                                                                                        |
|                     |                                                                                     |    AVAILABLE AND NOT EMPTY   | Events in the array are disabled from being computed or transmitted for the corresponding entity.     |                                                                                                                                                        |


### Examples

#### All enabled
To have all Telemetry Feature data enabled, no configuration payload needs to be injected through the SDK. All that is needed is to enable the Telemetry Feature itself.

However, if a payload needs to be specified it can be structured in the following way:
```JSON
{
  "DEXData": {
    "Version": 1.0
  }
}
```

#### Disable Data Categories
To disable an entire category of data event, set the "DisableAll" attribute to true  in the JSON payload for the specific Data category to disable.

Settings this attribute to false will continue to enable the data category.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "DisableAll": false
    },
    "DeviceData": {
      "DisableAll": true
    },
    "NetworkData": {
      "DisableAll": true
    }
  }
}
```
In the above example, we have disabled both `device` and `network` data categories entirely.

**Note:** Setting "DisableAll" to true will override any disabling of Attributes or Events for that category, as the entire category will be disabled.

#### Disable Specific Attributes
To disable specific attributes from being reported, please list them by Attribute name in their Specified Data Category under the JSON array element: "AttributesToDisable"

An empty list will not disable any attributes for the data category.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "AttributesToDisable": ["plugged_type"]
    },
    "DeviceData": {
      "AttributesToDisable": ["location_latitude", "location_longitude"]
    },
    "NetworkData": {
      "AttributesToDisable": []
    }
  } 
}
```

#### Disable Specific Events
To disable specific events from being reported, please list them by Event name in their Specified Data Category under the JSON array element: "EventsToDisable"

An empty list will not disable any events for the data category.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "EventsToDisable": ["charging_state_change"]
    },
    "DeviceData": {
      "EventsToDisable": []
    },
    "NetworkData": {
      "EventsToDisable": ["network_change"]
    }
  } 
}
```

#### Disable Specific Attributes and Events
This is an example combination of both Disabling Attributes and Disabling Events which can be read about in more detail above.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "AttributesToDisable": ["plugged_type"],
      "EventsToDisable": ["charging_state_change"]
    },
    "DeviceData": {
      "AttributesToDisable": ["location_latitude", "location_longitude"],
      "EventsToDisable": []
    },
    "NetworkData": {
      "AttributesToDisable": [],
      "EventsToDisable": ["network_change"]
    }
  } 
}
```


### All Attributes and Events Reported By IntelligenceSDK
To Find all Attributes and Events reported by the Intelligence SDK, please refer to the following links based on platform and data type:

#### Android
- [Battery](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#battery_-_android)
- [Device](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#device_-_android)
- [Network](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#network_-_android)

#### iOS
- [Battery](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#battery_-_ios)
- [Device](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#device_-_ios)
- [Network](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#network_-_ios)


