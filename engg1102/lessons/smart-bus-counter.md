# Smart Bus Capacity Counter

### @explicitHints true

## Welcome @showdialog

In this activity, you will build a **smart bus** that counts its passengers and closes its door when it's full.

Your bus will:

1. Start with its door **open** and the count at **0**.
2. Use an **ultrasonic sensor** at the doorway to count each LEGO person who gets on, **once each**.
3. Show the count on the micro:bit screen.
4. **Close the door** and turn on a **warning LED** when the bus is full (5 passengers).
5. Let one passenger **off** with a **tap** of the button, or **everyone** off with a **hold**.
6. **Reopen the door** and turn the LED off as soon as there's room again.

## Wire it up @showdialog

**Parts:** Development Board, breadboard, ultrasonic sensor, servo, LED, resistor, jumper wires.

Build the circuit from the **diagram provided with this activity**. The board's pins are on the **screw terminals** (or the matching gold **gator-clip contacts** beside them), labelled **GND, P0, P1, P2, P8, 3.3V**.

| Part | Where it connects | Pin in your code |
|:---|:---|:---|
| Servo | Plug into the **middle servo header (Servo 2)**: brown/black wire to **GND**, red to **+5V**, orange/yellow to **S** | **P1** |
| LED | **Long leg** through the resistor to the **P0** terminal, **short leg** to **GND** | **P0** |
| Ultrasonic sensor **Trig** | **P2** terminal | **P2** |
| Ultrasonic sensor **Echo** | **P8** terminal | **P8** |
| Ultrasonic sensor **VCC** | **3.3V** terminal | |
| Ultrasonic sensor **GND** | **GND** terminal | |

**Leave the P1 terminal empty.** The Servo 2 header and the P1 terminal are the same wire.

**Stay safe:**
- **Switch the board off and unplug it** while you wire, and ask your instructor to check your circuit before you power it again.
- Power the ultrasonic sensor from the **3.3V** terminal, not **+5V**. On 5 V, its **Echo** wire would send a 5 V signal into P8. The micro:bit pins are 3.3 V and a 5 V signal can damage them.
- Each pin can only supply a small current (15 mA), so the LED **must** have its resistor.

## How to use this tutorial @showdialog

- Blocks are grouped by colour into **drawers** in the toolbox. **Pins** and **Control** are under **Advanced**: click **Advanced** at the bottom of the toolbox to see them.
- ⚠️ **The A and B labels on the board are swapped.** The button your code calls **A** is the one printed **B** on the board.
- Stuck? Click the **lightbulb** 💡 to see what your code should look like.
- Click **Next** when you finish a step.

## Step 1: Make the passengers variable

The bus has to **remember** how many people are on it. A **variable** is a labelled box that holds a value.

1. Open ``||variables:Variables||`` and click **Make a Variable...**
2. Name it **passengers** and click **OK**.
3. Drag ``||variables:set passengers to 0||`` into ``||basic:on start||``.

#### ~ tutorialhint
After you make a variable, its **set** block appears in the ``||variables:Variables||`` drawer.

```blocks
let passengers = 0
passengers = 0
```

## Step 2: Start with the door open

The bus also has to remember whether its door is open. We use **1** for open and **0** for closed.

1. In ``||variables:Variables||``, click **Make a Variable...** and name it **doorOpen**.
2. Drag a ``||variables:set||`` block under **set passengers to 0**. Click its dropdown, choose **doorOpen**, and type **1**.
3. From ``||pins:Pins||``, add ``||pins:servo write pin P1 to 90||``. **90** degrees is our **open** door. Make sure the dropdown says **P1**.
4. From ``||basic:Basic||``, add ``||basic:show number||``. From ``||variables:Variables||``, drag ``||variables:passengers||`` onto its **0**.
5. From ``||pins:Pins||``, add ``||pins:digital write pin P0 to 0||``. **0** means the LED is **off**.

#### ~ tutorialhint
**on start** should hold **five** blocks.

```blocks
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
```

## Step 3: Send a sound pulse

The ultrasonic sensor works like a bat: it sends out a **sound pulse** too high for us to hear, then listens for the **echo**. We start a pulse by switching **Trig** on for 10 **microseconds** (millionths of a second).

1. From ``||basic:Basic||``, drag a ``||basic:forever||`` block onto the workspace.
2. Inside it, add these from ``||pins:Pins||`` and ``||control:Control||``, in order:
   - ``||pins:digital write pin P2 to 0||``
   - ``||control:wait (µs) 2||``
   - ``||pins:digital write pin P2 to 1||``
   - ``||control:wait (µs) 10||``
   - ``||pins:digital write pin P2 to 0||``

