# WHASSEMBLY!
Category: HARD, `1999` points

Heh. Assembly. Who even use it these days? 

Dare to find the flag?!

![TROLL](https://media.tenor.com/A0aM3JqMMJ0AAAAC/troll-meme.gif)

# Solution

## Method 1

1. Not really an assembly problem. It would be if the player decides to reverse engineer the source code file. (See [Method 2](#method-2-decompilation)) The file is a source code file that was made using GCC command:

```
$ gcc -c -S filename.c
```

2. To see what the flag is about, assemble the source code file.

```
$ as -o troll.o troll.s
```

3. Link the object file with the C runtime library using GCC.

```
$ gcc -o troll troll.o
```

4. Run the program.
```
$ ./troll
```

5. On running the program, it seems like the printed values are encrypted in Base64 (as shown by the equal sign at the end of the string)

6. Decrypt each line and it should generate an ASCII pattern of a Canadian Flag.

7. Lies in the middle of the flag (the maple leaf) is the flag, diagonally written!

## Method 2: Decompilation

1. There are several tools available for decompiling assembly code, including IDA Pro, Ghidra, and Hopper.

2. If they know these, well great! Because I don't. Yet. HAHAHA!

**Flag:** `RETROTECH{fishYfLAg}`