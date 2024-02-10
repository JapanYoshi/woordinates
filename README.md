# Woordinates

Woordinates (pronunciation follows) is a geohash system with a minimum resolution of 5m×5m, which uses a sequence of 4 English words to encode the longitude and latitude.

Woordinates can be used to quickly share a precise location (but not overly precise) when an address is not granular enough, e.g. in a park, in the countryside, on a highway, or in a mountain range.

This idea is based on FourWordsFindWhere, a proposal to fix some of the shortcomings of what3words [as described in Andrew Steele’s video](https://www.youtube.com/watch?v=SqK0ciE0rto).

## Differences from what3words

* Free and open source.
* Squares are 5 meters per side.
* Only 2500 words, in return for using 4 words per geohash.

## Differences from FourWordsFindWhere

* Same resolution on land and on sea.

## Pronunciation

* **IPA (Wikipedia standard, American English):** “Woordinates” is pronounced either /ˈwɜːrdɪnəts/ or /ˌwoʊˈɔːrdɪnəts/.

* **Merriam–Webster (American English):** “Woordinates” is pronounced either \\ˈwərdinəts\\ or \\wōˌȯrdinəts\\.

* **IPA (CUBE standard, Standard Southern British English):** “Woordinates” is pronounced either wə́ːdənəts or kə́wóːdənəts.

* **Shavian alphabet (Read Lexicon standard):** “Woordinates” is spelled either ·𐑢𐑻𐑛𐑦𐑯𐑩𐑑𐑕 or ·𐑢𐑴𐑹𐑛𐑦𐑯𐑩𐑑𐑕 in Shavian.

* **Pronunciation respelling:** “Woordinates” is pronounced either “WORD-in-its” or “woe-ORD-in-its”.

## How to encode a Woordinates geohash

### Step 1

Get the latitude and longitude of the point you want to encode. (North and East are positive.)

### Step 2

Remap the latitude from [-90°, 90°) to [0, 4194704).

(Note: 4194704 is 2^22.)

Latitudes fall into these four ranges:

* Equatorial: 1398101 <= latitude <= 2796203 (rounded from ±30°)
* Mid-latitude: 699050 <= latitude <= 3495254 (rounded from ±60°)
* Polar: 349525 <= latitude <= 3844779 (rounded from ±75°)
* Extreme polar: all other


### Step 3:

Remap the longitude from [-180°, 180°) depending on the range the latitude falls into. (Note: 8338608 is 2^23.)

* Equatorial: [0, 8338608)
* Mid-latitude: [0, 7340032) (7/8 density)
* Polar: [0, 4194304) (1/2 density)
* Extreme polar: [0, 2097152) (1/4 density)

## Step 4

Stretch that range to [0, 8338608) and round off.

* Equatorial: No change.
* Mid-latitude: Multiply by 8/7, i.e. Skip every 1 in 8 latitude values.
* Polar: Multiply by 2, i.e. Skip every other latitude value.
* Extreme polar: Multiply by 4, i.e. Only use every 4 latitude values.

## Step 5

Now you should have a 22-bit longitude and 23-bit latitude. Interleave the bits to create a 45-bit integer (ranging from 0 to 2^45 = 35 184 372 088 832).

Note: Because the 4th root of 2^45 is 2435.496172, our word list is going to be at least 2436 words long. We will round that up to 2500.

## Step 6

Reverse the order of the bits, so that similar addresses will get far off places.

## Step 7

Convert our binary integer into a base 2500 number, using the words in our word list as the digits.

To decode a Woordinates geohash, simply reverse these operations.

# Word list

We need 2500 unique words.

For a perspective on how many that is, there are 2309 possible Wordle answers, and that's only counting 5-letter words, but including confusables like SCENT, SENSE, and CENTS.

Ideally, the words in the list should have these properties:

* They have between 1 and 4 syllables, ideally 2 or 3.
* They avoid pairs of words that are confusable through one or more of these substitutions:
  * similar consonants: batch vs. bash, line vs. lime
  * similar vowels: marry vs. merry, coral vs. choral
  * omission of unstressed vowels: course vs. chorus, align vs. line, quota vs. quote
  * change in rhoticity: cheetah vs. cheater, quota vs. quoter
* Given that the averate American reading level is at a 7th or 8th grade level, the words should be common enough for those levels.

There is [a repository that publishes a lexicon that contains pronunciation and frequency data](https://github.com/Shavian-info/readlex/), so it should be possible to programmatically generate this list of 2500 words.