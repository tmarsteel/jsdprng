# jsdprng

Implementation of a DPRNG algorithm aimed at being simple to implement in almost any general-purpose programming language. Its intended use are simple games; scenarios where a pseudo-random sequence has to be replayed deterministically with low memory and computational cost. **The algorithm is not secure. DO NOT USE THIS FOR SECURITY RELATED TASKS.**

A PHP implementation that generates equal sequences for the same salts/seeds can be found [here](//github.com/tmarsteel/php-dprng)

Please contribute with your own implementation in your favourite language! A description of the algorithm and test vectors can be found in [algorithm.md](algorithm.md). I`ll cross-link compliant forks :)

## Usage
Include the file or require() it.

```js
// secure random numbers (seeded from Math.random())
var rng = new tmarsteel.DPRNG();

// deterministic random sequence
var seed = 0xA2F38C0;
var rng = new tmarsteel.DPRNG(seed);

// generate random numbers
var random = rng.next(); // random float from 0 inclusive to 1 exclusive (same range as Math.random())

var random = rng.nextInt(14, 300); // random integer in the range 14 to 299

var bytes = rng.nextBytes(30); // 30 random integers in the range 0 to 255
```

## Methods
Here is a full list of the methods supported by `tmarsteel.DPRNG` objects and their signatures + contracts:

**`float next()`**  
Returns an uniformly distributed float value in the range `[0, 1)` (0 inclusive to 1 exclusive).

**`float nextFloat(float from, float to)`**  
Returns an uniformly distributed float value in the range `[from, to)` (`from` inclusive to `to` exclusive).

**`int nextInt(int from, int to)`**  
Returns an uniformly distributed integer in the range `[from, to]` (`from` inclusive to `to` inclusive).

**`array nextBytes(int n)`**  
Returns an array of length `n`. Each entry is a uniformly distributed integer between 0 and 255.

## Entropy / Randomness inspection
[ENT](http://www.fourmilab.ch/random/) is a neat program to inspect the entropy and randomness of a sequence of bytes or bits. This table compares its output with values returned from Math.random() in the most common browsers to that produced by this DPRNG. I. For explanations on the metrics see [the ENT Website](http://www.fourmilab.ch/random/). **These numbers do only show that output of this DPRNG is sufficently random to be indistinguishable from random noise to HUMANS. Computers will be able to work out the initial salt given a RIDICULOUSLY SMALL SAMPLE!**

| RNG | Browser | Entropy | Arithmetic mean | Chi-Square % | Correlation coefficient | Monte-Carlo PI error % |
| :-- | :------ | -------------------------: | --------------: | ---------: | ----------------------: | ---------------------: |
Math.random() | Chrome 44.0.2403 | 7.993701 | 127.5596 | 92.65 | \-0.017640 | 0.7 |
Math.random() | Firefox 38.0.1 | 7.993797 | 127.8165 | 94.87 | -0.009468 | 0.94 |
Math.random() | Edge 25.10586 | 7.992885 | 128.0175 | 52.22 | -0.003019 | 0.85 |
Math.random() | IE 11.0.9600 | 7.992426 | 127.7681 | 27.49 | -0.008467 | 1.06 |
Math.random() | Opera 35.0.2066.37 | 7.992634| 127.5248 | 38.81 | \-0.001458 | 0.88 |
tmarsteel.DPRNG | any | 7.993173 | 127.2305 | 72.12 | 0.002086 | 1.24 |

*You can run these tests yourself with the ent executable and the [entTestfile.html](ent-test/entTestfile.html) script.*
