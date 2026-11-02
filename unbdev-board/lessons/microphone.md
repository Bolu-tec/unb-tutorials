# Explore the UNBdev.board microphone

## Meet the board microphone @showdialog

Read sound levels, choose a threshold and react to sound using **UNBdev.board / Microphone**. These blocks use the microphone on the integrated board through its controller; they are different from the standard micro:bit **Input** microphone blocks.

This is an **instructor preview: physical microphone behavior is unverified**. Use an integrated UNBdev.board with a micro:bit V2-compatible target and a data-capable USB cable. Ask your instructor to identify the board/controller firmware and approve the power setup. Use normal speech or a gentle clap nearby: do not shout, put the board against your ear or touch powered parts. Leave external motors, Click boards and speakers disconnected. Stop and disconnect power if the board becomes hot.

You can complete the software preview without hardware. For instructor-led external board observations, save each program version and use the editor's **Download** instructions to load that version onto the integrated micro:bit before comparing results.

The simulator does not supply the integrated board microphone signal. Compiling or navigating this lesson does not prove that the real microphone works.

## Read a level

In **on start**, set a variable named **level** to **UNBdev.board sound level**. Add the microphone block with **enabled**, then show **level** on the micro:bit display. The first sound-level read initializes the microphone; explicit enable also restores the remembered threshold and refreshes its baseline. The number is a relative controller RMS reading, **not decibels**. No exact number is guaranteed.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
basic.showNumber(level)
```

## Watch changing sound

Add a **forever** loop to read **level**, show the number and pause **500 ms**. On real hardware, compare quiet surroundings with normal speech. Record what actually happens instead of expecting a fixed value. In the simulator, integrated microphone readings are unavailable; a displayed zero does not prove silence on a real board.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
basic.showNumber(level)
basic.forever(function () {
    level = UNBdevBoardMic.soundLevel()
    basic.showNumber(level)
    basic.pause(500)
})
```

## Refresh the baseline quietly

Add **on button A pressed** and **update UNBdev.board microphone baseline** inside it. Let the surroundings become quiet, then briefly press and release logical A. This asks the controller to recalculate its ambient reference; the firmware calibration duration and acoustic behavior are not established here. On older boards printed A/B labels may be reversed: confirm logical buttons with your instructor.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
basic.showNumber(level)
input.onButtonPressed(Button.A, function () {
    UNBdevBoardMic.updateBaseline()
})
basic.forever(function () {
    level = UNBdevBoardMic.soundLevel()
    basic.showNumber(level)
    basic.pause(500)
})
```

## Choose a threshold

In **on start**, after enable, add **set UNBdev.board sound threshold to 50**. This is a starting value to adjust with your instructor, not a universal loudness boundary. Valid values are **1–65535**; the API rounds and constrains them. Keep your quiet/louder observations for comparison. Changing the threshold is different from reading a sound level.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
UNBdevBoardMic.setThreshold(50)
basic.showNumber(level)
input.onButtonPressed(Button.A, function () {
    UNBdevBoardMic.updateBaseline()
})
basic.forever(function () {
    level = UNBdevBoardMic.soundLevel()
    basic.showNumber(level)
    basic.pause(500)
})
```

## Ask whether the threshold was reached

Save a copy of your level-reading program first. **Replace the entire forever loop** with the loop below. The question block reads the controller threshold flag: a reply of 1 is true. A tick is expected when the flag is set; a cross means it was not set when read. Firmware/hardware observations remain unverified. Do not add the loud-sound event yet: it uses and clears the same flag.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
UNBdevBoardMic.setThreshold(50)
basic.showNumber(level)
input.onButtonPressed(Button.A, function () {
    UNBdevBoardMic.updateBaseline()
})
basic.forever(function () {
    if (UNBdevBoardMic.thresholdReached()) {
        basic.showIcon(IconNames.Yes)
    } else {
        basic.showIcon(IconNames.No)
    }
    basic.pause(500)
})
```

## Acknowledge the flag

Inside the true branch, after showing the tick, pause **500 ms**, then **clear UNBdev.board sound threshold flag**. In quiet conditions the next read is expected to be false; continuing sound can immediately set it again. A repeated tick alone does not prove clear failed. Keep the brief display so you can see the indication. Save this polling version separately before the next step.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
UNBdevBoardMic.setThreshold(50)
basic.showNumber(level)
input.onButtonPressed(Button.A, function () {
    UNBdevBoardMic.updateBaseline()
})
basic.forever(function () {
    if (UNBdevBoardMic.thresholdReached()) {
        basic.showIcon(IconNames.Yes)
        basic.pause(500)
        UNBdevBoardMic.clearThresholdFlag()
    } else {
        basic.showIcon(IconNames.No)
    }
    basic.pause(500)
})
```

## Use an event instead of polling

**Delete the entire forever loop before adding this event.** Keep on start and button A. Add **on UNBdev.board loud sound** with a tick, a **200 ms** pause and **clear screen**. The event wrapper clears the threshold flag when registered and again after the handler finishes; no competing polling/reset loop is needed. One clap is not guaranteed to produce exactly one event. On the simulator, integrated microphone events are not supplied.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
UNBdevBoardMic.setThreshold(50)
basic.showNumber(level)
input.onButtonPressed(Button.A, function () {
    UNBdevBoardMic.updateBaseline()
})
UNBdevBoardMic.onLoudSound(function () {
    basic.showIcon(IconNames.Yes)
    basic.pause(200)
    basic.clearScreen()
})
```

## Finish with the microphone disabled

Add **on button B pressed**, set the microphone to **disabled**, then show **STOP**. Briefly press and release logical B on the board to request disable. This program already performed its first sound-level read during startup. Do not add further sound-level reads or new event registration after stopping: a first lazy initialization can otherwise enable the microphone. Reset/restart the program to try again. Disable behavior must be confirmed externally; the simulator only checks supported program flow.

```blocks
let level = UNBdevBoardMic.soundLevel()
UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Enabled)
UNBdevBoardMic.setThreshold(50)
basic.showNumber(level)
input.onButtonPressed(Button.A, function () {
    UNBdevBoardMic.updateBaseline()
})
UNBdevBoardMic.onLoudSound(function () {
    basic.showIcon(IconNames.Yes)
    basic.pause(200)
    basic.clearScreen()
})
input.onButtonPressed(Button.B, function () {
    UNBdevBoardMic.setEnabled(UNBdevBoardMic.State.Disabled)
    basic.showString("STOP")
})
```

## Check, explain and recover @showdialog

Save your event version. Explain the difference between a sound-level reading, a threshold, its flag and a loud-sound event. You used all seven microphone blocks, including enabled and disabled states.

Compilation checks API usage; the simulator checks supported display/control flow. For any externally performed board observation, record the lesson/extension/editor revision, board/controller firmware, threshold, setup, expected result and actual result separately. No hardware pass is claimed by this preview.

If nothing responds, check that you selected **UNBdev.board / Microphone**, that the polling loop was removed before adding events, and that you restarted after STOP. Confirm the loaded program, logical A/B buttons, threshold, cable, approved power and controller compatibility with your instructor. Do not raise sound volume aggressively or change powered wiring. Report unexpected results on the lesson's issue rather than treating simulator output as a board measurement.

Click **Done** to retain your project. The instructor coverage and validation checklist is maintained alongside this lesson in the UNBDev Board Guide repository.

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
