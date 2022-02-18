# ARKit

## Verifying Device Support and User Permission

- Check whether your app can use ARKit and respect user privacy at runtime.

### Overview

- If the basic functionality of your app requires AR (using the back camera) : Add the `arkit`key in the [UIRequiredDeviceCapabilities](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) section of your app's `Info.plist` file. Using this key makes your app available only to ARKit-compatible devices.

- **If augmented reality is a secondary feature of your app:** Check for whether the current device supports the AR configuration you want to use by testing the [`isSupported`](https://developer.apple.com/documentation/arkit/arconfiguration/2923553-issupported?language=objc) property of the appropriate [`ARConfiguration`](https://developer.apple.com/documentation/arkit/arconfiguration?language=objc) subclass. 

- **If your app uses face-tracking AR:** Face tracking requires the front-facing TrueDepth camera on iPhone X. Your app remains available on other devices, so you must test the [`ARFaceTrackingConfiguration`](https://developer.apple.com/documentation/arkit/arfacetrackingconfiguration?language=objc)`.`[`isSupported`](https://developer.apple.com/documentation/arkit/arconfiguration/2923553-issupported?language=objc) property to determine face-tracking support on the current device.

  - Tip

    Check the [`isSupported`](https://developer.apple.com/documentation/arkit/arconfiguration/2923553-issupported?language=objc) property before offering AR features in your app's UI, so that users on unsupported devices aren't disappointed by trying to access those features.

