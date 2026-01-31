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


## c) Packd

Let's check out the other find password challenge. We'll navigate to the folder and run the program using `./packd`.

![h3-c-01](https://github.com/whatmurder/spring-ICI012AS3AE/blob/main/img/h3-c-01.png)

Seems like it's the same type of program as `passtr` was, unfortunately `hello` wasn't the password so I'll have to go on and try to find the real password.

We'll run `strings packd` and see what's happening. Here's the entire output if you want to look through it:

<details>
<summary>Output</summary>
	
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

https://upx.github.io/
