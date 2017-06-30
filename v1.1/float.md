---
title: FLOAT
summary: The FLOAT data type stores inexact, floating-point numbers with up to 17 digits in total and at least one digit to the right of the decimal point.
toc: false
---

The `FLOAT` [data type](data-types.html) stores inexact, floating-point numbers with up to 17 digits of decimal precision.

They are handled internally using the [standard double-precision
(64-bit binary-encoded) IEEE754 format](https://en.wikipedia.org/wiki/IEEE_floating_point).

<div id="toc"></div>

## Aliases

In CockroachDB, the following are aliases for `FLOAT`:

- `REAL` 
- `DOUBLE PRECISION` 

## Syntax

A constant value of type `FLOAT` can be entered as a [numeric literal](sql-constants.html#numeric-literals).
For example: `1.414` or `-1234`.

The special IEEE754 values for positive infinity, negative infinity
and Not A Number (NaN) cannot be entered using numeric literals
directly and must be converted using an
[interpreted literal](sql-constants.html#interpreted-literals) or an
[explicit conversion](sql-expressions.html#explicit-type-coercions) from
a string literal instead. For example:

- `FLOAT '+Inf'`
- `'-Inf'::FLOAT`
- `CAST('NaN' AS FLOAT)`

## Size

A `FLOAT` column supports values up to 8 bytes in width, but the total storage size is likely to be larger due to CockroachDB metadata.  

## Examples

~~~ sql
> CREATE TABLE floats (a FLOAT PRIMARY KEY, b REAL, c DOUBLE PRECISION);

> SHOW COLUMNS FROM floats;
~~~
~~~
+-------+-------+-------+---------+
| Field | Type  | Null  | Default |
+-------+-------+-------+---------+
| a     | FLOAT | false | NULL    |
| b     | FLOAT | true  | NULL    |
| C     | FLOAT | true  | NULL    |
+-------+-------+-------+---------+
~~~
~~~ sql
> INSERT INTO floats VALUES (1.012345678901, 2.01234567890123456789, CAST('+Inf' AS FLOAT));

> SELECT * FROM floats;
~~~
~~~ 
+----------------+--------------------+------+
|       a        |         b          |  c   |
+----------------+--------------------+------+
| 1.012345678901 | 2.0123456789012346 | +Inf |
+----------------+--------------------+------+
# Note that the value in "b" has been limited to 17 digits.
~~~

## Supported Casting & Conversion

`FLOAT` values can be [cast](data-types.html#data-type-conversions-casts) to any of the following data types:

Type | Details
-----|--------
`INT` | Truncates decimal precision and requires values to be between -2^63 and 2^63-1
`DECIMAL` | Causes an error to be reported if the value is NaN or +/- Inf.
`BOOL` |  **0** converts to `false`; all other values convert to `true`
`STRING` | --

## See Also

[Data Types](data-types.html)