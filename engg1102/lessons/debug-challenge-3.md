# Debug Challenge 3: A Then B

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

- The LED should only turn **on** if **Button A** is pressed **first** and **Button B** is pressed **within two seconds** afterward.
- If Button B is pressed **first**, or if **more than two seconds** pass between the two presses, the LED should **stay off**.

⚠️ On the UNB Development Board, the **A** and **B** labels are swapped. The button your code calls **A** is the one printed **B** on the board, and the other way round.

The small button **between** A and B is **Reset**. Pressing it restarts the program, so don't use it while testing.

## Step 1: Run it and observe

**Don't change any code yet.** First, find out what the program actually does.

The simulator doesn't show BLiXels, so test this on the **board**: plug the **power adapter** into the barrel jack, connect the **micro USB** cable, and switch the board **on**. The red power light and yellow USB light should come on. Then click **Download**. If it only saves a file, click **...** next to it, choose **Connect device**, and download again. Once the program is on the board, try each test, and press **A** on its own between tests to start fresh:

1. Press **A**, then **B** about **1 second** later.
2. Press **A**, then wait about **4 seconds** before pressing **B**.
3. Press **B** first, then **A**.

## Step 2: Compare

Compare your notes with the expected behaviour:

- **A, then B within 2 s:** expected **on**. What did you see?
- **A, then B after more than 2 s:** expected **off**. What did you see?
- **B first:** expected **off**. What did you see?

Where don't they match? Write it on your worksheet next to **Fault:**.

## Step 3: Find the fault

Now read the code. Use what you observed to work out **which block** causes the difference.

**Questions to think about:**

- Imagine B is pressed 1 second after A, then imagine it's 4 seconds. What does the time calculation give each time?
- For each one, is the check true or false? Is that what you want?

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
let aPressedAt = 0
let aWasPressed = false
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
input.onButtonPressed(Button.A, function () {
    UNBdevBLiXel.clear()
    aWasPressed = true
    aPressedAt = input.runningTime()
})
input.onButtonPressed(Button.B, function () {
    if (aWasPressed && input.runningTime() - aPressedAt > 2000) {
        UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
    }
    aWasPressed = false
})
```

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
