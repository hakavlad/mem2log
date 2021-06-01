
# mem2log

[![Total alerts](https://img.shields.io/lgtm/alerts/g/hakavlad/mem2log.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/hakavlad/mem2log/alerts/)

Memory metrics (from `/proc/meminfo`) monitor and logger.

`mem2log` has three modes: `1` (default), `2` (logs more metrics), `3` (special mode for patched kernels).

Set the `--log LOG` option to log into the file. 

Set the `-i INTERVAL` option to specify the interval.

At the end (when catches `SIGINT`, `SIGTERM`, `SIGHUP` or `SIGQUIT` signal) `mem2log` prints peak (`min` and `max`) values.

## Usage

Just run the script. 
```
$ mem2log -h
usage: mem2log [-h] [-i INTERVAL] [-l LOG] [-m MODE]

optional arguments:
  -h, --help            show this help message and exit
  -i INTERVAL, --interval INTERVAL
                        interval in sec
  -l LOG, --log LOG     path to log file
  -m MODE, --mode MODE  mode (1, 2 or 3)
```

Output examples:

<details>
 <summary>mem2log</summary>

```
$ mem2log
Starting mem2log with interval 2s, mode: 1
Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
All values are in mebibytes
MemTotal: 9783.2, SwapTotal: 39132.9
--
MA is MemAvailable, MF is MemFree, A is Anon
F is File, AF is Active(file), IF is Inactive(file)
D is Dirty, C is Clean file (File - Dirty)
SF is SwapFree, SU is SwapUsed (SwapTotal - SwapFree)
--
MA=7448=76% MF=7351 A=1121 F=408 AF=219 IF=189 D=5 C=403 SF=38237 SU=896
MA=7447=76% MF=7350 A=1121 F=408 AF=219 IF=189 D=5 C=403 SF=38237 SU=896
MA=7448=76% MF=7351 A=1121 F=408 AF=219 IF=189 D=5 C=403 SF=38237 SU=896
MA=7447=76% MF=7350 A=1121 F=408 AF=219 IF=189 D=5 C=403 SF=38237 SU=896
^C--
Got the SIGINT signal
Peak values:
  MA:  min 7446.82, max 7447.94
  MF:  min 7350.03, max 7351.02
  A:   min 1121.0, max 1121.34
  F:   min 408.05, max 408.15
  AF:  min 219.29, max 219.33
  IF:  min 188.76, max 188.82
  D:   min 4.7, max 4.7
  C:   min 403.35, max 403.45
  SF:  min 38236.56, max 38236.56
  SU:  min 896.33, max 896.33
Exit.
```
</details>

<details>
 <summary>mem2log -m 2</summary>

```
$ mem2log -m 2
Starting mem2log with interval 2s, mode: 2
Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
All values are in mebibytes
MemTotal: 9783.2, SwapTotal: 39132.9
--
MA is MemAvailable, MF is MemFree, BU is Buffers, CA is Cached, SC is SwapCached
AA is Active(anon), IA is Inactive(anon), AF is Active(file), IF is Inactive(file)
ML is Mlocked, SF is SwapFree, SU is `SwapTotal - SwapFree`, DI is Dirty, WR is Writeback
CF is Clean File (AF + IF - DI), SH is Shmem, SL is Slab, SR is SReclaimable
--
MA=7430 MF=7333 BU=67 CA=642 SC=4 AA=151 IA=985 AF=222 IF=186 ML=11 SF=38238 SU=895 DI=1 WR=0 CF=408 SH=294 SL=138 SR=60
MA=7429 MF=7332 BU=67 CA=642 SC=4 AA=151 IA=985 AF=222 IF=186 ML=11 SF=38238 SU=895 DI=1 WR=0 CF=408 SH=295 SL=138 SR=60
MA=7429 MF=7332 BU=67 CA=641 SC=4 AA=151 IA=985 AF=222 IF=186 ML=11 SF=38238 SU=895 DI=1 WR=0 CF=408 SH=294 SL=138 SR=60
MA=7430 MF=7333 BU=67 CA=641 SC=4 AA=151 IA=985 AF=222 IF=186 ML=11 SF=38238 SU=895 DI=1 WR=0 CF=408 SH=294 SL=138 SR=60
^C--
Got the SIGINT signal
Peak values:
  MA:  min 7429.33, max 7430.14
  MF:  min 7332.27, max 7332.99
  BU:  min 66.97, max 66.97
  CA:  min 641.34, max 642.21
  SC:  min 3.72, max 3.72
  AA:  min 150.69, max 150.88
  IA:  min 984.83, max 984.88
  AF:  min 221.85, max 221.93
  IF:  min 186.49, max 186.5
  ML:  min 10.52, max 10.69
  SF:  min 38238.31, max 38238.31
  SU:  min 894.58, max 894.58
  DI:  min 0.74, max 0.74
  WR:  min 0.0, max 0.0
  CF:  min 407.6, max 407.69
  SH:  min 294.32, max 295.18
  SL:  min 137.93, max 138.04
  SR:  min 59.53, max 59.53
Exit.
```
</details>

<details>
 <summary>mem2log -m 3</summary>

```
$ mem2log -m 3
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
</details>

Log file example (started with cmd `mem2log -l /tmp/mem.log`):

<details>
 <summary>mem2log -l /tmp/mem.log</summary>

```
2021-06-02 01:00:25,844: Starting mem2log with interval 2s, mode: 1
2021-06-02 01:00:25,844: Log file: /tmp/mem.log
2021-06-02 01:00:25,845: Process memory locked with MCL_CURRENT | MCL_FUTURE | MCL_ONFAULT
2021-06-02 01:00:25,845: All values are in mebibytes
2021-06-02 01:00:25,845: MemTotal: 9783.2, SwapTotal: 39132.9
2021-06-02 01:00:25,845: --
2021-06-02 01:00:25,845: MA is MemAvailable, MF is MemFree, A is Anon
2021-06-02 01:00:25,845: F is File, AF is Active(file), IF is Inactive(file)
2021-06-02 01:00:25,845: D is Dirty, C is Clean file (File - Dirty)
2021-06-02 01:00:25,846: SF is SwapFree, SU is SwapUsed (SwapTotal - SwapFree)
2021-06-02 01:00:25,846: --
2021-06-02 01:00:25,846: MA=7431=76% MF=7333 A=1139 F=409 AF=222 IF=187 D=0 C=409 SF=38240 SU=893
2021-06-02 01:00:27,848: MA=7430=76% MF=7331 A=1139 F=409 AF=222 IF=187 D=0 C=409 SF=38240 SU=893
2021-06-02 01:00:29,851: MA=7425=76% MF=7327 A=1144 F=409 AF=222 IF=187 D=0 C=409 SF=38240 SU=893
2021-06-02 01:00:31,851: MA=7424=76% MF=7326 A=1144 F=409 AF=222 IF=187 D=0 C=409 SF=38240 SU=893
2021-06-02 01:00:32,393: --
2021-06-02 01:00:32,394: Got the SIGINT signal
2021-06-02 01:00:32,394: Peak values:
2021-06-02 01:00:32,394:   MA:  min 7424.3, max 7431.09
2021-06-02 01:00:32,394:   MF:  min 7326.14, max 7332.97
2021-06-02 01:00:32,394:   A:   min 1138.64, max 1144.34
2021-06-02 01:00:32,394:   F:   min 409.38, max 409.42
2021-06-02 01:00:32,394:   AF:  min 222.22, max 222.3
2021-06-02 01:00:32,394:   IF:  min 187.08, max 187.2
2021-06-02 01:00:32,394:   D:   min 0.03, max 0.04
2021-06-02 01:00:32,394:   C:   min 409.35, max 409.38
2021-06-02 01:00:32,395:   SF:  min 38239.56, max 38239.56
2021-06-02 01:00:32,395:   SU:  min 893.33, max 893.33
2021-06-02 01:00:32,395: Exit.
```
</details>

## mlockall()

`mem2log` calls `mlockall()` to work well under low-memory conditions. That's why running it with `sudo` is a good idea (to get `CAP_IPC_LOCK` capability).

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
