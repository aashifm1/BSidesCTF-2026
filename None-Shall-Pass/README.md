# None Shall Pass

> Given website: `http://noneshallpass.chals.mctf.io/`

The website uses JWT authentication and contains a FLAG product.

### Analysis

The server accepts JWTs using the `none` algorithm. The token payload also controls session values such as the balance and role.

### Steps:

1. Register and log in

Capture the JWT cookie issued after login.

2. Decode the token

A JWT has the following structure:

```text
header.payload.signature
```

3. Modify the payload

Set a high balance and an administrator role:

```json
{
  "username": "myuser",
  "role": "admin",
  "balance": 100000.0
}
```

4. Forge the JWT

Use this header and leave the signature empty:

```json
{"alg":"none","typ":"JWT"}
```

The forged token ends with a trailing dot:

```text
base64url(header).base64url(payload).
```

5. Purchase the FLAG product

Replace the session token, add the FLAG item to the cart, and complete checkout.

Flag found! Problem Solved!
