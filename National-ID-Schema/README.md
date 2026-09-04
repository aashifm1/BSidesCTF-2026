# National ID Scheme

> Given library: `libnid.so`
>
> Remote service: `nc -u host5.metaproblems.com 7544`

The challenge provides John Doe's personal details and a shared library that validates a National ID.

### Analysis

The library contains `check_block_1` through `check_block_16`. Each function validates an 8-byte block of a 128-byte ID. The expected block is calculated and then compared with the supplied block using `memcmp`.

### Steps:

1. Inspect the library

```bash
file libnid.so
nm -D libnid.so
objdump -d -M intel libnid.so
```

2. Hook `memcmp`

I created an `LD_PRELOAD` library that intercepts `memcmp` and prints the first argument for 8-byte comparisons.

```c
int memcmp(const void *a, const void *b, size_t n) {
    if (n == 8) {
        const unsigned char *p = a;
        printf("EXPECTED_BLOCK: ");
        for (int i = 0; i < 8; i++) printf("%02x", p[i]);
        printf("\n");
    }

    /* Call the real memcmp here. */
}
```

3. Extract the 16 blocks

```text
b83f881ef00bb866
152ae4f825ba8438
160e0429180c3c32
00192a2030051807
080d201d343f0a2c
362e0a11223c1936
0408000840004004
070e2c2a51373d0c
4610581464041250
ddfa5dbaddfaddfe
5214607056144040
0c28868224a8ae8a
454a14450b54155a
0000002000103010
4016660b88094606
8898208898a00810
```

4. Combine and send the ID

Concatenate the blocks into the 128-byte ID and send it as a UDP packet to port 7544.

The server validates the ID and returns the OTP.

Flag found! Problem Solved!
