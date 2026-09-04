# I Don't Note You Note

> Given file: `i-dont-note-you-note.zip`
>
> Remote service: `nc kubenode.mctf.io 30021`

The application is a note manager that allows users to read files from a notes directory.

### Analysis

The application filters `/` and `..` before applying Unicode NFKC normalization. Fullwidth Unicode characters are normalized into their ASCII equivalents after the security check.

```text
／  ->  /
．  ->  .
```

### Steps:

1. Create the path traversal payload

```text
．．／．．／．．／．．／flag.txt
```

2. Bypass the filter

The original input does not contain the blocked ASCII characters, so it passes the check.

3. Trigger normalization

The application converts the payload into:

```text
../../../../flag.txt
```

4. Read the file

The normalized traversal reaches the flag file.

Flag found! Problem Solved!
