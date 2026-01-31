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

Nice! The password wascorrect and we got the flag as well: `FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}`.
Now let’s see if we can edit the program to make those harder to find.


## b) passtr.c makeover

sdfushfgosuf


## c) Packd

ofjsdoifjsdio


### Sources:

https://terokarvinen.com/application-hacking/

