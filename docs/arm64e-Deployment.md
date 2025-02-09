As of iOS 12.0, a new architecture exists, named by Apple as **arm64e**. This implements the armv8.3 architecture, and is supported by the Apple A12 and newer processors. Specifically, this architecture is known for implementing pointer authentication.

This architecture is currently not considered public by Apple, and is in flux, so it is not advised to be used. However, it is necessary to build for this architecture in order to inject code into iOS platform binaries, which are built using the arm64e architecture. Due to the nature of pointer authentication, backwards compatibility with arm64 (armv8.0) binaries is not implemented.

For more information on pointer authentication, refer to the [official documentation](https://developer.apple.com/documentation/security/preparing_your_app_to_work_with_pointer_authentication) on the topic.

## Building for iOS 12.0–13.7
You may have been directed here by the following warning output by Theos:

> **Warning:** Building for iOS 7.0, but the current toolchain can’t produce arm64e binaries for iOS earlier than 14.0. More information: https://github.com/theos/theos/wiki/arm64e-Deployment

With iOS 14.0 and Xcode 12.0 (or clang 1200 and newer for toolchains on other platforms), the ABI (application binary interface) of arm64e changed. Binaries built with this version of the compiler will *not* be compatible with the arm64e implementation on iOS 12.0–13.7.

There are two ways to overcome this:

* **Increase your deployment version:** If you only intend for your product to run on iOS 14.0 or newer anyway, make this explicit by setting as such in your `TARGET`. For example, adding to the top of your Makefile:

  ```make
  export TARGET = iphone:latest:14.0
  ```

  This `TARGET` variable indicates to build for iOS using the latest SDK version you have installed, deploying the binary for iOS 14.0 and newer.
* **Install Xcode 11.7:** If you still plan to support versions of iOS earlier than 14.0, you will need to downgrade to Xcode 11.7.

  You can download Xcode 11.7 directly from Apple at the following link: [**Xcode_11.7.xip**](https://developer.apple.com/services-account/download?path=/Developer_Tools/Xcode_11.7/Xcode_11.7.xip). You will need to authenticate with your Apple ID to download.

  Once downloaded, opening the .xip file will begin extracting it. After extraction, rename the app to not conflict with your primary installation of Xcode. A good choice is `Xcode-11.7.app`.

  Finally, change your selected Xcode command line tools version by using the following command:

  ```bash
  sudo xcode-select -switch /Applications/Xcode-11.7.app

  # If you need to use the latest Xcode toolchain from the command line,
  # simply switch it back:
  sudo xcode-select -switch /Applications/Xcode.app
  ```

  You can also use the Xcode GUI to do this, via Xcode &rarr; Preferences &rarr; Locations &rarr; Command Line Tools.
