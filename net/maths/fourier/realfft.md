# real fast fourier transform

- see numerical recipes 2e, pp. 510-514
- see numerical recipes 3e, pp. 617-620


## fft input and output buffer sizes

- there's some confusion in the numerical recipes notation
- 2e uses 1-indexed values, 3e uses 0-indexed values
- it distinguishes complex array size (N) and real array size (2N)
- the reason is we store the imaginary values as reals
interleaved with the real values
- hence, N complex numbers, 2N real+imaginary numbers
- so, the output array's size is the same as the input array's
- this is the first source of confusion
- you'd think this was clear and obvious,
especially since the text explicitely mentions this,
multiple times,
but no


## fourier transform output

- next, the fourier transform yields
a positive and a negative frequency part
- each is of size N/2, so the total size is again N
- the array starts with freq=0
and the positive half, which ends at the most positive frequency
- the most positive frequency value is ambiguous
as it's also the most negative frequency's value
- hence the term "aliased" with most negative frequency
- the negative frequency half starts
from the second most negative frequency
and ends with the frequency just below 0


## complex vs. real fft

- a real signal has every sample's imaginary part set to 0
- this would be the case in an audio signal
- because of this property,
the transform's negative half is symmetrical to the positive,
and thus redundant
- this is why we only keep the positive half
- however, the output is still complex values
- with complex fft,
we would need an input vector twice the size of the signal,
to interleave the imaginary parts,
and an output vector of twice the size
for the positive and negative frequency halves
- here, we don't need to do that,
so N real samples = N input complex values
with imaginary component = 0
- N input complex values = N output complex values
- however, we only store the positive frequency half,
since the negative is the same
- hence N input real values = N output complex values


## implementation

- following the previous discussion,
the fourier transform of the real input values
is computed in place
and is the size of the input buffer
- the length arguments given to both four1 and realft
are the number of *complex* values,
hence for audio the number of samples,
not the size of the real buffer,
so it's still N
- the real source of confusion
is a difference in notation and a typo
between the two editions
- the implementations of four1 and realft
are strictly equivalent
- 2e uses nn as the number of complex samples,
and n as the number of real samples
- 3e does the opposite and changes the notation of one assignment
- 2e's realft has nn as argument, but uses n in the code
- 3e corrects both the typo and the notation:
it makes more sense to use nn for 2N and n for N
- hence, the two versions seem different at first,
and one has a silly bug
- add to this the slight indirection in 3e
because of the c++ boilerplate wrapping complex vectors
- the value of π inlined in the book code
seems equal to our libc PI,
so we can safely use that instead


## buffer sizes and fplay(1)

- there was much confusion because of the previous issues
- poorly understood implications lead to erroneous buffer sizes,
hence memory corruption issues
- from there on,
the input array's size was multiplied by 4,
once by 2 because output was assumed to be twice the size,
and again to account for the imaginary component of the output
- the input data was still just the N audio left channel values
- the output was assumed to be of size 2N (T*N),
with both positive and negative halves,
so the frequency content loop ignored the second half
- so, for each pixel,
twice as many points were used,
when the second half of the signal buffer,
empty at the start,
is also empty at the end
- so, assuming 2N complex values for N real values
is on problem
- the more serious one,
and the reason memory corruption still occasionally occurred,
is that realft was provided 2N as well
for the number of data points,
where it should've been N
- because N samples → nfft set to 4N real values,
and realft called with nfft,
under the assumption that we get 2N complex values from N real ones,
and that realft and four1 take as a size argument
the number of output real values,
not complex ones
- this is all a combination of understandable confusion
and cargo-culting trial-and-error
