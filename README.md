
# NachOS 4.0 Setup on Ubuntu
Hackmd 好讀版：https://hackmd.io/@YUIN/HyR6haYkGx <br>
<br>
This guide shows how to build and run NachOS 4.0 on Ubuntu.
<br>
<br>
<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/a863b4a3-75e3-44c9-890c-82b43eafde9d" />

## Environment

Tested on Ubuntu 24.04.

## 1. Install dependencies

```bash
sudo apt update
sudo apt install g++ csh make wget vim g++-multilib gcc-multilib libc6-dev-i386 -y
````

## 2. Download files

```bash
mkdir -p ~/oslab
cd ~/oslab

wget -O nachos-4.0.tar.gz \
https://raw.githubusercontent.com/Yuin0329/NachOS-Setup/8deec2b725c1bcf115cc81fc5694bf478afdb154/nachos-4.0.tar.gz

wget -O mips-x86.linux-xgcc.tar.gz \
https://raw.githubusercontent.com/Yuin0329/NachOS-Setup/8deec2b725c1bcf115cc81fc5694bf478afdb154/mips-x86.linux-xgcc.tar.gz
```

## 3. Verify downloaded files

```bash
file nachos-4.0.tar.gz
file mips-x86.linux-xgcc.tar.gz
```

They should show `gzip compressed data`.

## 4. Extract files

```bash
tar -xzvf nachos-4.0.tar.gz
sudo tar -xzvf mips-x86.linux-xgcc.tar.gz -C /
```

Check the cross compiler:

```bash
ls /mips-x86.linux-xgcc
```

## 5. Edit `code/test/Makefile`

```bash
cd ~/oslab/nachos-4.0/code/test
vim Makefile
```

Set:

```makefile
GCCDIR = /mips-x86.linux-xgcc/
CPP = /mips-x86.linux-xgcc/cpp0
CFLAGS = -G 0 -c $(INCDIR) -B/mips-x86.linux-xgcc/
```

## 6. Edit `code/Makefile.common`

```bash
cd ~/oslab/nachos-4.0/code
vim Makefile.common
```

Set:

```makefile
CFLAGS = -m32 -g -Wall $(INCPATH) $(DEFINES) $(HOST) -DCHANGED
LDFLAGS = -m32

CPP=/lib/cpp
CC = g++ -m32 -Wno-deprecated
LD = g++ -m32 -Wno-deprecated
AS = as --32
```

## 7. Edit `code/bin/Makefile`

```bash
cd ~/oslab/nachos-4.0/code/bin
vim Makefile
```

Set:

```makefile
CC=gcc -m32
CFLAGS=-m32 -I../lib -I../threads $(HOST)
LD=gcc -m32
```

## 8. Clean old build files

```bash
cd ~/oslab/nachos-4.0/code

find . -name "*.o" -delete
find . -name "nachos" -delete
find . -name "coff2noff" -delete
find . -name "swtch.s" -delete
find . -name "makedep" -delete
find . -name "eddep" -delete

cd test
rm -f *.o *.coff *.noff halt test1 test2 add createFile fileIO*
```

## 9. Build NachOS

```bash
cd ~/oslab/nachos-4.0/code
make
```

## 10. Run test

```bash
cd ~/oslab/nachos-4.0/code/userprog
./nachos -e ../test/test1
```

## Expected output

You should see something similar to:

```text
Total threads number is 1
Thread ../test/test1 is executing.
Print integer:9
Print integer:8
Print integer:7
Print integer:6
return value:0
Machine halting!
```
<img width="1412" height="831" alt="image" src="https://github.com/user-attachments/assets/f2228c90-6112-4c0c-8b01-84c86881c80c" />

## Notes

* If you already built with wrong settings, clean old build files before rebuilding.
* If `test1` runs successfully, the setup is complete.


