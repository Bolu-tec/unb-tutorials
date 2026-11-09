# Control UNBdev.board motors (instructor preview)

## Read this before opening the motor blocks @showdialog

This preview introduces the integrated motor-driver API for the UNBdev.board. The extension contains motor blocks, but the motor workflow has **not been physically validated** on the board. Do not connect, power, or operate motors from this lesson. Keep external motors disconnected and use the code examples for reading and software preview only. The simulator does not model the integrated motor outputs, so a successful compile or tutorial preview does not show that a motor works.

A hands-on activity needs instructor approval and a verified setup for the exact board revision and controller firmware, motor type, wiring and polarity, external supply voltage and current limits, driver limits, stopping behavior, and fault response. USB power alone is not an approved motor supply. Do not download or run the motor examples on hardware until that setup has been reviewed and tested.

## Meet the two motor channels

The extension names the outputs **left** and **right**. A positive signed speed means forward, a negative speed means backward, and zero requests a brake. Speed is a percentage from **-100 to 100**; it is a driver command, not a promise of a particular physical speed. This example is for API preview only; do not run it on a physical board.

```blocks
UNBdevMotor.enable(UNBdevMotor.State.Enabled)
UNBdevMotor.setSpeed(UNBdevMotor.Motor.Left, 30)
UNBdevMotor.setSpeed(UNBdevMotor.Motor.Right, 30)
basic.pause(500)
UNBdevMotor.stopAll()
```

## Compare forward and backward commands

Change the sign to request the opposite direction. In the preview, compare the values and explain which channel receives each command. The simulator cannot confirm wheel direction or motor movement.

```blocks
UNBdevMotor.enable(UNBdevMotor.State.Enabled)
UNBdevMotor.setSpeed(UNBdevMotor.Motor.Left, 30)
UNBdevMotor.setSpeed(UNBdevMotor.Motor.Right, -30)
basic.pause(500)
UNBdevMotor.stopAll()
```

The extension also has an explicit direction block. Compare it with the signed-speed version; both examples request a direction and magnitude. A plain ``||UNBDev:run left motor forward at 25%||`` command continues until another command changes that channel.

```blocks
UNBdevMotor.enable(UNBdevMotor.State.Enabled)
UNBdevMotor.run(UNBdevMotor.Motor.Left, UNBdevMotor.Direction.Forward, 25)
basic.pause(300)
UNBdevMotor.stop(UNBdevMotor.Motor.Left)
UNBdevMotor.runFor(UNBdevMotor.Motor.Right, -25, 300)
```

## Use a timed command

``||UNBDev:set both motor speeds to 25%||`` requests the same signed speed on both channels until changed. ``||UNBDev:run both motors at 25% for 500 ms||`` requests that speed for the requested duration, then calls the ``||UNBDev:stop all motors||`` block. The duration is in milliseconds. These API behaviors are visible in the code, but they are not a physical safety certification; keep this example disconnected from motors.

```blocks
UNBdevMotor.enable(UNBdevMotor.State.Enabled)
UNBdevMotor.setBothSpeeds(25)
basic.pause(300)
UNBdevMotor.runBothFor(25, 500)
```

The extension also provides ``||UNBDev:run left motor at 25% for 300 ms||`` for one channel. Both timed blocks stop their requested motor output when the call completes. A timeout is not a substitute for an emergency stop or verified hardware fault handling.

## Practice an in-place turn

Opposite signed speeds request opposite directions on the two channels. Read this short example and identify which channel receives each signed value, then identify the stop command that follows. Do not connect motors or execute this code on hardware.

```blocks
UNBdevMotor.enable(UNBdevMotor.State.Enabled)
UNBdevMotor.setSpeed(UNBdevMotor.Motor.Left, -20)
UNBdevMotor.setSpeed(UNBdevMotor.Motor.Right, 20)
basic.pause(300)
UNBdevMotor.stopAll()
```

Actual turning direction depends on installation, wiring, and motor orientation, none of which this preview verifies.

## Distinguish stopping from disabling

``||UNBDev:stop all motors||`` requests a brake for both channels. ``||UNBDev:motor driver disabled||`` requests that the motor driver be disabled. These are separate commands. Review the order below; do not interpret the snippet as a tested shutdown sequence for an assembled vehicle.

```blocks
UNBdevMotor.stopAll()
UNBdevMotor.enable(UNBdevMotor.State.Disabled)
```

## Check your understanding @showdialog

Explain what positive, negative, and zero signed speeds request; how ``||UNBDev:run both motors at 25% for 500 ms||`` differs from ``||UNBDev:set both motor speeds to 25%||``; and how ``||UNBDev:stop all motors||`` differs from disabling the driver. Keep the motors disconnected. The code preview checks that the extension API can be referenced; it does not test the motor driver, power, wiring, direction, braking, or safe operation.

If an instructor later authorizes physical testing, use a separate reviewed procedure for the exact board and power setup. Record board revision, controller firmware, extension commit, motor and supply specifications, wiring, expected behavior, actual behavior, and how the system was stopped. Stop and disconnect power if anything overheats or behaves unexpectedly.

Click **Done** to keep your project.

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
