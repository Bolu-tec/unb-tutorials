# Rock Paper Scissors

## Welcome @showdialog

In this activity, you will turn the UNB Development Board into a Rock Paper Scissors player!

When you press **button A**, the board will:

1. Count **1–2–3** using its colour lights (called **BLiXels**).
2. Randomly pick **Rock**, **Paper**, or **Scissors**.
3. Show its choice on the micro:bit screen.

You don't need any coding experience. Each step adds just one or two blocks.

## How to use this tutorial @showdialog

- Blocks are grouped by colour into **drawers** in the toolbox, such as **Input**, **Basic**, and **Math**. Click a drawer to open it.
- **Drag** a block onto the workspace. Blocks that click together run from top to bottom.
- Stuck? Click the **lightbulb** 💡 to see what your code should look like.
- Click **Next** when you finish a step.

## Step 1: Start with a button

Every round of the game will start when you press **button A**.

Open the ``||input:Input||`` drawer and drag an ``||input:on button A pressed||`` block onto the workspace.

**Check:** You should see a block that says **on button A pressed** with an empty space inside it. Everything you add from now on goes **inside** this block.

```blocks
input.onButtonPressed(Button.A, function () {

})
```

## Step 2: Get the lights ready

Before the countdown, the lights should start **off** and at a **safe brightness**.

Open **UNBdev.board → BLiXel** and drag these two blocks **inside** **on button A pressed**:

1. **set UNBdev.board BLiXel brightness to 20%**
2. **clear all UNBdev.board BLiXels**

**Why?** 20% is bright enough to see and is the safe level for this activity. Clearing turns off any lights left over from the last round.

```blocks
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
})
```

## Step 3: Count "1"

Now light up the first BLiXel to count **1**.

1. From **UNBdev.board → BLiXel**, drag **set UNBdev.board BLiXel 1 to** and place it under **clear all UNBdev.board BLiXels**. Leave the dropdown on **1**.
2. The block has a **red circle** at the end: that's where its colour goes. From the same drawer, drag the named colour block and drop it **onto the red circle**, then choose **blue** in its dropdown.
3. From ``||basic:Basic||``, add ``||basic:pause (ms) 500||`` underneath.

**Why the pause?** The board runs code extremely fast. Without a pause, the countdown would finish before you could see it. **500 ms** means half a second (1000 ms = 1 second).

```blocks
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
})
```

## Step 4: Count "2" and "3"

Repeat Step 3 two more times so the board counts **1–2–3**.

**Tip:** Right-click the **set UNBdev.board BLiXel 1 to** block and choose **Duplicate** to make a copy (the blue colour comes with it). Do the same for the pause.

- On the second copy, click the **1 ▾** dropdown and change it to **2**. Follow it with a pause of **500 ms**.
- On the third copy, change the dropdown to **3**. Follow it with a pause of **500 ms**.

**Check:** Inside **on button A pressed** you should now have three BLiXel blocks (1, 2, 3), each followed by a pause.

```blocks
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
})
```

## Step 5: Make a variable

The board needs somewhere to **remember** what it picked. In coding, that "somewhere" is called a **variable**: a labelled box that holds a value.

1. Open the ``||variables:Variables||`` drawer and click **Make a Variable...**
2. Name it **choice** and click **OK**.
3. Drag ``||variables:set choice to 0||`` to the **bottom** of **on button A pressed**, after the last pause.

```blocks
let choice = 0
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    choice = 0
})
```

## Step 6: Pick a random number

Instead of always picking 0, let the board pick a **random** number.

1. Open the ``||math:Math||`` drawer and find ``||math:pick random 0 to 10||``.
2. Drag it into the **0** space in ``||variables:set choice to||``.
3. Change **10** to **2**.

Now **choice** will be **0**, **1**, or **2**, picked by chance each time. Each number stands for one hand:

