# h4 Some Disassembly Required

## x) Read/watch/listen and summarize

### Hammond 2022: Ghidra for Reverse Engineering (PicoCTF 2022 #42 'bbbloat'):

* In the video John Hammond walks through how to use *Ghidra* using `PicoCTF 2022 #42 'bbbloat'` binary as the example.
* John introduces the commands `ltrace` and `strace` which can be used to trace library and system calls a program uses among other tools.
  * `ltrace` is for library calls.
  * `strace` is for system calls.
* The video works as a nice walkthrough what *Ghidra* is, what it can do and how to reverse engineer programs.


## a) Install Ghidra

### 1. Kali

I decided to install Kali Linux for this one since you can get a more modern *Ghidra* for this.

I went to Kali.org and downloaded the VirtualBox .vdi file, imported it and then updated the system with

```
sudo apt-get update
sudo apt-get upgrade -y
```

After that I rebooted the system and now it's time to install *Ghidra*.
To install *Ghidra* on Kali you simply run 

```
sudo apt-get install ghidra
```

After the installation we got a License Agreement which we accept and boom:

!pic 

Ghidra installed!


### 2. Debian

Decided to install it on my Debian VM as well since the Kali VM was pretty wonky.

To install *Ghidra* we need to install *Java JDK 21* as a dependency. To do that I ran these commands:

```
sudo apt update
sudo apt install openjdk-21-jdk -y
```

After that I downloaded the latest release (`12.0.2` at the time of writing) and then unzipped it to the `/opt` directory.

Then I navigated to the `/opt` folder, and ran the program by `./ghidraRun`.

!pic

Now we got *Ghidra* on the Debian 13 as well.


## b) rever-C

gkdfngipdgdfig


## c) If backwards

whrtuwehrew


## d) Nora CrackMe

hdfhofsdhuiofsdhio


## e) Nora crackme01

iipjgjipfdgji


## f) Nora crackme02

jipjipfijjdfbi


### Sources:

https://terokarvinen.com/application-hacking/#h3-no-strings-attached-tero

https://www.youtube.com/watch?v=oTD_ki86c9I

https://www.kali.org/

https://github.com/NationalSecurityAgency/ghidra

https://github.com/NoraCodes/crackmes
