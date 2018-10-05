Template reference
==================

File syntax
-----------

The template file should consist of one CREATE TABLE statement and one INSERT statement, like this:

```sql
CREATE TABLE _ (
    column_1    COLUMN_TYPE_1,
    -- ...
    column_n    COLUMN_TYPE_N
) OPTION_1 = 1, /*...*/ OPTION_N = N;

INSERT INTO _ VALUES (1, 2, /*...*/ 'N');
```

The table's name must be `_` (an unquoted underscore). This will be substituted with the real name
when written as the real data. The INSERT statement should not list the column names.

Expression syntax
-----------------

Each value in the INSERT statement can be an expression. `dbgen` will evaluate the expression to
generate a new row when writing them out.

### Literals

`dbgen` supports integer, float and string literals.

* **Integers**

    Decimal and hexadecimal numbers are supported. The value must be between 0 and
    2<sup>64</sup> − 1.

    Examples: `0`, `3`, `18446744073709551615`, `0X1234abcd`, `0xFFFFFFFFFFFFFFFF`

* **Floating point numbers**

    Numbers will be stored in IEEE-754 double-precision format.

    Examples: `0.0`, `1.5`, `.5`, `2.`, `1e100`, `1.38e-23`, `6.02e+23`

* **Strings**

    Strings must be encoded as UTF-8, and written between single quotes (double-quoted strings are
    *not* supported). To represent a single quote in the string, use `''`.

    Examples: `'Hello'`, `'10 o''clock'`

### Symbols

* **rownum**

    The current row number. The first row has value 1.

### Random functions

* **rand.int(32)**

    Generates a uniform random signed integer with the given number of bits (must be between 1 and
    64).

* **rand.uint(32)**

    Generates a uniform random unsigned integer with the given number of bits (must be between 1 and
    64).

* **rand.regex('[0-9a-z]+', 'i', 100)**

    Generates a random string satisfying the regular expression. The second and third parameters are
    optional. If provided, they specify respectively the regex flags, and maximum repeat count for
    the unbounded repetition operators (`+`, `*` and `{n,}`).

    The input string should satisfy the syntax of the Rust regex package. The flags is a string
    composed of these letters:

    * `x` (ignore whitespace)
    * `i` (case insensitive)
    * `s` (dot matches new-line)
    * `u` (enable Unicode mode)
    * `a` (disable Unicode mode)
    * `o` (recognize octal escapes)

    The flags `m` (multi-line) and `U` (ungreedy) does not affect string generation and are ignored.
