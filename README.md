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

sed [-n] -f 'your_sed_script_file'    # use SED script file
```

(Just think of SED script file as a list of SED commands.)

Supply text which you want to manipulate to SED in various ways.

```shell
echo 'your_text_here' | sed 'commands'  # use pipe

sed 'commands' your_text_file.txt       # use text file

sed 'commands'                          # use SED session
type_your_text_here
...and so on
(Ctrl-D to exit)
```

## Options

Let's look at some must-know options.

**-n: No auto-print.** By default, the content of the pattern buffer is printed before fetching new line from the text. **-n** option prevents this default printing of pattern buffer unless the explicit print command is provided. Thus, the example below does not show any output.

```shell
sed -n '' your_text_file.txt  # no output
```

**-e <command>: Specify the next argument is SED command.** With this option, we can specify multiple SED commands within a single call of SED program. The example below substitutes *old_pattern* with *new_pattern*, and prints the resulting line which is inside the pattern buffer. Don't worry that you don't know those **s** and **p** command now. They'll be covered soon.

```shell
sed -n -e 's/old_pattern/new_pattern/g' -e 'p' your_text_file.txt
```

**-i[SUFFIX]: Edit files in-place.** The resulting output of SED will replace the original file. When you specify SUFFIX, backup file will be generated. For the name of the backup file, SUFFIX will be appended to the name of original file.  The example below substitutes *old_pattern* with *new_pattern*, and replaces *your_text_file.txt* with substituted lines while saving *your_text_file.txt.bak* as a backup file.

```shell
sed -i.bak -n 's/old_pattern/new_pattern/g' your_text_file.txt
```

- *WARNING: You should be careful to use this option without SUFFIX since it does not make any backup file.*

**-f: Add the content of SED script file to the commands to be executed.** Note that you can use **-e** and **-f** option together. Just keep in mind that SED commands are processed in order they specified. Thus, in the second example below, the commands in script file will be invoked first, then the inline command will be invoked.

```shell
sed -f 'your_SED_script_file' your_text_file.txt

# you can do this, too
sed -f 'your_SED_script_file' -e 's/old_pattern/new_pattern/g' your_text_file.txt
```

## Addressing lines

You might want to manipulate not every lines of the text, but specific lines that you've selected. There are diverse ways in SED to select lines, which can be specified with **addresses** in SED commands. In this section, we'll deal with the text with line numbers for simplicity's sake.

```shell
seq 10 > my_text.txt
```

The basic form of addressing lines in SED is as below:

```shell
[address1[,address2]]
```

where address1, address2 can be any of addressing modes explained below. 

### Number addressing

Simply you can address lines with their line numbers. The first address defines the starting line from which the commands will be executed, and the optional second address defines the last line. Note that both ends are inclusive, and every address is . The example below prints out the third, fourth, and fifth line of the file. For now, just focus on the addressing 3,5, not p which prints the current contents of the pattern buffer.

```shell
sed -n '3 p' my_text.txt  # try this command without -n option. what happens?
```

```shell
3
```

```shell
sed -n '3,5 p' my_text.txt
```

```shell
3
4
5
```

Not only can you select consecutive lines, but also every n-th lines.

```shell
sed -n '1~2 p' my_text.txt  # print odd-numbered lines
```

```shell
1
3
5
7
9
```

Address 0 cannot be used solely, however, it is allowed to specify stepwise range like below:

```shell
sed -n '0~3 p' my_text
```

```shell
3
6
9
```

### Regular expression addressing

Use regular expression to select matching lines.

```shell
sed -n '/1/ p' my_text.txt  # Regular expression addresses should be /regexp/ form
```

```shell
1
10
```

 When they are used to define the range, it goes a bit tricky. 

```shell
/regexp1/[,/regexp2/]
```

The first line containing the pattern that matches regexp1 will be the first line from which the execution of the commands starts. Then sed reads a line, executes commands, and repeat, until the line containing the pattern that matches regexp2. Get the feel of regular expression addressing with examples below:

```shell
sed -n '/2/,/5/ p' my_text.txt
```

```shell
2
3
4
5
```

```shell
sed -n '/6/,/5/ p' my_text.txt  # it looks like an empty range, but...
```

```
6
7
8
9
10
```

### Addressing the last line

Simply select last line with special character '$'.

```shell
sed -n '$ p' my_text.txt
```

```shell
10
```

You might be curious about how to select the last two lines...

Unfortunately, there's no simple answer with pure SED. Go to [quizzes]() and solve tricky problems like this one!

### Hybrid addressing

Feel free to mix number addressing and regular expression addressing.

```shell
sed -n '1,/5/ p' my_text.txt
```

```shell
1
2
3
4
5
```

```shell
sed -n '1,/1/ p' my_text.txt  # notice the difference with the example just below
```

```shell
1
2
3
4
5
6
7
8
9
10
```

Here again, we can use address 0 when we use hybrid addressing (this is for GNU sed).

```shell
sed -n '0,/1/ p' my_text.txt
```

```shell
1
```

### Advanced addressing (for GNU sed)

It is always good to know fancy ones.

This addressing matches address1 and following N lines.

```shell
[address1,[+N]]
```

```shell
sed -n '/2/,+3 p' my_text.txt
```

```shell
2
3
4
5
```

This addressing matches address1 and following lines until the line number is a multiple of N.

```shell
[address1,[~N]]
```

```shell
sed -n '/6/,~4 p' my_text.txt
```

```shell
6
7
8  # 8 is a multiple of 4 :)
```

## Commands manipulating lines

Let's assume my_text.txt file is as below:

```Shell
One apple
Two banana
Three cat
Four dog
Five elephant
Six frog
Seven gorilla
```

### Print line (p)

We've already seen lots of p commands in examples above. As you can guess, it prints the content of the pattern buffer. Basic syntax of p command is as below:

```shell
[address1,[address2]] p
```

Do you remember **-n** option? By default, SED prints the content of the pattern buffer before it fetches a new line from text. **-n** option would prevent this auto-printing of pattern buffer. Then, can you guess the output of the following command?

```shell
sed 'p' my_text.txt
```

```shell
One apple
One apple
Two banana
Two banana
Three cat
Three cat
Four dog
Four dog
Five elephant
Five elephant
Six frog
Six frog
Seven gorilla
Seven gorilla
```

Each line is printed twice, because of SED's default auto-print and p command.

Usually we want this, each line printed once.

```shell
sed -n 'p' my_text.txt  # no auto-print
```

```shell
One apple
Two banana
Three cat
Four dog
Five elephant
Six frog
Seven gorilla
```

Print specific lines with addressing.

```shell
sed -n '2,4 p' my_text.txt
```

```shell
Two banana
Three cat
Four dog
```

When p command is used with other commands, it is often hard to expect the output. Just remember: p command prints the current data stored in the pattern buffer.

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

