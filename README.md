# user_location

[![pub package](https://img.shields.io/pub/v/user_location.svg)](https://pub.dartlang.org/packages/user_location) ![travis](https://api.travis-ci.com/lpongetti/flutter_map_marker_cluster.svg?branch=master)



A plugin for [FlutterMap](https://github.com/johnpryan/flutter_map)  package to handle and plot the current user location


<div style="text-align: center"><table><tr>
  <td style="text-align: center">
  <a href="https://github.com/igaurab/UserLocationPlugin/blob/master/example.gif">
    <img src="https://github.com/igaurab/UserLocationPlugin/blob/master/example.gif" width="200"/></a>
</td>
</tr></table></div>

## Usage

Add flutter_map and  user_location to your pubspec.yaml :

```yaml
dependencies:
  flutter_map: any
  user_location: any # or the latest version on Pub
```



Update your `gradle.properties` file with this:

```
android.enableJetifier=true
android.useAndroidX=true
org.gradle.jvmargs=-Xmx1536M
```



Also make sure that you have added those dependencies in your build.gradle:

```
  dependencies {
      classpath 'com.android.tools.build:gradle:3.3.0'
      classpath 'com.google.gms:google-services:4.2.0'
  }
  compileSdkVersion 28
```

## Getting Started 

### Android 

In order to use this plugin in Android, you have to add this permission in `AndroidManifest.xml` :

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

Permission check for Android 6+ was added.

### iOS

And to use it in iOS, you have to add this permission in Info.plist :

```xml
NSLocationWhenInUseUsageDescription
NSLocationAlwaysUsageDescription
```

* Note: I have not tested the plugin in ios

### Installation guide

- Declare and initialize ```  MapController mapController = MapController(); List<Marker> markers = [];```
- Add ` UserLocationPlugin()` to plugins
- Add  `MarkerLayerOptions` and `UserLocationOptions` in `layers`
> While performing a location update, We remove and add the location marker on the map. Using a single list to hold this location marker along with other markers will unnecessarily also clear them. For that reason, we need to provide an empty marker array to hold the location marker. 
 

### Sample code

```dart
import 'package:flutter/material.dart';
import 'package:user_location/user_location.dart';
import 'package:flutter_map/flutter_map.dart';
import 'package:latlong/latlong.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'User Location Plugin Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: HomePage(),
    );
  }
}

class HomePage extends StatelessWidget {
  // ADD THIS
  MapController mapController = MapController();
  // ADD THIS
  List<Marker> markers = [];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: Text("User Location Plugin")),
        body: FlutterMap(
          options: MapOptions(
            center: LatLng(0,0),
            zoom: 15.0,
            plugins: [
             // ADD THIS
              UserLocationPlugin(),
            ],
          ),
          layers: [
            TileLayerOptions(
              urlTemplate: "https://api.tiles.mapbox.com/v4/"
                  "{id}/{z}/{x}/{y}@2x.png?access_token={accessToken}",
              additionalOptions: {
                'accessToken': '<access_token>',
                'id': 'mapbox.streets',
              },
            ),
            // ADD THIS
            MarkerLayerOptions(markers: markers),
            // ADD THIS
            UserLocationOptions(
                context: context,
                mapController: mapController,
                markers: markers,
                ),
          ],
          // ADD THIS
          mapController: mapController,
        ));
  }
}
```


### Optional parameters
* `markerWidget` overrides the default marker widget
* `markerlocationStream` provides the current location of the marker
* `updateMapLocationOnPositionChange` moves the map to the current location of the user if set to `true`
* `showMoveToCurrentLocationFloatingActionButton` displays a floating action button at the bottom right of the screen which will redirect the user to their current location. This is useful if the above `updateMapLocationOnPositionChange` is set to `false`
* `moveToCurrentLocationFloatingActionButton` is a widget when passed overrides the default floating action button. Default floating action button code: 
``` 
Container(
    decoration: BoxDecoration(
    color: Colors.blueAccent,
    borderRadius: BorderRadius.circular(20.0),
    boxShadow: [
        BoxShadow(color: Colors.grey, blurRadius: 10.0)
        ]),
    child: Icon(
        Icons.my_location,
        color: Colors.white,
    ),
)
```
### Run the example

See the `example/` folder for a working example app.
