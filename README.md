
# Flutter Add to Existing App

A brief of how to add Flutter View into current Native application


## Setup
- For Android: [Integrate a Flutter module into your Android project](https://docs.flutter.dev/development/add-to-app/android/project-setup)
- For IOS: [Integrate a Flutter module into your IOS project](https://docs.flutter.dev/development/add-to-app/ios/project-setup)
- There are many ways, options to embed Flutter View into Native app. For current situation, add Flutter module related to this [example](https://medium.com/@ptruiz/adding-flutter-to-your-existing-ios-and-android-codebases-3e2c5a4797c1)

## Communication between Native and Flutter - via package Pigeon
- Using the [Pigeon](https://pub.dev/packages/pigeon) package for sending structured typesafe messages with code generation

#### Usage
1. Make a ".dart" file outside of your "lib" directory for defining the communication interface.
2. Run pigeon on your ".dart" file to generate the required Dart and host-language code:
```
flutter pub run pigeon \
  --input pigeons/message.dart \
  --dart_out lib/pigeon.dart \
  --objc_header_out ios/Runner/pigeon.h \
  --objc_source_out ios/Runner/pigeon.m \
  --swift_out ios/Runner/Pigeon.swift \
  --kotlin_out ./android/app/src/main/kotlin/dev/flutter/pigeon/Pigeon.kt \
  --experimental_kotlin_package "dev.flutter.pigeon" \
  --java_out ./android/app/src/main/java/dev/flutter/pigeon/Pigeon.java \
  --java_package "dev.flutter.pigeon"
```
3. Add the generated Dart code to Xcode workspace
4. Implement the host-language code and add it to your build
5. Call the generated Dart methods

#### Suported Data Types
- Custom classes used by APIs are defined as classes with fields of the supported datatypes [Suported Data Types](https://docs.flutter.dev/development/platform-integration/platform-channels?tab=type-mappings-swift-tab#codec)

#### Defining Rules
- Rules for defining your communication interface  [Rules](https://pub.dev/packages/pigeon#rules-for-defining-your-communication-interface)

#### Features
https://pub.dev/packages/pigeon#features

#### Code Reference
- [Example from package](https://github.com/flutter/packages/tree/main/packages/pigeon/example#messagedart)
- [Public source](https://github.com/zero-li/flutter_pigeon_plugin)

## Navigation
- Native and Flutter screens must work seamlessly together. But things appeared more complex, cause by native iOS app had some generic view controllers that didn’t play well with Flutter screens.
- For further usage, have to create a route or bridge in Dart, Kotlin, and Swift. But that still adds some development overhead that wouldn’t be the case for a pure Flutter app.
- Current solution is using Method Channel to communicate and create EngineBinding interface to handle app routing.

## Reference
- Sample that shows how to use the Flutter Engine Groups API to embed multiple instances of Flutter into an existing Android or iOS project. [Github](https://github.com/flutter/samples/tree/main/add_to_app/multiple_flutters)
- Medium post for completed tutorial [Medium](https://medium.com/flutter-community/add-flutter-to-existing-android-ios-app-ae8c4fb1582e)

## Debugging and Hot Reload
- Official document: [Debugging your add-to-app module](https://docs.flutter.dev/development/add-to-app/debugging)
- Note: With native recompilation, all screen state is lost, a developer needs to click through all screens to retest a feature and often need to debug across many IDEs.
- According to the lifecycle of Flutter engines in add-to-app, the screen state gets cached. So if you’re, for example, going from a native screen to a Flutter screen, do something on it, then go back to the native screen, and then again to the Flutter screen, it will look exactly the same once you have left it.

## Background services
- In our situation, current background services had been handled in MainActivity, which is confirmed kept in native. But for further development, this topic must be put on the table. 
- We could simply execute the existing native background services, but our goal was different - the business logic had to be a platform-independent module in Dart.
- Detail blog explain how to implement background service in Flutter add to app [Background Services in Flutter Add-to-App Case](https://leancode.co/blog/background-services-in-flutter-add-to-app)

## App size and Performance
- As Flutter has its rendering engine and runtime, it will impact the app size.
- When you’re adding Flutter runtime over a running native app, it is impossible not to add some extra performance overhead to the existing iOS and Android app.
- Actual result: Not been tested yet.

## Challenges
- It's nearly impossible to predict all edge cases.
- Add to app as a long-term solution can decrease the efficiency of your app. 
- Packing multiple Flutter libraries into an application isn’t supported.

## Conclusion
- [X] Setup intruction
- [X] Communication (i.e passing arguments, invoke method,...)
- [X] Navigation in app
- [X] Advanced implementation use case
- [X] Example and Reference
- [ ] Demo
- [ ] Testing app size and performance
