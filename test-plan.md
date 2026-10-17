# Test Plan — North Vacation Booking

## 1. Purpose

The purpose of this test plan is to define the approach for testing North Vacation Booking using black-box testing and equivalence partitioning. The test cases are designed from the application's requirements without examining its internal code.

## 2. Scope

### In Scope
- Reservation duration validation (1–31 nights).
- Guest count validation (1–12 guests).
- Check-in and checkout date validation.
- Calculation of the number of reserved nights.
- Availability of every requested night.
- Reservation cost calculations.
- Expected exceptions for invalid or unavailable reservations.

### Out of Scope
- User interface design and usability.
- Login and account management.
- Property listing management.
- Payment processing.
- Database implementation.
- Performance and security testing.
- Internal code coverage.

## 3. Testing Approach

Equivalence partitioning divides inputs into groups expected to produce the same behavior. Representative values are selected from each valid and invalid partition.

The test cases focus on normal conditions, invalid inputs, boundary values, and availability conditions.

## 4. Test Environment

The assignment does not provide application code. Therefore, the test cases document expected behavior and cannot be executed against the application unless an implementation is provided.

## 5. Entry Criteria

- Application requirements are available.
- Equivalence partitions have been identified.
- Test inputs and expected results have been documented.

## 6. Exit Criteria

- All identified partitions have test coverage.
- Each test case includes an ID, classification, partition, input, and expected result.
- If an application becomes available, actual results are recorded and compared with expected results.
- Any failures are documented for review.

## 7. Expected Exceptions

- `ValueError`: Invalid reservation information, including an invalid duration, guest count, or date order.
- `UnavailableError`: At least one reserved night is unavailable.

## 8. Assumptions and Limitations

- Guest counts are whole numbers.
- Checkout day is not included in the reserved nights.
- Every reserved night must be available.
- The total cost is calculated as:

  `(200 + max(guests - 5, 0) * 25) * nights + 50`

- The requirements do not define error precedence when a reservation has both invalid information and unavailable dates.
- Actual application behavior has not been verified because the application implementation was not supplied.
