.. _mag:

**mag.h** -- fixed-precision unsigned floating-point numbers for bounds
===============================================================================

The :type:`mag_t` type is an unsigned floating-point type with a
fixed-precision mantissa (30 bits) and an arbitrary-precision
exponent (represented as an :type:`fmpz_t`), suited for
representing and rigorously manipulating magnitude bounds efficiently.
Operations always produce a strict upper or lower bound, but for performance
reasons, no attempt is made to compute the best possible bound
(in general, a result may a few ulps larger/smaller than the optimal value).
The special values zero and positive infinity are supported (but not NaN).
Applications requiring more flexibility (such as correct rounding, or
higher precision) should use the :type:`arf_t` type instead.

Types, macros and constants
-------------------------------------------------------------------------------

.. type:: mag_struct

    A :type:`mag_struct` holds a mantissa and an exponent.
    Special values are encoded by the mantissa being set to zero.

.. type:: mag_t

    A :type:`mag_t` is defined as an array of length one of type
    :type:`mag_struct`, permitting a :type:`mag_t` to be passed by reference.

Memory management
-------------------------------------------------------------------------------

.. function:: void mag_init(mag_t x)

    Initializes the variable *x* for use. Its value is set to zero.

.. function:: void mag_clear(mag_t x)

    Clears the variable *x*, freeing or recycling its allocated memory.

.. function:: void mag_init_set(mag_t x, const mag_t y)

.. function:: void mag_swap(mag_t x, mag_t y)

.. function:: void mag_set(mag_t x, const mag_t y)

Special values
-------------------------------------------------------------------------------

.. function:: void mag_zero(mag_t x)

    Sets *x* to zero.

.. function:: void mag_one(mag_t x)

    Sets *x* to one.

.. function:: void mag_inf(mag_t x)

    Sets *x* to positive infinity.

.. function:: int mag_is_special(const mag_t x)

    Returns nonzero iff *x* is zero or positive infinity.

.. function:: int mag_is_zero(const mag_t x)

    Returns nonzero iff *x* is zero.

.. function:: int mag_is_inf(const mag_t x)

    Returns nonzero iff *x* is positive infinity.

.. function:: int mag_is_finite(const mag_t x)

    Returns nonzero iff *x* is not positive infinity (since there is no
    NaN value, this function is exactly the negation of :func:`mag_is_inf`).

Comparisons
-------------------------------------------------------------------------------

.. function:: int mag_equal(const mag_t x, const mag_t y)

    Returns nonzero iff *x* and *y* have the same value.

.. function:: int mag_cmp(const mag_t x, const mag_t y)

    Returns negative, zero, or positive, depending on whether *x*
    is smaller, equal, or larger than *y*.

.. function:: int mag_cmp_2exp_si(const mag_t x, long y)

    Returns negative, zero, or positive, depending on whether *x*
    is smaller, equal, or larger than `2^y`.

Arithmetic
-------------------------------------------------------------------------------

.. function:: void mag_mul_2exp_si(mag_t z, const mag_t x, long y)

.. function:: void mag_mul_2exp_fmpz(mag_t z, const mag_t x, const fmpz_t y)

    Sets `z` to `x \times 2^y`. This operation is exact.

.. function:: void mag_mul(mag_t z, const mag_t x, const mag_t y)

    Sets `z` to an upper bound for `xy`.

.. function:: void mag_addmul(mag_t z, const mag_t x, const mag_t y)

    Sets `z` to an upper bound for `z + xy`.

.. function:: void mag_add_2exp_fmpz(mag_t z, const mag_t x, const fmpz_t e)

    Sets `z` to an upper bound for `x + 2^e`.

.. function:: void mag_div(mag_t z, const mag_t x, const mag_t y)

    Sets `z` to an upper bound for `x / y`.

Fast versions
-------------------------------------------------------------------------------

The following methods assume that all inputs are finite and that all exponents
(in all inputs as well as the final result) fit as *fmpz* inline values.
They also assume that the output variables do not have promoted exponents,
as they will be overwritten directly (thus leaking memory).

.. function:: void mag_fast_init_set(mag_t x, const mag_t y)

    Initialises *x* and sets it to the value of *y*.

.. function:: void mag_fast_zero(mag_t x)

    Sets *x* to zero.

.. function:: int mag_fast_is_zero(const mag_t x)

    Returns nonzero iff *x* to zero.

.. function:: void mag_fast_mul(mag_t z, const mag_t x, const mag_t y)

    Sets `z` to an upper bound for `xy`.

.. function:: void mag_fast_addmul(mag_t z, const mag_t x, const mag_t y)

    Sets `z` to an upper bound for `z + xy`.

.. function:: void mag_fast_add_2exp_si(mag_t z, const mag_t x, long e)

    Sets `z` to an upper bound for `x + 2^e`.

Input and output
-------------------------------------------------------------------------------

.. function:: void mag_print(const mag_t x)

    Prints *x* to standard output.

Random generation
-------------------------------------------------------------------------------

.. function:: void mag_randtest(mag_t x, flint_rand_t state, long expbits)

    Sets *x* to a random finite value, with an exponent up to *expbits* bits large.

.. function:: void mag_randtest_special(mag_t x, flint_rand_t state, long expbits)

    Like :func:`mag_randtest`, but also sometimes sets *x* to
    infinity.

Conversions
-------------------------------------------------------------------------------

.. function:: void mag_set_d_2exp_fmpz(mag_t z, double x, const fmpz_t y)

.. function:: void mag_set_fmpz_2exp_fmpz(mag_t z, const fmpz_t x, const fmpz_t y)

    Sets *z* to an upper bound for `x \times 2^y`.

.. function:: void mag_set_fmpr(mag_t y, const fmpr_t x)

    Sets *y* to an upper bound for *x*.

.. function:: void mag_get_fmpr(fmpr_t y, const mag_t x)

    Sets *y* exactly to *x*.

.. function:: void mag_get_fmpq(fmpq_t y, const mag_t x)

    Sets *y* exactly to *x*. Assumes that no overflow occurs.
