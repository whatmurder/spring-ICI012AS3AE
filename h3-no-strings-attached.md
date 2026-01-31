# h3 No Strings Attached

*Disclaimer: I was absent from this weeks class so if something is done differently than shown in the class that's why.*

## a) Strings

Let's skip over all the boring set up stuff like downloading the file, installing the prerequisites etc. and jump straight into trying out the `passtr`.

We run it by opening the terminal and type in the command

```
./passtr
```

We're greeted by the prompt *"What's the password?"*
Let's just try out `password` and see what happens

![h3-a-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-a-01.png)

Dang. No bonus for me!

Since we can't look at the `passtr.c` for clues, let's see if using `cat` gives us any clues.
`cat` prints out the contents of the file to the terminal, i.e. if you had a text file called `dog.txt` and *woof* was written on it using `cat` on it would look like this:

```
$ cat dog.txt
$ woof
```

So let's see what happens!

![h3-a-02](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-a-02.png)

We got a bunch of compiling gibberish and the password and the flag in plaintext! Lucky us.

Let's try `sala-hakkeri-321` as the password:

![h3-a-03](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-a-03.png)

Nice! The password was correct and we got the flag as well: `FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}`.
Now let’s see if we can edit the program to make those harder to find.


***!!! Update just after pushing the first solution !!!***

Yeah I can't read at all apparently, I was meant to use `strings` and not whatever I felt like using lol. Let's do it the proper way now.

We'll run `strings passtr`:

![h3-a-04](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-a-04.png)

Compared to the output from `cat` it seems like we got all the same info as before but without the compilement gibberish.

Anyways we found the password and the flag from the lines 18 & 19. Now we can move on to the next exercise.

## b) passtr.c makeover

We'll move on to VSCodium outside of my VM so I can code a little easier, I've coded c using nano before and I'm not doing that again.

Here's the source code:

```
// passtr - a simple static analysis warm up exercise
// Copyright 2024 Tero Karvinen https://TeroKarvinen.com

#include <stdio.h>
#include <string.h>

int main() {
	char password[20];
	
	printf("What's the password?\n");
	scanf("%19s", password);
	if (0 == strcmp(password, "sala-hakkeri-321")) {
		printf("Yes! That's the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}\n");
	} else {
		printf("Sorry, no bonus.\n");
	}
	return 0;
}
```

Simple and clean! Now let's do a simple internet search to see if we can find a way obfuscate the password and the flag.

