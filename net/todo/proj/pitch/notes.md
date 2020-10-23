# pitch recognizer/tracker

## todo

- basic sample read, wraparound, see notes
- parse fft output, calculate magnitude, find dominant peak
- basic drawing: just peak value ± nearest note and Δ at first
- complete display (see below)
- restrict parsing range to note frequencies (see below)
- add low-pass filter
- use windowing function


## display

- y units are not Hz, we space notes evenly
- horizontal lines for each note to make clear how exact or not pitch is
- draw spectrogram over freq range, same idea as fplay
- light up current note
- scroll window once border reached
- don't keep more history than the screen size?
- alternative display possibility (or bottom): classic tuner with note centered


## fft output and range

- sampling frequency = 44.1kHz
- nfft = 44100 / factor, rounded up to next power of 2
	* 44100/20 = 2205 → 4096
	* this is stupid, just choose a power of 2
- freqs: multiples of Fs / nfft = resolution
- N input complex samples = N output complex samples
- but, positive and negative half
- highest absolute frequency is Fs/2 = 22.05kHz
- next, we take only the positive half of the output
- so, output = N/2 complex samples for freqs 0*Fs/N to N/2*Fs/N
- but in its simplest form, we don't need to look at all freqs up to Fs/2
- 128 note values C0 - B8
	* C0 16.36 Hz
	* B8 4186.01 Hz
- most common instrument: 88 key piano
	* A0 27.50 Hz
	* C8 4186.01 Hz
- most common range for voice (assumption)
	* C2 65.41 Hz
	* C6 1046.50 Hz


## accuracy vs delay

- for Fs=44100 and N=4096, resolution = 10.77Hz
- not great, difficult to accurately tell flat/sharp or exact
- we can only either decrease sampling frequency or increase sample size
- but, we can't really touch Fs,
the signal has already been sampled,
downsampling wouldn't change anything
- so, with this approach, we can only increase sample size
- this means increasing the delay
- resolution of ≤ 1Hz ⇒ slice = 44100 → N = 65536
	* 0.67Hz resolution, 1.46 seconds
	* that's fucking long
- N = 32768 → 1.35Hz, 0.74 seconds
- so, test which delay is acceptable, really


## approach

- what we want is an accurate pitch tracker with acceptable performance
- using a simple fft is one possible approach,
but naive use is insufficient
- http://blog.bjornroche.com/2012/07/frequency-detection-using-fft-aka-pitch.html
	* technical term = pitch tracking
	* at minima, we need low-pass filtering and windowing


## improvements

- read on low-pass filter design (dsp books)
- overlapping windows, or other functions
- other time-domain and frequency-domain solutions exist
	* see bookmarks on phone
