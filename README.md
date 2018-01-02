# NAME

Math::GF - Galois Fields arithmetics

# VERSION

This document describes Math::GF version {{\[ version \]}}.

# SYNOPSIS

    use Math::GF;

# DESCRIPTION

This module allows you to generate and handle operations inside a Galois
Field (GF) of _any_ allowed order:

- orders that are too big are likely to explode
- orders that aren't prime number powers do not have associated Galois
Fields.

It's easy to generate a new GF of a given order:

    my $GF5 = Math::GF->new(order => 5);
    # GF(8) is GF(2**3)
    my $GF8 = Math::GF->new(order => 8);

Since a GF of order _N_ has exactly _N_ elements, it's easy to refer to
them with integers from _0_ to _N - 1_. If you want to actually
generate the associated element you can use the ["e"](#e) method:

    my $e5_gf8 = $GF8->e(5);

If you're planning to work extensively with a specific GF, or just want
some syntactic sugar, you can import a factory function in your package
that will generate elements in the specific GF:

    # by default, import a function named GF_p_n for GF(p^n)
    Math::GF->import_builder(8);
    my $e5 = GF_2_3(5);

    # you can give your name though
    Math::GF->import_builder(8, name => 'GF8');
    my $e5_gf8 = GF8(5);

If you need all elements, look at the ["all"](#all) method. It's the same as
doing this:

    my @all = map { $GF8->e($_) } 0 .. 8 - 1;

but easier to type and possibly a bit quicker.

Elements associated to _0_ and _1_ have the _usual_ meaning of the
additive and multiplicative neutral elements, respectively. You can also
get them with ["additive\_neutral"](#additive_neutral) and ["multiplicative\_neutral"](#multiplicative_neutral).

# METHODS

In the following, `$GF` is supposed to be a `Math::GF` object.

## **additive\_neutral**

    my $zero = $GF->additive_neutral;

the neutral element of the Galois Field with respect to the addition
operation. Same as `$GF->e(0)`.

## **all**

    my @all_elements = $GF->all;

generate all elements of the Galois Field.

## **e**

    my $e5 = $GF->e(5);
    my @some = $GF->e(2, 3, 5, 7);

factory method to generate one or more elements in the field. When called
in scalar context it always operate on the first provided argument only.

## **element\_class**

    my $class_name = $GF->element_class;

the underlying class for generating elements. It defaults to
[Math::GF::Zn](https://metacpan.org/pod/Math::GF::Zn) when the ["order"](#order) is a prime number and
[Math::GF::Extension](https://metacpan.org/pod/Math::GF::Extension) when it is not; there is probably little
motivation for you to fiddle with this.

## **import\_builder**

    Math::GF->import_builder($order, %args);

import a factory function in the caller's package for easier generation of
elements in the GF of the specified `$order`.

By default, the name of the imported function is `GF_p` or `GF_p_n`
where `p` is a prime and `n` is the power of the prime such that `$order = p ** n` (the `n` part is omitted if it is equal to `1`). For
example:

    Math::GF->import_builder(5); # imports GF_5()
    Math::GF->import_builder(8); # imports GF_2_3()

You can pass your own `name` in the `%args` though:

    Math::GF->import_builder(8, name => 'GF8'); # imports GF8()

The imported function is a wrapper around ["e"](#e):

    my $one = GF_2_3(1);
    my @some = GF_5(1, 3, 4);

Allowed keys in `%args`:

- `level`

    by default the function is imported in the caller's package. This allows
    you to alter which level in the call stack you want to peek for importing
    the sub.

- `name`

    the name of the method, see above for the default.

## **multiplicative\_neutral**

    my $one = $GF->multiplicative_neutral;

the neutral element of the Galois Field with respect to the multiplication
operation. Same as `$GF>e(1)`.

## **n**

    my $power = $GF->n;

the ["order"](#order) of a Galois Field must be a power of a prime ["p"](#p), this
method provides the value of the power. E.g. if the _order_ is `8`, the
prime is `2` and the power is `3`.

## **order**

    my $order = $GF->order;

the _order_ of the Galois Field. Only powers of a single prime are
allowed.

## **order\_is\_prime**

    my $boolean = $GF->order_is_prime;

the _order_ of a Galois Field can only be a power of a prime, with the
special case in which this power is 1, i.e. the _order_ itself is a prime
number. This method provided a true value in this case, false otherwise.

## **p**

    my $prime = $GF->p;

the ["order"](#order) of a Galois Field must be a power of a prime, this method
provides the value of the prime number. E.g. if the _order_ is `8`, the
prime is `2` and the power is `3`. See also ["n"](#n).

## **prod\_table**

    my $pt = $GF->prod_table;

a table that can be used to evaluate the product of two elements in the
field.

The table is provided as a reference to an Array of Arrays. The elements
in the field are associated to indexes from `0` to `order - 1`;
table element `$pt->[$A][$B]` represents the result of the product
between element associated to `$A` and element associated to `$B`.

You shouldn't in general need to fiddle with this table, as it is used
behind the scenes by `Math::GF::Extension`, where all operations are
overloaded.

## **sum\_table**

    my $st = $GF->sum_table;

a table that can be used to evaluate the product of two elements in the
field.

The table is provided as a reference to an Array of Arrays. The elements
in the field are associated to indexes from `0` to `order - 1`;
table element `$pt->[$A][$B]` represents the result of the addition
between element associated to `$A` and element associated to `$B`.

You shouldn't in general need to fiddle with this table, as it is used
behind the scenes by `Math::GF::Extension`, where all operations are
overloaded.

# BUGS AND LIMITATIONS

Report bugs either through RT or GitHub (patches welcome).

# SEE ALSO

[Math::Polynomial](https://metacpan.org/pod/Math::Polynomial) is used behind the scenes to generate the tables in
case the order is not a prime.

[Math::GF::Zn](https://metacpan.org/pod/Math::GF::Zn) is used for generating elements in the field and handling
operations between them in an easy way in case of prime ["order"](#order).
[Math::GF::Extension](https://metacpan.org/pod/Math::GF::Extension) is used for elements in the field in case of
non-prime ["order"](#order)s.

# AUTHOR

Flavio Poletti <polettix@cpan.org>

# COPYRIGHT AND LICENSE

Copyright (C) 2017, 2018 by Flavio Poletti <polettix@cpan.org>

This module is free software. You can redistribute it and/or modify it
under the terms of the Artistic License 2.0.

This program is distributed in the hope that it will be useful, but
without any warranty; without even the implied warranty of
merchantability or fitness for a particular purpose.
