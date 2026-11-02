# Send messages with Wi-Fi and MQTT

## Meet the network @showdialog

Connect the integrated UNBdev.board to an approved lab Wi-Fi network, then exchange short text and number messages through an MQTT broker. A **broker** receives messages published to a named **topic** and forwards them to subscribers of that topic.

This is an **instructor preview: hardware and ESP32 firmware compatibility are unverified**. You need an integrated UNBdev.board with a micro:bit V2, a data-capable USB cable, and an instructor-approved power setup. Leave motors and optional modules disconnected. Do not change powered wiring; disconnect power if the board becomes hot.

Your instructor must provide a compatible lab SSID/password, a reachable lab broker hostname or IP address, and a unique team topic prefix. This extension uses **MQTT 3.1.1, anonymous access, unencrypted TCP port 1883 and QoS 0**. It does not support TLS, MQTT usernames/passwords or automatic reconnect. Do not weaken a school network or production broker to make it work. Use only an approved isolated lab setup and harmless practice messages.

The placeholders in this lesson are not working credentials. Real credentials entered into a project remain in its source and downloaded program. Never publish, commit, share or screenshot a project containing them. Use temporary lab credentials and replace them with placeholders before sharing.

## Find the blocks and firmware

Open **UNBDev / WiFi**; diagnostic blocks may be under its advanced or **More** section. Use the UNBdev.board blocks rather than another Wi-Fi extension. Add **Wi-Fi firmware version** inside **show string** in **on start**. Download the program to the integrated micro:bit using the editor's Download instructions. Record the scrolling ESP32 AT firmware identification with your instructor. It is different from the board controller firmware version.

```blocks
let firmware = UNBdevBoardWiFi.firmwareVersion()
if (firmware != "") {
    basic.showString(firmware)
} else {
    let wifiError = UNBdevBoardWiFi.lastError()
    basic.showString("FW ERR")
    basic.showNumber(wifiError)
}
```

An empty string means the query failed; record the error before calling another network block. The simulator cannot identify the real ESP32. A blank result or simulator display is not firmware evidence. Stop the hardware activity if firmware compatibility has not been approved.

## Join the lab Wi-Fi

Replace the previous program with this example. In your private lab copy only, replace `LAB_SSID` and `LAB_PASSWORD` with the values supplied by the instructor. Neither may be empty or contain a line break, double quote or backslash. Open networks with empty passwords and captive-portal or enterprise sign-in flows are not supported by this activity.

**Connect to Wi-Fi** returns true or false. If it succeeds, use **Wi-Fi is connected** to query status separately. A tick is expected only after both succeed. On failure, read **Wi-Fi last error** immediately and show its number. A later network operation may overwrite that error.

```blocks
if (UNBdevBoardWiFi.connectWiFi("LAB_SSID", "LAB_PASSWORD")) {
    if (UNBdevBoardWiFi.isConnected()) {
        basic.showIcon(IconNames.Yes)
    } else {
        let wifiError = UNBdevBoardWiFi.lastError()
        basic.showString("STATUS ERR")
        basic.showNumber(wifiError)
    }
} else {
    let wifiError = UNBdevBoardWiFi.lastError()
    basic.showString("WIFI ERR")
    basic.showNumber(wifiError)
}
```

A command can wait up to 30 seconds, and joining uses several commands. Wait for the result rather than repeatedly pressing reset. A successful Wi-Fi connection does not prove that MQTT is connected.

## Connect to the broker

Replace the previous program with this version. Replace `BROKER_HOST` with the instructor's hostname or IP address only: do not include `mqtt://`, a path or `:1883`. Confirm the broker is reachable from the board's network; `localhost` would refer to the board's network endpoint, not your laptop.

**MQTT connect to server** requests an anonymous clean session. A tick is expected after the extension receives a successful broker connection acknowledgement. If your broker requires TLS or credentials, stop and ask the instructor for a compatible lab setup.

```blocks
if (UNBdevBoardWiFi.connectWiFi("LAB_SSID", "LAB_PASSWORD")) {
    if (UNBdevBoardWiFi.connectMQTT("BROKER_HOST")) {
        basic.showIcon(IconNames.Yes)
    } else {
        let wifiError = UNBdevBoardWiFi.lastError()
        basic.showString("MQTT ERR")
        basic.showNumber(wifiError)
    }
} else {
    let wifiError = UNBdevBoardWiFi.lastError()
    basic.showString("WIFI ERR")
    basic.showNumber(wifiError)
}
```

## Publish and receive two kinds of message

Replace the previous program with the complete example below. Keep the network placeholders private as before. Replace **every** `team01` with the unique team name agreed with your instructor, including in the external MQTT client. Use short ASCII names and messages. Another team must not use your topics.

Add two **on MQTT received** blocks: one **text** topic and one **number** topic. In this example they are registered before connecting. Initial registration attempts to subscribe while disconnected; the later successful MQTT connection subscribes the registered topics again. Do not judge that initial registration error as a failed final connection, or register the same handler repeatedly in a loop.

When the tick appears, wait for the display to finish, then briefly press and release logical **A** to publish `hello` on your text topic. Use logical **B** to publish the number `23` on the number topic. If older printed A/B labels are reversed, confirm the logical buttons with the instructor. Wait for each result before the next press.