**Why forever?** The bus has to watch the door **all the time**, not just once.

#### ~ tutorialhint
**forever** should hold **five** blocks. The pin dropdown starts on **P0**, so change it to **P2** each time. The pattern is **0**, wait, **1**, wait, **0**.

```blocks
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
})
```

## Step 4: Measure the distance

Now the board works out how far away an object is. It times how long the echo takes to come back, then turns that time into **centimetres**.

1. In ``||variables:Variables||``, click **Make a Variable...** and name it **distance**.
2. At the bottom of **forever**, add a ``||variables:set||`` block. Click its dropdown and choose **distance**.
3. From ``||math:Math||``, drag the ``||math:0 / 0||`` block onto the **0** in **set distance to**.
4. From ``||pins:Pins||``, under **more**, drag ``||pins:pulse in (µs) pin P8 pulsed high||`` onto the **left 0** in **0 / 0**.
5. Type **58** into the **right 0**. It should now read **pulse in (µs) pin P8 pulsed high / 58**.

**Why 58?** Sound takes about **58 microseconds** to travel 1 cm and back, so dividing the time by 58 gives the distance in centimetres.

#### ~ tutorialhint
Check that **pulse in** uses pin **P8**, because Echo is wired there.

```blocks
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
})
```

## Step 5: Is someone in the doorway?

A LEGO person in the doorway will be **close** to the sensor. We'll say "close" means **less than 6 cm**.

1. From ``||logic:Logic||``, drag an ``||logic:if then||`` block under **set distance**.
2. From ``||logic:Logic||``, drag ``||logic:and||`` onto the **true** in the **if**.
3. From ``||logic:Logic||``, drag a ``||logic:0 < 0||`` block into **each** side of **and**.
4. From ``||variables:Variables||``, drag ``||variables:distance||`` onto the **left 0** of **both** comparisons.
5. In the **first** comparison, click **<** and choose **>**. Leave its right side as **0**.
6. In the **second** comparison, type **6** on the right.

It should now read **distance > 0 and distance < 6**.

**Why distance > 0?** If the sensor doesn't hear an echo, it reports **0**. Without this check, a missed echo would look like a person standing right at the sensor!

#### ~ tutorialhint
Build the check from the outside in: **and** first, then the two comparisons, then **distance** and the numbers.

```blocks
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {

    }
})
```

## Step 6: Spot a new person

The loop runs many times a second, and a passenger stays in the doorway for a while. If we added 1 **every** time the sensor saw them, one person could count as 20!

So we need a variable that remembers whether this person has already been counted: **0** means no, and **1** means yes.

1. In ``||variables:Variables||``, click **Make a Variable...** and name it **personAtDoor**.
2. **Inside** your if from Step 5 (someone is close), drag in a new ``||logic:if then||`` from ``||logic:Logic||``. It will ask, "Is this a new person, and is the door open?"
3. Drag ``||logic:and||`` onto its **true**, then drag a ``||logic:0 = 0||`` block into **each** side of **and**.
4. In the **first** comparison, drag ``||variables:personAtDoor||`` onto the left **0**. Leave the right side as **0**.
5. In the **second** comparison, drag ``||variables:doorOpen||`` onto the left **0**, and type **1** on the right. It should now read **personAtDoor = 0 and doorOpen = 1**.
6. **Below** your if from Step 5 (not inside it), add another ``||logic:if then||``. Drag a ``||logic:0 < 0||`` block onto its **true**, click **<** and choose **≥**, then make it read **distance ≥ 10**.
7. Inside this **if**, add a ``||variables:set||`` block. Choose **personAtDoor** and leave it at **0**, so the next person can be counted.

**Why 10 and not 6?** Sensor readings wobble a little. If we reset as soon as the reading passed 6, someone standing near 6 cm could be counted twice. Waiting for **10 cm** makes sure they have clearly moved away.

#### ~ tutorialhint
The **new person** if goes inside your if from Step 5. The **distance ≥ 10** if goes below it.

```blocks
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {

        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
})
```

## Step 7: Count the new person

Now say what happens when a new person arrives. These blocks all go **inside** the **if** you just built.