| Number | Hand |
|---|---|
| 0 | Rock |
| 1 | Paper |
| 2 | Scissors |

```blocks
let choice = 0
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    choice = randint(0, 2)
})
```

## Step 7: Show Rock

Now the board needs to **decide** what to show. An **if** block runs its code only when something is true.

1. Open the ``||logic:Logic||`` drawer and drag an ``||logic:if then else||`` block under ``||variables:set choice to||``.
2. From ``||logic:Logic||``, drag the ``||logic:0 = 0||`` comparison block into the **true** space.
3. From ``||variables:Variables||``, drag ``||variables:choice||`` into the **first** 0. The block should read **choice = 0**.
4. From ``||basic:Basic||``, put ``||basic:show icon||`` inside the **if** part and choose the **small square**. That's our Rock.

```blocks
let choice = 0
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    choice = randint(0, 2)
    if (choice == 0) {
        basic.showIcon(IconNames.SmallSquare)
    } else {

    }
})
```

## Step 8: Show Paper

Next, add a second check for Paper.

1. Click the **+** at the bottom of the **if** block. This adds an **else if** row.
2. Make the **else if** check say **choice = 1**, the same way you made **choice = 0** in Step 7.
3. Put ``||basic:show icon||`` inside **else if** and choose the **big square**. That's our Paper.

```blocks
let choice = 0
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    choice = randint(0, 2)
    if (choice == 0) {
        basic.showIcon(IconNames.SmallSquare)
    } else if (choice == 1) {
        basic.showIcon(IconNames.Square)
    } else {

    }
})
```

## Step 9: Show Scissors

If **choice** isn't 0 and isn't 1, it must be **2**: Scissors! The **else** part handles "everything else", so it doesn't need a check.

Put ``||basic:show icon||`` inside the **else** part and choose the **scissors** icon.

```blocks
let choice = 0
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    choice = randint(0, 2)
    if (choice == 0) {
        basic.showIcon(IconNames.SmallSquare)
    } else if (choice == 1) {
        basic.showIcon(IconNames.Square)
    } else {
        basic.showIcon(IconNames.Scissors)
    }
})
```

## Step 10: Turn the lights off

Finally, turn off the countdown lights so the board is ready for the next round.

From **UNBdev.board → BLiXel**, drag **clear all UNBdev.board BLiXels** to the very **bottom** of **on button A pressed**, **below** the whole if block (not inside it).

```blocks
let choice = 0
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.setBrightness(20)
    UNBdevBLiXel.clear()
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.One, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Two, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    UNBdevBLiXel.setPixel(UNBdevBLiXelIndex.Three, UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    basic.pause(500)
    choice = randint(0, 2)
    if (choice == 0) {
        basic.showIcon(IconNames.SmallSquare)
    } else if (choice == 1) {
        basic.showIcon(IconNames.Square)
    } else {
        basic.showIcon(IconNames.Scissors)
    }
    UNBdevBLiXel.clear()
})
```

## Step 11: Try it out @showdialog

Your game is finished! Time to test it.

1. Connect the UNB Development Board with a USB cable and click **Download**.
2. Press **button A**. You should see:
   - BLiXels **1**, **2**, and **3** light up blue, one at a time.
   - A Rock, Paper, or Scissors icon on the screen.
3. Press **button A** several more times. The result should change randomly.

**Not working?**
- **No lights?** The simulator doesn't show BLiXels, so test on the real board.
- **Always the same icon?** Make sure **pick random** goes from **0** to **2**.
- **Nothing happens?** Make sure every block is **inside** **on button A pressed**.

## Completion

🎉 Well done! You used **events** (button A), **pauses**, a **variable**, **random numbers**, and **if/else decisions**. These are core ideas in all programming.

**Challenge:** Play against a friend: each of you presses button A, and the best hand wins. Or use a different colour for each count of the countdown!

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#9c599beeb48972ae5d9f8108f9af66b320add00b
```
