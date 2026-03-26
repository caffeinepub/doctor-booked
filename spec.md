# Doctor Booked

## Current State
- When a doctor clicks a token (red/yellow), it immediately marks it as ongoing via `regulateToken`
- The Booking interface has no `complaint` field
- BookingDialog does not ask for patient complaint/difficulty
- The token grid has a top-bar with "Complete #X" button separate from the grid

## Requested Changes (Diff)

### Add
- `complaint?: string` field to the `Booking` interface in `types.ts`
- A complaint/difficulty input step in `BookingDialog` (before final confirmation) asking "What is your difficulty or reason for visit?"
- A `TokenActionDialog` component in DoctorDashboard that opens when a token is clicked, showing:
  - Token number (e.g., Token #1)
  - Patient name
  - Patient complaint section (from booking data)
  - If status is red/yellow: single button "Mark as Ongoing"
  - If status is orange (already ongoing): two buttons — "Mark as Completed" (green) and "Patient Not Available (Skip)" (orange)

### Modify
- `handleTokenClick` — instead of directly calling `regulateToken`, open the dialog
- Token grid: orange tokens should also be clickable to open the dialog with ongoing-state actions
- Remove the separate "Complete #X" top bar button (actions are now inside the dialog)
- `addBooking` in store: store the complaint field

### Remove
- Direct immediate token state change on click (replaced by dialog flow)

## Implementation Plan
1. Add `complaint` to Booking type in `types.ts`
2. Add complaint input step in `BookingDialog.tsx` (after session/token selection, before payment)
3. In `DoctorDashboard.tsx`: add state for `tokenActionDialog`, modify `handleTokenClick` to open dialog for red/yellow/orange tokens, add dialog UI matching the screenshot style
4. Wire "Mark as Ongoing" to `regulateToken`, "Mark as Completed" to `completeCurrentToken`, "Skip" to mark token as unvisited/skipped