1. Add a ``||variables:set||`` block. Choose **personAtDoor** and type **1**, so this person isn't counted again.
2. Under it, add another ``||logic:if then||``. It will ask, "Is there room?"
3. Drag a ``||logic:0 < 0||`` block onto its **true**. Drag ``||variables:passengers||`` onto the left **0**, and type **5** on the right.
4. Inside this **if**, add ``||variables:change passengers by 1||``. Use the dropdown to choose **passengers** if needed.
5. Under it, add ``||basic:show number||``, and drag ``||variables:passengers||`` onto its **0**.

**What happens:** a person arrives and is counted, and **personAtDoor** becomes **1**. While they stay, nothing more is added. When they have clearly moved away, it goes back to **0**, ready for the next person.

#### ~ tutorialhint
There are now **three** ifs nested: someone close, then a new person, then room on the bus.

```blocks
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {
            personAtDoor = 1
            if (passengers < 5) {
                passengers += 1
                basic.showNumber(passengers)
            }
        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
})
```

## Step 8: Close the door when full

When the 5th passenger gets on, the bus is **full**: close the door and turn the LED **on**.

1. At the bottom of **forever**, **below** the **distance ≥ 10** if (not inside it), add a new ``||logic:if then||``.
2. Drag ``||logic:and||`` onto its **true**, then drag a comparison into **each** side of **and**.
3. Make the **left** one read **passengers ≥ 5**. Click **<** to choose **≥**.
4. Make the **right** one read **doorOpen = 1**.
5. Inside this **if**, add these in order:
   - ``||variables:set passengers to 5||``
   - From ``||pins:Pins||``, ``||pins:servo write pin P1 to 0||`` (door **closed**)
   - ``||variables:set doorOpen to 0||``
   - From ``||pins:Pins||``, ``||pins:digital write pin P0 to 1||`` (LED **on**)
6. Below this **if**, at the very bottom of **forever**, add ``||basic:pause (ms) 50||`` from ``||basic:Basic||``. This gives the sensor a short rest between pulses.

#### ~ tutorialhint
This new **if** sits **below** the **distance ≥ 10** if, and **pause** sits below that.

```blocks
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {
            personAtDoor = 1
            if (passengers < 5) {
                passengers += 1
                basic.showNumber(passengers)
            }
        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
    if (passengers >= 5 && doorOpen == 1) {
        passengers = 5
        pins.servoWritePin(AnalogPin.P1, 0)
        doorOpen = 0
        pins.digitalWritePin(DigitalPin.P0, 1)
    }
    basic.pause(50)
})
```

## Step 9: Notice the button going down

Passengers need to get off too. A **tap** of button A will let **one** person off, and a **hold** will let **everyone** off. To tell them apart, the program times how long the button stays down.

1. Make two new variables: **buttonWasDown** (**0** = up, **1** = down) and **buttonStart** (the time it was pressed).
2. At the very **top** of **forever**, above the first pin block, add a new ``||logic:if then||``.
3. Drag ``||logic:and||`` onto its **true**.
4. Into the **left** side of **and**, drag ``||input:button A is pressed||`` from ``||input:Input||``.
5. Into the **right** side, drag a ``||logic:0 = 0||`` block. Make it read **buttonWasDown = 0**.
6. Inside this **if**, add:
   - ``||variables:set buttonWasDown to 1||``
   - A ``||variables:set||`` block for **buttonStart**. From ``||input:Input||``, under **more**, drag ``||input:running time (ms)||`` onto its **0**.

**Why buttonWasDown?** It's the same trick as **personAtDoor**: we only want to react when the button **first** goes down, not on every loop while it's held.

#### ~ tutorialhint
Can't find **running time**? Click **more** under ``||input:Input||``.

```blocks
let buttonStart = 0
let buttonWasDown = 0
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    if (input.buttonIsPressed(Button.A) && buttonWasDown == 0) {
        buttonWasDown = 1
        buttonStart = input.runningTime()
    }
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {
            personAtDoor = 1
            if (passengers < 5) {
                passengers += 1
                basic.showNumber(passengers)
            }
        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
    if (passengers >= 5 && doorOpen == 1) {
        passengers = 5
        pins.servoWritePin(AnalogPin.P1, 0)
        doorOpen = 0
        pins.digitalWritePin(DigitalPin.P0, 1)
    }
    basic.pause(50)
})
```

## Step 10: Notice the button coming up

When the button comes back **up**, we can work out how long it was held.

