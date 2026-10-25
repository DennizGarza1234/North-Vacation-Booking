# Decision Table Test Cases — North Vacation Booking

## 1. Purpose

These test cases verify the six rules defined in `decision-table.md`. They check reservation creation, price calculations, date availability, and validation errors.

## 2. Test Cases

### TC01 — Valid Reservation With Five or Fewer Guests

- **Rule:** Rule 1
- **Conditions:** Valid dates, valid number of nights, valid number of guests, all nights available, and no more than five guests.
- **Check-in:** November 1, 2026
- **Checkout:** November 4, 2026
- **Number of nights:** 3
- **Number of guests:** 3
- **Availability:** All requested nights are available.
- **Expected Action:** Create a reservation in the Pending state and calculate the standard price.
- **Expected Price:**
  - Nightly rate: 3 × $200 = $600
  - Cleaning fee: $50
  - Extra guest fee: $0
  - **Total: $650**
- **Expected Result:** The reservation is created successfully with a total price of $650.

### TC02 — Valid Reservation With More Than Five Guests

- **Rule:** Rule 2
- **Conditions:** Valid dates, valid number of nights, valid number of guests, all nights available, and more than five guests.
- **Check-in:** November 1, 2026
- **Checkout:** November 4, 2026
- **Number of nights:** 3
- **Number of guests:** 7
- **Availability:** All requested nights are available.
- **Expected Action:** Create a reservation in the Pending state, calculate the standard price, and add the extra guest fee.
- **Expected Price:**
  - Nightly rate: 3 × $200 = $600
  - Cleaning fee: $50
  - Extra guest fee: (7 − 5) × $25 × 3 = $150
  - **Total: $800**
- **Expected Result:** The reservation is created successfully with a total price of $800.

### TC03 — Reservation With Unavailable Dates

- **Rule:** Rule 3
- **Conditions:** All reservation information is valid, but at least one requested night is unavailable.
- **Check-in:** November 1, 2026
- **Checkout:** November 4, 2026
- **Number of nights:** 3
- **Number of guests:** 3
- **Availability:** November 2 is unavailable.
- **Expected Action:** Raise an `UnavailableError`.
- **Expected Price:** No reservation price is finalized because the reservation is rejected.
- **Expected Result:** The reservation is not created because every requested night must be available.

### TC04 — Reservation Exceeds the Maximum Number of Nights

- **Rule:** Rule 4
- **Conditions:** The dates are in the correct order, but the reservation exceeds 31 nights.
- **Check-in:** November 1, 2026
- **Checkout:** December 4, 2026
- **Number of nights:** 33
- **Number of guests:** 3
- **Availability:** Not applicable because the reservation fails validation.
- **Expected Action:** Raise a `ValueError`.
- **Expected Price:** No reservation price is finalized.
- **Expected Result:** The reservation is rejected because the maximum permitted stay is 31 nights.

### TC05 — Reservation Has an Invalid Number of Guests

- **Rule:** Rule 5
- **Conditions:** Dates and number of nights are valid, but the guest count exceeds the maximum of 12.
- **Check-in:** November 1, 2026
- **Checkout:** November 4, 2026
- **Number of nights:** 3
- **Number of guests:** 13
- **Availability:** Not applicable because the reservation fails validation.
- **Expected Action:** Raise a `ValueError`.
- **Expected Price:** No reservation price is finalized.
- **Expected Result:** The reservation is rejected because the maximum permitted number of guests is 12.

### TC06 — Invalid Check-in and Checkout Date Order

- **Rule:** Rule 6
- **Conditions:** The check-in date is not before the checkout date.
- **Check-in:** November 4, 2026
- **Checkout:** November 4, 2026
- **Number of nights:** 0
- **Number of guests:** 3
- **Availability:** Not applicable because the reservation fails validation.
- **Expected Action:** Raise a `ValueError`.
- **Expected Price:** No reservation price is finalized.
- **Expected Result:** The reservation is rejected because the check-in date must be before the checkout date.

## 3. Test Coverage Summary

| Test Case | Rule | Main Scenario | Expected Outcome |
|---|---|---|---|
| TC01 | Rule 1 | Valid reservation, 3 guests | $650; Pending |
| TC02 | Rule 2 | Valid reservation, 7 guests | $800; Pending |
| TC03 | Rule 3 | Requested night unavailable | `UnavailableError` |
| TC04 | Rule 4 | 33-night reservation | `ValueError` |
| TC05 | Rule 5 | 13 guests | `ValueError` |
| TC06 | Rule 6 | Same check-in and checkout date | `ValueError` |

## 4. Conclusion

These six test cases cover every rule in the decision table. They verify valid reservation pricing, the additional guest fee, unavailable dates, excessive stay length, invalid guest counts, and invalid date order. The expected results provide a basis for evaluating whether the North Vacation Booking application meets its reservation requirements.