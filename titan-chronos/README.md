# Titan Chronos

> Given website: `http://4cswkj2l.chals.mctf.io/`

The website provides a terminal emulator. The goal is to find the persistence mechanism used by the Titan.

### Analysis

A root cronjob periodically runs `/usr/local/bin/sysmaint`. The script creates or maintains a user named `chronos` and contains a Base64-encoded value.

### Steps:

1. Inspect scheduled tasks

```bash
crontab -l
```

2. Inspect the maintenance script

```bash
cat /usr/local/bin/sysmaint
```

3. Decode the Base64 value

```bash
echo 'TWV0YUNURntzbDR5XzdoM190MXQ0bl8wZl90MW0zfQ==' | base64 -d
```

Flag found! Problem Solved!
