# History

## 0.1.1 — 2026-09-01

Additive change to the conformance vectors. Every vector committed at 0.1.0 is
byte-identical at 0.1.1, and no field, matrix construction, encoding, or reconstruction
rule changes.

- Added a transport-scale encode and reconstruct case at `K = 6410`, `R = 1744`,
  `shardLen = 8`. Before this the shared vectors topped out at `K = 32`, which is 256
  generator-matrix entries over a handful of GF(2^16) elements. The new case has 11.2
  million entries whose Cauchy indices span most of the field, and drops 512 of its 8,154
  shards — 401 of them data — so the `E x E` submatrix solve runs at a width no small case
  reaches. Implementations that agree on every small case can still diverge there.

  Shard length is deliberately short. Shards are combined word by word, so shard length is
  independent of everything a large `K` and `R` exercise, and 8 bytes keeps the committed
  blobs to 65 KB rather than the megabytes a transport-sized shard would cost.

- Added the compact case layout (`SPEC.md` section 8.1) and raised `manifestVersion` from
  1 to 2. The per-shard file layout would have put 8,154 `*.bin` files in this repository
  for one case, and the full `present` mask would have been 8,154 inline booleans, so a
  compact case stores its shards as two concatenated blobs and names the absent indices
  instead. The version was raised deliberately rather than relying on the addition being
  ignorable: a reader that skipped the layout it did not understand would skip the only
  case above `K = 32` while still reporting a pass.

## 0.1.0 — 2026-07-10

- Initial systematic Cauchy Reed–Solomon profile over GF(2^16).
- Initial byte-exact field, encode, and reconstruct conformance vectors.

The specification version describes the normative format and vectors. Java and
.NET package versions describe their respective implementation releases.
