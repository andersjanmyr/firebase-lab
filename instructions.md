# Firebase Lab

## Preparations

* Clone the repo (or fork it) `git clone https://github.com/firebase/friendlychat`
* Create [Firebase Project](https://console.firebase.google.com/) with your
  Google account.

### IOS

* Install [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)
* `cd friendlychat/ios-starter/swift-starter`
* `pod install`

### Android

* `cd friendlychat/android-starter`
* Make sure you can build the project with Android Studio or with Gradle
  `./gradlew connectedAndroidTest` (will fail unless you have a device or
  emulator configured, but will install everything)

### Web

* `cd friendlychat/web-starter`
* `npm install -g firebase-tools`


## Instructions

### IOS

* https://codelabs.developers.google.com/codelabs/firebase-ios-swift

#### Issues

* If camera is not working, add `Privacy - Photo Library Usage Description` to
  your `info.plist`. (Suggested value: `$(PRODUCT_NAME) uses photos to collect
  information in order to blackmail you later.`)


### Android

* https://codelabs.developers.google.com/codelabs/firebase-android


### Web

* https://codelabs.developers.google.com/codelabs/firebase-web


## Extra

Choose what you think is interesting and add to your application.

* Make it possible to `edit` and `delete` messages.
* Change security settings, `database rules`, to only allow editing and
  deleting your own messages.
* Add dynamic linking to your application.
* Add Notifications.
* Add invites to your application.
* Set up another authentication provider.


