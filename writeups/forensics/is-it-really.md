# Is it really?

> A malicious file was downloaded and picked up by our antivirus...

This challenge comes with a `signup.pdf` file which triggers most antiviruses. Maybe there's some fragment inside the file thats causing the antivirus to act up? Let's try running `binwalk`.

```
$ binwalk signup.pdf

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PDF document, version: "1.3"
69            0x45            Zip archive data, at least v2.0 to extract, uncompressed size: 68, name: eicar.txt
226           0xE2            Zip archive data, at least v2.0 to extract, uncompressed size: 332, name: __MACOSX/._eicar.txt
687           0x2AF           End of Zip archive, footer length: 22
443555        0x6C4A3         End of Zip archive, footer length: 22
```

From the embedded `eicar.txt` file, we can determine the flag.

`WH2021{eicar.txt}`
