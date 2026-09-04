# V for Vardetta - The System

> Given binary: `v_for_vardetta`
>
> Remote service: `nc kubenode.mctf.io 31021`

The program asks for a codename and assigns an access level. The goal is to access the secret archives.

### Analysis

The codename input has a buffer overflow. The access-level variable is located after the input buffer, so it can be overwritten.

### Steps:

1. Find the offset

Testing different input lengths showed that the access-level variable is reached after 64 bytes.

2. Set the Director value

The Director access level is represented by the integer value `3`.

3. Send the payload

```python
payload = b"A" * 64 + b"\x03"
```

Send the payload and select the secret archives option.

Flag found! Problem Solved!
