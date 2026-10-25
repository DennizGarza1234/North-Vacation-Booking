# North Vacation Booking — Equivalence Partitioning

## 1. Project Overview

**Application:** North Vacation Booking

**Purpose:** An application that allows homeowners to list their homes as vacation rentals and guests to make reservations.

**Objective:** Apply equivalence partitioning to black-box testing by dividing inputs into valid and invalid groups and selecting representative test values. Testing is based on the requirements without examining the application's internal code.

## 2. Application Requirements

- Reservations must be between 1 and 31 nights, inclusive.
- Reservations must include between 1 and 12 guests, inclusive.
- The base reservation cost is $200 per night.
- Every valid reservation includes a one-time $50 cleaning fee.
- Each guest over five costs an additional $25 per night.
- Check-in must be before checkout.
- Number of nights is calculated as checkout date minus check-in date.
- The checkout date is not counted as a reserved night.
- Every night in the requested date range must be available.
- Invalid reservation information raises a `ValueError`.
- An unavailable reservation raises an `UnavailableError`.

## 3. Equivalence Partition Tables

### 3.1 Number of Nights

| Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|
| Number of nights | Invalid low | Fewer than 1 night | `ValueError` |
| Number of nights | Valid minimum | Exactly 1 night | Reservation accepted if available |
| Number of nights | Valid typical | 2–30 nights | Reservation accepted if available |
| Number of nights | Valid maximum | Exactly 31 nights | Reservation accepted if available |
| Number of nights | Invalid high | More than 31 nights | `ValueError` |

### 3.2 Number of Guests

| Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|
| Number of guests | Invalid low | Fewer than 1 guest | `ValueError` |
| Number of guests | Valid standard rate | 1–5 guests | No additional guest fee |
| Number of guests | Valid additional fee | 6–11 guests | $25 per guest over five, per night |
| Number of guests | Valid maximum | Exactly 12 guests | $175 additional per night |
| Number of guests | Invalid high | More than 12 guests | `ValueError` |

### 3.3 Check-in and Checkout Dates

| Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|
| Check-in and checkout dates | Invalid: same date | Check-in equals checkout | `ValueError` |
| Check-in and checkout dates | Invalid: reversed dates | Check-in is after checkout | `ValueError` |
| Check-in and checkout dates | Valid: one night | Checkout is one day after check-in | Valid 1-night reservation |
| Check-in and checkout dates | Valid: multiple nights | Checkout is 2–30 days after check-in | Valid reservation |
| Check-in and checkout dates | Valid: maximum duration | Checkout is 31 days after check-in | Valid 31-night reservation |
| Check-in and checkout dates | Invalid: excessive duration | Checkout is more than 31 days after check-in | `ValueError` |

### 3.4 Availability

| Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|
| Availability | All nights available | Every reserved night is available | Reservation accepted |
| Availability | First night unavailable | Check-in night is unavailable | `UnavailableError` |
| Availability | Middle night unavailable | An intermediate reserved night is unavailable | `UnavailableError` |
| Availability | Last reserved night unavailable | Night before checkout is unavailable | `UnavailableError` |
| Availability | Checkout date unavailable | Only the checkout date is unavailable | Reservation accepted |

## 4. Test Cases

### TC01 — Invalid Low Number of Nights

- **Classification:** Number of nights
- **Partition:** Invalid low
- **Input:** Check-in October 20, 2026; checkout October 20, 2026; 3 guests.
- **Expected result:** `ValueError` because the duration is zero nights.

### TC02 — Valid Minimum Number of Nights

- **Classification:** Number of nights
- **Partition:** Valid minimum
- **Input:** Check-in October 20, 2026; checkout October 21, 2026; 3 guests; the reserved night is available.
- **Expected result:** Reservation accepted for 1 night. Total cost: $250.

### TC03 — Valid Typical Number of Nights

- **Classification:** Number of nights
- **Partition:** Valid typical
- **Input:** Check-in October 20, 2026; checkout October 27, 2026; 3 guests; all reserved nights are available.
- **Expected result:** Reservation accepted for 7 nights. Total cost: $1,450.

