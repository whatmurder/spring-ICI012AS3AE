# h0 Binary

For this exercise you had to create a simple program and then compile it. I decided to use C for the task since that's the language I've compiled with the most.
I hadn't coded in a while or compiled for that matter so I used Google for help.

Tools used for this task:
*VScode

## Writing the code

Writing the code was pretty simple. I decided to do a "Hello World" program.
I tried getting as far as I could without googling and ended up with this:
```
int main() {
    printf("Hello World");
}
```

Then I tried to compile it but got errors because I forgot to include the necessary library for the printf command.

The revised code looked like this:

```
#include <stdio.h>

int main() {
    printf("Hello World");
}
```

Then I tried compiling the code again and it looked like and now the compiling worked. My hello-world.c turned into hello-world.

Then I opened the terminal and ran the code like this

```
./hello-world
```

and the output was:

```
Hello world%
```

And that's it. We've compiled the code succesfully.

## The Binary

In VScode previewing the file gives this warning:
*The file is not displayed in the text editor because it is either binary or uses an unsupported text encoding.*

Pressing the open anyway button opens the file and then just gives unicode gibberish for you to view.
The amount of characters is 33674. A machine understands it can read it but I can't. Too bad.

### Sources:

https://www.geeksforgeeks.org/c/c-hello-world-program/
