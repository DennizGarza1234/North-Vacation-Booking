# Equivalence Partitions — North Vacation Booking

## 1. Number of Nights

| Partition | Range or Condition | Representative Value | Expected Result |
|---|---|---|---|
| Invalid low | Fewer than 1 night | 0 nights | `ValueError` |
| Valid minimum | Exactly 1 night | 1 night | Valid duration |
| Valid typical | 2–30 nights | 7 nights | Valid duration |
| Valid maximum | Exactly 31 nights | 31 nights | Valid duration |
| Invalid high | More than 31 nights | 32 nights | `ValueError` |

## 2. Number of Guests

| Partition | Range or Condition | Representative Value | Expected Result |
|---|---|---|---|
| Invalid low | Fewer than 1 guest | 0 guests | `ValueError` |
| Valid standard rate | 1–5 guests | 3 guests | No additional guest fee |
| Valid additional fee | 6–11 guests | 8 guests | $75 additional per night |
| Valid maximum | Exactly 12 guests | 12 guests | $175 additional per night |
| Invalid high | More than 12 guests | 13 guests | `ValueError` |

## 3. Check-in and Checkout Dates

| Partition | Range or Condition | Representative Input | Expected Result |
|---|---|---|---|
| Invalid: same date | Check-in equals checkout | Oct 20 to Oct 20, 2026 | `ValueError` |
| Invalid: reversed dates | Check-in is after checkout | Oct 21 to Oct 20, 2026 | `ValueError` |
| Valid: one night | Checkout is one day later | Oct 20 to Oct 21, 2026 | 1 night |
| Valid: multiple nights | Checkout is 2–30 days later | Oct 20 to Oct 27, 2026 | 7 nights |
| Valid: maximum duration | Checkout is 31 days later | Oct 20 to Nov 20, 2026 | 31 nights |
| Invalid: excessive duration | Checkout is more than 31 days later | Oct 20 to Nov 21, 2026 | `ValueError` |

## 4. Availability

| Partition | Condition | Representative Input | Expected Result |
|---|---|---|---|
| All nights available | Every reserved night is available | Oct 20–22 available; checkout Oct 23 | Reservation accepted |
| First night unavailable | Check-in night unavailable | Oct 20 unavailable | `UnavailableError` |
| Middle night unavailable | Intermediate reserved night unavailable | Oct 21 unavailable | `UnavailableError` |
| Last reserved night unavailable | Night before checkout unavailable | Oct 22 unavailable | `UnavailableError` |
| Checkout date unavailable | Only checkout date unavailable | Oct 20–22 available; Oct 23 unavailable | Reservation accepted |

## 5. Reservation Cost Formula

`Total = (200 + max(guests - 5, 0) * 25) * nights + 50`

The formula includes the $200 base nightly rate, the additional $25 per guest above five for every reserved night, and the one-time $50 cleaning fee.
