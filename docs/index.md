# Why?

The main motivation for writing [Math::GF][] was for investigating increasingly
bigger versions of a game I bought some time ago, named [Dobble][] (or *Spot
it* in some markets). The main motivation for writing these notes is... to
avoid forgetting about them.

# The Game(s)

[Dobble][] is based on a deck of 55 round cards with pictures printed on them.
Each card holds 8 different pictures. I didn't physically count the pictures,
but there MUST be 57 of them, otherwise the maths don't work.

The interesting property of the deck is that, however you choose a pair of
cards, they always share exactly one picture. The deck comes with a few
*mini-games* where you are supposed to *spot* the common picture in the
cards on the table, trying to be the quickest among the players.

This property would have allowed two additional cards, to cope with
a 57-cards deck. I don't know why they tossed two cards away. This skews
the game more towards some of the pictures (15 of them are shown one time
less than the others, and one of them is shown two times less) but
whatever.

# Some Internet Research

Looking around, I came across [this article][se-math] in [StackExchange
Mathematics][] where I got most of the facts. This is what stuck in my
mind:

- the game is easily associated to [projective plane][]s built over
  a finite number of elements (points)
- the only known [projective plane][]s are those build over [finite
  field][]s

To have a generalization, I now had to figure out how to generate
a [projective plane][] based on a [finite field][], and of course how to
find a [finite field][] of a given size. This is what the rest of this
page is about; we will start from [finite field][]s as they are needed to
build [projective plane][]s.

# Finite Field

[StackExchange Mathematics] has some interesting [notes regarding
a generic approach for building][se-math-fields] [finite
field][]s.

The quick facts are:

- every [finite field][] has an order that MUST be a positive integer
  power of a prime \\( p \\) (i.e. \\( p^n \\) with \\( n >= 1 \\));

- if the power is \\( 1 \\), the field is isomorphic to \\( Z_p \\) and it
  is usually called \\( GF(p) \\);

- otherwise, if the power is \\( n > 1 \\), the field can be built as an
  *extension* of \\( Z_p \\) that is usually called \\( GF(p^n) \\).

The `GF` here stands for *Galois Field*, which is the usual name assigned
to these [finite field][]s from the great mathematician [Évariste
Galois][].

## \\( GF(p) \\)

It is immediately quick to build infinitely finite fields: just take
a prime \\( p \\) and consider the field \\( Z_p \\), based on the set of
remainders modulo \\( p \\) with the *usual* definitions of sum and
product over this set (sum is modulo \\( p \\), product is modulo \\(
p \\)). So:

- elements can be represented by all integers between \\( 0 \\) and \\(
  p - 1 \\). We will denote these elements as \\( [0] \\), \\( [1] \\) and
  so on up to \\( [p-1] \\);

- sum of two elements is as follows:

\\[ [z] = [x] + [y] \rightarrow z = (x + y)_p \\]

- product of two elements is as follows:

\\[ [z] = [x] \cdot [y] \rightarrow z = (x \cdot y)_p \\]

where \\( (x)_p \\) represents the remainder of \\( x \\) modulo
\\( p \\).

## \\( GF(p^n) \\) with \\( n > 1 \\)

For orders that are not prime but still *possible*, e.g \\( 4 = 2^2 \\),
\\( 8 = 2^3 \\) and \\( 9 = 3^2 \\), the trick is to build an *extension*.

Just as a reminder, \\( Z_q \\) with non-prime \\( q \\) is a commutative
ring but it is **not** a field. In fact, in this case we can factor the
integer order as \\( q = a \cdot b \\) with both \\( a \\) and \\( b \\)
less than \\( q \\), and have:

\\[ [a] \cdot [b] = [a \cdot b] = [0] \\]

i.e. the product is not an internal operation in \\( Z_q \ {[0]} \\).

So, we have to resort to the *extension* trick, which will work only for
powers of primes. I do really think this is a trick, although a neat one,
so here's how it works more or less:

