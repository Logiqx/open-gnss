## NMEA - Fix Mode

### Overview

Fix mode is included with the [GSA](../messages/gsa.md) sentence and is associated with the last fix.

The fix mode reports whether the fix was no good, sufficient for 2D, or sufficient for 3D.



### Proprietary Messages

In addition to the [GSA](../messages/gsa.md) message, positioning mode is associated with a number of vendor-specific messages:

| Message ID | Description                         |
| ---------- | ----------------------------------- |
| PGRMZ      | Garmin - Altitude                   |
| PJLTS      | Jackson Labs - Time and 3D velocity |
| PMGNST     | Magellan - Status                   |



### Values

There are three distinct fix modes in NMEA:

| Fix Mode | Description |
| ---- | ---- |
| 1 | Fix was no good |
| 2 | Fix was sufficient for 2D |
| 3 | Fix was sufficient for 3D |
