# Finance Summary

Finance summary include the total expense and the top expense category for the defined period by the relevant person.

For eg, John spent 15k this month and top category is Food with 5k.

Display Format: `CM: 15,000/- (Food: 5,000/-)`

## Current Month's Summary Formula

```
lets(
    numberOfMonthsToSubtract, /* helps to define the period */
    0,

    /* Get start and end date */
    startDate,
    now().dateSubtract(numberOfMonthsToSubtract, "months").formatDate("YYYY-MM-01").parseDate(),

    endDate,
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
    ),

    /* All the cashflow entries for the above defined time period */
    filteredCashflow,
    prop("Cashflow")
    .filter(current.prop("Date") >= startDate and current.prop("Date") <= endDate),

    /* List of categories from Cashflow Area database*/
    kCategories,
    [
        "Bills",
        "Career & Education",
        "Daily Commute",
        "Entertainment",
        "Fashion & Accessories",
        "Food & Dining",
        "Getaways & Vacations",
        "Health & Fitness",
        "Home Maintenance",
        "Income",
        "Investment",
        "Miscellaneous",
        "Personal Care",
        "Purchases & Acquisitions",
        "Religious",
        "Repayments & Debts"
    ],

    /* Getting Amount for each category */
    categoryAmounts,
    kCategories
    .map(
        lets(
            categoryName,
            current,

            categoryCashflow,
            filteredCashflow
            .filter(current.prop("Area's Parent").format() == categoryName),

            categoryCashflowAmountList,
            categoryCashflow
            .map(current.prop("Amount")),

            sum,
            categoryCashflowAmountList.sum(),

            sum
        )
    ),

    maxAmountForACategory, /* Max amount spent on a category */
    categoryAmounts.max(),

    maxAmountIndex, /* Index of max amount */
    categoryAmounts.findIndex(current == maxAmountForACategory),

    topCategory, /* Category name which has max expense */
    kCategories.at(maxAmountIndex),

    totalSpent, /* total spent in the given period */
    filteredCashflow.map(current.prop("Expense Amount")).sum(),


    /* Formatting Amount in INR */
    formattedAmountInInr,
    lets(
        amount,
        maxAmountForACategory,

        amountInString,
        amount.format(),

        isBelowThousand,
        amount < 1000,

        isInSingleDigitThousand,
        amount >= 1000 and amount <= 9999,

        formatSingleDigitThousand,
        amountInString.substring(0,1) + "," + amountInString.substring(1),

        isInDoubleDigitThousand,
        amount >= 10000 and amount <=99999,

        formatDoubleDigitThousand,
        amountInString.substring(0, 2) + "," + amountInString.substring(2),

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
    ),

    totalSpentInInr, /* Total spent in INR */
    lets(
        amount,
        totalSpent,

        amountInString,
        amount.format(),

        isBelowThousand,
        amount < 1000,

        isInSingleDigitThousand,
        amount >= 1000 and amount <= 9999,

        formatSingleDigitThousand,
        amountInString.substring(0,1) + "," + amountInString.substring(1),

        isInDoubleDigitThousand,
        amount >= 10000 and amount <=99999,

        formatDoubleDigitThousand,
        amountInString.substring(0, 2) + "," + amountInString.substring(2),

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
    ),

    monthNameShort,
    now().formatDate("MMM"),

    displayText,
    (monthNameShort + ": " + totalSpentInInr + " (" + topCategory + ": " + formattedAmountInInr + ")").style("b", "purple"),

    displayText
)
```

# Last Month's Summary

> **Callout:** It excludes the current month entries.
> If current month is November, it will include October.

Display Format: `1M: 30,000/- (Food: 10,000/-)`

Same as the [Current Month's Summary Formula](#current-months-summary-formula) with `numberOfMonthsToSubtract` as `1`.

# Last Quarter's Summary

> **Callout:** It excludes the current month entries.
> If current month is November, it will include August, September, and October.

Display Format: `3M: 45,000/- (Food: 15,000/-)`

Same as the [Current Month's Summary Formula](#current-months-summary-formula) with `numberOfMonthsToSubtract` as `3`.

## Last 6 Months Summary

> **Callout:** It excludes the current month entries.
> If current month is November, it will include May, June, July, August, September, and October.

Display Format: `6M: 90,000/- (Food: 30,000/-)`

Same as the [Current Month's Summary Formula](#current-months-summary-formula) with `numberOfMonthsToSubtract` as `6`.
