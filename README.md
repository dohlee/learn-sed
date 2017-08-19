## Contents

[What is SED?](#What-is-SED?)

[SED cycle](#SED-cycle)

[Basic syntax](#Basic-syntax)

[Options](#Options)

[Addressing lines](#Addressing-lines)

[Commands manipulating lines](#Commands-manipulating-lines)

- [Print line (p)](#Print-line-(p))
- [Delete line (d)](#Delete-line-(d))
- [Quit (q)](#Quit-(q))
- [Substitute (s)](#Substitute-(s))
- [Append (a)](#Append-(a))
- [Translate (y)](#Translate-(y))
- [Show line numbers (=)](#Show-line-numbers (=))

[Commands manipulating buffers](#Commands-manipulating-buffers)

- [Replace pattern buffer (n)](#Replace-pattern-buffer-(n))
- [Exchange pattern buffer and hold buffer (x)](#Exchange-pattern-buffer-and-hold-buffer-(x))
- [Copy contents of pattern buffer to hold buffer (h)](#Copy-contents-of-pattern-buffer-to-hold-buffer-(h))
- [Append contents of pattern buffer to hold buffer (H)](#Append-contents-of-pattern-buffer-to-hold-buffer-(H))
- [Copy contents of hold buffer to pattern buffer (g)](#Copy-contents-of-hold-buffer-to-pattern-buffer-(g))
- [Append contents of hold buffer to pattern buffer (G)](#Append-contents-of-hold-buffer-to-pattern-buffer-(G))

[Regular expressions](#Regular-expressions)



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

TODO

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

