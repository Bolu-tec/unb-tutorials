# Debug Challenge 1: Toggle Counter

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

- Every time **Button A** is pressed, the LED should **toggle** on or off.
- After **four** presses, the LED should **stay off**, no matter how many more times Button A is pressed...
- ...until **Button B** is pressed to **reset** the count back to zero.

⚠️ On the UNB Development Board, the **A** and **B** labels are swapped. The button your code calls **A** is the one printed **B** on the board.

The small button **between** A and B is **Reset**. Pressing it restarts the program, so don't use it while testing.

## Step 1: Run it and observe

**Don't change any code yet.** First, find out what the program actually does.

The simulator doesn't show BLiXels, so test this on the **board**: plug the **power adapter** into the barrel jack, connect the **micro USB** cable, and switch the board **on**. The red power light and yellow USB light should come on. Then click **Download**. If it only saves a file, click **...** next to it, choose **Connect device**, and download again. Once the program is on the board, try:

- Press **A** once at a time, and count the presses. Write down whether the LED is **on** or **off** after each one.
- Keep pressing **A** well past four presses. What does the LED do?
- Press **B**, then press **A** a few more times.

## Step 2: Compare

Compare your notes with the expected behaviour:

- **After presses 1, 2, 3, 4:** expected on, off, on, off. What did you see?
- **After more presses:** expected to stay **off**. What did you see?
- **After pressing B:** expected the count to reset and A to toggle again. What did you see?

Where is the **first** place they don't match? Write it on your worksheet next to **Fault:**.

## Step 3: Find the fault

Now read the code. Use what you observed to work out **which block** causes the difference.

**Questions to think about:**

- Think about **presses**. What is its value just before each press of A?
- At which press should the program stop responding? Does the code agree?

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
let ledOn = false
let presses = 0
UNBdevBLiXel.setBrightness(20)
UNBdevBLiXel.clear()
input.onButtonPressed(Button.A, function () {
    if (presses <= 4) {
        presses += 1
        ledOn = !(ledOn)
        if (ledOn) {
            UNBdevBLiXel.setAll(UNBdevBLiXel.colour(UNBdevBLiXelColour.Blue))
        } else {
            UNBdevBLiXel.clear()
        }
    }
})
input.onButtonPressed(Button.B, function () {
    presses = 0
})
```

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
