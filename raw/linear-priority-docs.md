---
source_url: https://linear.app/docs/priority
fetched: 2026-04-27
note: Extracted summary of Linear's official priority documentation.
---

# Linear Priority Documentation

## Priority Levels

Linear supports four priority levels for issues:
- **Low**
- **Medium**
- **High**
- **Urgent**

(Plus "No priority" as the default unset state.)

## Design Rationale

Linear deliberately maintains a limited priority system. The documentation states: *"We don't have the option to set custom priorities or more granular priorities since it's easy to get carried away with specificity."*

The team recognizes that excessive options create friction in decision-making and diminishing returns. For teams needing additional differentiation, the recommended workarounds involve leveraging workflow statuses or labels instead.

## Setting Priority

Users can assign priority by selecting an issue and pressing `P`, then choosing the desired level. This keyboard shortcut also enables changing or removing priority assignments.

## Priority Ordering

When views are sorted by priority, users may use drag-and-drop to reorder issues and projects, indicating relative importance. These positions persist globally across the workspace. By default, items without an assigned priority appear last in sorted views.

## Urgent Priority Notifications

When an issue receives Urgent status, Linear automatically notifies the assigned person and sends an email alert if they have email notifications enabled.
