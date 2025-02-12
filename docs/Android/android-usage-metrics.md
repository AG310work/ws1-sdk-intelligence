---
layout: page
title: App Usage Metrics
hide:
  #- navigation
  - toc
---

# What are App Usage Metrics?

App Usage Metrics are an entity that can be tracked through our DEX Telemetry Feature.
- **Entity Name:** "app_usage_metrics"

The App Usage Metrics entity captures daily usage and performance metrics for applications running on the device. Each day, the top 30 applications are retrieved based on foreground usage time. Historical data is fetched for the prior days (does not include the current day) up to the last fetch time. The data is only fetched for a maximum of 7 days in the case the host app has not been opened for more than 7 days.

# Attributes
App Usage Attributes:
- **app_name** - Name of the app.
- **app_version** - Version of the app.
- **app_usage_date_time** - Starting timestamp at which the app usage was computed of the app.
- **total_time_in_foreground** - Total time the app spent in the foreground.
- **count_of_foreground_events** - Number of times the app was foregrounded.
- **average_time_in_foreground** - Average foreground session duration of the app.
- **longest_app_session** - Longest session duration the app was in the foreground.
- **shortest_app_session** - Shortest session duration the app was in the foreground.
- **battery_drain** - Average battery drain per hour of the app.
- **data_usage** - Total network data (transmitted and received) usage of the app.

# How to enable App Usage Metrics

## DEX Telemetry Opt In
Since this is a DEX Telemetry Feaute entity, the DEX Telemetry Feature must be enabled. Please see section [DEX Telemetry Opt-in](crittercism.md#dex-telemetry-opt-in).

## Privacy Configuration Flag
If configuring Telemetry Feature Data through the [Privacy Config](privacy-config.md) API, the following Privacy flag for App Usage Metrics has been added. 
The flag structure will follow the format below - 
``` JSON
"AppUsageData": {
  "DisableAll": false
}
```

**Note:** Flag "AppUsageData" only responds to "DisableAll" key value pair and does not honor "AttributesToDisable", "EventsToDisable" KVP’s. Either the entire entity should be enabled or disabled. 
Disabling / enabling individual attributes within AppUsageData is not possible. 

## Permissions Required
Android Permissions needed to fully enable App Usage Metrics are the following:
- [**PACKAGE_USAGE_STATS**](https://developer.android.com/reference/android/Manifest.permission#PACKAGE_USAGE_STATS)
- [**QUERY_ALL_PACKAGES** ](https://developer.android.com/reference/android/Manifest.permission#QUERY_ALL_PACKAGES) (API Level 30+)

## Battery Drain Information
### Battery Performance Optimization
In order to stay battery conscious of the applications implementing the SDK, we have opted to for periodic Battery recordings facilitated through [Android WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager) APIs.

This allows us to record when the device isn't in the foreground, while allowing the system to determine if the device is in an appropriate state battery-wise to allow recording.

Battery modes such as Battery-Saver, Doze Mode, and low device battery could affect battery level recordings leading to slighlty inaccurate Battery Drain levels under the App Usage Metrics Entity.

To mitigate these inaccuracies, App Developers can considered revoking Battery Restrictions for their applications if deemed necessary.

### WorkManager Intialization
Applications that are customizing [WorkManager Intialization](https://developer.android.com/develop/background-work/background-tasks/persistent/configuration/custom-configuration) should understand that our SDK also utilizes WorkManager to schedule App Usage Metrics Battery Recordings.

The SDK will initialize WorkManager with a [Default Configuration](https://developer.android.com/reference/androidx/work/Configuration) if the application has disabled automatic initialization and has not initialized it yet when the SDK triggers it's first Battery Recording either after DEX Feature enablement or Device Reboot.
With this in mind, if the application intends to initialize WorkManager with a specific configuration, it is recommended to do so before enabling DEX Telemetry.