- start from \\( Z_p \\), which we know is a field;
- build a vector space over \\( Z_p \\) with \\( n \\) dimensions. For
  reasons that I studied a long time ago, every such vector space ends up
  being isomorphic to *n-ples* of elements in \\( Z_p \\), where the sum
  of two vectors is "just" the sum of the respective coordinates in
  \\(Z_p\\)
- start considering these vectors as elements of your new candidate field,
  and...
- find a suitable *product* operation for these vectors, also known as
  *elements of your new field*, such that it respects the definition of
  a product in a field.

The first step is trivial, just consider the rests modulo \\(p\\) as we
did in the previous section.

The second step is trivial as well: just build all the *n-ples*. Each
position in the *n-ple* can range from \\([0]\\) to \\([p-1]\\) (\\(p\\)
possible values) and there are \\(n\\) of them, so there are in total
\\(p^n\\) possible vectors.

The third step is just a mind shift. Take each *n-ple* as an object of
a set. This set has \\(p^n\\) elements. Curious, we're after a field with
this exact number of elements inside! It also has a *sum* operation out of
the box, and the elements form a commutative group with respect to this
operation (it's a vector space!).

So yes, we're just a *product* operation away from our field! If this
vector-space turned into a field thing seemed a bit tricky, here come
dragons.

## Primes in a Vector Space

As we saw in a previous section, primes work fine in building up fields.
It is so because, by definition, you cannot factor a prime into two
smaller integers, hence the product of non-zero elements in \\(Z_p\\)
always yields a non-zero elements, which means it is good for a field. It
also helps that it is commutative, of course.

Now, the other very tricky intuition was that we can try to replicate some
of this in a vector space too. It all boils down on defining the right
product operation, but to do this it is useful to map our vectors onto
*polynomials*, because these are object we are somehow comfortable with.

It's easy to associate a polynomial to each *n-ple*: just do this:

\\[(a_0, a_1, ..., a_{n-1}) \rightarrow a_0 + a_1x + ... + a_{n-1}x^{n-1}\\]

Varying the different coefficients \\(a_i\\) we can generate any
polynomial whose degree is less than \\(n\\).

It's also easy to see that the sum of two polynomials is a polynomial
whose coefficients are the sum of the respective coordinates of the two
initial polynomials. So it's basically the same sum operation, just with
a different dress.

Now, we need something that can take the role of a *prime*, but in the
polynomial world.

If we follow our analogy with prime integers, we know that this "prime"
must be "slightly" bigger than each polynomial we can generate with our
*n-ples*. This is easy: with \\(n\\) coefficients we can generate all
possible polynomials of degree less than \\(n\\), so a "suitable"
polynomial of degree \\(n\\) will suffice.

This is where [irreducible polynomial][]s of degree \\(n\\) come to the
rescue. As the name says, they cannot be *reduced*, i.e. it is not
possible to express them in terms of a product of two other polynomials
(over the same field) that have a lower degree greater than 0. It's what
you can expect from a polynomial to be considered *just like* a prime.

## Product Modulo Irreducible Polynomial

So, if we find out such a polynomial, interesting consequences arise, the
best being that we can eventually define our product operation for the
field: just multiply the two polynomials associated to the *n-ples*
(assuming that both have at least one coefficient that is not zero), then
divide the result by the irreducible polynomial and consider the
polynomial that is left as a *rest*. Its degree will be less than that of
the divisor, i.e. it will be less than \\(n\\); additionally, the
mathematical properties of the irreducible polynomial also make it
possible to say that this product is not the zero-th polynomial. Yay!

To fix ideas, let's consider \\(GF(2^2)\\). The vector field has the
following elements (we will assign a letter to each):

      A        B        C        D
    (0, 0)   (0, 1)   (1, 0)   (1, 1)

It's easy to see that \\(A\\) is the neutral element with respect to the
sum, and that the following summing table applies taking into
consideration the sum of respective coordinates modulo 2:

    + A B C D
     +-------
    A|A B C D
    B|B A D C
    C|C D A B
    D|D C B A

This is actually the same table we would find considering the polynomials
associated to each element, namely:

\\[A \rightarrow 0 \\\
B \rightarrow 1 \\\
C \rightarrow x \\\
D \rightarrow x + 1 \\\]

Now we need to compute the product table, and for this we need an
irreducible polynomial of degree 2 over \\(Z_2\\). It turns out that this
polynomial is one and only one: \\(x^2 + x + 1\\). Let's see what happens
by first computing the products

\\[A \cdot X = X \cdot A \rightarrow (0) \cdot X = X \cdot (0) = (0) \\\
B \cdot X = X \cdot B \rightarrow (1) \cdot X = X \cdot (1) = X \\\
C \cdot C \rightarrow (x) \cdot (x) = x^2 \\\
C \cdot D = D \cdot C \rightarrow (x) \cdot (x + 1) = (x + 1) \cdot (x) = x^2 + x \\\
D \cdot D \rightarrow (x + 1) \cdot (x + 1) = x^2 + 1 \\]

where uppercase \\(X\\) is any of \\({A, B, C, D}\\).

Now we must compute the rests modulo the irreducible polynomial:

\\[(0) \mod (x^2 + x + 1) = (0) \rightarrow A \\\
(X) \mod (x^2 + x + 1) = (X) \rightarrow X \\\
(x^2) \mod (x^2 + x + 1) = (x + 1) \rightarrow D \\\
(x^2 + x) \mod (x^2 + x + 1) = (1) \rightarrow B \\\
(x^2 + 1) \mod (x^2 + x + 1) = (x) \rightarrow C \\]

So, we have our multiplicative table at last:

    * A B C D
     +-------
    A|A A A A
    B|A B C D
    C|A C D B
    D|A D B C

It's easy to see that this is indeed a *good* multiplicative table for a field.

## Irreducible Polynomial of Order \\(n\\)?

Now that we have a trick, we still have to find out one last way to
actually find an irreducible polynomial of the desired order \\(n\\) over
the field we are extending. There are some results about it:

- they actually exist! There is someone that calculated a formula to count
  them, which also implies that fields of order \\(p^n\\) exists, of
  course! See [this question][irred-count] for further information;
- there always exist *monic* ones, i.e. where the coefficient for the
  highest power of \\(x\\) is \\(1\\) (which simplifies the division and
  the rest calculation);
- there's more than one way to test for the irreducibility of
  a polynomial... but we only need one, of course.

For the second bullet, we will refer to [Rabin's test for
irreducibility][rabin-test] with the algorithm that follows (in Perl)
built using `Math::GF` (and in particular using elements in
`Math::GF::Zp`) and [Math::Polynomial][]:

    # Input $f is a Math::Polynomial object built over Zp
    sub rabin_irreducibility_test {
       my $f    = shift;
       my $n    = $f->degree;
       my $one  = $f->coeff_one;
       my $pone = Math::Polynomial->monomial(0, $one);
       my $x    = Math::Polynomial->monomial(1, $one);
       my $q    = $one->n;
       my $ps   = prime_divisors_of($n);
    
       for my $pi (@$ps) {
          my $ni  = $n / $pi;
          my $qni = $q**$ni;
          my $h = (Math::Polynomial->monomial($qni, $one) - $x) % $f;
          my $g = $h->gcd($f, 'mod');
          return if $g->degree > 1;
       } ## end for my $pi (@$ps)

       my $t = (Math::Polynomial->monomial($q**$n, $one) - $x) % $f;
       return $t->degree == -1;
    } ## end sub rabin_irreducibility_test

The call to `prime_divisors_of($n)` returns all distinct prime divisors of
`$n`.

The test above is slightly different from the one described in the
Wikipedia page, but not that much.

So... to find an irreducible polynomial of a pre-defined degree \\(n\\)
over a pre-defined field \\(Z_p\\), we can just start enumerating all
*monic* polynomials from \\(x^n + 1\\) on and apply the test... we will
eventually hit one!


# Projective Plane

The definition of [projective plane][] is the following:

> A projective plane consists of a set of lines, a set of points, and
> a relation between points and lines called incidence, having the
> following properties:
>
> - Given any two distinct points, there is exactly one line incident with
> both of them.
>
> - Given any two distinct lines, there is exactly one point incident with
> both of them.
>
> - There are four points such that no line is incident with more than two
> of them.

It's easy to see how this relates to [Dobble][] actually: if we call the
pictures *points* and the cards *lines*, the second property is the same
as the deck's property ("given any two distinct *cards*, there is exactly
one *picture* incident with both of them"). The definition has additional
stuff of course, but I wasn't bothered by that (the gut feeling being:
it's probably there to make stuff work).

Building a [projective plane][] out of a [finite field][] turns out to be
quite simple but I didn't find really exhaustive explanations around. This
pushed me to play with the idea programmatically, and it was actually
where I started before thinking about [Math::GF][].

The steps are the following:

- fix the field. As we saw, there are infinite [finite field][]s, but not
  for every possible integer order (e.g., there is none for order 6, as it
  is [nicely explained here][finite-field-order-6]);
- find out all possible points in the projective plane. If the field has
  order \\( n \\), it results in a projective plane with
  \\( n^2 + n + 1 \\) points (and lines, for duality)
- find out how points are grouped into lines.

The first bullet has already been addressed. To cope with the other two,
it's useful to start from [homogeneous coordinates][], because they make
it so easy to address the second bullet!

## Homogeneous Coordinates

The [homogeneous coordinates][] have probably been invented to cope with
[projective plane][]s. Well, this is my understanding/wish at least, but
they blend so well.

We will not get into the details of how they are built and their
properties - look at the articles around for this - but it's useful to
remember a few things.

In a plane, you are used to use two different coordinates, one for each of
the two *axes*. [Homogeneous coordinates][homogeneous coordinates] add
a third one, which can be set to 1 for each point in the plane, like this:

\\[ (x, y) \rightarrow (x_1, x_2, x_3) = (x, y, 1) \\]

We will use \\( (x_1, x_2, x_3) \\) to represent the [homogeneous
coordinates][], as you might have catched by now. Up to now it's pretty
boring.

The definition is actually a bit wider: if you want to turn a point in
[homogeneous coordinates][] into the *regular* ones, just divide the first
two by the third. This is why we put a \\( 1 \\) here:

\\[ \frac{x_1}{x_3} = \frac{x}{1} = x \\\
\frac{x_2}{x_3} = \frac{y}{1} = y \\]

This also tells us that not all distinct triples turn out to be distinct
[homogeneous coordinates][], because all triples that are multiple of each
other by some non-zero factor are actually mapped onto the same point:

\\[
(x_1, x_2, x_3) = (2 x, 2 y, 2) \\\
\frac{x_1}{x_3} = \frac{2 x}{2} = x \\\
\frac{x_2}{x_3} = \frac{2 y}{2} = y
\\]

The other interesting thing is that [homogeneous coordinates][] allow us
to represent points that are not inside the regular plane, which is what
happens when we set the last coordinate to \\( 0 \\). This puts
a technical requirement to have at least one of the other two coordinates
to be different from \\( 0 \\), and each of these *additional external
points* are called *points at infinity*.

It's easy to relate these points at infinity with regular lines in the
starting plane. As a matter of fact, each of these points represents where
*parallel lines* meet at infinity.

The set comprising all these points at infinity is defined to be a line by
itself, called the *line at infinity*.

## Points In The Projective Plane

A [projective plane][]'s points can be easily represented using triples
representing [homogeneous coordinates][], taking care to remember that
these coordinates are the same if they are multiples of each other
(because of how [homogeneous coordinates][] work). The triples will be
formed using elements from the selected field, of course.

To make an example, let's consider the [projective plane][] built over the
field \\( GF(3) \\) (also known as \\( Z_3 \\)). All possible triples are
the following:

    (0, 0, 0)  (1, 0, 0)  (2, 0, 0)
    (0, 0, 1)  (1, 0, 1)  (2, 0, 1)
    (0, 0, 2)  (1, 0, 2)  (2, 0, 2)
    (0, 1, 0)  (1, 1, 0)  (2, 1, 0)
    (0, 1, 1)  (1, 1, 1)  (2, 1, 1)
    (0, 1, 2)  (1, 1, 2)  (2, 1, 2)
    (0, 2, 0)  (1, 2, 0)  (2, 2, 0)
    (0, 2, 1)  (1, 2, 1)  (2, 2, 1)
    (0, 2, 2)  (1, 2, 2)  (2, 2, 2)

There are 27 of them (namely, \\( 3^3 \\)), but we know that only
\\( 3^2 + 3 + 1 = 13 \\) of them are actually good. We know why:

- the triple \\( (0, 0, 0) \\) violates the rule that at least one of the
  coordinates must be different from \\( 0 \\)
- some of them are multiple to each other, e.g. \\( (0, 0, 2) \\) is
  a multiple of \\( (0, 0, 1) \\).

If you eliminate the impossible triple, and the "duplicates", you actually
end up with 13 remaining triples, representing the 13 distinct points in
the example [projective plane][].

It turns out that there is a simple trick to find out all the distinct
points in the general case: just keep all the triples whose "first
non-zero coordinate from left to right" is \\( 1 \\). Hence, in the example
above:

- \\( (0, 0, 0) \\) is rejected as it has no \\( 1 \\) inside;

- \\( (0, 1, 2) \\) is kept, because the first non-zero coordinate from
  left to right is \\( x_2 \\) and is actually valued \\( 1 \\)

- \\( (2, 0, 0) \\) is rejected, because the first non-zero coordinate
  from the left is \\( x_1 \\) but it has value \\( 2 \\) (i.e. it is
  different from \\( 1 \\)).

Intuitively, we can notice that:

- there is always one single element with two leading zeroes, namely
  \\( (0, 0, 1) \\);

- there are exactly \\( n \\) elements with one leading zero, namely
  \\( (0, 1, a) \\) with \\( a \\) any element in the field;

- there are exactly \\( n^2 \\) elements with a leading one, namely
  \\( (1, a, b) \\) with \\( a \\) and \\( b \\) elements in the field;

which amounts to a total \\( 1 + n + n^2 \\) triples, i.e. what we expect.




[Math::GF]: https://github.com/polettix/Math-GF
[Dobble]: https://boardgamegeek.com/boardgame/63268/spot-it
[se-math]: http://math.stackexchange.com/a/466379/264102
[StackExchange Mathematics]: http://math.stackexchange.com/
[projective plane]: https://en.wikipedia.org/wiki/Projective_plane
[finite field]: https://en.wikipedia.org/wiki/Finite_field
[finite-field-order-6]: http://math.stackexchange.com/questions/183462/can-you-construct-a-field-with-6-elements
[homogeneous coordinates]: https://en.wikipedia.org/wiki/Homogeneous_coordinates
[se-math-fields]: http://math.stackexchange.com/a/42163/264102
[Évariste Galois]: https://en.wikipedia.org/wiki/%C3%89variste_Galois
[irreducible polynomial]: https://en.wikipedia.org/wiki/Irreducible_polynomial
[irred-count]: http://math.stackexchange.com/questions/152880/how-many-irreducible-polynomials-of-degree-n-exist-over-mathbbf-p
[rabin-test]: https://en.wikipedia.org/wiki/Factorization_of_polynomials_over_finite_fields#Rabin.27s_test_of_irreducibility
[Math::Polynomial]: https://metacpan.org/pod/Math::Polynomial
