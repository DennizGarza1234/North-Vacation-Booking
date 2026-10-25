# State Transition Test Cases — North Vacation Booking

## 1. Purpose

These test cases verify that North Vacation Booking correctly handles reservation state changes caused by payment, cancellation, check-in, and checkout events. They also verify that invalid transitions raise an `InvalidStateError`.

## 2. State Transition Test Cases

### ST01 — Successful Payment

- **Starting State:** Pending
- **Event:** Payment succeeds
- **Expected State:** Confirmed
- **Expected Result:** The reservation moves to Confirmed after successful payment.
- **Transition Type:** Valid

### ST02 — Failed Payment

- **Starting State:** Pending
- **Event:** Payment fails
- **Expected State:** Cancelled
- **Expected Result:** The reservation is cancelled and the reserved dates are released.
- **Transition Type:** Valid

### ST03 — Guest Cancels a Pending Reservation

- **Starting State:** Pending
- **Event:** Guest cancels
- **Expected State:** Cancelled
- **Expected Result:** The pending reservation is cancelled and the reserved dates are released.
- **Transition Type:** Valid

### ST04 — Guest Checks In

- **Starting State:** Confirmed
- **Event:** Guest checks in on the check-in date
- **Expected State:** Checked In
- **Expected Result:** The reservation moves to Checked In.
- **Transition Type:** Valid

### ST05 — Guest Cancels a Confirmed Reservation

- **Starting State:** Confirmed
- **Event:** Guest cancels
- **Expected State:** Cancelled
- **Expected Result:** The confirmed reservation is cancelled and the reserved dates are released.
- **Transition Type:** Valid

### ST06 — Guest Checks Out

- **Starting State:** Checked In
- **Event:** Guest checks out after the stay
- **Expected State:** Completed
- **Expected Result:** The reservation is marked Completed.
- **Transition Type:** Valid

### ST07 — Attempt to Check In While Pending

- **Starting State:** Pending
- **Event:** Guest attempts to check in before payment confirmation
- **Expected State:** Pending
- **Expected Result:** An `InvalidStateError` is raised, and the reservation remains Pending.
- **Transition Type:** Invalid

### ST08 — Attempt to Check In After Cancellation

- **Starting State:** Cancelled
- **Event:** Guest attempts to check in
- **Expected State:** Cancelled
- **Expected Result:** An `InvalidStateError` is raised, and the reservation remains Cancelled.
- **Transition Type:** Invalid

### ST09 — Attempt to Cancel a Completed Reservation

- **Starting State:** Completed
- **Event:** Guest attempts to cancel
- **Expected State:** Completed
- **Expected Result:** An `InvalidStateError` is raised, and the reservation remains Completed.
- **Transition Type:** Invalid

### ST10 — Complete Reservation Sequence

- **Starting State:** Pending
- **Events:** Payment succeeds → Guest checks in on the check-in date → Guest checks out after the stay
- **Expected State Sequence:** Pending → Confirmed → Checked In → Completed
- **Expected Result:** Each event produces the correct next state, and the reservation finishes as Completed.
- **Transition Type:** Valid sequence

### ST11 — Cancellation Sequence

- **Starting State:** Pending
- **Events:** Guest cancels → Guest attempts to check in
- **Expected State Sequence:** Pending → Cancelled → Cancelled
- **Expected Result:** The cancellation succeeds. The subsequent check-in attempt raises an `InvalidStateError`, and the reservation remains Cancelled.
- **Transition Type:** Valid and invalid sequence

## 3. Additional Invalid Transition Tests

### ST12 — Payment Succeeds After Cancellation

- **Starting State:** Cancelled
- **Event:** Payment succeeds
- **Expected State:** Cancelled
- **Expected Result:** An `InvalidStateError` is raised, and the reservation remains Cancelled.
- **Transition Type:** Invalid

### ST13 — Checkout Without Checking In

- **Starting State:** Confirmed
- **Event:** Guest attempts to check out without checking in
- **Expected State:** Confirmed
- **Expected Result:** An `InvalidStateError` is raised, and the reservation remains Confirmed.
- **Transition Type:** Invalid

## 4. Test Coverage Summary

| Test ID | Scenario | Transition Type |
|---|---|---|
| ST01 | Successful payment | Valid |
| ST02 | Failed payment | Valid |
| ST03 | Cancel pending reservation | Valid |
| ST04 | Check in | Valid |
| ST05 | Cancel confirmed reservation | Valid |
| ST06 | Check out | Valid |
| ST07 | Check in while pending | Invalid |
| ST08 | Check in after cancellation | Invalid |
| ST09 | Cancel completed reservation | Invalid |
| ST10 | Complete reservation sequence | Valid sequence |
| ST11 | Cancellation sequence | Valid and invalid sequence |
| ST12 | Payment after cancellation | Invalid |
| ST13 | Checkout without check-in | Invalid |

## 5. Conclusion

These 13 test cases cover all six valid state transitions, five invalid transition attempts, a complete reservation lifecycle, and a cancellation sequence. They verify that successful events produce the expected state changes and invalid events raise an `InvalidStateError` without changing the current state.