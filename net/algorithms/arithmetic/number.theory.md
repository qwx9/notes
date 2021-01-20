# Number theory

## Modular exponentiation

...

### Implementation pitfalls

Integer overflow can occur if base ≥ 0x10000:

	acid: 0x10000*0x10000 % 7
	0x00000004
	acid: (0x10000*0x10000 & 0xffffffff) % 7
	0x00000000

Hence, it can occur if mod > 0x10000 as well.

Wikipedia adds an assertion guarding against overflows,
but it is unclear to me why this is a problem at all in this case.

_Cormen et al_ describe a left-to-right algorithm,
which can be slightly more efficient,
but only if the first bit set is already known,
since the exponent needs to be scanned left to right.
Generally, a right-to-left variant is more convenient.

According to Schneier, _Montgomery's_ and _Barrett's_ algorithms
are another two efficient methods to do modular reduction many times
using the same modulo.

A paper cited in the book compares the three algorithms' performance.
The right-to-left square and multiply method
is the best choice for singular modular reductions.
Barrett's algorithm is best for small arguments.
Montgomery's method is best for general modular exponentiations,
and can also take advantage of small exponents,
using something called mixed arithmetic.


### References
- Cormen et al, p 965: raising to powers with repeated squaring
- Knuth, vol2, p 462: evaluation of powers
- Schneier, p 244: modular arithmetic
