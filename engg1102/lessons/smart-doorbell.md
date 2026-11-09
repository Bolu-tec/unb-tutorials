# Smart Doorbell

## Welcome @showdialog

In this activity, you will turn the UNB Development Board into a **smart doorbell**.

When a visitor presses the bell, your device will:

1. **Instantly** show that the press was detected.
2. Sound a **ding-dong** on the buzzer.
3. Flash the **lights** so someone who can't hear the bell still knows.
4. Return to its **normal**, waiting state.

| Part | Job |
|:---|:---|
| Button A | **Input**: the doorbell button |
| Speaker (buzzer) | **Output**: the ding-dong |
| BLiXels and micro:bit screen | **Output**: the visual alert |

## How to use this tutorial @showdialog

- Blocks are grouped by colour into **drawers** in the toolbox, such as **Input**, **Music**, and **Loops**. Click a drawer to open it.
- The BLiXel blocks are in ``||UNBDev:UNBdev.board||`` → **BLiXel**.
- ⚠️ **The A and B labels on the board are swapped.** The button your code calls **A** is the one printed **B** on the board.
- Stuck? Ask your instructor or TA.
- Click **Next** when you finish a step.

## Step 1: The normal state

While nobody is at the door, the doorbell should be quiet and dark, with one small sign that it's switched on.

1. From ``||UNBDev:UNBdev.board||`` → **BLiXel**, drag ``||UNBDev:set UNBdev.board BLiXel brightness to 20%||`` and ``||UNBDev:set all UNBdev.board BLiXels to black||`` into ``||basic:on start||``.
2. From ``||basic:Basic||``, add ``||basic:show icon||`` and choose the **small diamond**. That's our "ready" sign.

**Black** means off. To set the colour, drop the named colour block onto the circle and choose **black**.

## Step 2: Add the doorbell button

The visitor presses **button A** to ring the bell.

Open the ``||input:Input||`` drawer and drag ``||input:on button A pressed||`` onto the workspace. Keep it **separate** from **on start**.

## Step 3: Show the press was detected

The visitor should know **right away** that the bell worked. The first thing inside the button block should be a clear, visible change.

Inside **on button A pressed**, add:

1. ``||basic:show icon||`` with the **house**.
2. ``||UNBDev:set all UNBdev.board BLiXels to blue||``.

**Why first?** Code runs from top to bottom. Putting the feedback first means the visitor sees it before anything else happens.

## Step 4: Ding-dong!

Now sound the buzzer. A doorbell plays a **high** note, then a **low** note.

From ``||music:Music||``, add two ``||music:play tone||`` blocks under the BLiXel block:

1. The first plays **High E** for **1/2** beat (the "ding").
2. The second plays **Middle C** for **1** beat (the "dong").

**Tip:** Click the note to pick it from the piano keyboard.

## Step 5: Flash the lights

A flashing light gets attention even if the sound is missed. Let's flash the BLiXels three times.

1. From ``||loops:Loops||``, drag ``||loops:repeat 4 times||`` under the second tone. Change **4** to **3**.
2. Inside the repeat, add:
   - ``||UNBDev:set all UNBdev.board BLiXels to black||``
   - ``||basic:pause (ms) 200||``
   - ``||UNBDev:set all UNBdev.board BLiXels to blue||``
   - ``||basic:pause (ms) 200||``

## Step 6: Return to normal

After the alert, the doorbell must go back to its **normal state** so it's ready for the next visitor.

At the very bottom of **on button A pressed**, **below** the repeat loop (not inside it), add:

1. ``||UNBDev:set all UNBdev.board BLiXels to black||``
2. ``||basic:show icon||`` with the **small diamond**

These are the same blocks you used for the normal state in Step 1.

## Step 7: Try it out @showdialog

Your doorbell is finished! Time to test it.

1. Plug in the board's **power adapter**, connect the USB cable, switch the board **on**, and click **Download**.
2. Check the **normal state**: BLiXels off, a small diamond on the screen.
3. Press **button A** (the button labelled **B** on the board). You should see and hear:
   - The **house** icon and **blue** lights straight away.
   - A **ding-dong** from the speaker.
   - The blue lights **flashing** 3 times.
   - Back to the **small diamond** with the lights off.

**Not working?**
- **No colour lights?** The simulator doesn't show BLiXels, so test on the real board.
- **Button does nothing?** Press the **other** button: the A and B labels on the board are swapped.
- **No sound?** The sound comes from the small speaker marked **Microbit Speaker**, next to the USB port. Check that it isn't covered.
- **Lights stay on?** The last two blocks must be **below** the repeat loop, not inside it.

## Step 8: Make One Change

Now that your doorbell works, make **one change** to the system.

Choose one:

- Play a **different tune**.
- Use **button B** as a back-door bell with a **different colour** and sound, so you know which door to answer.
- **Count** how many visitors have rung and show the total when you press **A+B**.

Test your change on the UNB Development Board.

```ghost
let visitors = 0
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Black))
UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Red))
input.onButtonPressed(Button.A, function () {
})
input.onButtonPressed(Button.B, function () {
})
input.onButtonPressed(Button.AB, function () {
})
for (let index = 0; index < 4; index++) {
}
visitors = 1
visitors += 1
basic.showIcon(IconNames.SmallDiamond)
basic.pause(100)
basic.showNumber(visitors)
music.playTone(Note.C, music.beat(BeatFraction.Half))
music.playMelody("C D E F G A B C5 ", 120)
```

## Completion

🎉 Well done! You built a device with an **input** (button A) and three **outputs**: sound, lights, and the screen. It gives **instant feedback**, uses a **loop** to flash, and always returns to a known **normal state**.

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
