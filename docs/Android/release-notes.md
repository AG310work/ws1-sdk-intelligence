---
layout: page
title: Omnissa Intelligence SDK for Android Release Notes
hide:
  #- navigation
  - toc
---

Updated on 2/10/2025

What's in the Release Notes

Omnissa Intelligence SDK for Android Release Notes describe the new features and enhancements in each release. This page contains a summary of the new capabilities, issues that have been resolved, and known issues that have been reported in each release. 

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
found in the Release section of our public Github Page: https://github.com/euc-releases/ws1-intelligencesdk-sdk-android

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
    Breadcrumbs not specified under an active userflow will operate on the traditional breadcrumb logic. Tenant Region Reporting will be calculated based on the Crittercism configuration options.

### Resolved Issues

- Computing upload / download speeds are disabled.

### Known Issues

- Android Devices Below SDK version level 23 instrumenting WebViews will not be able to report HTTP errors in their Network Insights,  this is due to the following APIs not being supported previous to Android Marshmallow (23):
  - `onReceivedHttpError(WebView, WebResourceRequest, WebResourceResponse)`
  - `onReceivedError(WebView, WebResourceRequest, WebResourceError)`
