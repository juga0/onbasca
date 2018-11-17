.. _tor_digest:

Digests code in ``tor``
==========================

In ``src/lib/defs/digest_sizes.h``:

  #define DIGEST_LEN 20
  /** Length of the output of our second (improved) message digests.  (For now
   * this is just sha256, but it could be any other 256-bit digest.) */
  #define DIGEST256_LEN 32
  /** Length of the output of our 64-bit optimized message digests (SHA512). */
  #define DIGEST512_LEN 64

In ``src/lib/crypt_ops/crypto_digest.h``:

  #define BASE64_DIGEST_LEN 27
  /** Length of a sha256 message digest when encoded in base64 with trailing =
   * signs removed. */
  #define BASE64_DIGEST256_LEN 43

  /** Length of hex encoding of SHA1 digest, not including final NUL. */
  #define HEX_DIGEST_LEN 40
  /** Length of hex encoding of SHA256 digest, not including final NUL. */
  #define HEX_DIGEST256_LEN 64

In ``src/lib/crypt_ops/crypto_digest.c``:

  crypto_digest256

Encoding
---------

In ``src/lib/encoding/binascii.c``:

  base16_encode

In ``src/lib/crypt_ops/crypto_format.c``:

  digest_to_base64
  digest_to_base16
  digest256_to_base64
