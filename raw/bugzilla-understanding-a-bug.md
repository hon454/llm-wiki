---
source_url: https://bugzilla.readthedocs.io/en/latest/using/understanding.html
fetched: 2026-04-27
note: Extracted summary of Bugzilla 5.2+ "Understanding a Bug" documentation. Bugzilla is open-source (MPL 2.0); docs are CC-licensed.
---

# Understanding a Bug — Bugzilla Documentation

## Overview

The core of Bugzilla centers on displaying individual bugs with multiple fields. Field labels typically function as hyperlinks to context-sensitive help, and fields marked with an asterisk (*) may not appear in every installation.

## Field Descriptions

**Summary:** A single-sentence problem description displayed in the header next to the bug number.

**Status and Resolution:** Define the bug's current state, ranging from unconfirmed through fixed and verified by QA.

**Alias:** A unique short text identifier usable instead of the bug number.

**Product and Component:** Bugs are organized hierarchically, with products containing one or more components.

**Version:** Indicates which released product version(s) are affected by the bug.

**Hardware (Platform and OS):** Specify the computing environment where the bug occurred.

### Importance (Priority and Severity)

The **Priority** field enables bug prioritization by assignees or managers. Default values are **P1 to P5**.

The **Severity** field indicates problem severity: "from blocker ('application unusable') to trivial ('minor cosmetic issue')." This field also marks enhancement requests.

**Key Distinction:** Priority directs work allocation, while Severity measures impact scope.

### Other Fields

- **Target Milestone:** A future version targeting the bug fix (e.g., 4.4, 5.0, 6.0).
- **Assigned To:** The responsible person for fixing the bug.
- **QA Contact:** The quality assurance person responsible for the bug.
- **URL:** Associated URLs, if applicable.
- **Whiteboard:** Free-form text for notes and tags.
- **Keywords:** Administrator-defined tags (examples: crash, regression).
- **Personal Tags:** User-specific, non-global tags invisible to others.
- **Dependencies:** Records bugs this one depends on or blocks.
- **Reported / Modified:** Reporter name with timestamps.
- **CC List:** Additional recipients for bug change notifications.
- **Ignore Bug Mail:** Prevent future notifications from this bug.
- **See Also:** Related bugs across Bugzilla instances or external trackers.
- **Flags:** Status indicators showing bug/attachment states.
- **Time Tracking:** Hours tracking fields.
- **Attachments:** Associated files like test cases or patches.
- **Additional Comments:** Discussion area for contributors.

## Flags Section

Flags attach specific statuses (`+`, `-`, or `?`) to bugs or attachments, with meanings depending on flag names.

### Flag Values

- `?` = Status request pending
- `-` = Negative status
- `+` = Positive status
- Blank/unset = No opinion expressed

### Flag Requests

Requestable flags allow users to set `?` status, requesting another user respond. Specifically requestable flags include a username textbox; the requestee receives email notification. Non-specifically requestable flags are "asking the wind" — open requests anyone may answer.

### Attachment Flags

Used for specific attachment questions, commonly for code review requests before check-in. Display locations: attachment list, edit attachment screen, and Request Queue.

### Bug Flags

Set status on the bug itself. Only privileged users may set bug flags, not necessarily assignees or reporters.