### TC04 — Valid Maximum Number of Nights

- **Classification:** Number of nights
- **Partition:** Valid maximum
- **Input:** Check-in October 20, 2026; checkout November 20, 2026; 3 guests; all reserved nights are available.
- **Expected result:** Reservation accepted for 31 nights. Total cost: $6,250.

### TC05 — Invalid High Number of Nights

- **Classification:** Number of nights
- **Partition:** Invalid high
- **Input:** Check-in October 20, 2026; checkout November 21, 2026; 3 guests.
- **Expected result:** `ValueError` because the duration is 32 nights.

### TC06 — Invalid Low Number of Guests

- **Classification:** Number of guests
- **Partition:** Invalid low
- **Input:** 0 guests; check-in October 20, 2026; checkout October 21, 2026.
- **Expected result:** `ValueError`.

### TC07 — Valid Minimum Number of Guests

- **Classification:** Number of guests
- **Partition:** Valid standard rate
- **Input:** 1 guest; check-in October 20, 2026; checkout October 21, 2026; the reserved night is available.
- **Expected result:** Reservation accepted. Total cost: $250.

### TC08 — Valid Standard Number of Guests

- **Classification:** Number of guests
- **Partition:** Valid standard rate
- **Input:** 3 guests; check-in October 20, 2026; checkout October 23, 2026; all reserved nights are available.
- **Expected result:** Reservation accepted for 3 nights. Total cost: $650.

### TC09 — Valid Number of Guests With Additional Fee

- **Classification:** Number of guests
- **Partition:** Valid additional fee
- **Input:** 8 guests; check-in October 20, 2026; checkout October 23, 2026; all reserved nights are available.
- **Expected result:** Reservation accepted for 3 nights. Additional fee: $75 per night. Total cost: $875.

### TC10 — Valid Maximum Number of Guests

- **Classification:** Number of guests
- **Partition:** Valid maximum
- **Input:** 12 guests; check-in October 20, 2026; checkout October 22, 2026; all reserved nights are available.
- **Expected result:** Reservation accepted for 2 nights. Additional fee: $175 per night. Total cost: $800.

### TC11 — Invalid High Number of Guests

- **Classification:** Number of guests
- **Partition:** Invalid high
- **Input:** 13 guests; check-in October 20, 2026; checkout October 21, 2026.
- **Expected result:** `ValueError`.

### TC12 — Check-in and Checkout on the Same Date

- **Classification:** Check-in and checkout dates
- **Partition:** Invalid: same date
- **Input:** Check-in October 20, 2026; checkout October 20, 2026; 2 guests.
- **Expected result:** `ValueError`.

### TC13 — Check-in After Checkout

- **Classification:** Check-in and checkout dates
- **Partition:** Invalid: reversed dates
- **Input:** Check-in October 21, 2026; checkout October 20, 2026; 2 guests.
- **Expected result:** `ValueError`.

### TC14 — Valid One-Night Date Range

- **Classification:** Check-in and checkout dates
- **Partition:** Valid: one night
- **Input:** Check-in October 20, 2026; checkout October 21, 2026; 2 guests; October 20 is available.
- **Expected result:** Reservation accepted for 1 night. October 21 is not counted as a reserved night.

### TC15 — Valid Multiple-Night Date Range

- **Classification:** Check-in and checkout dates
- **Partition:** Valid: multiple nights
- **Input:** Check-in October 20, 2026; checkout October 27, 2026; 2 guests; all reserved nights are available.
- **Expected result:** Reservation accepted for 7 nights, October 20 through October 26.

### TC16 — Valid Maximum Date Range

- **Classification:** Check-in and checkout dates
- **Partition:** Valid: maximum duration
- **Input:** Check-in October 20, 2026; checkout November 20, 2026; 2 guests; all reserved nights are available.
- **Expected result:** Reservation accepted for 31 nights.

