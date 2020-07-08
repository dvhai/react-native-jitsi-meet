# react-native-jitsi-meet
React native wrapper for Jitsi Meet SDK

## Install

`npm install dvhai/react-native-jitsi-meet --save` 

----- Cấu Hình --------
## Use (>= 2.0.0)

The following component is an example of use:

```
import React from 'react';
import { View } from 'react-native';
import JitsiMeet, { JitsiMeetView } from 'react-native-jitsi-meet';

class VideoCall extends React.Component {
  constructor(props) {
    super(props);
    this.onConferenceTerminated = this.onConferenceTerminated.bind(this);
    this.onConferenceJoined = this.onConferenceJoined.bind(this);
    this.onConferenceWillJoin = this.onConferenceWillJoin.bind(this);
  }

  componentDidMount() {
    setTimeout(() => {
      const url = 'https://your.jitsi.server/roomName'; // can also be only room name and will connect to jitsi meet servers
      const userInfo = { displayName: 'User', email: 'user@example.com', avatar: 'https:/gravatar.com/avatar/abc123' };
      JitsiMeet.call(url, userInfo);
      /* You can also use JitsiMeet.audioCall(url) for audio only call */
      /* You can programmatically end the call with JitsiMeet.endCall() */
    }, 1000);
  }

  onConferenceTerminated(nativeEvent) {
    /* Conference terminated event */
  }

  onConferenceJoined(nativeEvent) {
    /* Conference joined event */
  }

  onConferenceWillJoin(nativeEvent) {
    /* Conference will join event */
  }

  render() {
    return (
      <View style={{ backgroundColor: 'black',flex: 1 }}>
        <JitsiMeetView onConferenceTerminated={this.onConferenceTerminated} onConferenceJoined={this.onConferenceJoined} onConferenceWillJoin={this.onConferenceWillJoin} style={{ flex: 1, height: '100%', width: '100%' }} />
      </View>
    );
  }
}

export default VideoCall;
```

You can also check the [ExampleApp](https://github.com/skrafft/react-native-jitsi-meet/tree/master/ExampleApp)

### Events

You can add listeners for the following events:
- onConferenceJoined
- onConferenceTerminated
- onConferenceWillJoin


### Events

You can add listeners for the following events:
- CONFERENCE_JOINED
- CONFERENCE_LEFT
- CONFERENCE_WILL_JOIN

## iOS Configuration

1.) navigate to `<ProjectFolder>/ios/<ProjectName>/`  
2.) edit `Info.plist` and add the following lines

```
<key>NSCameraUsageDescription</key>
<string>Camera Permission</string>
<key>NSMicrophoneUsageDescription</key>
<string>Microphone Permission</string>
```
3.) in `Info.plist`, make sure that 
```
<key>UIBackgroundModes</key>
<array>
</array>
```
contains `<string>voip</string>`

## iOS Install for RN >= 0.60
1.) Modify your Podfile to have ```platform :ios, '10.0'``` and execute ```pod install```  
2.) In Xcode, under Build setting set Enable Bitcode to No  

## Android Install
1.) In `android/app/build.gradle`, add/replace the following lines:

```
project.ext.react = [
    entryFile: "index.js",
    bundleAssetName: "app.bundle",
]
```

2.) In `android/app/src/main/java/com/xxx/MainApplication.java` add/replace the following methods:

```
  import androidx.annotation.Nullable; // <--- Add this line if not already existing
  ...
    @Override
    protected String getJSMainModuleName() {
      return "index";
    }

    @Override
    protected @Nullable String getBundleAssetName() {
      return "app.bundle";
    }
```

3.) In `android/build.gradle`, add the following code 
```
allprojects {
    repositories {
        mavenLocal()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
        maven {
            url "https://maven.google.com"
        }
        maven { // <---- Add this block
            url "https://github.com/jitsi/jitsi-maven-repository/raw/master/releases"
        }
        maven { url "https://jitpack.io" }
    }
}
```


### Side-note

If your app already includes `react-native-locale-detector` or `react-native-vector-icons`, you must exclude them from the `react-native-jitsi-meet` project implementation with the following code (even if you're app uses autolinking with RN > 0.60):

```
    implementation(project(':react-native-jitsi-meet')) {
      exclude group: 'com.facebook.react',module:'react-native-locale-detector'
      exclude group: 'com.facebook.react',module:'react-native-vector-icons'
    }
```
