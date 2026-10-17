## Test Cases

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