# Fire TV Application Strings

## Variables and configuration
In this documentation, there are only two variables one needs to worry about for launching applications:
- `$APP_PACKAGE` - the internal name of the application to be run
- `$ACTIVITY` - a string specified by an individual app for a specific part of the app to jump to on opening.

These are explained more fully in [**`README.md`**](README.md)

## Using these strings
Many apps can be opened directly from an `adb shell` using the `monkey` command, while others require launching a specific `$ACTIVITY` with `am start`:

```sh
# Directly launched package
monkey -p $APP_PACKAGE 1
# Launched from activity
am start -n $APP_PACKAGE/$ACTIVITY
```
For more detailed information, see [**`README.md`**](README.md)

## `$APP_PACKAGE` strings

### Usage
Many apps can be opened directly from an `adb shell` using the `monkey` command, while others require launching a specific `$ACTIVITY` with `am start`:

```sh
# Directly launched package
monkey -p $APP_PACKAGE 1
# Launched from activity
am start -n $APP_PACKAGE/$ACTIVITY
```
For more detailed information, see [**`README.md`**](README.md)

In the table of application strings, those which can be launched with `monkey` are marked '**Direct**'.

Those which REQUIRE `am start` are marked with '_Activity_' in the 'Method' column. If the string you need is marked with '_Activity_', consult the **`$ACTIVITY` strings** section below.

### Formatting

There are no line-breaks or whitespace characters within an `$APP_PACKAGE` string. Any text breaks you see are a result of rendering and not part of the strings themselves. They should be removed from your commands where applicable.

### `$APP_PACKAGE` strings table

<!-- ROW TEMPLATES:
|****|``|**Direct**||
|****|``|_Activity_||
-->

|Application|`$APP_PACKAGE`|Method|Notes|
|-|-|-|-|
|**2nd Try**|`ott.thetryguys`|**Direct**|by _The Try Guys_ (_2nd Try, LLC_)|
|**5** _(UK)_|`com.mobileiq.demand5`|_Activity_|**UK only**. Formerly _My5_ and _Demand 5_|
|**Apple TV**|`com.apple.atve.amazon.appletv`|**Direct**|formerly _Apple TV+_|
|**BBC iPlayer**|`uk.co.bbc.iplayer`|**Direct**|**UK only**. Requires TV licence to use legally|
|**BBC Sounds**|`uk.co.bbc.sounds`|**Direct**||
|**Channel 4** _(UK)_|`com.channel4.ondemand`|**Direct**|**UK only**. Formerly _All 4_ and _4oD_ (_4 on Demand_)|
|**Discovery+**|`com.discovery.discoplus`|_Activity_|Availability depends on territory. May or may not exist alongside _HBO Max_ app|
|**Disney+**|`com.disney.disneyplus`|**Direct**|including former _Hulu_ and _ESPN+_ content|
|**Dropout**|`com.collegehumor.chdropout`|**Direct**|also _Dropout TV_, formerly _CollegeHumor_|
|**HBO Max**|`com.hbo.hbonow`|_Activity_|Availability depends on territory. Formerly _Max_, _HBO Now_, _HBO Go_, among others. May or may not exist alongside _Discovery+_ app.|
|**ITVX**|`air.ITVMobilePlayer`|_Activity_|**UK only**. Formerly _ITV Hub_ and _ITV Player_. Includes some _Disney+_ content. _STV Player_ is a separate app.|
|**National Theatre at Home**|`com.ntathome`|**Direct**||
|**Nebula**|`tv.standard.nebula`|_Activity_||
|**Netflix**|`com.netflix.ninja`|**Direct**||
|**NOW**|`com.bskyb.nowtv.beta`|**Direct**|formerly _NOW TV_. Also provides _HBO Max_, _Sky Sports_, _Sky Cinema_ and _Hayu_ content.|
|**Paramount+**|`com.cbs.ca`|**Direct**|formerly _CBS All-Access_ in some territories|
|**Prime Video**|`com.amazon.firebat`|_Activity_|Built-in component of FireOS.|
|**U**|`uk.co.uktv.dave`|**Direct**|formerly _UKTV Play_. Provides the free-to-air UKTV channels. Paid ones are on _NOW_|
|**YouTube**|`com.amazon.firetv.youtube`|**Direct**|Appears to be an Amazon-provided built-in app rather than a Google-provided one|

## `$ACTIVITY` strings
### Usage
```sh
am start -n $APP_PACKAGE/$ACTIVITY
```
For more detailed information, see [**`README.md`**](README.md)

### Formatting
So far, `$ACTIVITY` strings are listed only for those apps that require them. Full information about these apps is in the previous table.

There are no line-breaks or whitespace characters within an `$APP_PACKAGE` or `$ACTIVITY` string. Any text breaks you see are a result of rendering and not part of the strings themselves. They should be removed from your commands where applicable.

Some `$ACTIVITY` strings begin with a dot (`.`). This is an integral part of the string and should not be removed.

### `$ACTIVITY` strings table

<!-- ROW TEMPLATES:
|****|``|``|
-->

|Application|`$APP_PACKAGE`|`$ACTIVITY`|
|-|-|-|
|**5** _(UK)_|`com.mobileiq.demand5`|`com.channel5.my5.tv.ui.splash.view.SplashActivity`|
|**Discovery+**|`com.discovery.discoplus`|`com.wbd.beam.BeamActivity`|
|**HBO Max**|`com.hbo.hbonow`|`com.wbd.beam.BeamActivity`|
|**ITVX**|`air.ITVMobilePlayer`|`air.ITVMobilePlayer.ITVActivity`|
|**Nebula**|`tv.standard.nebula`|`.tv.features.splash.view.activities.SplashActivity`|
|**Prime Video**|`com.amazon.firebat`|`com.amazon.firebatcore.deeplink.DeepLinkRoutingActivity`|