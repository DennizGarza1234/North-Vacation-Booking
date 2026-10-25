# Decision Table Testing — North Vacation Booking

## 1. Purpose

This document uses decision table testing to verify that the North Vacation Booking application accepts valid reservations, rejects invalid reservation information, handles unavailable dates, and calculates reservation prices correctly.

## 2. Conditions

The following five conditions determine the expected reservation outcome.

1. **C1 — Valid dates:** The check-in date is before the checkout date.
2. **C2 — Valid number of nights:** The reservation is between 1 and 31 nights, inclusive.
3. **C3 — Valid number of guests:** The reservation includes between 1 and 12 guests, inclusive.
4. **C4 — Available dates:** Every night in the requested date range is available.
5. **C5 — More than five guests:** The reservation includes at least 6 guests.

## 3. Actions

1. **A1 — Create reservation:** Create a reservation in the Pending state.
2. **A2 — Calculate standard price:** Calculate $200 per night plus the one-time $50 cleaning fee.
3. **A3 — Add extra guest fee:** Add $25 per night for each guest over five.
4. **A4 — Raise ValueError:** Reject invalid dates, night counts, or guest counts.
5. **A5 — Raise UnavailableError:** Reject a reservation when any requested night is unavailable.

## 4. Decision Table

| Conditions and Actions | Rule 1 | Rule 2 | Rule 3 | Rule 4 | Rule 5 | Rule 6 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **Conditions** | | | | | | |
| C1: Dates are valid | T | T | T | T | T | F |
| C2: Number of nights is valid | T | T | T | F | T | - |
| C3: Number of guests is valid | T | T | T | T | F | - |
| C4: Requested nights are available | T | T | F | - | - | - |
| C5: More than five guests | F | T | - | - | - | - |
| **Actions** | | | | | | |
| A1: Create reservation | X | X | | | | |
| A2: Calculate standard price | X | X | | | | |
| A3: Add extra guest fee | | X | | | | |
| A4: Raise ValueError | | | | X | X | X |
| A5: Raise UnavailableError | | | X | | | |

**Rule 1 — Valid reservation with five or fewer guests:** All dates and guest information are valid, and every requested night is available. Create a Pending reservation and calculate the standard price.

**Rule 2 — Valid reservation with extra guest fees:** All reservation information is valid, every requested night is available, and there are more than five guests. Create a Pending reservation, calculate the standard price, and add the extra guest fee.

**Rule 3 — Unavailable dates:** Reservation information is valid, but at least one requested night is unavailable. Raise an `UnavailableError`.

**Rule 4 — Invalid number of nights:** The reservation exceeds the maximum of 31 nights. Raise a `ValueError`.

**Rule 5 — Invalid number of guests:** The reservation contains fewer than 1 guest or more than 12 guests. Raise a `ValueError`.

**Rule 6 — Invalid date order:** The check-in date is not before the checkout date. Raise a `ValueError`.

## 5. Pricing Formula

The standard reservation price is:

Standard price = (Number of nights × $200) + $50 cleaning fee

For reservations with more than five guests:

Extra guest fee = (Number of guests − 5) × $25 × Number of nights

Total price = Standard price + Extra guest fee

## 6. Conclusion

The decision table identifies representative combinations of valid and invalid reservation conditions. The corresponding test cases will verify that the application creates valid reservations, calculates the correct prices, and raises the appropriate exceptions for invalid or unavailable reservations.