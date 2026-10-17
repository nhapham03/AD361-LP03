## Equivalence Partition

The main test classifications are `number of nights`, `number of guests`, `check-in and checkout dates`, and `availability`.

- **Number of nights:** Reservations must be between 1 and 31 nights. Values below 1 or above 31 are invalid.
- **Number of guests:** Reservations must include 1–12 guests. Guests 1–5 get standard rate, while guests 6–12 get an additional fee.
- **Check-in and checkout dates:** The check-in date must be before the checkout date, and the reservation must be between 1 and 31 nights.
- **Availability:** Every night between check-in and checkout must be available for the reservation to go through.

For Step 1.3: I have divided the groups in my tables below. Explanation: the input values are divided into equivalence partitions based on how the application should handle them. Values within the same partition are expected to produce the same type of result, such as a valid reservation, a `ValueError`, or an `UnavailableError`. For example, reservations with fewer than 1 night belong to the invalid-low partition because they should all raise a `ValueError`.

One representative value is selected from each partition to test the expected behavior without testing every possible input from the tables below.

### Number of nights

| Classification   | Partition    | Range or Condition  | Expected Result   |
| ---------------- | ------------ | ------------------- | ----------------- |
| Number of nights | Invalid low  | Fewer than 1 night  | `ValueError`      |
| Number of nights | Valid        | 1–31 nights         | Valid Reservation |
| Number of nights | Invalid high | More than 31 nights | `ValueError`      |

### Number of guests

| Classification   | Partition             | Range or Condition  | Expected Result                     |
| ---------------- | --------------------- | ------------------- | ----------------------------------- |
| Number of guests | Invalid low           | Fewer than 1 guest  | `ValueError`                        |
| Number of guests | Valid, standard rate  | 1–5 guests          | No additional guest fee             |
| Number of guests | Valid, additional fee | 6–12 guests         | Additional $25 per guest, per night |
| Number of guests | Invalid high          | More than 12 guests | `ValueError`                        |

### Check-in and checkout dates

| Classification              | Partition | Range or Condition                                                 | Expected Result                                 |
| --------------------------- | --------- | ------------------------------------------------------------------ | ----------------------------------------------- |
| Check-in and checkout dates | Invalid   | Check-in date is after checkout date                               | `ValueError`                                    |
| Check-in and checkout dates | Invalid   | Check-in date equals checkout date                                 | `ValueError`                                    |
| Check-in and checkout dates | Valid     | Check-in date is before checkout date, and the stay is 1–31 nights | Valid reservation with correct number of nights |

### Availability

| Classification | Partition   | Range or Condition                                            | Expected Result      |
| -------------- | ----------- | ------------------------------------------------------------- | -------------------- |
| Availability   | Available   | Every night in the requested date range is available          | Reservation succeeds |
| Availability   | Unavailable | At least one night in the requested date range is unavailable | `UnavailableError`   |

## Test Cases

### Number of nights

| Test-case ID | Classification   | Partition    | Input     | Expected Result   |
| ------------ | ---------------- | ------------ | --------- | ----------------- |
| TC01         | Number of nights | Invalid Low  | 0 night   | `ValueError`      |
| TC02         | Number of nights | Valid        | 5 nights  | Valid Reservation |
| TC03         | Number of nights | Invalid High | 32 nights | `ValueError`      |

### Number of guests

| Test-case ID | Classification   | Partition             | Input     | Expected Result                                           |
| ------------ | ---------------- | --------------------- | --------- | --------------------------------------------------------- |
| TC04         | Number of guests | Invalid low           | 0 guests  | `ValueError`                                              |
| TC05         | Number of guests | Valid, standard rate  | 3 guests  | Reservation Accepted, no additional guest fee             |
| TC06         | Number of guests | Valid, additional fee | 6 guests  | Reservation Accepted, additional $25 per guest, per night |
| TC07         | Number of guests | Invalid high          | 13 guests | `ValueError`                                              |

### Check-in and checkout dates

| Test-case ID | Classification              | Partition | Input                                | Expected Result                 |
| ------------ | --------------------------- | --------- | ------------------------------------ | ------------------------------- |
| TC08         | Check-in and checkout dates | Invalid   | Check-in: June 10, Checkout: June 9  | `ValueError`                    |
| TC09         | Check-in and checkout dates | Invalid   | Check-in: June 10, Checkout: June 10 | `ValueError`                    |
| TC10         | Check-in and checkout dates | Valid     | Check-in: June 10, Checkout: June 15 | Valid reservation with 5 nights |

### Availability

| Test-case ID | Classification | Partition   | Input                                      | Expected Result      |
| ------------ | -------------- | ----------- | ------------------------------------------ | -------------------- |
| TC11         | Availability   | Available   | June 10–15; all nights available           | Reservation succeeds |
| TC12         | Availability   | Unavailable | June 10–15; at least one night unavailable | `UnavailableError`   |

More explanation from table above, due to table being too long and formatting issue:
- TC11 Check-in: June 10, Checkout: June 15 - all nights from June 10 through June 14 are available. June 15 is not counted as a reserved night.
- TC12 Check-in: June 10, Checkout: June 15 - the night of June 12 is unavailable. June 15 is not counted as a reserved night.
