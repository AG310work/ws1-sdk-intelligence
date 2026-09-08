---

layout: page
title: Omnissa Intelligence SDK for Android Release Notes
hide:
  #- navigation

- toc

---

Updated on 08/26/2026

What's in the Release Notes

Omnissa Intelligence SDK for Android Release Notes describe the new features and enhancements in each release. This page contains a summary of the new capabilities, issues that have been resolved, and known issues that have been reported in each release. 

## Omnissa WS1 Intelligence SDK 26.8.0 for Android - August 26, 2026

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2410+
- Workspace ONE SDK Version 26.06+
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

#### New DEX Telemetry Attributes: System-On-Chip

New device attributes are available for Android 31 and later devices:

- **SoC Manufacturer**
  - Name: `soc_manufacturer`
  - Value: String representing the manufacturer of the device's primary system-on-chip.
  - Default Return Value: Null
- **SoC Model**
  - Name: `soc_model`
  - Value: String representing the model name of the device's primary system-on-chip.
  - Default Return Value: Null

#### Telemetry Data Listener APIs

Added telemetry data listener registration for DEX or Zero Trust data and Attributes, Events, or All event types. Callbacks arrive on each telemetry poll cycle. Up to 10 listeners are supported, and listeners can be unregistered.

```java
public static void registerTelemetryDataListener(
    @NonNull TelemetryFeature feature,
    @NonNull TelemetryEventType eventType,
    @NonNull TelemetryDataListener listener);

public static void unregisterTelemetryDataListener(
    @NonNull TelemetryDataListener listener);
```

```kotlin
fun interface TelemetryDataListener {
  fun onDataDispatched(data: String?)
}
```

```java
TelemetryDataListener listener = data -> {
  // Consume the telemetry JSON payload.
};

Crittercism.registerTelemetryDataListener(
    TelemetryFeature.DEX,
    TelemetryEventType.ALL,
    listener);
Crittercism.unregisterTelemetryDataListener(listener);
```

### Enhancements

- WebView instrumentation now uses weak references, removes stale references, and ignores duplicate instrumentation, reducing object-retention and duplicate-hook risks.
- Improved Android 36 compatibility by modernizing locale and app-specific storage handling and scoping legacy Bluetooth and external-storage permissions to the Android versions where they apply.
- Compile and Target SDK versions have been increased to 36 from 35.
- Kotlin version has been upgraded to 2.3.21.

### Bug Fixes

- Fixed CritterWebViewClient JavaScript injection on Android 9 by using `evaluateJavascript` instead of `loadUrl("javascript:...")`, preventing synthetic navigation callbacks, address-bar pollution, and repeated page-loading state.
- Resolved an issue where `Crittercism.setTelemetryOptInStatus` could become unresponsive under certain authentication conditions.

### Breaking Changes

Region lookup now uses `workspaceone.com` endpoints exclusively. The legacy `vmwservices` fallback and migration probe are removed. Ensure network policies allow the applicable Workspace ONE region endpoints before upgrading.

## Omnissa Intelligence SDK 26.5.1 for Android - July 6, 2026

