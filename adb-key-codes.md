# ADB key codes 

Remote control actions on Fire TV can be invoked over ADB with shell commands. All commands must be either run in an interactive `adb shell` session or prefixed with `adb shell` at the start of the command. Within Home Assistant, 'run command' prompts don't need the `adb shell` prefix.

`input keyevent [KEYCODE]` works with both the keyword-based keycodes and the numeric ones. The two actions are interchangeable, e.g. `input keyevent KEYCODE_POWER` and `input keyevent 26`.

For text entry, while you can do multiple `input keyevent KEYCODE_[LETTER/NUMBER/SPACE]` inputs, this can get bogged down by latency and potentially return out of order depending on your script configuration. `input text “[TEXT]”` is a available as a single command for writing out multiple characters faster.

## Buttons on my remote

### Without keycodes
The following remote control buttons are NOT handled by keyevent, and bypass Android to operate the TV controls more directly.
| Button        |
| ------------- | 
| INPUT         | 
| 0..9<br />_(channel number input)_ |
| TXT           |
| SUBT          |
| RED, GREEN, YELLOW, BLUE<br />_(Teletext/interactive buttons)_ |
| LIVE          |

### With keycodes
These buttons all have keycodes. In some cases, especially with media and volume controls, these keyevents are not the fastest ways to perform these operations over ADB, and can suffer significant latency especially when called repeatedly.
| Button        | ADB keycode                 |
| ------------- | ----------------------------- |
| POWER         | 26<br />`KEYCODE_POWER`              |
| ALEXA         | `KEYCODE_ALEXA`                 |
| SETTINGS      | 176<br />`KEYCODE_SETTINGS`          |
| UP            | `KEYCODE_DPAD_UP`               |
| LEFT          | `KEYCODE_DPAD_LEFT`             |
| RIGHT         | `KEYCODE_DPAD_RIGHT`            |
| DOWN          | `KEYCODE_DPAD_DOWN`             |
| SELECT        | `KEYCODE_DPAD_CENTER`           |
| BACK          | 4<br />`KEYCODE_BACK`                |
| HOME          | `KEYCODE_HOME`                  |
| LIST          | 82<br />`KEYCODE_MENU`               |
| VOL UP        | 24<br />`KEYCODE_VOLUME_UP`          |
| VOL DOWN      | 25<br />`KEYCODE_VOLUME_DOWN`        |
| TV            | 300<br />`KEYCODE_LIVE_TV`           |
| MUTE          | 164<br />`KEYCODE_VOLUME_MUTE`       |
| CH UP         | 166<br />`KEYCODE_CHANNEL_UP`        |
| CH DOWN       | 167<br />`KEYCODE_CHANNEL_DOWN`      |
| REWIND        | 89<br />`KEYCODE_MEDIA_REWIND`       |
| PLAYPAUSE     | 85<br />`KEYCODE_MEDIA_PLAY_PAUSE`   |
| FASTFORWARD   | 90<br />`KEYCODE_MEDIA_FAST_FORWARD` |
| PRIME         | 294<br />`KEYCODE_APP_2`             |
| NETFLIX       | 293<br />`KEYCODE_APP_1`             |
| APPS          | 299<br />`KEYCODE_APP_4`             |
| FREELY        | 295<br />`KEYCODE_APP_3`             |

## Buttons not on my remote but with accessible keycodes
These are keycodes which exist but for which a button does not exist on my remote. In some cases, especially with media and volume controls, these keyevents are not the fastest ways to perform these operations over ADB, and can suffer significant latency especially when called repeatedly.

### Media controls
| Button        | ADB keycode                 | Notes |
| ------------- | --------------------------- | ----- |
| PLAY          | `KEYCODE_MEDIA_PLAY`            |
| PAUSE         | `KEYCODE_MEDIA_PAUSE`           |
| STOP          | `KEYCODE_MEDIA_STOP`            |
| NEXT          | `KEYCODE_MEDIA_NEXT`            |
| PREVIOUS      | `KEYCODE_MEDIA_PREVIOUS`        |
| SKIP_FORWARD  | `KEYCODE_MEDIA_SKIP_FORWARD`    |
| SKIP_BACKWARD | `KEYCODE_MEDIA_SKIP_BACKWARD`   |
| STEP_FORWARD  | `KEYCODE_MEDIA_STEP_FORWARD`    |
| STEP_BACKWARD | `KEYCODE_MEDIA_STEP_BACKWARD`   |

### Text input
| Button        | ADB keycode                 | Notes |
| ------------- | --------------------------- | ----- |
| [LETTER] | `KEYCODE_`[LETTER] | (e.g. a: `KEYCODE_A`, etc.) |
| [DIGIT] | `KEYCODE_`[DIGIT] | (e.g. 1: `KEYCODE_1`, etc.) |
| SPACE         | `KEYCODE_SPACE`                 |
| ENTER         | `KEYCODE_ENTER`                 | (This usually works like 'select' rather than 'newline') |
| DELETE | `KEYCODE_DEL`                   | (Backspace) |

### Input selection
| Button        | ADB keycode                 | Notes |
| ------------- | --------------------------- | ----- |
| INPUT         | `KEYCODE_TV_INPUT`              |
| HDMI[n]       | `KEYCODE_TV_INPUT_HDMI_`[n] | _(`HDMI_1`, etc.)_     |
| COMPOSITE[n]  | `KEYCODE_TV_INPUT_COMPOSITE`[n] | _(`COMPOSITE_1`, etc.)_ |
| VGA[n]        | `KEYCODE_TV_INPUT_VGA`[n] | _(`VGA_1`, etc.)_       |

### Other
| Button        | ADB keycode                 |
| ------------- | --------------------------- |
| REFRESH       | `KEYCODE_REFRESH`               |