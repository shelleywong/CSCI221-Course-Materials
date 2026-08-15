# Lab 1: Data Lab (Part 1)

Learning Bit Magic

## Introduction

The purpose of this assignment is to become more familiar with bit-level representations of integers and floating point numbers. You’ll do this by solving a series of programming “puzzles”. Many of these puzzles are quite artificial, but you’ll find yourself thinking much more about bits in working your way through them.

### Logistics

This is an individual project. Clarifications and corrections will be posted in this Git repo or on Canvas.

## Handout Instructions

Start by downloading `datalab-handout1.tar` from Canvas and copying it to a (protected) directory on ecc-linux or another Linux x86_64 machine in which you plan to do your work. To securely transfer files between a remote host (like ecc-linux) and your local computer, use the `scp` command or another option that supports SFTP (secure file transfer protocol).

Create a folder for your lab:
```bash
$ mkdir lab1
```

Then give the command to extract the files from the provided tar file into your `lab1` folder:

```bash
$ tar -xvf datalab-handout1.tar -C lab1 --strip-components=1
```

This will cause a number of files to be unpacked in the directory. The only file you will be modifying and turning in is **bits.c**.

The bits.c file contains a skeleton for each of the programming puzzles you will need to complete. For Data Lab (Part 1), there are 8 puzzles. Your assignment is to complete each function skeleton using only straight-line code for the integer puzzles (i.e., no loops or conditionals) and a limited number of C arithmetic and logical operators.

Specifically, you are only allowed to use the following:
1. Integer constants 0 through 255 (0xFF - up to 8 bits), inclusive. You are not allowed to use big constants such as 0xffffffff.
2. Function arguments and local variables (no global variables).
3. Unary integer operations `! ~`
4. Binary integer operations `& ˆ | + << >>`

A few of the functions further restrict this list.

You are expressly forbidden to:
1. Use any control constructs such as if, do, while, for, switch, etc.
2. Define or use any macros.
3. Define any additional functions in this file.
4. Call any functions.
5. Use any other operations, such as &&, ||, -, or ?:
6. Use any form of casting.
7. Use any data type other than int. This implies that you cannot use arrays, structs, or unions.

You may assume that your machine:
1. Uses two's complement, 32-bit representations of integers.
2. Performs right shifts arithmetically.
3. Has unpredictable behavior when shifting if the shift amount is less than 0 or greater than 31.

See the comments in bits.c for detailed rules and a discussion of the desired coding style.

## The Puzzles

The puzzles that you will be solving in bits.c will include functions that manipulate and test sets of bits, as well as functions that make use of the two’s complement representation of integers. Each function will include a description of the expected function behavior, a comment with a “Rating” field that gives the difficulty rating (the number of points) for the puzzle, and the “Max ops” field that gives the maximum number of operators you are allowed to use to implement each function.

For Data Lab (Part 1), you will need to complete 8 puzzles. Three of the puzzles will have a rating of 1, and five of the puzzles will have a rating of 2.

See the comments in bits.c for more details on the desired behavior of the functions. You may also refer to the test functions in tests.c. These are used as reference functions to express the correct behavior of your functions, although they don’t satisfy the coding rules for your functions.

## Evaluation

For Data Lab Part 1, your score will be computed out of a maximum of 29 points based on the following distribution:
- 13 Correctness points
- 16 Performance points

* **Correctness points.** The 8 puzzles you must solve have been given a difficulty rating of 1 or 2, such that their weighted sum totals to 13. We will evaluate your functions using the `btest` program, which is described in the next section. You will get full credit for a puzzle if it passes all of the tests performed by `btest`, and no credit otherwise.

* **Performance points.** Our main concern at this point in the course is that you can get the right answer. However, we want to instill in you a sense of keeping things as short and simple as you can. Furthermore, some of the puzzles can be solved by brute force, but we want you to be more clever. Thus, for each function we’ve established a maximum number of operators that you are allowed to use for each function. This limit is very generous and is designed only to catch egregiously inefficient solutions. You will receive two points for each correct function that satisfies the operator limit.

### Auto-grading your work

We have included some auto-grading tools in the handout directory — `btest`, `dlc`, and `driver.pl` — to help you check the correctness of your work.

* **btest**: This program checks the functional correctness of the functions in bits.c. To build and use it, type the following two commands:

```bash
$ make
$ ./btest
```

Notice that you must rebuild btest each time you modify your bits.c file.
You’ll find it helpful to work through the functions one at a time, testing each one as you go. You can use the -f flag to instruct btest to test only a single function:

```bash
$ ./btest -f bitAnd
```

You can feed it specific function arguments using the option flags -1, -2, and -3:

```bash
$ ./btest -f bitAnd -1 7 -2 0xf
```

Check the file README for documentation on running the btest program.

* **dlc**: This is a modified version of an ANSI C compiler from the MIT CILK group that you can use to check for compliance with the coding rules for each puzzle. The typical usage is:

```bash
$ ./dlc bits.c
```

The program runs silently unless it detects a problem, such as an illegal operator, too many operators, or non-straight-line code in the integer puzzles. Running with the -e switch causes dlc to print counts of the number of operators used by each function:

```bash
$ ./dlc -e bits.c
```
 Type ```./dlc -help``` for a list of command line options.

* **driver.pl**: This is a driver program that uses btest and dlc to compute the correctness and performance points for your solution. It takes no arguments:

```bash
$ ./driver.pl
```
I will use **driver.pl** to grade your solution. If it fails to run you can almost guarantee that you will be losing significant credit on the assignment, if not getting a 0.

## Submitting your Assignment

Submit your bits.c file to INGInious.

After Data Lab Part 1 is due, you will get 10 points for correctly answering in-class quiz questions about the bit puzzles and bit-level representations of integers. If you do not submit anything for Data Lab Part 1, you will not receive credit for the in-class quiz.

## Advice
* Using `printf` for debugging can be helpful, but you should not include the <stdio.h> header file in your bits.c file, as it confuses `dlc` and results in some non-intuitive error messages. You will still be able to use `printf` in your bits.c file for debugging without including the <stdio.h> header, although gcc will print a warning that you can ignore.
* All variables must be declared at the beginning of the function. The dlc program enforces a stricter form of C declarations than is the case for C++ or that is enforced by gcc. In particular, any declaration must appear in a block (what you enclose in curly braces) before any statement that is not a declaration. For example, it will complain about the following code:

```C
int foo(int x){
  int a = x;
  a *= 3;     /* Statement that is not a declaration */
  int b = a;  /* ERROR: Declaration not allowed here */
}
```

* You will need 32-bit libraries to compile this assignment on your machines. For Ubuntu and Debian systems, this can be installed via the `gcc-multilib` library.
* Order of operations is important. Compiler optimizations can change the order of the source code, but the provided Makefile is set to not optimize generated code (you want the code to execute in the same order as the source code).
* Depending on your implementation, the rating may not appear to reflect the difficulty of the problem. If you are feeling stuck on one problem, move on to another one. You can go back to the previous problem later.
