# Debug Challenge 2: Tap or Hold

## Part C: Debug the System @showdialog

Engineers need to be able to figure out why a system is not behaving as expected.

The program in your workspace **runs**, but it contains **one fault**, so it doesn't behave the way it should. Your team will:

1. Read the **expected behaviour**.
2. **Run** the program and observe what actually happens.
3. **Compare** what happens with what was expected.
4. **Find and correct** the fault.
5. **Demonstrate** the corrected program to your instructor or TA.

You are not expected to solve this without support. Ask your instructor or TA if your team gets stuck.

## Expected behaviour @showdialog

**"The LED"** in this challenge means the **BLiXels**: the row of five colour lights between the two mikroBUS sockets, labelled **RGB LEDs** on the board. They turn on and off together.

- A **quick tap** of **Button B** should make the LED **blink once**.
- **Holding** Button B for **more than one second** should make the LED **stay on** for as long as it is held, then turn **off** the moment it is released.
- A quick tap should **never** leave the LED on.

⚠️ On the UNB Development Board, the **A** and **B** labels are swapped. The button your code calls **B** is the one printed **A** on the board.

The small button **between** A and B is **Reset**. Pressing it restarts the program, so don't use it while testing.

## Step 1: Run it and observe

**Don't change any code yet.** First, find out what the program actually does.

The simulator doesn't show BLiXels, so test this on the **board**: plug the **power adapter** into the barrel jack, connect the **micro USB** cable, and switch the board **on**. The red power light and yellow USB light should come on. Then click **Download**. If it only saves a file, click **...** next to it, choose **Connect device**, and download again. Once the program is on the board, try:

- **Tap** B quickly. Watch the LED for a few seconds afterwards.
- **Hold** B for about 3 seconds, then let go.
- Try a tap, then a hold, then a tap again.

## Step 2: Compare

Compare your notes with the expected behaviour:

- **Quick tap:** expected one blink, ending **off**. What did you see?
- **Hold, while held (after 1 s):** expected **on**. What did you see?
- **Hold, after release:** expected **off** straight away. What did you see?

Where don't they match? Write it on your worksheet next to **Fault:**.

## Step 3: Find the fault

Now read the code. Use what you observed to work out **which part** causes the difference.

**Questions to think about:**

- Follow a quick tap through the code, one block at a time. Which blocks run?
- What is the last thing that happens to the LED on that path? How is that different from a long hold?

## Step 4: Fix it and test again

Change the code to correct the fault. Then **test it again**, using every check from Step 2.

A fix only counts once you've shown it works. If something still doesn't match, go back to Step 3.

Write what you changed on your worksheet next to **How we fixed it:**.

## Check-off @showdialog

Show your corrected program to your instructor or TA. Be ready to briefly explain:

- what the program was **supposed** to do;
- what was **going wrong**; and
- what your team **changed** to fix it.

```template
let start = 0
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
basic.forever(function () {
    if (input.buttonIsPressed(Button.B)) {
        start = input.runningTime()
        while (input.buttonIsPressed(Button.B) && input.runningTime() - start < 1000) {
            basic.pause(10)
        }
        if (input.buttonIsPressed(Button.B)) {
            UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
            while (input.buttonIsPressed(Button.B)) {
                basic.pause(10)
            }
            UNBdevBLiXel.clear()
        } else {
            UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
            basic.pause(200)
        }
    }
    basic.pause(10)
})
```

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
