# TAKE CARE - THIS IS A FORK

This repository is but a fork of https://github.com/wnyc/cordova-plugin-wakeuptimer.git and **I will not maintain it**!

# Wakeup/Alarm Clock PhoneGap/Cordova Plugin

### Platform Support

This plugin supports PhoneGap/Cordova apps running on both iOS and Android.

### Version Requirements

This plugin is meant to work with Cordova 3.5.0+.

## Installation

#### Automatic Installation using PhoneGap/Cordova CLI (iOS and Android)
1. Make sure you update your projects to Cordova iOS version 3.5.0+ before installing this plugin.

        cordova platform update ios
        cordova platform update android

2. Install this plugin using PhoneGap/Cordova cli:

        cordova plugin add https://github.com/lentschi/cordova-plugin-wakeuptimer.git

## Usage

```typescript
// all responses from the audio player are channeled through successCallback and errorCallback


// Request required extra permission (see https://developer.android.com/guide/components/activities/background-starts )
window.wakeuptimer.requestPermissions(result => {
    // `result.canDrawOverlays` will be
    //  -> true, if...
    //      a) the user accepted to grant the permission
    //      b) the user is running Android < 10 (no extra permission required -> calls back immediately)
    //  -> false, if
    //      a) the user denied to grant the permission
    //      b) the user is running "Android 10 Go Edition" (no support for starting
    //          activities from the background)
}, errorCallback);

// set wakeup timer
window.wakeuptimer.wakeup( successCallback,
    errorCallback,
    // a list of alarms to set
    {
        alarms : [{
            type : 'onetime',
            time : { hour : 14, minute : 30 },
            extra : { message : 'json containing app-specific information to be posted when alarm triggers' },
            message : 'Alarm has expired!'
        }]
    }
);

// snooze...
window.wakeuptimer.snooze( successCallback,
    errorCallback,
    {
        alarms : [{
            type : 'snooze',
            time : { seconds : 60 }, // snooze for 60 seconds
            extra : { }, // json containing app-specific information to be posted when alarm triggers
            message : this.get('message'),
            sound : this.get('sound'),
            action : this.get('action')
        }]
    }
    );

// example of a callback method
var successCallback = function(result) {
    if (result.type==='wakeup') {
        console.log('wakeup alarm detected--' + result.extra);
    } else if(result.type==='set'){
        console.log('wakeup alarm set--' + result);
    } else {
        console.log('wakeup unhandled type (' + result.type + ')');
    }
};
```