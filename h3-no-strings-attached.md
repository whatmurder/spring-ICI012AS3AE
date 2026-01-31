# h3 No Strings Attached

*Disclaimer: I was absent from this weeks class so if something is done differently than shown in the class that's why.*

## a) Strings

Let's skip over all the boring set up stuff like downloading the file, installing the prerequisites etc. and jump straight into trying out the `passtr`.

We run it by opening the terminal and type in the command

```
./passtr
```

We're greeted by the prompt *"What's the password?"*

!pic

Let's just try out `password` and see what happens

!pic

Dang. No bonus for me!

Since we can't look at the `passtr.c` for clues, let's see if using `cat` gives us any clues.
`cat` prints out the contents of the file to the terminal, i.e. if you had a text file called `dog.txt` and *woof* was written on it using `cat` on it would look like this:

```
$ cat dog.txt
$ woof
```

So let's see what happens!

!pic

We got a bunch of compiling gibberish and the password and the flag in plaintext! Lucky us.

Let's try `sala-hakkeri-321` as the password:

!pic

Nice! The password was correct and we got the flag as well: `FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}`.
Now let’s see if we can edit the program to make those harder to find.


**Update just after pushing the first solution:**

Yeah I can't read at all apparently, I was meant to use `strings` and not whatever I felt like using lol. Let's do it the proper way now.

We'll run `strings passtr`:

!pic

Compared to the output from `cat` it seems like we got all the same info as before but without the compilement gibberish.

Anyways we found the password and the flag from the lines 18 & 19. Now let's if we can make it so we can't find the password or the answer.

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

ofjsdoifjsdio


### Sources:

https://terokarvinen.com/application-hacking/