### TC17 — Date Range Exceeds Maximum Duration

- **Classification:** Check-in and checkout dates
- **Partition:** Invalid: excessive duration
- **Input:** Check-in October 20, 2026; checkout November 21, 2026; 2 guests.
- **Expected result:** `ValueError`.

### TC18 — All Requested Nights Are Available

- **Classification:** Availability
- **Partition:** All nights available
- **Input:** Check-in October 20, 2026; checkout October 23, 2026; 2 guests; October 20, 21, and 22 are available.
- **Expected result:** Reservation accepted for 3 nights. Total cost: $650.

### TC19 — First Reserved Night Is Unavailable

- **Classification:** Availability
- **Partition:** First night unavailable
- **Input:** Check-in October 20, 2026; checkout October 23, 2026; 2 guests; October 20 is unavailable.
- **Expected result:** `UnavailableError`.

### TC20 — Middle Reserved Night Is Unavailable

- **Classification:** Availability
- **Partition:** Middle night unavailable
- **Input:** Check-in October 20, 2026; checkout October 23, 2026; 2 guests; October 21 is unavailable.
- **Expected result:** `UnavailableError`.

### TC21 — Last Reserved Night Is Unavailable

- **Classification:** Availability
- **Partition:** Last reserved night unavailable
- **Input:** Check-in October 20, 2026; checkout October 23, 2026; 2 guests; October 22 is unavailable.
- **Expected result:** `UnavailableError`.

### TC22 — Checkout Date Is Unavailable

- **Classification:** Availability
- **Partition:** Checkout date unavailable
- **Input:** Check-in October 20, 2026; checkout October 23, 2026; 2 guests; October 20, 21, and 22 are available, but October 23 is unavailable.
- **Expected result:** Reservation accepted for 3 nights because checkout day is not reserved.

## 5. Conclusion

Equivalence partitioning organizes inputs into groups expected to produce the same behavior. These 22 test cases cover valid and invalid reservation durations, guest counts, date relationships, and availability. They also verify the expected exceptions, nightly rate, additional guest fees, cleaning fee, and checkout-date rule.

## 6. Reference

GeeksforGeeks. (2026). *Equivalence Partitioning Method*. https://www.geeksforgeeks.org/software-engineering/equivalence-partitioning-method/


## Black Box Testing — Part 2

### Overview

Part 2 builds on the equivalence partitioning and black-box testing techniques documented in Part 1. It uses decision table testing and state transition testing to verify the North Vacation Booking application's reservation rules, pricing, availability, and reservation lifecycle.

### Decision Table Testing

The decision table identifies combinations of reservation conditions and their expected actions.

- `docs/decision-table.md` — Conditions, actions, decision rules, and pricing formula.
- `docs/decision-table-test-cases.md` — Six test cases covering valid reservations, pricing, unavailable dates, invalid stay lengths, invalid guest counts, and invalid date order.

### State Transition Testing

The state transition documents verify the reservation lifecycle and the handling of invalid state changes.

- `docs/state-transition-table.md` — Reservation states, events, valid transitions, and invalid transitions.
- `docs/state-transition-test-cases.md` — Thirteen test cases covering valid transitions, invalid transitions, a complete reservation sequence, and a cancellation sequence.

### Reservation Requirements Covered

Testing covers:

- Reservations of 1–31 nights.
- Reservations for 1–12 guests.
- The $200 nightly rate and one-time $50 cleaning fee.
- The additional $25 per night for each guest over five.
- Date validation and availability checks.
- Appropriate `ValueError`, `UnavailableError`, and `InvalidStateError` exceptions.
- Reservation states: Pending, Confirmed, Checked In, Completed, and Cancelled.

### Testing Approach

These documents define expected outcomes for black-box testing. They describe the inputs, conditions, expected actions, prices, exceptions, and state changes without requiring knowledge of the application's internal implementation.

**Note:** The test cases specify expected results; they do not claim that the application has been executed or that the results have been verified against a running implementation.
