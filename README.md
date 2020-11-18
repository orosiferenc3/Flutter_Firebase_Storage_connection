# Flutter and Firebase Storage connection

In this project i show you how to get images from Firebase Storage

## Firebase Storage

My Firebase Storage look like this:
[picture]

## android/app

You need to copy the google-services.json to android/app folder

## pubspec.yaml

You need these packages in pubspec.yalm

```dart
dependencies:
  flutter:
    sdk: flutter
  firebase_core: "^0.5.2"
  firebase_storage: "^5.0.1"
  firebase_auth: "^0.18.3"
  cached_network_image: ^2.3.3
```

## android/build.gradle

Your android/build.gradle file should look like this:

```dart
buildscript {
    ext.kotlin_version = '1.3.50'
    repositories {
        google() // Google's Maven repository
        jcenter()
    }

    dependencies {
        classpath 'com.google.gms:google-services:4.3.4' // Google Services plugin
        classpath 'com.android.tools.build:gradle:3.5.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
allprojects {
    repositories {
        google() // Google's Maven repository
        jcenter()
    }
}
```

## android/app/build.gradle

Your android/app/build.gradle file need to contains the following codes:

```dart
apply plugin: 'com.google.gms.google-services' // Google Services plugin
```

```dart
dependencies {
    // Import the BoM for the Firebase platform
    implementation platform('com.google.firebase:firebase-bom:26.1.0')

    // Declare the dependency for the Cloud Storage library
    // When using the BoM, you don't specify versions in Firebase library dependencies
    implementation 'com.google.firebase:firebase-storage'
    implementation 'com.google.firebase:firebase-auth'
}
```

## main.dart

In the main.dart file you need these imports:

```dart
import 'package:firebase_storage/firebase_storage.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:cached_network_image/cached_network_image.dart';
```

void main() should look like this:

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  UserCredential userCredential = await FirebaseAuth.instance
      .signInAnonymously(); // you need to indentify your user
  print(userCredential);
  runApp(MyApp());
}
```

In your code, where your image would be, you need to insert this code.

```dart
FutureBuilder<dynamic>(
              future: FirebaseStorage().ref('madar.jpg').getDownloadURL(),
              builder: (BuildContext context, AsyncSnapshot<dynamic> snapshot) {
                if (snapshot.connectionState != ConnectionState.waiting) {
                  return Image(
                    image: CachedNetworkImageProvider(snapshot.data.toString()),
                    fit: BoxFit.cover,
                  );
                } else {
                  return Text('Loading image....');
                }
              },
            ),
```
