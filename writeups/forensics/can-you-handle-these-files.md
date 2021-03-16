# Can you handle these files?

> Download the memory image from one of the sources.
>
> An attacker is in our system and has left a note behind. Can you find the link in his note that leads us to the flag? Hint: Flag can be found by accessing the URL.

To analyse the `memdump`, we can use [`volatility`](https://github.com/volatilityfoundation/volatility).

```bash
# Get list of running processes and filter out lines with 'notepad'.
$ vol.py -f memdump.mem --profile=Win7SP1x64 pstree | grep 'notepad'

Volatility Foundation Volatility Framework 2.6.1
. 0xfffffa8002990920:notepad.exe                     3800   2332      1     61 2021-02-20 14:17:11 UTC+0000
. 0xfffffa80039481a0:notepad.exe                     3380   2332      1     61 2021-02-20 14:17:12 UTC+0000
. 0xfffffa800426c6a0:notepad.exe                     3352   2332      1     61 2021-02-20 14:16:43 UTC+0000
. 0xfffffa8002627640:notepad.exe                     3164   2332      1     61 2021-02-20 14:17:10 UTC+0000
```

We can attempt to dump any of the above processes and `strings` it to see if anything interesting shows up.

```bash
$ vol.py -f memdump.mem --profile=Win7SP1x64 memdump --dump-dir=. -p 3800

Volatility Foundation Volatility Framework 2.6.1
************************************************************************
Writing notepad.exe [  3800] to 3800.dmp

$ strings 3800.dmp | grep -C 10 flag.txt

hackunit.txt
dead1.lnk
wordmacro.txt
acronyms.lnk
acronyms.lnk
cars.lnk
drunk.lnk
coffee.lnk
coffee.lnk
dead1.lnk
flag.txt.lnk
flag2.txt.lnk
Untitled.lnk
Untitled.lnk
ViMm
ViMm
ViMm
ViMm
{CDC82860-468D-4D4E-B7E7-C298FF23AB2C}
DxgK0Y3
ACPI_HAL
--
--
MmSt
MmSt
hbin
acronyms.txt
batvirus.txt
drunk.lnk
cars.txt
MRUList
troj.txt
troj.txt
flag.txt.lnk
coffee.txt
vir.txt
vir.txt
BLBe
ExteP9
Pref(
Stab
Thirnk
dead1.txt
virus3.lnk
--
--
1702
FILE0
FILE0
FILE0
2|Cc
FILE0
FILE0
FILE0
FILE0
FLAGTX~1.TXT
C:\Users\whitehacks\Desktop\flag.txt.txt
1SPS
whitehacks-pc
FILE0
FILE0
FILE0
FILE0
]VN

FILE0
https://imgur.com/a/pRWCNyo
I see you have found me............
```

If we visit [https://imgur.com/a/pRWCNyo](https://imgur.com/a/pRWCNyo), we find the flag.

![imgur image](/images/can-you-handle-these-files-1.png)

`WH2021{iSEEuHANDLEDthisWELL}`
