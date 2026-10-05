# FitBand Native Android v1

This project wraps the FitBand HTML UI in an Android WebView and adds a native Android BLE scanner/GATT connection layer.

## What works
- Android WebView loads the FitBand interface from assets/index.html.
- Bottom navigation and UI buttons work.
- Android 12+ Nearby Devices permissions are requested.
- BLE scan starts from the HTML `Connect` button.
- The native layer looks for a device advertising a Mi Band-style name.
- GATT connection and service discovery are attempted.
- Connection/disconnection status is sent back to JavaScript.
- BLE notifications are exposed to JavaScript for diagnostics.

## Important Mi Band 3 limitation
A BLE connection is NOT the same thing as a working Mi Band 3 data sync. Mi Band 3 uses device-specific authentication/commands and data parsing. This project deliberately does not fabricate steps/heart-rate/sleep data.

The next protocol layer must implement the Mi Band 3 authentication, characteristics, commands, and parsers. Gadgetbridge is an established open-source Android project that supports Mi Band 3 and is a useful protocol reference. Review its license before copying any code.

## Build
Open the folder in Android Studio and let Gradle sync. Then run the `app` configuration on an Android phone. The phone must support BLE.

If Android Studio is too heavy for your 4 GB RAM PC, use an Android/cloud build service that accepts an Android Studio/Gradle project.

## UI bridge
The HTML calls:
- Android.fitBand.scanAndConnect()
- Android.fitBand.disconnect()
- Android.fitBand.requestSync()
- Android.fitBand.startHeartRate()

Native callbacks:
- window.fitBandNative.onConnected(...)
- window.fitBandNative.onDisconnected()
- window.fitBandNative.onScanStatus(...)
- window.fitBandNative.onBleNotification(...)
- window.fitBandNative.onData(...)

## Safety
Do not treat the generic BLE notification bytes as fitness readings. Real watch data should only be displayed after the Mi Band 3 protocol is correctly authenticated and parsed.