```blocks
let networkReady = false
UNBdevBoardWiFi.onMessage("unb/lab/team01/text", UNBdevBoardWiFi.DataType.Text, function (value) {
    basic.showString(value)
})
UNBdevBoardWiFi.onMessage("unb/lab/team01/number", UNBdevBoardWiFi.DataType.Number, function (value) {
    basic.showNumber(value)
})
input.onButtonPressed(Button.A, function () {
    if (networkReady) {
        if (!UNBdevBoardWiFi.publish("unb/lab/team01/text", "hello")) {
            let wifiError = UNBdevBoardWiFi.lastError()
            networkReady = false
            basic.showString("SEND ERR")
            basic.showNumber(wifiError)
        }
    }
})
input.onButtonPressed(Button.B, function () {
    if (networkReady) {
        if (!UNBdevBoardWiFi.publish("unb/lab/team01/number", 23)) {
            let wifiError = UNBdevBoardWiFi.lastError()
            networkReady = false
            basic.showString("SEND ERR")
            basic.showNumber(wifiError)
        }
    }
})
if (UNBdevBoardWiFi.connectWiFi("LAB_SSID", "LAB_PASSWORD")) {
    if (UNBdevBoardWiFi.connectMQTT("BROKER_HOST")) {
        networkReady = true
        basic.showIcon(IconNames.Yes)
    } else {
        let wifiError = UNBdevBoardWiFi.lastError()
        basic.showString("MQTT ERR")
        basic.showNumber(wifiError)
    }
} else {
    let wifiError = UNBdevBoardWiFi.lastError()
    basic.showString("WIFI ERR")
    basic.showNumber(wifiError)
}
```

Your instructor should subscribe an external MQTT client to both exact topics before you press the buttons. Expected observations are `hello` and `23` at the broker client. The board is also subscribed, so it may receive its own publications. That alone is not independent proof of broker delivery: confirm with the external client.

Next, have the external client publish `reply` on the text topic and `42` on the number topic, separately. The board should scroll `reply`, then show `42`. Do not send words on the number topic: numeric conversion can produce an invalid number. Exact topic spelling and case matter; this lesson does not use wildcard subscriptions or retained messages. Record missing or delayed messages rather than assuming success. QoS 0 provides no delivery acknowledgement for each publication.

## Disconnect deliberately

Save the exchange program privately. Add this **A+B pressed** handler to it. Briefly press and release A+B together after messages have finished. It first disables the program's send buttons, then requests Wi-Fi disconnection. `STOP` is expected only if that command returns true. Query **Wi-Fi is connected** afterward to check the result; a cross is expected on a successfully disconnected board.

```blocks
let networkReady = false
input.onButtonPressed(Button.AB, function () {
    networkReady = false
    if (UNBdevBoardWiFi.disconnect()) {
        basic.showString("STOP")
        if (UNBdevBoardWiFi.isConnected()) {
            basic.showString("STILL ON")
        } else {
            let wifiError = UNBdevBoardWiFi.lastError()
            if (wifiError == 3) {
                basic.showIcon(IconNames.No)
            } else {
                basic.showString("STATUS ERR")
                basic.showNumber(wifiError)
            }
        }
    } else {
        let wifiError = UNBdevBoardWiFi.lastError()
        basic.showString("STOP ERR")
        basic.showNumber(wifiError)
    }
})
```

Here error **3** means **NotConnected** at the pinned extension revision; other errors must not be mistaken for a confirmed disconnection. When adding this handler, reuse the existing `networkReady` variable; do not add another initialization to the exchange program. The standalone snippet initializes it only so that the hint is self-contained. `networkReady` is a program guard, not a live network-status measurement.

## Restart and recover

Save the stopped version. **Replace the entire program**, including all receive and button handlers, with this diagnostic program. **Restart Wi-Fi** requests an ESP32 restart; it does not factory-reset the module or erase saved credentials. `RESTART` means the command returned successfully, not that Wi-Fi or MQTT has reconnected.

```blocks
if (UNBdevBoardWiFi.restart()) {
    basic.showString("RESTART")
    let firmware = UNBdevBoardWiFi.firmwareVersion()
    if (firmware != "") {
        basic.showString(firmware)
    } else {
        let wifiError = UNBdevBoardWiFi.lastError()
        basic.showString("FW ERR")
        basic.showNumber(wifiError)
    }
} else {
    let wifiError = UNBdevBoardWiFi.lastError()
    basic.showString("RESTART ERR")
    basic.showNumber(wifiError)
}
```

To resume, reload the saved exchange program and deliberately connect again. Repeat the external-client checks for both topics. Successful recovery must be observed on the board and broker; simulator behavior is insufficient.

## Explain your results and finish @showdialog

You used all nine public Wi-Fi/MQTT blocks: firmware version, connect Wi-Fi, connection status, connect MQTT, publish, message received, last error, disconnect and restart. Explain why Wi-Fi status, broker connection and observed message delivery are three different checks.

If a step fails, capture the error immediately, then check the loaded program, extension revision, logical buttons, compatible firmware, approved network and broker, topic spelling and instructor-provided setup. Do not retry in a tight loop or change powered wiring. Keep topics and payloads short: this implementation only supports MQTT packet bodies smaller than 127 bytes. Stop after a repeated failure and report the actual observations.

Finish with the disconnect procedure, then remove real credentials before sharing or committing the project. Follow the instructor's process for removing private downloads and retiring temporary credentials; disconnect/restart does not erase credentials from saved project files or necessarily from the ESP32.

The simulator does not reproduce the integrated ESP32, network or broker. Compiler success checks API usage only. This preview claims no deployed-editor completion or physical validation. The instructor coverage matrix and evidence form are in `wifi-mqtt-validation.md` in the UNBDev Board Guide collection. Click **Done** to retain your project privately.

```package
unbdev-board=github:UNB-ECE/pxt-unbdev-board#22ed7d76838be66d456de13efc1604b1037dbefe
```
