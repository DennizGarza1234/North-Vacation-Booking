# State Transition Table — North Vacation Booking

## 1. Purpose

This document identifies the reservation states, the events that change those states, and the expected results of valid and invalid transitions.

## 2. Reservation States

1. **Pending:** A valid reservation has been created, but payment has not yet succeeded.
2. **Confirmed:** Payment has succeeded and the reservation is confirmed.
3. **Checked In:** The guest has checked in on the reservation's check-in date.
4. **Completed:** The guest has checked out and the reservation is complete.
5. **Cancelled:** The reservation has been cancelled or payment has failed.

## 3. Events That Change Reservation States

- Payment succeeds.
- Payment fails.
- Guest cancels the reservation.
- Guest checks in on the check-in date.
- Guest checks out after the stay.

## 4. State Transition Table

| Current State | Event | Expected State | Expected Result |
|---|---|---|---|
| Pending | Payment succeeds | Confirmed | The reservation is confirmed. |
| Pending | Payment fails | Cancelled | The reservation is cancelled and the dates are released. |
| Pending | Guest cancels | Cancelled | The reservation is cancelled and the dates are released. |
| Confirmed | Guest checks in on the check-in date | Checked In | The guest is checked in. |
| Confirmed | Guest cancels | Cancelled | The reservation is cancelled and the dates are released. |
| Checked In | Guest checks out after the stay | Completed | The reservation is marked completed. |
| Pending | Guest attempts to check in | Pending | An `InvalidStateError` is raised because the reservation is not confirmed. |
| Cancelled | Guest attempts to check in | Cancelled | An `InvalidStateError` is raised because cancelled reservations cannot change states. |
| Completed | Guest attempts to cancel | Completed | An `InvalidStateError` is raised because completed reservations cannot change states. |
| Cancelled | Payment succeeds | Cancelled | An `InvalidStateError` is raised because cancelled reservations cannot change states. |
| Confirmed | Guest attempts to check out without checking in | Confirmed | An `InvalidStateError` is raised because the reservation has not been checked in. |

## 5. Valid State Transitions

The valid transitions are:

1. Pending → Confirmed when payment succeeds.
2. Pending → Cancelled when payment fails.
3. Pending → Cancelled when the guest cancels.
4. Confirmed → Checked In when the guest checks in on the check-in date.
5. Confirmed → Cancelled when the guest cancels.
6. Checked In → Completed when the guest checks out after the stay.

## 6. Invalid State Transitions

The following transitions must be rejected:

1. Pending → Checked In: The reservation must be confirmed first.
2. Cancelled → Checked In: A cancelled reservation cannot change states.
3. Completed → Cancelled: A completed reservation cannot change states.
4. Cancelled → Confirmed: A cancelled reservation cannot change states.
5. Confirmed → Completed without checking in: The reservation must be checked in before it can be completed.

Each invalid transition must raise an `InvalidStateError` and leave the reservation in its original state.

## 7. Conclusion

This table covers six valid state transitions and five invalid transition attempts. It establishes the expected behavior for payment, cancellation, check-in, and checkout. The next document will translate these transitions into individual test cases.