After some googling I found a [reddit thread](https://www.reddit.com/r/C_Programming/comments/gri58y/how_to_hide_characters_when_doing_strings_on_my/) about hiding strings in a `c` program with the same problem I'm facing here. 
In the thread there was a comment linking to a page called [Binary obfuscation - String obfuscating in C](https://yurisk.info/2017/06/25/binary-obfuscation-string-obfuscating-in-C/) by [Yuri Slobodyanyuk](https://github.com/yuriskinfo) which had an example code snippet on obfuscating strings. Here's the snippet:

```
#include <stdio.h>

#define HIDE_LETTER(a)   (a) + 0x50
#define UNHIDE_STRING(str)  do { char * ptr = str ; while (*ptr) *ptr++ -= 0x50; } while(0)
#define HIDE_STRING(str)  do {char * ptr = str ; while (*ptr) *ptr++ += 0x50;} while(0)
int main()
{   // store the "secret password" as mangled byte array in binary
        char str1[] = { HIDE_LETTER('s') , HIDE_LETTER('e') , HIDE_LETTER('c') , HIDE_LETTER('r') , HIDE_LETTER('e') 
        , HIDE_LETTER('t') , HIDE_LETTER(' ') , HIDE_LETTER('p') , HIDE_LETTER('a') , HIDE_LETTER('s'),
                HIDE_LETTER('s') ,HIDE_LETTER('w') ,HIDE_LETTER('o') ,HIDE_LETTER('r') ,HIDE_LETTER('d'),'\0' }; 

        UNHIDE_STRING(str1);  // unmangle the string in-place
        printf("Here goes the secret we hide: %s", str1);
        HIDE_STRING(str1);  //mangle back

    return 0;
}
```

My plan here is to basically convert Yuris example to the `passtr` program and see if it works or not.

So after combining the two programs together I've come up with this:

```
// passtr - a simple static analysis warm up exercise
// Copyright 2024 Tero Karvinen https://TeroKarvinen.com
// Obfusctation done by following https://yurisk.info/2017/06/25/binary-obfuscation-string-obfuscating-in-C/ 

#include <stdio.h>
#include <string.h>

#define HIDE_LETTER(a)   (a) + 0x50
#define UNHIDE_STRING(str)  do { char * ptr = str ; while (*ptr) *ptr++ -= 0x50; } while(0)

int main() {
	char password[20];
	char hidden_pwd[] = { HIDE_LETTER('s'), HIDE_LETTER('a'), HIDE_LETTER('l'), HIDE_LETTER('a'), 
		HIDE_LETTER('-'), HIDE_LETTER('h'), HIDE_LETTER('a'), HIDE_LETTER('k'), HIDE_LETTER('k'), 
		HIDE_LETTER('e'), HIDE_LETTER('r'), HIDE_LETTER('i'), HIDE_LETTER('-'), HIDE_LETTER('3'), 
		HIDE_LETTER('2'), HIDE_LETTER('1'), '\0' };
	
	printf("What's the password?\n");
	scanf("%19s", password);
	
	UNHIDE_STRING(hidden_pwd);
	if (0 == strcmp(password, hidden_pwd)) {
		printf("Yes! That's the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}\n");
	} else {
		printf("Sorry, no bonus.\n");
	}
	return 0;
}
```

The `HIDE_LETTER` function shifts the password characters to make them unreadable as plaintext after compiling.
The `UNHIDE_STRING` function on the otherhand reverses the shift.
Then the program checks if the password typed is the same as the obfuscated password by comparing them.

Now let's see if it actually works. We'll compile the program by running `gcc -o passtr-v2 passtr-v2.c`.

First let's see if the code actually works. We'll start by using a wrong password `hello` followed by running the correct password `sala-hakkeri-321`: 

![h3-b-02](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-b-02.png)

Wonderful, the program still runs as expected. Now let's see if the obfuscating actually worked or not by running `strings passtr-v2`:

![h3-b-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-b-03.png)

No password found! The obfuscation worked properly and the password is hidden.

I've included the entire output down below if you're curious.

<details>
<summary>passtr-v2 output</summary>

```
7/lib64/ld-linux-x86-64.so.2
puts
__libc_start_main
__cxa_finalize
__isoc99_scanf
strcmp
libc.so.6
GLIBC_2.7
GLIBC_2.2.5
GLIBC_2.34
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
PTE1
u+UH
What's the password?
%19s
Yes! That's the password. FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}
Sorry, no bonus.
;*3$"
GCC: (Debian 14.2.0-19) 14.2.0
Scrt1.o
__abi_tag
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
passtr-v2.c
__FRAME_END__
_DYNAMIC
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_start_main@GLIBC_2.34
_ITM_deregisterTMCloneTable
puts@GLIBC_2.2.5
_edata
_fini
__data_start
strcmp@GLIBC_2.2.5
__gmon_start__
__dso_handle
_IO_stdin_used
_end
__bss_start
main
__isoc99_scanf@GLIBC_2.7
__TMC_END__
_ITM_registerTMCloneTable
__cxa_finalize@GLIBC_2.2.5
_init
.symtab
.strtab
.shstrtab
.note.gnu.property
.note.gnu.build-id
.interp
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.note.ABI-tag
.init_array
.fini_array
.dynamic
.got.plt
.data
.bss
.comment
```

</details>

	
## c) Packd

Let's check out the other find password challenge. We'll navigate to the folder and run the program using `./packd`.

![h3-c-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-c-01.png)

Seems like it's the same type of program as `passtr` was, unfortunately `hello` wasn't the password so I'll have to go on and try to find the real password.

We'll run `strings packd` and see what's happening. Here's the entire output if you want to look through it:

<details>
<summary>packd output</summary>
	
```
wUPX!
Wv& 
leg8
,o,Q
/lib64
nux-x86-
so.2
puts
c_start_ma
cxa_f	a
c99"canf
(rcmp
GLIBC_2.7	
d534@ITM_deregH
CloneTabl\gm
}_*(
	>>H
}E]/
 CrvP
PTE1
u+UH
What's the password?
piilos-An
Yes! T,
W. FLAG{Tero-0e3bed0a89d88
51da933c64fefad
S1ry
, no bonus.
;*3$F
NDeo2
USQRH
W^YH
PROT_EXEC|PROT_WRITE failed.
_j<X
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 4.21 Copyright (C) 1996-2023 the UPX Team. All Rights Reserved. $
_RPWQM)
j"AZR^j
PZS^
/proc/self/exe
IuDSWH
s2V^
XAVAWPH
YT_j
   =>
AY^_X
D$ [I
PX!u
slIT$}
t .u
([]A\A]
0LK(L	
tL	G
+xHf
p(E1
hW 1
g{l	
s5Iw
uC^I
 k1(
p[>Z
L6AI[u
A^A_)&
csmo
m@S 
AZ,}
JAPC
SRVW~
RY?WV|YX
GCC: (Debian 12.
0-14)
I@/(
F_SYvC
{8@j"_
8crt1.o
_tag
stuff.c
deregi
m_clones)do_g
o	tors9ux5omple)do
!_fin`arr
ay_entry
me ummy
_)t*
packYcFRAME_END
DYNIC
GLOBAL_OFFSET_TABL
(libc_
\`ma
g@&IBC_
ITM_
s h!dYuO
dF_usSW
1ic9987
c5f[7
.symnb
h	 Np
.gnu.prop
bui=.
ld-idlI-J
K	dynb
ngmX
vEsi
la(	
.ehDhdr
iTQdG
dH6X
dI||
7dCv
(9yr
UPX!
UPX!
```

</details>

If we compare it to the `passtr` output we can see that the password and flag seem to be incomplete:
```
What's the password?
piilos-An
Yes! T,
W. FLAG{Tero-0e3bed0a89d88
51da933c64fefad
S1ry
, no bonus.
```

We can probably make a guess that the code has been obfuscated with something. Another thing that's different compared to `passtr` are these lines:

```
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 4.21 Copyright (C) 1996-2023 the UPX Team. All Rights Reserved. $
```

That looks like it could be a clue. Let's look into that.

Typing the url to a browser redirects you to a github page for UPX, which explains what it is. From what I gather it seems to packs/compresses programs, so deducting from that it possibly can also unpack a program.
Let's download it and see if we can do that. 

I searched for `upx debian` and found a package called `upx-ucl`, installed that using `sudo apt-get install upx-ucl`.

Now that we have it installed let's check the manual if there's some clues on how to unpack the program.

```
man upx
```

Scrolling down a little we find what were looking for:

![h3-c-02](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-c-02.png)

Let's try to uncompress `packd` by running `upx -d packd`.

![h3-c-03](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-c-03.png)

Unpacking succesful. Ran `ls` to see if it unpacked something else to the folder but it didn't. Let's see if `strings` works correctly this time.

![h3-c-04](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-c-04.png)

It did! We got the password and the flag:

```
piilos-AnAnAs
FLAG{Tero-0e3bed0a89d8851da933c64fefad4ff2}
```

Let's pack up `packd` again to see if we're correct by running `upx packd`:

![h3-c-05](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-c-05.png)

It was correct, task done!


### Sources:

https://terokarvinen.com/application-hacking/

https://www.reddit.com/r/C_Programming/comments/gri58y/how_to_hide_characters_when_doing_strings_on_my/



https://upx.github.io/
