# Start of Month

Returns the date where the start date is the first day of the month irrespective of the current date.

## Example

| Input      | Output     |
| ---------- | ---------- |
| 2024-01-15 | 2024-01-01 |
| 2024-12-31 | 2024-12-01 |

## Formula

```
now().formatDate("YYYY-MM-01").parseDate()

```

# End of Month

Returns the date where the end date is the last day of the month irrespective of the current date.

## Example

| Input      | Output     |
| ---------- | ---------- |
| 2024-01-15 | 2024-01-31 |
| 2024-12-31 | 2024-12-31 |

## Formula

```
lets(
	month,
	now().month(),

	year,
	now().year(),

	isLeapYear,
	(year.mod(4) == 0 and year.mod(100) != 0) or (year.mod(400) == 0),

	lastDateNumber,
	month.mod(2) != 0
			? 31
			: ((month != 2)
				? 30
				: (isLeapYear ? 29 : 28)),

		lastDate,
		now().formatDate("YYYY-MM-" + lastDateNumber).parseDate(),

		lastDate
	)
```
