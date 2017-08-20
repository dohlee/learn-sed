## Contents

[What is SED?](#what-is-sed)

[SED cycle](#sed-cycle)

[Basic syntax](#basic-syntax)

[Options](#options)

[Addressing lines](#addressing-lines)

[Commands manipulating lines](#commands-manipulating-lines)

- [Print line (p)](#print-line-p)
- [Delete line (d)](#delete-line-d)
- [Quit (q)](#quit-q)
- [Substitute (s)](#substitute-s)
- [Append (a)](#append-a)
- [Translate (y)](#translate-y)
- [Show line numbers (=)](#show-line-numbers-)

[Commands manipulating buffers](#commands-manipulating-buffers)

- [Replace pattern buffer (n)](#replace-pattern-buffer-n)
- [Exchange pattern buffer and hold buffer (x)](#exchange-pattern-buffer-and-hold-buffer-x)
- [Copy contents of pattern buffer to hold buffer (h)](#copy-contents-of-pattern-buffer-to-hold-buffer-h)
- [Append contents of pattern buffer to hold buffer (H)](#append-contents-of-pattern-buffer-to-hold-buffer-h)
- [Copy contents of hold buffer to pattern buffer (g)](#copy-contents-of-hold-buffer-to-pattern-buffer-g)
- [Append contents of hold buffer to pattern buffer (G)](#append-contents-of-hold-buffer-to-pattern-buffer-g)

[Regular expressions](#regular-expressions)



## What is SED?

SED stands for **Stream EDitor**, developed by Lee E. McMahon of Bell Labs. SED can be used for text manipulation, such as text substitution, selective line deletion, and selective line output. Although there exists another powerful stream editing tool, **awk**, you still can solve any text transformation task with SED. In this tutorial, we will focus on solving many real-life task with SED. 



## SED cycle

You should be acquainted with the workflow of SED in order to write your SED codes proficiently. When you type SED commands and hit `ENTER` , the internal structure of SED program looks as below:

![](/img/sed_cycle.png)

**Details**

1. *Read*: Read a line from input stream(e.g. stdin, text file, pipe, ...). The line will be stored in *pattern buffer*, at which SED commands will be executed.
2. *Execute*: Execute SED commands in pattern space. 
3. *Display*: Display the result of execution to stdout.





## Basic syntax 

You can use SED in either of two ways; using **inline commands** or **SED script file**. 

The syntax is as follows:

```shell
sed [-n] [-e] 'inline commands'  # use inline commands

sed [-n] -f 'SED script file'    # use SED script file
```

You can supply text which you want to manipulate to SED in various ways.

```shell
echo 'your_text_here' | sed 'commands'  # use pipe

sed 'commands' your_text_file.txt       # use text file

sed 'commands'                          # use SED session
type_your_text_here
...and so on
(Ctrl-D to exit)
```

## Options

TODO

## Addressing lines

TODO

## Commands manipulating lines

### Print line (p)

TODO

### Delete line (d)

TODO

### Quit (q)

TODO

### Substitute (s)

TODO

### Append (a)

TODO

### Translate (y)

TODO

### Show line numbers (=)

TODO

## Commands manipulating buffers

### Replace pattern buffer (n)

TODO

### Exchange pattern buffer and hold buffer (x)

TODO

### Copy contents of pattern buffer to hold buffer (h)

TODO

### Append contents of pattern buffer to hold buffer (H) 

TODO

### Copy contents of hold buffer to pattern buffer (g)

TODO

### Append contents of hold buffer to pattern buffer (G)

TODO



## Regular expressions

