# ADB and Home Assistant Configuration for Fire TV apps

I went through the legwork of finding a bunch of Fire TV streaming apps' package names, intents and activities, and determining how best to access them.

This work was done over `adb` from both macOS and Home Assistant (Docker), for my 2024 **TCL 55PF650K** TV (the UK model). This should at the very least be the same for other `##PF650*` series TVs, though exact application availability and some button configuration obviously differs by model and territory.

This document compiles the information you need to know to make use of these discoveries. It is a work in progress, so not everything is fully expanded upon yet.

For the compilation list of discovered (working) strings see [**Fire TV Application Strings**](firetv-app-strings.md) (`firetv-app-strings.md`)

## Variables and configuration
In this documentation, there are only two variables one needs to worry about for launching applications:
- `$APP_PACKAGE` - the internal name of the application to be run
- `$ACTIVITY` - a string specified by an individual app for a specific part of the app to jump to on opening.

### `$APP_PACKAGE`
This can be discovered very easily for most apps within Developer Tools on FireOS. Some apps obfuscate this somewhat and still show a system application package when launched. There are other ways to get the package name for these applications from `adb shell`, though I won't expound on these here yet.

This is almost always in a 'reverse URL' format. These package names usually DON'T change over time, so you may find that some package names use extremely outdated brand names.
#### Examples:
- `uk.co.bbc.iplayer` - BBC iPlayer
- `air.ITVMobilePlayer` - ITVX (formerly ITV Hub, ITV Player)
- `com.cbs.ca` - Paramount+ (formerly CBS All Access)

### `$ACTIVITY`
This is harder to discover and cannot be discovered directly on FireOS at all as far as I am able to tell. It can sometimes be sniffed from `adb` and system logs, but the easiest way I have found to discover them is by decompiling the installed APK for the package and finding `<activity android:name="$ACTIVITY">` strings manually in the `AndroidManifest` file.

This is also in a similar 'reverse URL' format, though usually longer than `$APP_PACKAGE`. It also often includes the `$APP_PACKAGE` within the string, though this is not always true. Sometimes the string includes what looks like a *different* `$APP_PACKAGE` within it. This is fine, just something to be aware of when trying to find them.

#### Examples:
<!-- No hope of making this table look nice in the source file. -->
|`$APP_PACKAGE`<br />Application<br />*(former names)*|`$ACTIVITY`<br />*(line-broken for readability)*|Notes|
|-|-|-|
|`com.hbo.hbonow`<br />**HBO Max**<br />*(Max, HBO Now, HBO Go, etc.)*|`com.wbd.beam`<br />`.BeamActivity`|Uses a broader 'Warner Bros. Discovery' format for the `$ACTIVITY` string.|
|`com.discovery.discoplus`<br />**Discovery+**|`com.wbd.beam`<br />`.BeamActivity`|Uses the same `$ACTIVITY` format as HBO Max.|
|`com.mobileiq.demand5`<br />**5** (Channel 5)<br />*(My5, Demand 5)*|`com.channel5.my5.tv`<br />`.ui.splash.view.SplashActivity`|Uses a newer `$ACTIVITY` name than its `$APP_PACKAGE` name|


### `adb shell`
Finally, for all `adb` commands, they need to be run in `adb shell`. I won't be covering how to set up `adb` on your Fire TV and system currently, but once it is set up, you can either run `adb shell` alone to get an interactive shell (best for when you're doing a bunch of work inside `adb`), or prefix any individual command with `adb shell` to run simply that command in the shell.

When setting up a command to be run in Home Assistant, the end goal for me, you need to include the `adb shell` command explicitly in your scripts.

## Methods

There are two methods that can be used to open apps, depending on which options the app in question supports. These are `monkey` (simpler) and `am start` (more complex).

### Directly open app using `monkey`
Many apps can be opened directly from `adb` using the `monkey` command, launching into their main activity without any hassle.

The full `adb shell` command is:

```sh
monkey -p $APP_PACKAGE 1
```

Monkey is technically supposed to be a development tool designed for stress-testing apps by sending many events to them. The `1` at the end of the line means this command only sends a single event to the app and nothing more, which makes this a useful shortcut that doesn't requiring digging into package files to find alternative ways to launch.

#### Example (Apple TV)
```sh
monkey -p com.apple.atve.amazon.appletv 1
```

### Open specific activity using `am start`
For apps that don't open directly when called using `monkey`, a specific `$ACTIVITY` needs to be run instead.

The full `adb shell` command is:
```sh
am start -n $APP_PACKAGE/$ACTIVITY
```

There is some trial-and-error in finding the right `$ACTIVITY` string to load, and be warned that there is nothing stopping the app author from changing the `$ACTIVITY` strings they use in future updates (though I have not run into this issue in the nearly 16 months I have had this TV so it is unlikely to be a major problem).

#### Example (HBO Max)
```sh
am start -n com.hbo.hbonow/com.wbd.beam.BeamActivity
```