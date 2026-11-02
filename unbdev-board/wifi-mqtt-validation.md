# Wi-Fi and MQTT tutorial coverage and validation

Governing issue: [unb-platform #143](https://github.com/UNB-ECE/unb-platform/issues/143).
Lesson: [Send messages with Wi-Fi and MQTT](lessons/wifi-mqtt.md).
Status: **instructor preview; local compilation and blocks-XML decompilation checked; deployed-editor completion, visual block rendering and physical validation pending**.

## Declared source baseline

- Extension: `UNB-ECE/pxt-unbdev-board` commit `22ed7d76838be66d456de13efc1604b1037dbefe` (v0.3.0), matching this collection's manifest and lesson package fence.
- Inspected implementation: [`wifi.ts`](https://github.com/UNB-ECE/pxt-unbdev-board/blob/22ed7d76838be66d456de13efc1604b1037dbefe/wifi.ts) and [`docs/wifi-mqtt.md`](https://github.com/UNB-ECE/pxt-unbdev-board/blob/22ed7d76838be66d456de13efc1604b1037dbefe/docs/wifi-mqtt.md).
- Target family: micro:bit V2 only. The collection declares target version 9.1.1; this is not proof of the currently deployed build.
- Intended editor: `https://d200xhrnashxy5.cloudfront.net`. Record its actual release/build and target/framework revisions when tested; currently **not measured**.
- Controller firmware, board revision and ESP32 AT firmware: **not supplied or tested for this lesson**. An extension document's controller firmware reference does not establish ESP32 compatibility.

## Public block coverage

| Block / API | Lesson step | Expected observation on approved hardware |
| --- | --- | --- |
| `firmwareVersion` | Find the blocks and firmware; Restart and recover | Nonempty AT firmware identification; record exact text separately from controller version |
| `connectWiFi` | Join the lab Wi-Fi | Correct temporary lab credentials connect; failure returns an immediate error without an infinite application loop |
| `isConnected` | Join the lab Wi-Fi; Disconnect deliberately | True after association; false with NotConnected after successful disconnection, not a malformed-response/timeout false |
| `connectMQTT` | Connect to the broker; Publish and receive | Successful CONNACK and SUBACK for registered handlers before ready indication |
| `publish` | Publish and receive | Text `hello` and number `23` observed by an independent broker client |
| `onMessage` | Publish and receive | Exact text and numeric topics display external `reply` and `42` respectively |
| `lastError` | Failure branches throughout | Error captured before any later network call overwrites it |
| `disconnect` | Disconnect deliberately | Send guard off, disconnect command succeeds, subsequent status confirms disconnection |
| `restart` | Restart and recover | Restart acknowledgement and firmware query; explicit reconnection restores both topics |

All nine student-facing function blocks and both DataType choices are covered;
no public block is intentionally excluded. `buildConnectPacket`,
`buildPublishPacket` and `buildSubscribePacket` are explicitly blockHidden
packet helpers, outside the student-facing block scope. ErrorCode values used
for diagnosis are None=0, Timeout=1, Rejected=2, NotConnected=3,
InvalidArgument=4, MalformedResponse=5 and PacketTooLarge=6 at this pin.
Read them immediately: subsequent successful commands can reset the error.

## Instructor preparation

1. Approve board revision, micro:bit V2, controller firmware, ESP32 AT firmware,
   cable, power and BuiltIn UART compatibility. Leave actuators/modules disconnected.
2. Supply a compatible isolated lab access point with temporary credentials.
   Record SSID alias/security mode and network topology privately, never passwords.
   Empty passwords, captive portals and enterprise sign-in are not this API's workflow.
3. Supply an approved anonymous MQTT 3.1.1 broker, reachable TCP port 1883,
   broker software/version and access/firewall configuration. No TLS or MQTT
   credential fields exist in this API; do not expose a production broker.
4. On an instructor client connect to the broker, subscribe to the two exact team
   topics, and prepare separate publishes of `reply` and `42`, QoS 0, retain off.
   If using Mosquitto tools, substitute the approved host/team in these examples:

   ```sh
   mosquitto_sub -h BROKER_HOST -p 1883 -t unb/lab/team01/text -t unb/lab/team01/number -v
   mosquitto_pub -h BROKER_HOST -p 1883 -t unb/lab/team01/text -m reply
   mosquitto_pub -h BROKER_HOST -p 1883 -t unb/lab/team01/number -m 42
   ```

5. Review temporary-credential handling, per-team topics, acceptable content,
   logical A/B controls, serial ownership and expected command delays.
   Keep program credentials out of screenshots, logs, shared downloads and commits.

## Software checks and editor acceptance

Local authoring checks on 2026-11-02 used the pinned extension sources, micro:bit
target package **9.1.1**, PXT core reporting **13.0.1**, and framework checkout
`1723fbb84e49df29408ff0d0bf2869cbf5f00e25`. These identify local tools, not the
deployed editor. All six complete blocks fences passed
`pxt build --hwvariant mbcodal --ignoreTests` in an isolated project; the combined
exchange program plus A+B disconnect handler also compiled. In a separate learner
project with the pinned extension as a file dependency, all six fences
decompiled using `pxtc.decompile` to blocks XML with **zero**
`typescript_statement` or `typescript_expression` grey blocks. The disconnected
error comparison uses numeric code 3 because this pin's ErrorCode enum is not a
student enum block. These checks do not execute board networking or verify
visual rendering in the editor.

Both repository checks passed: `node scripts/validate.mjs` and
`node scripts/selftest.mjs` (all eight fixtures). Re-run them on any revision.

- Run `node scripts/validate.mjs` and `node scripts/selftest.mjs` in this repository.
  They validate content paths/shape, not compilation or device behavior.
- Recompile and decompile every complete blocks fence against the exact extension
  pin and declared target. Check the callback's `any` value and both enum choices
  render into the intended draggable blocks. Record actual diagnostics.
- Open the branch lesson with the UNB IDE Tutorial Tool. Navigate all steps and
  inspect snippets, toolbox, package loading, A+B addition and full-program
  replacement. Confirm Done retains the project. Record the content commit and
  actual editor release/target/framework revisions; do not infer these from main.
- The A+B hint is standalone for snippet checking. When combining it with the
  exchange program, retain only one initialization of `networkReady`.
- After a separately approved merge, compare the served lesson with the merged
  commit, then repeat deployed-editor navigation. A branch preview is not proof
  that the lesson has been published or completed in the deployed editor.

## Physical evidence procedure (not executed)

For each row record setup, expected result, actual result, timestamp/operator,
pass/fail/blocked and an evidence reference. The expectation is a source contract,
not a claim that the installed firmware satisfies it.

| ID | Procedure | Expected result | Actual / verdict |
| --- | --- | --- | --- |
| W1 | Load firmware-query program | Nonempty ESP32 identification; record exact value | NOT RUN |
| W2 | Join approved Wi-Fi and query status | Connect true; separate isConnected true | NOT RUN |
| W3 | In an instructor-controlled copy, use an incorrect temporary password | False with captured error/rejection or timeout; application eventually returns | NOT RUN |
| M1 | Join Wi-Fi, connect to approved broker | Broker acknowledgement; tick; broker client sees connection | NOT RUN |
| M2 | External client subscribes; press A then B separately | External client receives exact topics with `hello` and `23` | NOT RUN |
| M3 | External client publishes `reply`, then `42` on the two topics | Matching text and number handlers update the board display | NOT RUN |
| M4 | Instructor stops/disconnects lab broker, then restores it | Record failure detection and elapsed time; reload exchange program for explicit reconnect and resubscription; repeat M2/M3 | NOT RUN |
| W4 | Press/release A+B in exchange program plus stop handler | Send guard disabled; STOP and status NotConnected; further buttons send nothing | NOT RUN |
| W5 | Replace program with restart diagnostic; reload exchange program | Firmware query completes; explicit reconnect restores both topics | NOT RUN |

Do not simulate malformed/fragmented packets against shared infrastructure in a
student activity. The extension's broader parser/transport acceptance matrix
belongs to its maintainers; this lesson does not certify every protocol fault.
If failures occur, preserve sanitized evidence and link a defect rather than
changing expected results to match observed behavior. No retained messages,
wildcards, QoS 1/2, TLS or automatic reconnect are tested or promised.

### Per-run record

```text
Date/time and operator:
Content repository commit and saved program revision:
Editor release URL/build, target revision, framework revision:
Extension commit: 22ed7d76838be66d456de13efc1604b1037dbefe
Board revision / micro:bit revision:
Controller firmware revision:
ESP32 AT firmware identity (W1):
Approved power/cable setup:
Network alias/security mode/topology (no password):
Broker host alias/software/version/port/client version:
Team topics and harmless payloads:
Test ID and exact procedure:
Expected result:
Actual result and elapsed time:
Verdict: PASS / FAIL / BLOCKED
Sanitized evidence reference and defect links:
Student clarity/safety/compatibility reviewer and outcome:
```

## Acceptance status

Physical evidence is unavailable; the hardware warning stays. This content PR
does not close #143 or revise its acceptance criteria. #143 remains the owner of
the deployed-editor, instructor review and physical evidence gates. If the team
chooses to defer physical testing formally, it must revise #143 and link an
integration-validation issue before claiming completion; no such revision is
claimed by this authoring change.
