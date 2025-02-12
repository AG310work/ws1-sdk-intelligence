---
layout: page
title: Omnissa Intelligence SDK for Android
hide:
  #- navigation
  - toc
---

## Introduction

This topic describes how to use Omnissa Intelligence with Android apps. This guide assumes you have an Omnissa Intelligence account set up with a valid app ID.

## Release Notes

Release Notes can be viewed [here](release-notes.md).

## Requirements

- Android 5.0 or later
- API Level 21 or later
- Workspace ONE UEM Console 2109 or later
- Android Studio with the Gradle Android Build System (Gradle) 4.1.3 or later
- Workspace ONE Intelligent Hub for Android version 21.9 or later.

## Guide

- [How to install and setup the Omnissa Intelligence SDK?](android-install.md)
- [SDK Data Sheet](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileIntelligenceSDK.html)

## SDK References

Import `com.crittercism.app` package to use the Omnissa Intelligence SDK.

- [Crittercism](crittercism.md)
- [CrittercismConfig](crittercism-config.md)
- [CrittercismCallback](crittercism-callback.md)
- [CrashData](crash-data.md)
- [NetworkInstrumentation](network-instrumentation.md)
- [Telemetry Feature Integration](dex-telemetry-integration.md)

## Features

### Out-of-the-box Features

Some features are available as soon as you integrate Omnissa Intelligence SDK into your app.

- [Network Insights](android-apm.md)
- [Android Intelligence SDK Network Insights Clients User Guide](https://developer.omnissa.com/ws1-intelligence-sdk/guides/Android-Intelligence-SDK-Network-Insights.pdf)

### Main Features

- [Logging Breadcrumbs](crittercism.md#logging-breadcrumbs)
- [Logging Handled Exceptions](crittercism.md#logging-handled-exceptions)
- [Logging User Flows](crittercism.md#logging-user-flows)
- [Logging Network Request](crittercism.md#logging-network-request)
- [Instrumenting OkHttpClient (Beta)](crittercism.md#instrumenting-okhttpclient-beta)
- [Instrumenting WebView](crittercism.md#instrumenting-webview)
- [Enabling DEX](crittercism.md#dex-telemetry-opt-in)

### Other Features

- [Enabled App Usage Metrics](android-usage-metrics.md)
- [Detecting a Crash Occurred](crittercism.md#detecting-a-crash-occurred)
- [Delay Sending App Load Data](crittercism.md#delay-sending-app-load-data)
- [Opt In Status](crittercism.md#opt-in-status)
- [Include System Log Data (Logcat)](crittercism-config.md#include-system-log-data-logcat)
- [Setting Log Verbosity of Omnissa Intelligence SDK](crittercism.md#setting-log-verbosity-of-workspace-one-intelligence-sdk)
- [Send Data On Wifi Only](crittercism-config.md#send-data-on-wifi-only)
- [Sending UEM Attributes To Intelligence SDK](android-integrate-ws1sdk.md)
