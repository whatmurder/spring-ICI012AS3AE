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

We start by creating a new project and importing the `passtr`. Just to be safe I'll make a copy of the original `passtr` by running this in the terminal:

```
cp passtr passtr-orig
```

Now we have a backup of the `passtr` and we're free to mess around with the binary.

Now let's open up the `passtr` on *Ghidra*:

!pic

Here's the main function:

```
undefined8 main(void)

{
  int iVar1;
  char local_28 [32];
  
  puts("What\'s the password?");
  __isoc99_scanf(&DAT_0010201d,local_28);
  iVar1 = strcmp(local_28,"sala-hakkeri-321");
  if (iVar1 == 0) {
    puts("Yes! That\'s the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}");
  }
  else {
    puts("Sorry, no bonus.");
  }
  return 0;
}
```

We can rename the variables like we did in the previous exercise:

* `iVar1` => `ifMatch`
* `local_28` => `userInput`

So from my understanding the only change we'd have to do here is change this `if (iVar1 == 0)` to this ` if (iVar1 != 0)`.

So what the code would look like is this:

```
undefined8 main(void)

{
  int ifMatch;
  char userInput [32];
  
  puts("What\'s the password?");
  __isoc99_scanf(&DAT_0010201d,local_28);
  iVar1 = strcmp(local_28,"sala-hakkeri-321");
  if (iVar1 != 0) {
    puts("Yes! That\'s the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}");
  }
  else {
    puts("Sorry, no bonus.");
  }
  return 0;
}
```

So next up is figuring out how one would do this. You can't just edit the code in the decompiler window at all so that's out of the question. Let's see what else I could do.

Clicking around the disassebler highlights rows in the listing view. The way I'm reading this is how the assembly version of the program flows. In example if I highlight the `if (iVar1 == 0)` it highlights these rows in the listening view:

!pic


```
001011a1 85 c0           TEST       ifMatch,ifMatch
001011a3 75 11           JNZ        LAB_001011b6
```

The `001011a1` and `001011a3` are memory addresses, and the `TEST` and `JNZ` are instructions. I can deduce that the `TEST` instructions tests something (🤯), in our case the `ifMatch` variable. What `JNZ` and `LAB_001011b6` mean I got no clue. Let's do some research.

I found a wiki page explaining what `JNZ` means:

> #### Description
> * The jnz (or jne) instruction is a conditional jump that follows a test.
> * It jumps to the specified location if the Zero Flag (ZF) is cleared (0).
> * jnz is commonly used to explicitly test for something not being equal to zero whereas jne is commonly found after a cmp instruction.

There's also the `JZ` instruction:

> #### Description
> * The jz instruction is a conditional jump that follows a test.
> * It jumps to the specified location if the Zero Flag (ZF) is set (1).
> * jz is commonly used to explicitly test for something being equal to zero whereas je is commonly found after a cmp instruction.

There's also an arrow in the listing view in the `JNZ` row that leads down to a part of the code that says 

!pic

```
                             LAB_001011b6                                    XREF[1]:     001011a3(j)  
        001011b6 48 8d 05        LEA        ifMatch,[s_Sorry,_no_bonus._0010207e]            = "Sorry, no bonus."
                 c1 0e 00 00

```

Sooooo... Let's try switching `JNZ` to `JZ` and see what happens. To edit the `JNZ` I'll just right click it and select *Patch Instruction*.

!pic

!pic

Nice. Let's save the file and export it:

!pic

and now let's move in to the directory where I saved it and run it.

We run it and use *hello* as the password and see if that works:

!pic

That worked! Now let's use the actual password *sala-hakkeri-321*:

!pic

No bonus which means it worked and now we're done! 


## d) Nora CrackMe

We download the *CrackMe* by running

```
git clone https://github.com/NoraCodes/crackmes
```

Except I can't because I didn't have `git` installed so we run 

```
sudo apt-get install git
```

Now let's git clone it again and boom we did it

!pic

For the next set of exercises we have to compile the files `crackme01`, `crackme01e` and `crackme02`.

According to the readme, to compile the files we need to run `make <name>` to compile the files:

>To work with them, run `make <name>` where `<name>` is one of `crackme01`, `crackme02`, etc. Figure out how to make the crackme exit with the status code 0.

Because I'm lazy I'll just run `make all` in the directory:

!pic

Boom. Now we can move on to cracking the programs.


## e) Nora crackme01

iipjgjipfdgji


## e) (again?) Nora crackme01e

aaaaaaaaaaaaaaaaaaaaaaaaa

## f) Nora crackme02

jipjipfijjdfbi


### Sources:

https://terokarvinen.com/application-hacking/#h3-no-strings-attached-tero

https://www.youtube.com/watch?v=oTD_ki86c9I

https://www.kali.org/

https://github.com/NationalSecurityAgency/ghidra

https://www.w3schools.com/c/ref_string_strcmp.php

https://www.aldeid.com/wiki/X86-assembly/Instructions/jnz

https://www.aldeid.com/wiki/X86-assembly/Instructions/jz

https://github.com/NoraCodes/crackmes
