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

*Ghidra* installed!


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

We'll start this one by downloading the `eszbin-challenges.zip` and unzipping it to a new folder. The files could be the same as the ones in the previous *h3* exercise and we can check that pretty easily by running `sha256sum` on them.

Running `sha256sum` on both of the `packd` files before decompressing them with `upx -d` gives us the shasum of `547c98003b9db6852b00851b09435cb41c134db920bb4b18455bd5214b40e4a4` on both of the files.

Since the files are the same and the `packd` is compressed, we'll decompress it first by running `upx -d packd`.

AND NOW we can open up the program on *Ghidra*.

I created a new project file and imported the `packd`, analyzed it and now let's get to it.

The goal being finding the main function I don't know if I lucked out but it seems like the main function just popped up on the main compiler immediately.

!pic

this is what the main function looks like before any editing:

```
undefined8 main(void)

{
  int iVar1;
  char local_28 [32];
  
  puts("What\'s the password?");
  __isoc99_scanf(&DAT_0010201d,local_28);
  iVar1 = strcmp(local_28,"piilos-AnAnAs");
  if (iVar1 == 0) {
    puts("Yes! That\'s the password. FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}");
  }
  else {
    puts("Sorry, no bonus.");
  }
  return 0;
}
```

So what we can remember from the *h3* exercise this program asks for a password and then checks if it's correct.

We have two variables in the program: `int iVar1;` and `char local_28 [32];`. There's a string compare (`strcmp`) that compares the user input to the real password. How the `strcmp` works is that if the strings match, it returns a *0*, and if not, it'll either return a positive or a negative number.

So the basic flow of the program is this:

1. Program initializes the variables.
2. It prints out *"What's the password?"*
3. The user types in a password guess.
4. If the user input and the password match the value of `iVar1` becomes 0, the if statement becomes true and the program prints *"Yes! That's the password. FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}"*
5. If the user input and the password doesn't match, the program prints out *"Sorry, no bonus."*.

So to rename the variables I'm thinking these would be logical:

* `iVar1` => `ifMatch`
* `local_28` => `userInput`

This is what the program would look like:

!pic

Beautiful.

I've recreated what the code for `packd` could look like down below if you want to check it out:

<details>
<summary>packd.c</summary>

```
#include <stdio.h>
#include <string.h>

int main(void) {
  int ifMatch;
  char userInput[32];
  
  printf("What's the password?\n");
  scanf("%31s", userInput);
  ifMatch = strcmp(userInput, "piilos-AnAnAs");
  if (ifMatch == 0) {
    printf("Yes! That's the password. FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}\n");
  }
  else {
    printf("Sorry, no bonus.\n");
  }
  return 0;
}
```

</details>


## c) If backwards


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

https://www.w3schools.com/c/ref_string_strcmp.php

https://github.com/NoraCodes/crackmes
