Numpad driver for Android Things
================================

This driver supports numpad with 12 buttons.<br>
![numpad12](https://gitlab2.polidea.com/pawel.byszewski/android_things_drivers/raw/08fdbd12154cdfeb2b7c93fa0d8f3efa5edd5c13/numpad/readme/numpad.jpg)

How to use the driver
---------------------

### Gradle dependency

To use the `numpad` driver, simply add the line below to your project's `build.gradle`,
where `<version>` matches the last version of the driver available on TBD.

```
dependencies {
    TBD
}
```

### Connection
Every `GPIO` connected to row on a numpad MUST be [pulled-up](https://developer.android.com/things/hardware/hardware-101.html#pull-ups_and_pull-downs) to 3.3V


### Sample usage

```kotlin
import com.polidea.androidthings.driver.numpad.NumpadDriver

// Access the Button and listen for events:

val numpad = Numpad(c1GpioId = c1GpioPinName,
                    c2GpioId = c2GpioPinName,
                    c3GpioId = c3GpioPinName,
                    r1GpioId = r1GpioPinName,
                    r2GpioId = r2GpioPinName,
                    r3GpioId = r3GpioPinName,
                    r4GpioId = r4GpioPinName
        )
        .apply {
            keyListener = { keyEvent ->
                Log.d("keypad event", "${keyEvent.displayLabel}")
            }
        }
numpad.register()
// Close the Numpad when finished:

numpad.close()
```

Alternatively, you can register a `NumpadDriver` with the system and receive `KeyEvent`s
through the standard Android APIs:
```kotlin
val numpadInputDriver = NumpadDriver(c1GpioId = c1GpioPinName,
                                    c2GpioId = c2GpioPinName,
                                    c3GpioId = c3GpioPinName,
                                    r1GpioId = r1GpioPinName,
                                    r2GpioId = r2GpioPinName,
                                    r3GpioId = r3GpioPinName,
                                    r4GpioId = r4GpioPinName
)
numpadInputDriver.register()

// Override key event callbacks in your Activity:

override fun onKeyDown(keyCode: Int, event: KeyEvent): Boolean {
    Log.d("onKeyDown", "${event.displayLabel}")
    return true
}

override fun onKeyUp(keyCode: Int, event: KeyEvent?): Boolean {
    Log.d("onKeyUp", "${event?.displayLabel}")
    return true
}

// Unregister and close the input driver when finished:

numpadInputDriver.unregister()
```

The `keycode` could be one of:
```
KeyEvent.KEYCODE_1
KeyEvent.KEYCODE_2
KeyEvent.KEYCODE_3
KeyEvent.KEYCODE_4
KeyEvent.KEYCODE_5
KeyEvent.KEYCODE_6
KeyEvent.KEYCODE_7
KeyEvent.KEYCODE_8
KeyEvent.KEYCODE_9
KeyEvent.KEYCODE_0
KeyEvent.KEYCODE_STAR
KeyEvent.KEYCODE_POUND
```