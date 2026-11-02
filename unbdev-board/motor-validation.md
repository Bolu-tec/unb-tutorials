# Motor preview coverage and physical readiness

Lesson: [Control UNBdev.board motors](lessons/motor-control.md). The lesson uses
extension revision `22ed7d76838be66d456de13efc1604b1037dbefe`, which contains the
motor API. This identifies the software reviewed; it is not evidence of physical
board or firmware compatibility. The lesson remains an instructor preview.

## API coverage

| API | Lesson concept | Software behavior |
| --- | --- | --- |
| `enable` | Driver enabled/disabled | Sends the requested motor-driver state. |
| `setSpeed` | Signed speed | Constrains the command to -100..100; negative requests backward, positive forward, and zero brakes. |
| `setBothSpeeds` | Same signed speed on both | Sends the same constrained speed to left and right. |
| `run` | Explicit direction and speed | Runs one channel with forward/backward direction and a 0..100 magnitude. |
| `runFor` | Timed single-channel run | Sets signed speed, waits, and requests brake for that channel. |
| `runBothFor` | Timed two-channel run | Sets both channels, waits, and requests brake for both. |
| `stop` | Brake one channel | Sends the brake command for the selected channel. |
| `stopAll` | Brake both channels | Sends a brake request to left and right. |

These are extension-level software contracts. They do not establish that the
integrated board accepts the commands or that a connected motor stops as expected.

## Physical readiness gate

Do not turn this preview into a hands-on motor activity until an instructor has
reviewed and recorded all of the following for the specific setup:

- Board revision and controller firmware version.
- Motor model, rated voltage, stall current, and mechanical load.
- Approved external supply voltage/current, polarity, and common-ground arrangement.
- Motor wiring and left/right orientation; no wiring changes while powered.
- Driver voltage/current/thermal limits and any protections.
- Actual forward/reverse behavior, braking behavior, driver disable behavior, and
  what happens on disconnect, reset, timeout, or communication failure.
- A tested emergency stop and fault response, including how power is disconnected.
- Exact editor, extension, lesson, and firmware revisions used in the test, with
  expected and actual observations.

USB power alone is not an approved motor supply. A timed software stop is not an
emergency stop. The MakeCode simulator and tutorial preview do not model the
integrated motor outputs. Until the readiness gate is complete, keep physical
motors disconnected and retain the lesson's **Hardware unverified** card label.
