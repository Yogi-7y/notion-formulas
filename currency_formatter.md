# INR Currency Formatter

Notion formula to format a number to INR (Indian Rupee) currency format.

## Examples

- Input: `1000` -> Output: `₹1,000/-`
- Input: `10000` -> Output: `₹10,000/-`

## Formula

```
lets(
	amount, /* Input amount here */
	10000,

	amountInString,
	format(amount),

	isBelowThousand,
	amount < 1000,

	isInSingleDigitThousand,
	amount >= 1000 and amount <= 9999,

	formatSingleDigitThousand,
	join([substring(amountInString, 0, 1), substring(amountInString, 1)], ","),

	isInDoubleDigitThousand,
	amount >= 10000 and amount <=99999,

	formatDoubleDigitThousand,
	join([substring(amountInString, 0, 2), substring(amountInString, 2)], ","),

	displayAmount,
	ifs(
		isBelowThousand,
		amount,
		isInSingleDigitThousand,
		formatSingleDigitThousand,
		isInDoubleDigitThousand,
		formatDoubleDigitThousand,
		amount
	),

	amountWithRupee,
	"₹" + displayAmount + "/-",

	amountWithRupee
)
```