1. Make a new variable called **heldFor**.
2. **Directly below** the if from Step 9, add a new ``||logic:if then||``.
3. Drag ``||logic:and||`` onto its **true**.
4. Into the **left** side of **and**, drag ``||logic:not||`` from ``||logic:Logic||``. Then drag ``||input:button A is pressed||`` into **not**.
5. Into the **right** side, drag a ``||logic:0 = 0||`` block. Make it read **buttonWasDown = 1**.
6. Inside this **if**, add ``||variables:set buttonWasDown to 0||``.
7. Under it, add a ``||variables:set||`` block for **heldFor**. From ``||math:Math||``, drag ``||math:0 - 0||`` onto its **0**. Put ``||input:running time (ms)||`` on the left and ``||variables:buttonStart||`` on the right.

We'll call this the **button released** if.

#### ~ tutorialhint
**heldFor** is the time now minus the time the button went down.

```blocks
let heldFor = 0
let buttonStart = 0
let buttonWasDown = 0
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    if (input.buttonIsPressed(Button.A) && buttonWasDown == 0) {
        buttonWasDown = 1
        buttonStart = input.runningTime()
    }
    if (!(input.buttonIsPressed(Button.A)) && buttonWasDown == 1) {
        buttonWasDown = 0
        heldFor = input.runningTime() - buttonStart
    }
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {
            personAtDoor = 1
            if (passengers < 5) {
                passengers += 1
                basic.showNumber(passengers)
            }
        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
    if (passengers >= 5 && doorOpen == 1) {
        passengers = 5
        pins.servoWritePin(AnalogPin.P1, 0)
        doorOpen = 0
        pins.digitalWritePin(DigitalPin.P0, 1)
    }
    basic.pause(50)
})
```

## Step 11: Tap or hold?

Still inside the **button released** if, under **set heldFor**:

1. Add a ``||logic:if then else||``. Drag a comparison onto its **true** and make it read **heldFor ≥ 1000** (1000 ms is 1 second).
2. In the **if** part (a hold), add ``||variables:set passengers to 0||``.
3. In the **else** part (a tap), add a new ``||logic:if then||`` and make its check read **passengers > 0**.
4. Inside that **if**, add ``||variables:change passengers by 1||``, choose **passengers**, and type **-1**.

**Why check passengers > 0?** Without it, tapping the button on an empty bus would give you **-1** passengers!

#### ~ tutorialhint
A **hold** empties the bus. A **tap** takes one person off.

```blocks
let heldFor = 0
let buttonStart = 0
let buttonWasDown = 0
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    if (input.buttonIsPressed(Button.A) && buttonWasDown == 0) {
        buttonWasDown = 1
        buttonStart = input.runningTime()
    }
    if (!(input.buttonIsPressed(Button.A)) && buttonWasDown == 1) {
        buttonWasDown = 0
        heldFor = input.runningTime() - buttonStart
        if (heldFor >= 1000) {
            passengers = 0
        } else {
            if (passengers > 0) {
                passengers += -1
            }
        }
    }
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {
            personAtDoor = 1
            if (passengers < 5) {
                passengers += 1
                basic.showNumber(passengers)
            }
        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
    if (passengers >= 5 && doorOpen == 1) {
        passengers = 5
        pins.servoWritePin(AnalogPin.P1, 0)
        doorOpen = 0
        pins.digitalWritePin(DigitalPin.P0, 1)
    }
    basic.pause(50)
})
```

## Step 12: Reopen the door

Once someone gets off, there's room again, so the door should open and the LED should go off.

Still inside the **button released** if, **below** the tap or hold if/else (not inside it):

1. Add a new ``||logic:if then||`` and make its check read **passengers < 5**.
2. Inside it, add these in order:
   - ``||pins:servo write pin P1 to 90||`` (door **open**)
   - ``||variables:set doorOpen to 1||``
   - ``||pins:digital write pin P0 to 0||`` (LED **off**)
3. **Below** that if, add ``||basic:clear screen||`` (under **more** in ``||basic:Basic||``).
4. Under it, add ``||basic:show number||``, and drag ``||variables:passengers||`` onto its **0**.

**Why turn the LED off here?** It doesn't turn off by itself. Without this block, the bus would still look full after the door reopens.

#### ~ tutorialhint
**Clear screen** and **show number** go **outside** the **passengers < 5** if, so the count updates every time.

