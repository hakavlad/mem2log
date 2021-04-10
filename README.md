# mem2log

Memory metrics (from `/proc/meminfo`) monitor and logger.

mem2log has three modes: 1 (default), 2 (logs more metrics), 3 (special mode for patched kernels).

Set the `--log LOG` option to log into the file. 

Set the `-i INTERVAL` option to specify the interval.

## Usage

Just run the script.
```
$ ./mem2log -h
usage: mem2log [-h] [-i INTERVAL] [-l LOG] [-m MODE]

optional arguments:
  -h, --help            show this help message and exit
  -i INTERVAL, --interval INTERVAL
                        interval in sec
  -l LOG, --log LOG     path to log file
  -m MODE, --mode MODE  mode (1, 2 or 3)
```

Output examples:
```
$ ./mem2log
Starting mem2log with interval 2s, mode: 1
Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
All values are in mebibytes
MemTotal: 9788.1, SwapTotal: 0.0
--
MA is MemAvailable, MF is MemFree, A is Anon
F is File, AF is Active(file), IF is Inactive(file)
D is Dirty, C is Clean file (File - Dirty), SF is SwapFree
--
MA=7899=81% MF=7622 A=1334 F=483 AF=249 IF=233 D=0 C=483 SF=0=0%
MA=7897=81% MF=7621 A=1334 F=483 AF=249 IF=233 D=0 C=483 SF=0=0%
MA=7897=81% MF=7620 A=1334 F=483 AF=249 IF=233 D=0 C=483 SF=0=0%
MA=7897=81% MF=7621 A=1334 F=483 AF=249 IF=233 D=0 C=483 SF=0=0%
^C--
Got the SIGINT signal
Peak values:
  MA:  min 7896.95, max 7898.69
  MF:  min 7620.21, max 7621.77
  A:   min 1333.88, max 1333.96
  F:   min 482.62, max 482.8
  AF:  min 249.19, max 249.35
  IF:  min 233.43, max 233.46
  D:   min 0.0, max 0.1
  C:   min 482.61, max 482.71
  SF:  min 0.0, max 0.0
Exit.
```

<details>
 <summary>`mem2log -m2`</summary>
```
$ ./mem2log -m2
Starting mem2log with interval 2s, mode: 2
Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
All values are in mebibytes
MemTotal: 9788.1, SwapTotal: 0.0
--
MA is MemAvailable, MF is MemFree, BU is Buffers, CA is Cached
AA is Active(anon), IA is Inactive(anon), AF is Active(file), IF is Inactive(file)
SF is SwapFree, SU is `SwapTotal - SwapFree`, DI is Dirty, WR is Writeback
CF is Clean File (AF + IF - DI), SH is Shmem, SR is SReclaimable
--
MA=7899 MF=7622 BU=76 CA=650 AA=3 IA=1330 AF=249 IF=233 SF=0 SU=0 DI=0 WR=0 CF=483 SH=238 SR=40
MA=7899 MF=7622 BU=76 CA=650 AA=3 IA=1330 AF=249 IF=233 SF=0 SU=0 DI=0 WR=0 CF=483 SH=238 SR=40
MA=7900 MF=7623 BU=76 CA=650 AA=3 IA=1330 AF=249 IF=233 SF=0 SU=0 DI=0 WR=0 CF=483 SH=237 SR=40
MA=7900 MF=7623 BU=76 CA=650 AA=3 IA=1330 AF=249 IF=233 SF=0 SU=0 DI=0 WR=0 CF=483 SH=237 SR=40
^C--
Got the SIGINT signal
Peak values:
  MA:  min 7899.06, max 7900.0
  MF:  min 7622.3, max 7623.24
  BU:  min 76.23, max 76.24
  CA:  min 649.58, max 650.23
  AA:  min 2.91, max 2.91
  IA:  min 1330.34, max 1330.44
  AF:  min 249.2, max 249.38
  IF:  min 233.45, max 233.46
  SF:  min 0.0, max 0.0
  SU:  min 0.0, max 0.0
  DI:  min 0.0, max 0.0
  WR:  min 0.0, max 0.0
  CF:  min 482.64, max 482.84
  SH:  min 237.41, max 238.07
  SR:  min 40.16, max 40.16
Exit.
```
</details>

