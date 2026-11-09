# Pedestrian Traffic Light

## Welcome @showdialog

In this activity, you will turn the UNB Development Board into a **pedestrian crossing light**.

Your device will:

1. Start in its **normal state**: green light for cars, "don't walk" for people.
2. Wait for a pedestrian to press **button A**.
3. Cycle the lights: **yellow**, then **red** while people cross.
4. Return to the normal state on its own.

| Part | Job |
|:---|:---|
| Button A | **Input**: the pedestrian's "cross" button |
| BLiXels (colour lights) | **Output**: the traffic light for cars |
| micro:bit screen | **Output**: the walk / don't walk sign |

## How to use this tutorial @showdialog

- Blocks are grouped by colour into **drawers** in the toolbox, such as **Input**, **Basic**, and **Loops**. Click a drawer to open it.
- The BLiXel blocks are in ``||UNBDev:UNBdev.board||`` → **BLiXel**.
- ⚠️ **The A and B labels on the board are swapped.** The button your code calls **A** is the one printed **B** on the board.
- Stuck? Ask your instructor or TA.
- Click **Next** when you finish a step.

## Step 1: Set up the lights

Traffic lights should be easy to see but not blinding.

From ``||UNBDev:UNBdev.board||`` → **BLiXel**, drag these two blocks into ``||basic:on start||``:

1. ``||UNBDev:set UNBdev.board BLiXel brightness to 20%||``
2. ``||UNBDev:set all UNBdev.board BLiXels to black||``

**Why?** **on start** runs once, when the board powers on. **Black** means off, so this turns off any lights left on from an earlier program. To set the colour, drop the named colour block onto the circle and choose **black**.

## Step 2: The normal state

Normally, cars have a **green** light and people see **don't walk**.

1. From ``||UNBDev:UNBdev.board||`` → **BLiXel**, add ``||UNBDev:set UNBdev.board BLiXel 1 to||`` under the **black** block. Change the dropdown to **5**.
2. Drop the named colour block onto its **red circle** and choose **green**.
3. From ``||basic:Basic||``, add ``||basic:show icon||`` and choose the **X**. That's our "don't walk" sign.

We'll use BLiXel **5** for green, **3** for yellow, and **1** for red, just like the three lamps on a real traffic light.

## Step 3: Add the cross button

A pedestrian presses **button A** to ask to cross.

Open the ``||input:Input||`` drawer and drag ``||input:on button A pressed||`` onto the workspace. Keep it **separate** from **on start**.

Everything that happens after the button press goes inside this block.

## Step 4: Yellow means slow down

Cars need a warning before the light turns red. Inside **on button A pressed**, add:

1. ``||UNBDev:set all UNBdev.board BLiXels to black||``, to turn the green light off.
2. ``||UNBDev:set UNBdev.board BLiXel 3 to yellow||``.
3. ``||basic:pause (ms) 2000||`` from ``||basic:Basic||``.

**Why the pause?** Without it, the board would rush to the next light instantly. **2000 ms** is 2 seconds.

## Step 5: Red means stop, people walk

Now the cars stop and the pedestrian can cross. Under the pause, add:

1. ``||UNBDev:set all UNBdev.board BLiXels to black||``
2. ``||UNBDev:set UNBdev.board BLiXel 1 to red||``
3. ``||basic:show icon||`` with the **stick figure**. That's our "walk" sign.
4. ``||basic:pause (ms) 5000||``, so people have 5 seconds to cross.

## Step 6: Warn that time is up

Real crossings flash a warning before the walk time ends. Let's flash the **X** three times.

1. From ``||loops:Loops||``, drag ``||loops:repeat 4 times||`` under the 5000 ms pause. Change **4** to **3**.
2. Inside the repeat, add:
   - ``||basic:show icon||`` with the **X**
   - ``||basic:pause (ms) 300||``
   - ``||basic:clear screen||``
   - ``||basic:pause (ms) 300||``

**Why a loop?** Instead of copying the same four blocks three times, **repeat** runs them for you.

## Step 7: Return to normal

After the crossing, the light must go back to its **normal state** by itself.

At the very bottom of **on button A pressed**, **below** the repeat loop (not inside it), add the same three blocks that set up the normal state in Step 2:

1. ``||UNBDev:set all UNBdev.board BLiXels to black||``
2. ``||UNBDev:set UNBdev.board BLiXel 5 to green||``
3. ``||basic:show icon||`` with the **X**

**Tip:** Right-click a block and choose **Duplicate** to copy it.

## Step 8: Try it out @showdialog

Your traffic light is finished! Time to test it.

1. Plug in the board's **power adapter**, connect the USB cable, switch the board **on**, and click **Download**.
2. Check the **normal state**: BLiXel 5 is green and the screen shows an **X**.
3. Press **button A** (the button labelled **B** on the board). You should see:
   - **Yellow** for 2 seconds.
   - **Red** with the **stick figure** for 5 seconds.
   - The **X** flashing 3 times.
   - Back to **green** with an **X**.

**Not working?**
- **Button does nothing?** Press the **other** button: the A and B labels on the board are swapped.
- **No colour lights?** The simulator doesn't show BLiXels, so test on the real board.
- **Two lights on at once?** Make sure there's a ``||UNBDev:set all UNBdev.board BLiXels to black||`` before each new colour.
- **Doesn't go back to green?** The last three blocks must be **below** the repeat loop, not inside it.

## Step 9: Make One Change

Now that your traffic light works, make **one change** to the system.

Choose one:

- Add a **beep** during the walk time, like real crossings for people who can't see the light.
- Use **button B** as a second cross button on the other side of the road.
- Show a **countdown** of the seconds left to cross.

Test your change on the UNB Development Board.

```ghost
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Black))
UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Red))
input.onButtonPressed(Button.A, function () {
})
input.onButtonPressed(Button.B, function () {
})
for (let index = 0; index < 4; index++) {
}
basic.showIcon(IconNames.No)
basic.clearScreen()
basic.pause(100)
basic.showNumber(0)
music.playTone(Note.C, music.beat(BeatFraction.Quarter))
```

## Completion

🎉 Well done! You built a device with an **input** (button A), **outputs** (BLiXels and the screen), a **sequence** with **timing**, and a **loop**. It always returns to a known **normal state**, which is how real control systems stay safe.

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
