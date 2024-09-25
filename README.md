# Interaction Tracking with Firebase Analytics in Flutter

![Firebase Analytics Event Tracking](https://github.com/user-attachments/assets/a8d9fb65-1f13-4c10-80b8-77698a2d2172)

## Overview

This guide demonstrates how to integrate Firebase Analytics into a Flutter application to track user interactions. It covers the setup process, event logging, and includes details about other analytics platforms like Mixpanel, Amplitude, Segment, and Facebook Analytics. However, Firebase Analytics is selected for its simplicity and ease of integration in Flutter projects.

## Table of Contents

- [Interaction Tracking with Firebase Analytics in Flutter](#interaction-tracking-with-Firebase-analytics-in-flutter)
  - [Overview](#overview)
  - [Table of Contents](#table-of-contents)
  - [Why Firebase Analytics?](#why-firebase-analytics)
  - [Setup Firebase in Flutter](#setup-firebase-in-flutter)
    - [`Firebase Console` Setup](#firebase-console-setup)
    - [`Flutter Project`Setup](#flutter-project-setup)
    - [`Logging Events`](#logging-events)
  - [Integrating Firebase Analytics in Flutter](#integrating-firebase-analytics-in-flutter)
  - [Tracking Events](#tracking-events)
  - [Viewing Analytics Data](#viewing-analytics-data)
  - [Conclusion](#conclusion)
  - [References](#references)


## Why Firebase Analytics?

While there are various platforms available for tracking user interactions, Firebase Analytics stands out for several reasons:
- Seamless integration with Flutter and Firebase services.
- It's free, with generous quotas for small to medium-scale apps.
- Rich real-time data collection and event logging.

Other options such as Mixpanel, Amplitude, Segment, and Facebook Analytics also provide powerful analytics features. However, Firebase is the simplest to set up and manage, particularly within the Flutter ecosystem.


## Setup Firebase in Flutter

### Firebase Console Setup
- Go to the [Firebase Console](https://console.firebase.google.com/u/0/), and create a new project.

### Flutter Project Setup
1. Install Firebase CLI and set up Firebase for your project by running the following commands:
   
   ```sh
   firebase login
   dart pub global activate flutterfire_cli
   flutterfire configure
   ```
   During this setup, add your app's package name when prompted.

2. Add the required Firebase secrets for Firebase Analytics:

   - Go to your project's repository on GitHub.
   - Navigate to Settings > Secrets and Variables > Actions.
   - Add the following secrets:
     - `FIREBASE_OPTIONS_ANDROID_API_KEY`
     - `FIREBASE_OPTIONS_ANDROID_APP_ID`
     - `FIREBASE_OPTIONS_IOS_API_KEY`
     - `FIREBASE_OPTIONS_IOS_APP_ID`
 
3. Update the `android/build.gradle` file as follows:

   ```gradle
   // Add the buildscript block at the top
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
       dependencies {
           // Add the Google Services classpath here
           classpath 'com.google.gms:google-services:4.3.15'
       }
   }
   ```

4. Update `android/app/build.gradle` file as follows:
   ```gradle
   apply plugin: 'com.google.gms.google-services'
   dependencies {
      // Firebase SDK dependencies
      implementation platform('com.google.firebase:firebase-bom:32.2.0') // or latest version
       implementation 'com.google.firebase:firebase-analytics'
   }
   ```

### Logging Events

Firebase Analytics allows you to track specific user actions through event logging. First, initialize Firebase in the `main.dart` file and log events using `FirebaseAnalytics`.


## Integrating Firebase Analytics in Flutter

1. Initialize Firebase in your `main.dart` file:
   
   ```dart
   Future<void> main() async {
     WidgetsFlutterBinding.ensureInitialized();
     await Firebase.initializeApp();
     runApp(const MyApp());
   }
   ```

2. Set up your app to track user interactions:
   ```dart
   class MyApp extends StatelessWidget {
     final FirebaseAnalytics analytics = FirebaseAnalytics.instance;
  
     MyApp({super.key});
  
     @override
     Widget build(BuildContext context) {
       return const MaterialApp(
         title: 'Firebase Analytics Demo',
         home: HomeView(),
       );
     }
   }
   ```

## Tracking Events

To track specific interactions, implement the following in your `HomeView` widget:

1. Log custom events by calling `analytics.logEvent()`.
2. Set user properties using `analytics.setUserProperty()`. This can be useful for understanding specific behaviors tied to user segments.

```dart
import 'package:firebase_analytics/firebase_analytics.dart';
import 'package:flutter/material.dart';

class HomeView extends StatefulWidget {
  const HomeView({super.key});

  @override
  HomeViewState createState() => HomeViewState();
}

class HomeViewState extends State<HomeView> {
  final FirebaseAnalytics analytics = FirebaseAnalytics.instance;

  @override
  void initState() {
    super.initState();
    _setUserProperty();
  }

  // Setting user property
  void _setUserProperty() async {
    await analytics.setUserProperty(
      name: 'favorite_color',
      value: 'blue',
    );
  }

  // Log button press event
  void _logButtonPressed() {
    analytics.logEvent(
      name: 'button_pressed',
      parameters: {'button_name': 'start_button'},
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Firebase Analytics Demo',
          style: TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.bold,
          ),
        ),
      ),
      body: SafeArea(
        child: Center(
          child: ElevatedButton(
            onPressed: () {
              _logButtonPressed();
            },
            child: const Text('Start'),
          ),
        ),
      ),
    );
  }
}
```

## Viewing Analytics Data

To view the logged data:

1. Go to the Firebase Console.
2. Navigate to the Analytics section.
3. Here, you can explore user events, set up custom dashboards, and gain insights into user behavior.


## Conclusion

Integrating Firebase Analytics into your Flutter app is a powerful way to track user behavior and interactions. It provides deep insights into how users interact with your application, enabling you to make data-driven decisions and optimize your app's user experience.

Happy Tracking! 🎯


## References

- [Firebase Analytics Documentation](https://firebase.google.com/docs/analytics/get-started?platform=flutter)
- [FlutterFire Documentation](https://firebase.flutter.dev/docs/overview/)
- [Firebase CLI Documentation](https://firebase.google.com/docs/cli#install-cli-windows)