- **If your app uses both SDKs, upgrading to this release also requires upgrading [Workspace ONE SDK](https://developer.omnissa.com/ws1-sdk-for-android/) to [version 26.06](https://developer.omnissa.com/ws1-sdk-for-android/release-notes/#workspace-one-sdk-2606-for-android) or later; for more information, see article [KB 6001611](https://kb.omnissa.com/s/article/6001611).**

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2402+
- Workspace ONE SDK Version 26.06+
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

#### Cryptography Library Updates

- Updated to consume **OpenSSL 3.5.7 libraries.**

## Omnissa Intelligence SDK 26.5.0 for Android - May 13, 2026

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 24.10 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

**New DEX Telemetry Attributes**

**All Entities - Location Information**

- **Altitude**
    - Name: `altitude_meters`
    - Value: Double representing the Mean Sea Level altitude in meters of the reported location.
    - Data Type: Double
    - Permissions: [ACCESS_FINE_LOCATION](https://developer.android.com/reference/android/Manifest.permission#ACCESS_FINE_LOCATION) or [ACCESS_COARSE_LOCATION](https://developer.android.com/reference/android/Manifest.permission#ACCESS_COARSE_LOCATION)

- **Altitude Accuracy**
    - Name: `altitude_accuracy_meters`
    - Value: Double representing the Mean Sea Level altitude accuracy in meters of the reported location altitude.
    - Data Type: Double
    - Permissions: [ACCESS_FINE_LOCATION](https://developer.android.com/reference/android/Manifest.permission#ACCESS_FINE_LOCATION) or [ACCESS_COARSE_LOCATION](https://developer.android.com/reference/android/Manifest.permission#ACCESS_COARSE_LOCATION)

- **Location Radius Accuracy**
    - Name: `location_accuracy_radius_meters`
    - Value: Double representing the horizontal accuracy radius in meters of the reported location.
    - Data Type: Double
    - Permissions: [ACCESS_FINE_LOCATION](https://developer.android.com/reference/android/Manifest.permission#ACCESS_FINE_LOCATION) or [ACCESS_COARSE_LOCATION](https://developer.android.com/reference/android/Manifest.permission#ACCESS_COARSE_LOCATION)

**Battery Entity**

- **Battery Cycle Count (Android 14 / API 34+)**
    - Name: `battery_cycle_count`
    - Value: Integer representing the total number of battery cycles the device has undergone.
    - Data Type: Integer
    - This Data point requires Android 34+. On devices below this, or if the device does not expose the battery cycle count, a default value is returned from the system.

**Network Entity**

- **Cellular Data Enabled**
    - Name: `cellular_data_enabled`
    - Value: Boolean flag representing if the cellular network is enabled.
    - Data Type: Boolean
    - Checks whether the device has an active cellular data connection on the primary SIM. Secondary SIMs are not considered when making this check. Additionally, the device cannot be in a data suspended state at the time of measuring this attribute. For example, in a 2G network, data activity may be suspended when a voice call arrives.

**Cryptography Library Updates**
  - Updated to consume OpenSSL 3.4.4 libraries.

### Resolved Issues

- **Network Latency Reporting**
    - Fixed an issue where instrumented URLConnection classes were reporting incorrect elapsed time / latency values for network requests.
    - Latency metrics are now reported accurately.

!!!Note
    After upgrading, users may observe an apparent increase in reported latency values. This is expected behavior due to corrected measurement rather than an actual performance regression.

- **WebView JavaScript Injection on Android 9**
    - Fixed an issue where CritterWebViewClient caused the JavaScript injection script to appear as the page URL in the address bar on Android 9 (API 28) and below after any page finished loading.
    - Pages no longer load on a loop after the fix.

## Omnissa Intelligence SDK 26.2.1 for Android - March 24, 2026

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2402 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### Patch Fixes

- Based on Release [**26.2.0**](#omnissa-intelligence-sdk-2620-for-android---march-2-2026).
- Resolved an issue where Intelligence Network Events were incorrectly denylisted when their URLs contained the term `workspaceone`. 
  
### Known Issues

- Instrumented URLConnection classes may report inaccurate network request elapsed times.
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be reported inaccurately.

## Omnissa Intelligence SDK 25.11.2 for Android - March 24, 2026

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2402 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### Patch Fixes

- Based on Release [**25.11.0**](#omnissa-intelligence-sdk-25110-for-android---december-3-2025).
- Resolved an issue where Intelligence Network Events were incorrectly denylisted when their URLs contained the term `workspaceone`. 
  
### Known Issues

- Instrumented URLConnection classes may report inaccurate network request elapsed times.
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be reported inaccurately.

## Omnissa Intelligence SDK 26.2.0 for Android - March 2, 2026

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2402 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

- **New Required Method in WorkspaceONE UEM Configuration Interface**
  - A new required method was added to the WorkspaceONE UEM configuration interface UemInfo, which is set through IntelligenceSDK singleton call `Crittercism.setUemInfo`.
  - Method name: `getDeviceUUID()`
    - This allows access to the WorkspaceONE UEM Global Device Unique Identifier (UUID) for the respective device.
    - When operating in Unauthenticated Mode (when the IntelligenceSDK has not been authenticated through enrollment), this UUID will be used to associate Intelligence and Telemetry records to UEM devices.
- **API to Report Health Status**
  - A new API was added to get the health status of Telemetry Features.
  - This API will be expanded in future releases to include broader diagnostics.
  - API signature:

```JAVA
/**
 * Asynchronously generate a status report on the health metrics of the Telemetry Features. Telemetry Features
 * must be started before calling this API otherwise incomplete results will be reported.
 * Health Metrics include information such as:
 *  1. Telemetry Configuration
 *  2. App permissions and their status (granted or denied)
 *  3. Telemetry Features Initialization status
 *  4. Telemetry Features Version
 *  5. Data sent vs Max Data Limit for the current day
 *  6. Telemetry Attributes Report - including attribute enablement status, entity enablement status,
 *        event enablement status, config settings, features, and reporters
 *
 * @param dataHandler Interface called when data is ready to be exported.
 */
fun generateStatusReport(dataHandler: TelemetryExportHandler)
```

- The health report may include:
  - Telemetry Features Version
  - Initialization Stage
  - Enabled Features List
  - Current DEX State (connected, started, or dimmed)
  - Telemetry enablement status
  - Event enablement features, callers, settings, and reporters
  - Whether the Intelligence interface is ready to be reported
- Example:

```JAVA
Crittercism.generateStatusReport(object : TelemetryExportHandler {
    override fun onDataExport(data: String?) {
        // Handle the status report JSON data string as needed if non-null
    }
})
```

### Resolved Issues

- **Fix for Tenant Handled Exception Reporting**
  - Fixed an issue on some Android platform versions where Tenant Handled Exceptions were reported as Crash Events if they were present during an unhandled application crash.

- **Removed Direct Boot Awareness**
  - Updated the DEX Telemetry Feature to no longer be a Direct Boot Aware component. Telemetry that featured logic around Reboot events are now only recorded after the device has booted and the device has been unlocked (reboots that occur before the device is fully unlocked can not be captured).
    - This allows apps that do not support Direct Boot Awareness to not accidentally trigger non-compliant logic before device unlock. 

### Known Issues

- Instrumented URLConnection classes may report inaccurate network request elapsed times.
- Application Lifecycle Breadcrumbs for "Foreground" and "Background" events may be reported inaccurately.

## Omnissa Intelligence SDK 25.11.0 for Android - December 3, 2025

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2402 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

- New KVP introduced to the Custom UEM SDK Settings: `IntelSDKAllowedApps`.
    - A JSON array listing Application IDs allowed to transmit DEX data.
    - If missing or empty: All apps can transmit DEX data (default behavior).
    - If present: Only the listed apps can transmit DEX data.
    - For more details, see [Intelligence SDK Allowed Apps](allowed-apps.md).

- New public API: `setSDKControlConfig`.
    - Apps pass the control config JSON string (Custom UEM SDK Settings) from UEM to the new `setSDKControlConfig` API.
    - Apps / SDK's are expected to fetch the control config JSON (Custom UEM SDK Settings) string and call this API. In the case the control config is not available or not configured, a null value should be passed for the config parameter.
    - The SDK parses the `IntelSDKAllowedApps` key and the `DEXData` key (Privacy Configuration) from the control config string.
    - Deprecated the `setPrivacyConfiguration` API in favor of the new `setSDKControlConfig` API.

```JAVA
public static void setSDKControlConfig(String config)
```
!!!Important
    Apps / SDK's should no longer call the deprecated `setPrivacyConfiguration` API as this API is mutually exclusive to `setSDKControlConfig`. For more details on the control configuration API, see [setSDKControlConfig](crittercism.md#setsdkcontrolconfigconfig).

- New DEX Telemetry event added: Device Reboot
    - Name: "device_reboot"
    - This Device Entity event triggers on device boot. It caches events (up to three of the latest boots) and are posted once TelemetrySDK starts. The event time will indicate when the device booted.

- New DEX Telemetry event added: SIM State Change Inserted
    - Name: "SIM_state_change_inserted"
    - This Network Entity event is triggered when the device has detected that a SIM card has been inserted into the device.

- New DEX Telemetry event added: SIM State Change Removed
    - Name: "SIM_state_change_removed"
    - This Network Entity event is triggered when the device has detected that a SIM card has been removed from the device.

- WiFi Efficiency Metrics added to DEX Network Entity data:
    - RSSI
    - Current Receive (Rx) Link Speed
    - Current Transmit (Tx) Link Speed
    - Max Receive (Rx) Link Speed
        - Android 11+ supported
    - Max Transmit (Tx) Link Speed
        - Android 11+ supported
    - Receive (Rx) Link Speed Efficiency Percentage
        - Android 11+ supported
    - Transmit (Tx) Link Speed Efficiency Percentage
        - Android 11+ supported

### Known Issues

- Instrumented URLConnection classes may report inaccurate network request elapsed times.
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be reported inaccurately.

## Omnissa Intelligence SDK 25.10.0 for Android - November 12, 2025

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

- Added support for Android 16KB page sizes.

### Known Issues

- Instrumented URLConnection classes may report inaccurate network request elapsed times.
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be reported inaccurately.

## Omnissa Intelligence SDK 25.7.0 for Android - September 2, 2025

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.6.0 or later

### New Features

- Added Tenant support for Intelligence Handled Exceptions events. 
  - Previously, Handled Exceptions were only sent to the App Owner and Development endpoints. Now, they can be routed to the authenticated Tenant Region without being an App Owner.
- Fixed issue around rapid Breadcrumb creation potentially causing main thread hangs: 
  - Breadcrumb creation now asynchronous to the main thread.
- New DEX event would be sent when the daily limit of 5MB (data send) is reached.
- Add the following Battery Attribute to the Telemetry DEX Feature data:
  - “battery_average_current”: represents the average current of the battery in milli-Amps (mA). 
  - “battery_capacity”: represents the maximum battery current capacity in milliAmp-Hours (mAh). 
  - “battery_remaining_charge”: represents the remaining battery current capacity in milliAmp-Hours (mAh).
- Upgrade Kotlin version to 2.1.0 from 1.8.21.
- Upgrade Android Gradle Plugin to 8.6.0 from 8.5.0.

### Known Issues

- Instrumented URLConnection Classes network request elapsed time inaccurate.
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be inaccurate.

### Resolved Issues

- Update Battery Level recording WorkManager logic as Android 11 devices were seeing duplicate jobs on reboot. 
  - Deprecated old Battery Level recording Worker, migrate to new version 2. 
  - Version 2 features a lightweight CoroutineWorker that is scheduled to run every 2 minutes with WorkManager scheduling discretion. 
  - Update Consumer Proguard to keep Deprecated Battery Level Worker. 
  - Battery Worker will now replace its own instance when creating the next iteration of Battery Recording work, instead of using Android APIs to chain which can cause duplicate jobs on reboot. 
  - Battery Workers will now prune old records as well as save Battery Level on run.
- Older Android devices below SDK 26 with low memory may see DEX events cease after hours of continuous usage.

## Omnissa Intelligence SDK 25.4.0 for Android - May 12, 2025

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.2.2 or later

### New Features

- New DEX Telemetry event added: Critical Cell Strength
  - Name: "critical_battery_level"
  - This event is triggered when the device reaches the following battery thresholds while the SDK is running.
    - Reports once when the device reaches a battery level between 16-20%.
    - Reports once when the device reaches a battery level between 11-15%.
    - Reports on every battery level between 0-10%.
  - When the device is charging these events will not be sent even if the device is in the specified range.
  - Device charging will reset the ability to send for all given sections, for example: If we have already sent an event for values between 16-20%, charging followed by discharging will allow another event to be sent for this battery level range.
- New DEX Telemetry attribute added: Is SD Card Portable
  - Name: "is_sdcard_portable"
  - SD card volumes that are marked as a public type are considered to be in the portable state.
- New DEX Telemetry attribute added: Percent Asleep
  - Name: "percent_asleep"
  - Represents the duration percentage that the device has been asleep since it was booted (on a scale from 0 to 100).
- New DEX Telemetry attribute added: Percent Awake
  - Name: "percent_awake"
  - Represents the duration percentage that the device has been awake since it was booted (on a scale from 0 to 100).
- App Usage Metrics reporting will now be checked every 10 minutes and will be triggered if no report has been submitted for the current day (once per day). This change will ensure that previous app metrics is reported earlier during the next day. For more details, see [App Usage Metrics](android-usage-metrics.md)
- Individual entity events are now limited to a maximum of 3 events per second. Additionally, the total amount of event data that can be transmitted is now capped at 5MB per day.

!!!Note
    Attributes and events are only triggered when DEX is enabled.

### Known Issues

- Instrumented URLConnection Classes network request elapsed time inaccurate. 
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be inaccurate.
- Older Android devices below SDK 26 with low memory may see DEX events cease after hours of continuous usage.

## Omnissa Intelligence SDK 25.1.0 for Android - February 10, 2025

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.2.2 or later

### New Features

- New DEX Telemetry entity added: App Usage Metrics. For more info, visit page: [Enabling App Usage Metrics](android-usage-metrics.md)

### Known Issues

- Instrumented URLConnection Classes network request elapsed time inaccurate. 
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be inaccurate.
- Older Android devices below SDK 26 with low memory may see DEX events cease after hours of continuous usage.

## Omnissa Intelligence SDK 24.11.0 for Android - December 18, 2024

### Minimum Requirements

- Android 7.0 or later
- API Level 24 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.2.2 or later

### New Features

- Minimum SDK version has been upgraded from 21 to 24 (Android 7.0 Nougat).
- Branding changes for migration to Omnissa.
- New DEX Telemetry event added: Critical Cell Strength
  - Name: "critical_cell_signal"
  - This event is triggered when the device has detected the Cellular Signal Strength level has reached a 1 or lower on a scale from 0-4.
  - NOTE - this is triggered only if DEX is enabled.

### Resolved Issues

none

### Known Issues

- Instrumented URLConnection Classes network request elapsed time inaccurate. 
- Application Lifecycle Breadcrumbs for “Foreground”, “Background” events may be inaccurate.

## Omnissa Intelligence SDK 24.8.0 for Android - September 16, 2024

### Minimum Requirements

- Android 5.0 or later (Android 7.0+ recommended)
- API Level 21 or later (API Level 24+ recommended)
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.2.2 or later

### New Features

- CrittercismConfig Configuration APIs around blocking the reporting of specified URL patterns when sending Network Events have been updated:

```JAVA
CrittercismConfig:
public void setURLBlacklistPatterns(java.util.List) -> public void setURLDenylistPatterns(java.util.List);
public java.util.List getURLBlacklistPatterns() -> public java.util.List getURLDenylistPatterns();
```

- Android NDK Crash Events have been re-worked. Native and NDK crashes originating from C/C++ code will now be reported to the Intelligence Console if enabled through the CrittercismConfig:
  - NOTE: Default enablement value for NDK Crash events: False

```JAVA
    CrittercismConfig:
    public void setNdkCrashEnabled(boolean);
    public boolean isNdkCrashEnabled();
```

- New SDK API, `setPrivacyConfiguration`, has been introduced which helps inject privacy configuration to control transmission of certain TelemetrySDK attributes.

```JAVA
/**
 * Asynchronously set the privacy configuration for a specific TelemetrySDK Feature.
 * IntelligenceSDK is agnostic to the configuration delivery mechanism as long as the payload conforms to
 * the required format.
 * Example of a privacy configuration:
 * 	"DEXData": {
 * 		"Version": 1.0,
 * 		"BatteryData": {
 * 			"DisableAll": false,
 * 			"AttributesToDisable": ["plugged_type", "battery_charging_rate"],
 * 			"EventsToDisable": []
 * 		},
 * 		"DeviceData": {
 * 			"DisableAll": false,
 * 			"AttributesToDisable": ["location_latitude", "location_longitude"],
 *          "EventsToDisable": []
 * 		},
 * 		"NetworkData": {
 * 			"DisableAll": false,
 * 			"AttributesToDisable": ["jitter", "latency"],
 * 			"EventsToDisable": []
 * 		}
 * 	}
 *
 * @param privacyConfig Map containing the necessary key value pairs for parsing the privacy config. If the privacy
 *                      config is fetched via WS1 UEM SDK, then it is in the JSON format. Apps / SDK's are expected
 *                      to fetch the value for key "DEXData" and convert it into a map. If the property "DEXData"
 *                      and its value does not exist, apps are still required to call this API with a null value for
 *                      the privacyConfig parameter.
 * @param feature       The Feature classification of the data to be exported, eg. DEX, ZeroTrust.
 */
public static void setPrivacyConfiguration(Map<String, Object> privacyConfig, @NonNull TelemetryFeature feature);
```

- New “DEX” Telemetry Feature Event: Network State Change. 
  - Event name: `network_change`
  - This event is triggered when there is a switch in network SSID, cellular type, ethernet, or a disconnection. Disconnections are reported if 5 seconds have surpassed and a reconnection of the same network type has not occurred.

### Known Issues

- Instrumented `URLConnection` Classes network request elapsed time inaccurate.

## Omnissa Intelligence SDK 24.6.1 for Android - August 9, 2024

### Minimum Requirements

- Android 5.0 or later
- API Level 21 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.2.2 or later

### New Features

- SDK Integration through Local Repository

The IntelligenceSDK and it’s dependencies will now be hosted in a Maven folder structure 
found in the Release section of our public Github Page: [https://github.com/euc-releases/ws1-intelligencesdk-sdk-android](https://github.com/euc-releases/ws1-intelligencesdk-sdk-android)

After downloading and unzipping the following repository folder, you can now reference and pull the
IntelligenceSDK using the following structure:

```groovy
   
   // App level Build.Gradle Configuration (Groovy)
   
   repositories {
       maven {
           url uri("/PATH/TO/FOLDER/ws1-intelligence-sdk-repository")
       }
       mavenCentral()
       google()
   }
   
   android {
      packagingOptions {
           pickFirst '**/libc++_shared.so'
           pickFirst '**/libcrypto.1.0.2.so'
           pickFirst '**/libssl.1.0.2.so'
       }
   }
   
   def ws1IntelSdkVersion = "24.11.0"
   
   dependencies {
       // Declare a dependency on the Intelligence SDK
       implementation "com.ws1:ws1intelligencesdk:$ws1IntelSdkVersion"
   }
```

### Resolved Issues

- Protobuf Dependency upgraded from older `protobuf-java` implementation to `protobuf-javalite`.
  - This allows for greater compatibility within projects that import other dependencies that rely on Protobuf, such as Google’s Firestore.

### Known Issues

none

## Omnissa Intelligence SDK 24.6.0 for Android - July 2024

### Minimum Requirements

- Android 5.0 or later
- API Level 21 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 8.2.2 or later

### New Features

- Opt-Int and Out status APIs have been refactored to separate SDK and Telemetry Feature interaction.
- The following have been added or refactored in the Crittercism singleton:

```JAVA
Refactored:
// Used to get/set SDK Opt-In / Opt-Out Status
void setOptInStatus(Crittercism.TelemetryType telemetryType, boolean optIn) -> void setSdkOptInStatus(boolean optIn)
boolean getOptInStatus(Crittercism.TelemetryType telemetryType) -> boolean getSdkOptInStatus()
Added:
// Used to get/set Telemetry Feature Opt-In Status
boolean getTelemetryOptInStatus(TelemetryFeature)
// Used to Opt-Out of all Telemetry Features
void telemetryOptOut();
```

- New API introduced to export data collected through our Telemetry Features!
Users are now able to export data collected from the Telemetry Features they have Opted-Into. Use the following API to get Feature, Event, and Export formatted specific data in an Asynchronous fashion!

```JAVA
  /**
  * Interface for handling Telemetry Feature exported data.
  */
  interface TelemetryExportHandler {
      /**
      * Callback when data export has completed.
      * @param data String which will contain exported Telemetry Feature data,
      * null if error occurs.
      */
      fun onDataExport(data: String?)
  }
  /**
    * Asynchronously Export Data collected through enabled Telemetry Features.
    * @param feature Telemetry Feature to query data for.
    * @param eventType Type of Telemetry Event data to query for: Attributes, Events, or All.
    * @param exportType Format of the data exported, currently offering JSON support only.
    * @param dataHandler Interface called when data is ready to be exported.
    */
  public static void exportTelemetryFeatureData( @NonNull TelemetryFeature feature,
                                                  @NonNull TelemetryEventType eventType,
                                                  @NonNull TelemetryExportType exportType,
                                                  @NonNull TelemetryExportHandler dataHandler) 
  Example: -------------------------------------------------------------------------------
  // Implement the TelemetryExportHandler Interface to reach exported data asynchronously.
  val dataHandler = object:TelemetryExportHandler { 
    // Callback method for export. Data parameter will either have formatted Telemetry
    // data or be null if an issue occurs. 
    override fun onDataExport(data: String?) {
        Log.d(TAG, "Here is my exported data: ${data ?: "An issue has occurred."}")
      } 
  }
  Crittercism.exportTelemetryFeatureData(
            TelemetryFeature.All, // Query data from all enabled features.
            TelemetryEventType.ALL, // Returns values for all data types.
            TelemetryExportType.JSON, // Format to export data in.
            dataHandler) // Exported data handler callback.
```

### Resolved Issues

none

### Known Issues

none

## Omnissa Intelligence SDK 24.3.1 for Android - March 2024

### Minimum Requirements

- Android 5.0 or later
- API Level 21 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 4.1.3 or later
- Workspace ONE Intelligent Hub for Android version 21.9 or later.

### New Features

none

### Resolved Issues

- Fixed an issue where interacting with the SDK's API's immediately after initializing IntelligenceSDK was causing the events to not be sent to the Intelligence backend.

### Known Issues

- Android Devices Below SDK version level 23 instrumenting WebViews will not be able to report HTTP errors in their Network Insights,  this is due to the following APIs not being supported previous to Android Marshmallow (23):
  - `onReceivedHttpError(WebView, WebResourceRequest, WebResourceResponse)`
  - `onReceivedError(WebView, WebResourceRequest, WebResourceError)`

## Omnissa Intelligence SDK 24.3.0 for Android - March 2024

### Minimum Requirements

- Android 5.0 or later 
- API Level 21 or later 
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 4.1.3 or later
- Workspace ONE Intelligent Hub for Android version 21.9 or later.

### New Features

- Developers can assign breadcrumbs to a specific userflow using the following API:

```JAVA
Crittercism.leaveUserflowSpecificBreadcrumb(final String userflowName, final String text)
```

!!!Note
    Breadcrumbs not specified under an active Userflow will operate on the traditional breadcrumb logic. Tenant Region Reporting will be calculated based on the Crittercism configuration options.

### Resolved Issues

- Computing upload / download speeds are disabled.

### Known Issues

- Android Devices Below SDK version level 23 instrumenting WebViews will not be able to report HTTP errors in their Network Insights,  this is due to the following APIs not being supported previous to Android Marshmallow (23):
  - `onReceivedHttpError(WebView, WebResourceRequest, WebResourceResponse)`
  - `onReceivedError(WebView, WebResourceRequest, WebResourceError)`

