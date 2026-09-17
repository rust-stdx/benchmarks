# Benchmarks

Benchmarking `stdx` packages against other crates.

See https://github.com/rust-stdx/stdx for the stable packages.

## crypto_native

Compares `stdx/crypto` against `aws-lc-rs` and RustCrypto (`sha2`, `sha3`,
`ascon-hash256`, `ascon-aead128`, `hmac`, `poly1305`, `aes-gcm`,
`chacha20poly1305`, `chacha20`, `aes`/`ctr`, `ed25519-dalek`, `ml-dsa`, and
upstream `blake3`) on native targets. Each algorithm is a criterion group; the
size is the benchmark parameter and the implementation is the benchmark id
(`stdx-crypto`, `aws-lc-rs`, `RustCrypto`).

`ascon-aead128` is pinned to a RustCrypto/AEADs git revision because the
published 0.1.x releases are not source-compatible with the stable `ascon`
release used by `ascon-hash256`.

| Category | Compared algorithms |
| --- | --- |
| Hash | SHA-256, SHA-512, SHA3-256, SHA3-512, SHAKE256, BLAKE3, Ascon-Hash256 |
| MAC | HMAC-SHA256, HMAC-SHA512, Poly1305, BLAKE3-keyed |
| Stream cipher | AES-256-CTR, ChaCha8, ChaCha12, ChaCha20 |
| AEAD | AES-256-GCM, AES-128-GCM, ChaCha20-Poly1305, Ascon-AEAD128 (encrypt and decrypt) |
| Signatures | Ed25519, ML-DSA-44/65/87 (sign and verify) |

The following `stdx/crypto` algorithms are intentionally **not** benchmarked
here because none of the compared crates provide an equivalent to compare
against:

- ChaCha8-Poly1305
- ChaCha20-BLAKE3
- KMAC256

SHAKE256, Ascon-Hash256 and Ascon-AEAD128 are only compared against RustCrypto
because `aws-lc-rs` does not expose them.

## crypto_wasm

Benchmarks `stdx/crypto` on `wasm32-wasip1` (with `+simd128`) using its own
timing harness. Categories: `hash`, `mac`, `stream`, `aead`, `sign`.

```bash
$ RUSTFLAGS="-C target-feature=+simd128" cargo run --release --target=wasm32-wasip1 -p crypto_wasm -- [category ...]
```

## License

MIT ([LICENSE.txt](./LICENSE.txt))

Any contribution submitted for inclusion in this project shall be licensed as above without any additional terms or conditions.