```blocks
let heldFor = 0
let buttonStart = 0
let buttonWasDown = 0
let personAtDoor = 0
let distance = 0
let passengers = 0
passengers = 0
let doorOpen = 1
pins.servoWritePin(AnalogPin.P1, 90)
basic.showNumber(passengers)
pins.digitalWritePin(DigitalPin.P0, 0)
basic.forever(function () {
    if (input.buttonIsPressed(Button.A) && buttonWasDown == 0) {
        buttonWasDown = 1
        buttonStart = input.runningTime()
    }
    if (!(input.buttonIsPressed(Button.A)) && buttonWasDown == 1) {
        buttonWasDown = 0
        heldFor = input.runningTime() - buttonStart
        if (heldFor >= 1000) {
            passengers = 0
        } else {
            if (passengers > 0) {
                passengers += -1
            }
        }
        if (passengers < 5) {
            pins.servoWritePin(AnalogPin.P1, 90)
            doorOpen = 1
            pins.digitalWritePin(DigitalPin.P0, 0)
        }
        basic.clearScreen()
        basic.showNumber(passengers)
    }
    pins.digitalWritePin(DigitalPin.P2, 0)
    control.waitMicros(2)
    pins.digitalWritePin(DigitalPin.P2, 1)
    control.waitMicros(10)
    pins.digitalWritePin(DigitalPin.P2, 0)
    distance = pins.pulseIn(DigitalPin.P8, PulseValue.High) / 58
    if (distance > 0 && distance < 6) {
        if (personAtDoor == 0 && doorOpen == 1) {
            personAtDoor = 1
            if (passengers < 5) {
                passengers += 1
                basic.showNumber(passengers)
            }
        }
    }
    if (distance >= 10) {
        personAtDoor = 0
    }
    if (passengers >= 5 && doorOpen == 1) {
        passengers = 5
        pins.servoWritePin(AnalogPin.P1, 0)
        doorOpen = 0
        pins.digitalWritePin(DigitalPin.P0, 1)
    }
    basic.pause(50)
})
```

## Step 13: Try it out @showdialog

Your smart bus is finished! Time to test it.

1. Check your wiring against the diagram. Plug in the **power adapter**, connect the USB cable, switch the board **on**, and click **Download**.
2. At the start: door **open**, LED **off**, screen shows **0**.
3. Move a LEGO person through the doorway, past the sensor, **one at a time**. The count should go up by **one** each time.
4. After the **5th** person: the door **closes** and the LED turns **on**.
5. **Tap** button A (the button labelled **B** on the board): the count drops to **4**, the door **opens**, and the LED turns **off**.
6. Bring the count back up, then **hold** button A for more than a second and let go: the count goes back to **0**.

**Not working?**
- **Count jumps by more than 1?** Make sure **set personAtDoor to 0** is inside the **distance ≥ 10** if, and **set personAtDoor to 1** is inside the **new person** if.
- **Only counts one person?** The doorway must read **10 cm or more** when it's empty. Make sure the person moves well away from the sensor, and that the sensor faces open space.
- **Never counts?** Check the Trig and Echo wires and pins. Try moving the person closer, or change **6** to a bigger number.
- **Counts with nobody there?** Something else may be within 6 cm of the sensor. Point it across the doorway at open space.
- **Button does nothing?** Press the **other** button: the A and B labels on the board are swapped.
- **A tap gets missed?** The program checks the button once each time around the loop, so press it for a little longer.
- **LED stays on?** Check that **digital write pin P0 to 0** is inside the **passengers < 5** if from Step 12.
- **LED never lights?** Check it's the right way round (**long leg** toward P0), and that the pin dropdown says **P0**. Check its resistor and its GND wire.
- **Servo doesn't move?** The servo header gets its 5 V from the **power adapter**, not USB, so check the adapter is plugged in and the switch is on. Check the servo is in the **middle** header, the right way round (brown/black wire on **GND**).

## Step 14: Make One Change

Now that your bus works, make **one change** to the system.

Choose one:

- Play a **warning beep** when the door is about to close. *Hint: look in the ``||music:Music||`` drawer.*
- Change the bus **capacity** to a different number. Which blocks need to change?
- Make the screen a **seat display** that fills up as passengers get on. *Hint: look for the **plot bar graph** block in the ``||led:Led||`` drawer.*

Test your change with your bus.

```ghost
music.playTone(Note.C, music.beat(BeatFraction.Quarter))
led.plotBarGraph(0, 5)
```

## Completion

🎉 Well done! You built a device that uses a **sensor** to measure the world, **variables** to remember its **state**, and **decisions** to control a **servo** and an **LED**. Counting each passenger only once, by watching for when they **arrive**, is called **edge detection**. You used it again to time the button. Engineers use it everywhere, from turnstiles to factory conveyor belts.
