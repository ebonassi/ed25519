Ed25519
-------

This is a portable implementation of [Ed25519](http://ed25519.cr.yp.to/). All
code is in the public domain.

All code is pure ANSI C without any dependencies, except for the random seed
generation which uses standard OS cryptography APIs. If you wish to be
entirely portable define `ED25519_NO_SEED`. This does disable the
`ed25519_create_seed` function however (you can use your own seeding function
if you wish.)

Usage
-----

Simply add all .c and .h files in the `src/` folder to your project and
include `ed25519.h` in any file you want to use the API. If you prefer to use
a shared library, only copy `ed25519.h` and define `ED25519_DLL` before
importing. A windows DLL is pre-built.

There are no defined types for seeds, signing keys, verifying keys or
signatures. Instead simple `unsigned char` buffers are used with the following
sizes:

    unsigned char seed[32]
    unsigned char signature[64]
    unsigned char verify_key[32]
    unsigned char signing_key[64]

API
---

    int ed25519_create_seed(unsigned char *seed);

Creates a 32 byte random seed in `seed` for key generation. `seed` must be a
writable 32 byte buffer.

    int ed25519_create_keypair(unsigned char *verify_key, unsigned char *sign_key, const unsigned char *seed);

Creates a new key pair from the given seed. `verify_key` must be a writable 32
byte buffer, `sign_key` must be a writable 64 byte buffer and `seed` must be a
32 byte buffer.

    int ed25519_sign(unsigned char *signature, const unsigned char *message, size_t message_len, const unsigned char *sign_key);
    int ed25519_verify(const unsigned char *signature, const unsigned char *message, size_t message_len, const unsigned char *verify_key);