```
$ ./mem2log -m3
Starting mem2log with interval 2s, mode: 3
Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
All values are in mebibytes
MemTotal: 9788.1, SwapTotal: 0.0
--
MA is MemAvailable, MF is MemFree, A is AnonAcF is Active(file), InF is Inactive(file)
IsF is Isolated(file), ReF is Reclaimable(file) (AcF + InF + IsF)
D is Dirty, W is Writeback, C is Clean file (ReF - D), SF is SwapFree
--
MA=7900=81% MF=7623 A=1334 AcF=249.4 InF=233.5 IsF=0.0  ReF=482.8  D=0.0  W=0.0  C=482.84  SF=0=0%
MA=7898=81% MF=7621 A=1334 AcF=249.2 InF=233.4 IsF=0.0  ReF=482.6  D=0.0  W=0.0  C=482.65  SF=0=0%
MA=7899=81% MF=7622 A=1334 AcF=249.2 InF=233.4 IsF=0.0  ReF=482.6  D=0.0  W=0.0  C=482.65  SF=0=0%
MA=7899=81% MF=7622 A=1334 AcF=249.2 InF=233.4 IsF=0.0  ReF=482.7  D=0.0  W=0.0  C=482.64  SF=0=0%
^C--
Got the SIGINT signal
Peak values:
  MA:  min 7897.73, max 7899.74
  MF:  min 7620.97, max 7622.79
  A:   min 1333.55, max 1333.64
  AcF: min 249.2, max 249.39
  InF: min 233.45, max 233.46
  IsF: min 0.0, max 0.0
  ReF: min 482.65, max 482.84
  D:   min 0.0, max 0.01
  W:   min 0.0, max 0.0
  C:   min 482.64, max 482.84
  SF:  min 0.0, max 0.0
Exit.
```

Log file example (started with cmd `mem2log -l /tmp/mem.log`):
```
2021-04-10 14:37:57,206: Starting mem2log with interval 2s, mode: 1
2021-04-10 14:37:57,206: Log file: /tmp/mem.log
2021-04-10 14:37:57,207: Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
2021-04-10 14:37:57,207: All values are in mebibytes
2021-04-10 14:37:57,208: MemTotal: 9788.1, SwapTotal: 0.0
2021-04-10 14:37:57,208: --
2021-04-10 14:37:57,208: MA is MemAvailable, MF is MemFree, A is Anon
2021-04-10 14:37:57,208: F is File, AF is Active(file), IF is Inactive(file)
2021-04-10 14:37:57,208: D is Dirty, C is Clean file (File - Dirty), SF is SwapFree
2021-04-10 14:37:57,208: --
2021-04-10 14:37:57,208: MA=7933=81% MF=7649 A=1285 F=490 AF=251 IF=239 D=0 C=490 SF=0=0%
2021-04-10 14:37:59,211: MA=7934=81% MF=7649 A=1284 F=490 AF=251 IF=239 D=0 C=490 SF=0=0%
2021-04-10 14:38:01,212: MA=7934=81% MF=7650 A=1284 F=490 AF=251 IF=239 D=0 C=490 SF=0=0%
2021-04-10 14:38:03,215: MA=7934=81% MF=7649 A=1284 F=490 AF=251 IF=239 D=0 C=490 SF=0=0%
2021-04-10 14:38:03,747: --
2021-04-10 14:38:03,747: Got the SIGINT signal
2021-04-10 14:38:03,747: Peak values:
2021-04-10 14:38:03,747:   MA:  min 7933.15, max 7934.48
2021-04-10 14:38:03,747:   MF:  min 7648.88, max 7650.3
2021-04-10 14:38:03,747:   A:   min 1284.44, max 1284.55
2021-04-10 14:38:03,747:   F:   min 489.86, max 489.96
2021-04-10 14:38:03,748:   AF:  min 250.66, max 250.84
2021-04-10 14:38:03,748:   IF:  min 239.12, max 239.19
2021-04-10 14:38:03,748:   D:   min 0.0, max 0.0
2021-04-10 14:38:03,748:   C:   min 489.86, max 489.96
2021-04-10 14:38:03,748:   SF:  min 0.0, max 0.0
2021-04-10 14:38:03,748: Exit.
```

## mlockall()

`mem2log` calls `mlockall()` to work well under low-memory conditions. That's why running it with `sudo` is a good idea.

## Requirements

- Python >= 3.3

## Install
```
$ git clone https://github.com/hakavlad/mem2log.git
$ cd mem2log
$ sudo make install
```

## Uninstall
```
$ sudo make uninstall
```
