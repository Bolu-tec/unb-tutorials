# UNBDev Board Guide

Reusable instructor-preview activities for the integrated UNBdev.board.

- Publication reviewer: the UNB-ECE repository maintainer. Physical/student-readiness review remains required under [#140](https://github.com/UNB-ECE/unb-platform/issues/140).
- [BLiXel lesson](lessons/blixel.md) and [card image](static/blixel.svg) were copied without content changes from UNB-ECE/pxt-microbit revision `94b7bfe64ae6348f477f42d0a52ab97e8ea77a3b`, paths `docs/projects/unbdev-board/blixel.md` and `docs/static/tutorials/unbdev-blixel.svg`.
- [Motor control preview](lessons/motor-control.md) introduces the implemented motor API for code review. Physical motor operation is not validated or authorized by the lesson; see the [motor validation checklist](motor-validation.md).
- The lesson retains extension revision `22ed7d76838be66d456de13efc1604b1037dbefe`. Relocated catalog URLs use the editor's reserved `/unb-tutorials` namespace; hosting/editor integration is tracked in [#185](https://github.com/UNB-ECE/unb-platform/issues/185) and [#186](https://github.com/UNB-ECE/unb-platform/issues/186).

## Hardware warning

The BLiXel card remains **Hardware test pending** with an orange ribbon. This migration does not certify hardware or resolve [#171](https://github.com/UNB-ECE/unb-platform/issues/171): older boards have reported reversed printed A/B labels, the tested clear operation left the lights lit, and A+B needs a quick press and release. The maintainer reported showing colour black as a successful workaround. The declared TypeScript clear wrapper already sends Clear then Show; the underlying controller Clear command alone does not send Show. The exact tested extension/firmware and root cause still need confirmation.

The copied lesson is an instructor test procedure, not a corrected or physically validated student lesson. Consult the linked issues before testing. No lesson instructions or dependency pins were silently changed during migration.

Use the UNB IDE Tutorial Tool for Markdown preview; simulator/compiler evidence is separate from physical validation. Course-specific explanations and assessment belong in the course collection.

## Microphone instructor preview

[Explore the microphone](lessons/microphone.md) covers all seven public microphone
blocks at extension commit `c870228e696624519cee1c312c65b229270dfa95`.
[Coverage and validation](microphone-validation.md) separates software evidence from
external board observations. The card remains **Hardware unverified**.
Authoring/publication is governed by [unb-platform #141](https://github.com/UNB-ECE/unb-platform/issues/141);
no lesson is bundled into pxt-microbit.

## Motor control instructor preview

The [motor control lesson](lessons/motor-control.md) uses extension revision
`22ed7d76838be66d456de13efc1604b1037dbefe`. It is a software/API preview only.
Do not connect, power, or operate motors until the board, firmware, motor supply,
wiring, driver limits, stop behavior, and fault response have been reviewed and
physically validated. The [motor validation checklist](motor-validation.md) lists
the evidence needed before a hands-on lesson can be considered.
