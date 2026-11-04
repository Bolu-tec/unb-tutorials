# UNBdev.board BLiXel light patterns

## Meet the five BLiXels @showdialog

BLiXels are the five colour lights integrated into UNBdev.board. You will make a pattern, move it, and display a value. Use a micro:bit V2-based integrated board and a data-capable USB cable. Ask your instructor to confirm the board revision, compatible controller firmware, and approved power setup before connecting it.

Keep brightness at 20% for this activity. Do not attach external LED strips, motors, or Click boards. Keep conductive objects away from the board; disconnect power and tell your instructor if it becomes hot. Physical behaviour is awaiting the board test; the simulator does not model these BLiXels.

## Start dim and clear

In **on start**, add **set UNBdev.board BLiXel brightness to 20%**, then **clear all UNBdev.board BLiXels**. Add **show number 0** from Basic. BLiXel blocks are under **UNBdev.board → BLiXel**. The micro:bit display is separate from the five BLiXels.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)
```

## Choose a named colour

Add **on button A pressed** from Input. Inside it, put **set all UNBdev.board BLiXels to** and insert the named-colour block with **blue**. On the board, pressing A should make all five BLiXels blue. The nested colour block supplies a number to the setting block.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})
```

## Make an off button

Add **on button B pressed** and put **clear all UNBdev.board BLiXels** inside it. Press B after A: all five should turn off. This gives you an easy off button between tests.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})

input.onButtonPressed(Button.B, function () {
    UNBdevBLiXel.clear()
})
```

## Build two colours

Add **on button A+B pressed**. Clear the lights, then set BLiXel **1** to RGB **255, 0, 0** (red), and BLiXel **2** to HSL **120, 99, 50** (green). Show number **1** and pause **2000 ms**. RGB mixes red/green/blue channels; HSL uses hue/saturation/luminosity. The dropdowns label pixels 1–5; code stores them as indices 0–4.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})

input.onButtonPressed(Button.B, function () {
    UNBdevBLiXel.clear()
})

input.onButtonPressed(Button.AB, function () {
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.rgb(255, 0, 0))
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.hsl(120, 99, 50))
    basic.showNumber(1)
    basic.pause(2000)
})
```

## Shift the pattern

After the pause in A+B, add **shift UNBdev.board BLiXels by 3**, show number **2**, and pause **2000 ms**. The red and green move three positions toward larger pixel numbers: positions 4 and 5. Newly exposed positions turn off; colours pushed beyond position 5 are lost.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})

input.onButtonPressed(Button.B, function () {
    UNBdevBLiXel.clear()
})

input.onButtonPressed(Button.AB, function () {
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.rgb(255, 0, 0))
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.hsl(120, 99, 50))
    basic.showNumber(1)
    basic.pause(2000)
    UNBdevBLiXel.shift(3)
    basic.showNumber(2)
    basic.pause(2000)
})
```

## Rotate with wrapping

Next add **rotate UNBdev.board BLiXels by 1**, show number **3**, and pause **2000 ms**. Red moves from position 4 to 5; green wraps from position 5 to 1. Unlike shift, rotate keeps colours that cross the end.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})

input.onButtonPressed(Button.B, function () {
    UNBdevBLiXel.clear()
})

input.onButtonPressed(Button.AB, function () {
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.rgb(255, 0, 0))
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.hsl(120, 99, 50))
    basic.showNumber(1)
    basic.pause(2000)
    UNBdevBLiXel.shift(3)
    basic.showNumber(2)
    basic.pause(2000)
    UNBdevBLiXel.rotate(1)
    basic.showNumber(3)
    basic.pause(2000)
})
```

## Display a value

Next set all BLiXels to the named colour **green**, then add **show UNBdev.board BLiXel bar graph of 50 with max 100**, show number **4**, and pause **2000 ms**. The default minimum is 0 (expand the block to see it). Half of five is 2.5, rounded to **three lit BLiXels**: positions 1–3. Positions 4–5 should be off. The graph uses the last colour selected by set-all.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})

input.onButtonPressed(Button.B, function () {
    UNBdevBLiXel.clear()
})

input.onButtonPressed(Button.AB, function () {
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.rgb(255, 0, 0))
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.hsl(120, 99, 50))
    basic.showNumber(1)
    basic.pause(2000)
    UNBdevBLiXel.shift(3)
    basic.showNumber(2)
    basic.pause(2000)
    UNBdevBLiXel.rotate(1)
    basic.showNumber(3)
    basic.pause(2000)
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Green))
    UNBdevBLiXel.showBarGraph(50, 100, 0)
    basic.showNumber(4)
    basic.pause(2000)
})
```

## End with lights off

Finish A+B with **clear all UNBdev.board BLiXels**, then **show number 0**. This is the complete program. Press A+B once and wait for 0 before pressing another button, so the tests do not overlap.

```blocks
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.showNumber(0)

input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
})

input.onButtonPressed(Button.B, function () {
    UNBdevBLiXel.clear()
})

input.onButtonPressed(Button.AB, function () {
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.rgb(255, 0, 0))
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.hsl(120, 99, 50))
    basic.showNumber(1)
    basic.pause(2000)
    UNBdevBLiXel.shift(3)
    basic.showNumber(2)
    basic.pause(2000)
    UNBdevBLiXel.rotate(1)
    basic.showNumber(3)
    basic.pause(2000)
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Green))
    UNBdevBLiXel.showBarGraph(50, 100, 0)
    basic.showNumber(4)
    basic.pause(2000)
    UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Black))
    basic.showNumber(0)
})
```

## Download and check @showdialog

Save your project, then download it to the integrated micro:bit using the editor's Download instructions. After restart, the micro:bit shows 0 and the five BLiXels should be off. Press A (five blue), then B (all off). Press A+B once: at 1 expect red at pixel 1 and green at 2; at 2 expect red at 4 and green at 5; at 3 expect green at 1 and red at 5; at 4 expect three green lights; at 0 expect all off. Record each actual result, board/controller firmware revision, and a photo or video. Ask your instructor to confirm the physical pixel order rather than assuming the drawing's orientation.

## Compare brightness and recover

With the lights off, change the on-start brightness from **20% to 40%**, download again, and press A. The five blue lights should look brighter. Press B, restore **20%**, and download once more. If a result differs, stop and record it. Check the cable, correct downloaded program, approved power, and controller firmware with your instructor. Wait until A+B finishes before pressing B; if needed disconnect power to stop immediately. Do not solve a failure by increasing brightness or rewiring a powered board.

## Finish and explain

Keep the completed project at 20% brightness. Explain why shift can lose a colour but rotate wraps it, why 50/100 lights three of five pixels, and how RGB, HSL, and named colours feed the same setting block. Compilation and the micro:bit simulator check the program flow; only your recorded board test verifies the BLiXels. Click **Done** to keep your project.

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
