# Doctor Booked

## Current State
The token regulator has these behaviors:
- When doctor marks a token as ongoing (regulateToken): current token → orange, next yellow token → orange automatically (auto-promotes next patient to ongoing)
- When doctor marks completed (completeCurrentToken): current → green, next yellow token → orange automatically (auto-promotes)
- When doctor skips (skipToken): current → 'unvisited', next yellow → orange automatically
- Skipped tokens ('unvisited') cannot be interacted with (dialog doesn't open for them)
- Token color for skipped: uses 'unvisited' status with no distinct visible color defined in the grid

## Requested Changes (Diff)

### Add
- `completeSkippedToken(sessionId, tokenNum)` store function: marks a specific skipped token as green without affecting queue flow
- Unique color for skipped ('unvisited') tokens: purple/indigo style distinct from all other statuses
- Dialog for skipped tokens: opens when clicking a skipped token, shows patient info, allows "Mark as Completed" if patient arrives later

### Modify
- `regulateToken` (mark as ongoing): when marking a token ongoing, set next red token to yellow (notification) but do NOT auto-promote it to orange
- `completeCurrentToken` (mark as completed): current → green, next yellow token stays yellow (notification only), does NOT auto-promote to orange
- `skipToken`: current → skipped (purple), next yellow stays yellow OR next red becomes yellow — same notification-only behavior
- `handleTokenClick` in DoctorDashboard: also open dialog for 'unvisited' (skipped) tokens
- Token dialog: add skipped state UI showing "Mark as Completed" option for skipped tokens
- Token color map: add purple color for 'unvisited' status

### Remove
- Auto-promotion of next patient to orange/ongoing in completeCurrentToken and skipToken

## Implementation Plan
1. Update `TokenStatus` type if needed (already has 'unvisited')
2. Modify `regulateToken` in useAppStore.ts: after setting current to orange, set next red to yellow only (no orange promotion)
3. Modify `completeCurrentToken`: after setting current to green, keep next yellow as yellow (just ensure it's yellow, don't promote to orange)
4. Modify `skipToken`: same — don't auto-promote next to orange
5. Add `completeSkippedToken(sessionId, tokenNum)` that sets a specific token to green
6. In DoctorDashboard: add 'unvisited' to clickable statuses, handle skipped dialog state, add "Mark as Completed" button for skipped tokens
7. Update token color map to give 'unvisited' a distinct purple/indigo color
