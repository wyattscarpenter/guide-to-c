"What does my program do
Besides fill memory?" --Jonathan Blow

1. Introduction

1.1. Preamble

Hello and welcome to the weird and wacky world of C programming. This is a guide to C programming for the complete beginner.

1.2. Disclaimer

I release this work into the public domain under CC0. If you use it or find it useful in some way, I would appreciate credit and notice, but that request is not legally binding because, as I just said, I release this work into the public domain under CC0.

THE TEXT IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS TEXT INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS TEXT. ALL NAMES, VARIABLES, AND STRINGS IN THIS TEXT ARE PURELY FICTITIOUS. ANY RESEMBLANCE TO ACTUAL PROGRAMS, LIVING OR DEAD, IS ENTIRELY COINCIDENTAL.

1.3. Forgetting Everything

The first step to learning C, if you haven't learned any low-level programming languages before, is to completely forget any knowledge you have about programming. Once you learn C, you will be able to relate it to other programming languages you might know in a productive way, but while you are learning C any similarities you notice will simply trip you up. Some would argue that you should also forget your preconceptions about the English language to productively learn C, but unfortunately I can't advise that as I need you to read the rest of this guide.

1.4. Chekhov's Gun: Installing a C Compiler

A C program is a text document on your computer. You will need to install a program on your computer to transform C programs into files that the computer can execute ("executables"). The most popular such program is GCC, which is also Free, so I suggest you use it. Google how to install gcc on your operating system and do that. This may take you some time to figure out; I suggest you treat this as a task of comparable difficulty to writing a complete program. On Ubuntu this may be as simple as running `sudo apt install gcc` on the command line, and on Windows this may be as "simple" as installing Bash for Windows and running `sudo apt install gcc` on the Bash command line. It doesn't really matter what compiler you install or how you install it, so long as it works. You are free to make substitutions so long as you can get something working.

Once you have a compiler, you should be able to run "cc" (stands for "c compiler") on the command line (google how to use the command line on your operating system) and observe the message from your compiler complaining that you haven't provided a c program. Note that the message from your compiler complaining it can't find a c program looks much different than the message your command line will give you if it can't find your compiler, even though both are complaints about not finding something. If that doesn't work, maybe your compiler didn't take the conventional step of setting the name "cc" to refer to it, so you should trying running your compiler directly. Wherever you installed your compiler from should have instructions about this, so I won't go into it. Again, remember that this is the hardest part, so don't get discouraged.

2. The Von Neumann Architecture

2.1. Memory: All Alone in the Moonlight

A computer is, essentially, an enormous number of tiny electrical switches set to either on or off. The entire purpose of a computer program is to manipulate the switches to form a particular pattern in a judicious manner. On is known as 1 and off is known as 0. In this way, you can conceive of the computer as a big bank of 1s and 0s scattered around in a big line or grid, like so:

01001000011001010110110001101100011011110010000001100001011011100110010000100000011101110110010101101100011000110110111101101101011001010010000001110100011011110010000001110100011010000110010100100000011101110110010101101001011100100110010000100000011000010110111001100100001000000111011101100001011000110110101101111001001000000111011101101111011100100110110001100100001000000110111101100110001000000100001100100000011100000111001001101111011001110111001001100001011011010110110101101001011011100110011100101110001000000101010001101000011010010111001100100000011010010111001100100000011000010010000001100111011101010110100101100100011001010010000001110100011011110010000001000011001000000111000001110010011011110110011101110010011000010110110101101101011010010110111001100111001000000110011001101111011100100010000001110100011010000110010100100000011000110110111101101101011100000110110001100101011101000110010100100000011000100110010101100111011010010110111001101110011001010111001000101110...

Each 1 or 0 here is a digit. Since each digit can have one of two values, 0 or 1, they are refered to as "binary digits" or "bits" for short. "Binary" comes from the latin word "binarius" meaning "consisting of two". You are probably familiar with a different type of digit, the conventional sort of digit in our society, which can have one of ten values, from 0 to 9. Those are referred to as "decimal digits" from the latin "decimalis" meaning "consisting of ten". There are other sorts of digits, and they can all be used to represent the same underlying values.

For convenience, we group these bits into bytes of 8 bits. 8b = 1B. The byte is the smallest amount of memory you can talk about to a computer. A byte can hold 256 values (often 0 to 255 are picked), so we could represent a series of bytes as:

72 101 108 108 111 32 97 110 100 32 119 101 108 99 111 109 101 32 116 111 32 116 104 101 32 119 101 105 114 100 32 97 110 100 32 119 97 99 107 121 32 119 111 114 108 100 32 111 102 32 67 32 112 114 111 103 114 97 109 109 105 110 103 46 32 84 104 105 115 32 105 115 32 97 32 103 117 105 100 101 32 116 111 32 67 32 112 114 111 103 114 97 109 109 105 110 103 32 102 111 114 32 116 104 101 32 99 111 109 112 108 101 116 101 32 98 101 103 105 110 110 101 114 46...

There are other ways, of course. For instance, programmers are very fond of powers of two, because one bit has 2^1=2 possible states, two bits have 2^2=4 possible states, three bits have 2^3=8 possible states, four bits have 2^4=16 possible states, and so on. One byte, being 8 bits, has 2^8=256 possible states. In choosing how many possible values a digit can have, one has to acknowledge the trade-off: more possible values per digit means larger numbers can be written more compactly, but each possible value per digit must be represented by a glyph which must be remembered, which can be difficult for the reader. A good trade-off seems to be around 10 values per digit. As such, programmers often decide to use a digit system with 16 values per digit ("hexadecimal" or "hex" for short). The values that represent 10 through 15 are represented by A through F. This has the convenient property that each byte is represented by exactly two hexadecimal digits:

48656C6C6F20616E642077656C636F6D6520746F2074686520776569726420616E64207761636B7920776F726C64206F6620432070726F6772616D6D696E672E2054686973206973206120677569646520746F20432070726F6772616D6D696E6720666F722074686520636F6D706C65746520626567696E6E65722E...

Choosing octal (0-8) is a tempting alternative to hexadecimal, and is used in some rare occasions, but you run into the problem that a byte is 8 bits, an octal digit is 3 bits, so you can fit 2.7 octal digits in a byte, which just gets confusing.

Hex numbers are usually written as, for example, 0x48 to indicate that they are hex numbers and not decimal numbers. The x represents the x in hex, and the 0 lets you know at the start that you are dealing with a number.

Another possibility is saying that each possible byte value gets its own unique character to represent it. In fact, this is basically how text is stored in a computer. If we interpret the bytes we have as textual characters ("char"s for short) under the ASCII standard, a very popular text-encoding standard, we get:

Hello and welcome to the weird and wacky world of C programming. This is a guide to C programming for the complete beginner.

Note that while this representation is sometimes useful, almost no text encodings actually have exactly 256 printable characters, one for each possible value of a byte. This is because they are bad. So, when looking at arbitrary values as ascii data, only a slice of possible values will be readable.

At this point you should have a good handle on multiple ways to represent the bits inside computers. Hopefully, this will make later explanations easier. Collectively, these bits are known as "memory". There are different types of memory inside a computer, but the C programming language is mostly designed so you can ignore those more subtle distinctions.

2.2. Numbers

At this point, with a bit of reflection, it should become apparent to you that we can add bytes. As simply as we might 72 and 101, we could add the bytes hold those values and receive the value 173. You can do all sorts of arithmetic operations like this. If you want to deal with numbers larger than 255, you can treat several consecutive bytes as representing parts of the same number. For instance, two bytes containing 0x48 and 0x65 might be treated as two parts of one number containing 0x4865.

Naturally, the question arises: if I have two bytes that represent the value 0x4865 by being 0x48 and 0x65, should the 0x48 come before the 0x65 or after? The answer is obviously that 0x48 should come first and then 0x65, but computer scientists are bad so they usually make the 0x65 come first and then the 0x48. That is, on most modern machines the string of two bytes we might write out as 6548 represents the integer 0x4865. I find this personally disappointing, but it is ultimately an arbitrary decision of ordering. Machines on which the string of two bytes we might write out as 6548 represents the integer 0x6548 are known as "big-endian" because they start eating the big end of the integer first. Most modern machines are little-endian. Knowing about this is probably usless to you right now, but it will be useful to you later, probably once you've forgotten about it.

The most common type of integer in C programs is 4 bytes long, which is good enough for many purposes.

2.3. Numbers 2: The Numbering

The more mathematically inclined among you may have noticed that, technically, an integer can be negative. This is correct. Most integers stored on computers are "signed" meaning they can be either positive or negative (ie they may be thought of as having a negative sign in front of them if they are negative) and are stored in something called "two's complement". The details of this are not important right now, as evidenced by the fact that I can't remember them off the top of my head, but suffice to say that some of the possible values of the integer are relagated to representing negative numbers instead.

If you don't want negative values, you can choose to use "unsigned integers", which use their full range of values to represent numbers greater than or equal to 0.

It's important to note that the bytes themselves do not store information about what type of thing they represent. If we encounter the string of four bytes FFFFFFFF and chose to interpret them as a signed integer, we will interpret them as representing -1. If we chose to interpret them as an unsigned integer, we will interpret them as representing 0xFFFFFFFF.

Similar to the various types of integers, there are various ways to store a non-integer number (ie a rational number) on a computer. One of the most popular is called "floating point" and again we won't get into the implementation details.

2.4. Addresses

An important note, though you may find its purpose obscure at this point, is that we number all the bytes on your computer. If you wish to refer to a specific byte in memory, you can refer to it by number, its "address". Addresses are usually stored as an unsigned integer eight bytes long.

Now, this is well and good, but you might notice that typically you have more than one program running on your computer. What if they both try to reference the same byte by number? They would probably interfere with each other. Since we typically don't want that, each program on your computer gets to live in its own "virtual memory" where the addresses they reference are mapped to unique addresses in the underlying machine. If you don't understand this, don't worry, the whole point is that you and your program can pretend, for addresssing purposes, that you're only running one program on the machine.

2.5. Instructions

You're almost ready to write a C program and have a vague idea of what you're doing. You only need to understand one more concept: machine instructions.

Keep in mind that everything that follows is a vast oversimplication and a mere illustration of a concept. It does not demonstrate any real instruction set.

The general way a computer operates is that a list of bytes is interpreted as a series of instructions to the machine. The computer goes down this list in order, executing each instruction in turn.

For instance, let us suppose, completely arbitrarily, that the byte 48 represents an instruction called ADD, and so the byte string 48 56C6C6F20616E64 2077656C636F6D65 20746F2074686520 represents the command to the computer "store into the 4-byte integer at starting at byte 0x56C6C6F20616E64 the result of adding the 4-byte integers starting at bytes 0x2077656C636F6D65 and 0x20746F2074686520". There are similar instructions for subtraction, multiplication, etc.

That covers all the mundane operations, as it were. The other important type of instruction controls program flow. For instance, we might imagine that the byte 77 is a JUMP (or "JMP") instruction, so that 77 6569726420616E64 means "when you encounter this instruction, go immediately to the list of instructions beginning at byte 0x6569726420616E64 and 
begin executing that instead". We might also imagine a sort of conditional jump instruction represented by the byte 78, such that 78 7761636B7920776F 726C64206F662043 means "when you encounter this instruction, if the value of the 4-byte integer starting at byte 0x7761636B7920776F is zero, go immediately to the list of instructions beginning at byte 0x726C64206F662043 and begin executing that instead". Some investment of thought should reveal that even this crude and clunky instruction set of our imagination would allow us to make computer programs of arbitrary purpose and complexity.

But, further, consider one more instruction, which I present to you not as a strictly necessary addition to our set but rather something that will make later concepts easier. Imagine an instruction GO-SUBROUTINE (or "GOSUB") represented by byte 20, and an instruction RETURN (or "RET") represented by byte 73, such that 20 70726F6772616D6D 696E672E20546869 represents the command "Begin executing the instructions beginning at address 696E672E20546869. When you hit the instruction 73 XXXXXXXXXXXXXXXX, store the the value at XXXXXXXXXXXXXXXX into 70726F6772616D6D, then come back here and begin executing again, starting with the instruction right after this command". I fear this paragraph may be too poorly written to be useful, and it omits some crucial details, but this is an important functionality of computer languages: we like to call out to other code, have it calculate some value, and then come back to our original code and use the value. Indeed, this is the primary form of abstraction in a computer, as it essentially allows you to write your own machine instructions. You can set up a complex chain of instructions at a certain place in memory, and in a single instruction, as it were, in your primary chain of instructions invoke the complex chain of instructions and then return to your primary chain of instructions.

3. A C Program

3.1. Declarations

So anyway, time to write a C program. Somewhere convenient on your computer, create the file myprogram.c. Now open myprogram.c in a text editor, such as notepad.

Let us begin by marking our territory. 

Type "int i;" into the text document. Having recovered from the overwhelming feeling of power surging from your fingertips, let us now analyze what you just did. That's a declaration. This tells the compiler, "mark an area of memory as containing an integer (of whatever size you feel appropriate so long as it's at least 2 bytes (but all modern compilers will give you a 4 byte integer here)) and let me use the name i to refer to the integer". Except in certain circumstances that we will discuss later, the compiler, in its infinite grace and wisdom, will additionally do you a favor here and set the bytes in this area of memory with 0s for you. If you want to specific a different value that the compiler should initialize this area with, you may write, for example, "int i = 1;" instead. You can even write "int i = 0;" to redundantly specify that you want an the compiler to set the area to 0.

Now type "int j = 2+2;" This is extremely similar to the above example, I just want to draw your attention to the fact that the compiler, infinite grace etc, will evaluate 2+2 while compiling and set the value of j to 4 to start out with.

Note that each of these declarations ends with a semicolon. That's an important feature of C. Semicolons all over the place. Each declaration ends with a semicolon.

3.2. Functions

Now that you have learned how to get the computer to store arbitrary values for you, let us turn to the question of how you might get the computer to perform calculations for you. Type:

int doublei(int integertodouble){
  return integertodouble * 2;
}

This tells the compiler to create a function named "doublei". We have chosen the name "doublei" because we want to double an integer. (Also, "double" is a keyword in C that allows you to declare double-precision floating point numbers.) When we put doublei(someint) somewhere in our code, when the program reaches that point, it will copy someint into a special temporary part of memory called a "stack frame" that we have set up for this invocation of the function. The location of memory wherein the copy resides will be known by the name "integertodouble". The program will then multiply 2 and integertodouble and "return" that value, meaning that wherever we put doublei(someint) it will be as though we typed the resulting value instead. The compiler may optimize away any number of the steps I have just described if it is sure it will be able to get the same result in fewer steps. The "int" in front of the function tells us that the value returned will be an int.

3.4. Statements

We have already used them, but there is another type of instruction in C besides declarations. They are "statements", and they do things. For instance, "2+2;" would instruct the machine to compute the result of adding 2 to 2, doublei(someint) would compute the result of doubling someint. Now, you might notice that it's not very useful to compute 2+2 in a vacuum. The program will compute a value of 4 and immediately move on, unless the value is stored somewhere. This leads us to contemplation of "assignment", setting a declared variable equal to a value. Assignment in a statement is written like assignment in a declaration, but without the type specifier. For example, once we have a declared variable i somewhere, we can write "i = doublei(someint);" to set the value of the memory referred to by i to whatever result we get out of that invocation of doublei.

Statements can only be placed outside of functions if they involve only compile time constants, that is values that are known at compile time and known not to change, like a literal 2. Otherwise they have to be placed inside functions.

3.5. Compiling

So how do we make a program that runs all of these statements and such? Define a special function, main:

int main(void){}

This is pretty the simplest form main can take. When you compile your program by running cc myprogram.c, and then run the executable that your compiler produces, you will be running main. This form of main takes no arguments, as indicated by the keyword void provided in the arguments list. Main returns an int because C programs return a value from 0-255 to indicate whether they have encountered an error, with 0 indicating no error and other numbers indicating specific errors defined by the program in question. C programs return 0 by default, unless otherwise specified (to specify otherwise, use a return statement like "return 1;").

So, for instance, you could write a program

int i = 1;
int j = 2+2;
int doublei(int integertodouble){
  return integertodouble * 2;
}
int main(void) {
  return doublei(j);
}

then compile it at the commandline ("cc myprogram.c"), run it (probably with the command "./a.out"), and check the return value ("echo $?", a command which prints out (echos) the special value $?, the return value of the command which was last run).

Now you can write any program you want, so long as you don't need any input and you only need one byte of output!

3.6. Exercise 1: Celsius to Fahrenheit

If you would like to pretend that what you have learned so far can be of use, try programming a simple Celsius to Fahrenheit converter. Some tips: You won't be able to output a number below 0 or above 255. You can write "int c = 20;" and change the right hand side directly in order to calculate different fahrenheits from different celsiuses. Due to the way integer division is implemented, you should multiply BEFORE you divide. 

4. Things Such As These

4.1. Cunning Conjunctions

4.2. Pernicious Prepositions

4.3. Arrays

4.4. Strings

4.5. Arguments to Main

4.6. C Pre-processor

4.6.1 #include

4.6.2 #define

The most important feature of the C pre-processor is that they all apply at compile time, when your source code is being transformed into an executable that will eventually be run.

The second most important feature of the cpp is that it is unbound by many of the rules of C syntax and semantics.

It's rather hard to come up with simple motivating examples for using the C pre-processor to #define things, because using the C pre-processor in simple cases is usually poor style, as it makes your code less readable for other programmers, who are more familiar with the C programming language than with your bespoke personal C macros.

In real life, the main purpose for #define is to define platform-specific constants that have to be known at compile time. For instance, supposed that on Windows you need to specify something with the number 1 and for linux the number 2. You don't want to do a lot of mucking around and guessing at run time, so it's much simpler to define a macro for each one.

For example, let's say you have made the == = mistake too many times, and want to replace those symbols with the word "is" and "set". Immediately, you run into some inconvenience. There are various types in C that can automatically be compared with ==, but since you user have very limited ability to write function that take various types, best practice sugggests you're going to have to write several functions, along the lines of int is_i(double x, double y){return x==y;} int is_f(double x, double y){return x==y;}. And set() is even worse: the intuitive int set_i(int x, int y){return x=y;} won't work at all, because the x and y inside the function are local variables, and won't change the values in the original context. Your best bet would be a series of functions like int set_i(int *x, int *y){return *x=*y;} and hope that the compiler will optimize out the overhead usually associated with dealing with pointers.

But there's an easier way.

#define set =
#define is ==

This will replace all instances of the identifier "set" in your code with =, and all instances of the identifier "is" in your code with ==, and let the compiler compile the resulting code as regular C code.

In a way, the preprocessor can be used to work around weaknesses in C syntax. However, there is no standard tool to work around weaknesses in the preprocessor's syntax.

#include <stdio.h>
#define set =
#define is ==
#define swap(x,y) typeof(x) tmp = x; x = y; y = tmp;
int x set 100;
int y set 10;
int main(void) {
  swap(x,y);
  printf("x %d, y %d, x is y, x is 100 %d \n", x, y, x is y, x is 100);
  return 0;
}

4.6.3 #if, #ifdef, #ifndef, #else, #elif, #endif

4.6.4 Others

#error allows you to trigger custom compiler errors in case your code detects something is wrong while in the preprocessing stage.

#pragma: from the greek πρᾶγμᾰ (meaning "a thing done" or "a fact") #pragma allows you to specify various additional command to the compiler that the creators of C didn't think of in time to make them real preprocessor commands.

4.7. Input-Output (Standard IO)

Now you should have enough knowledge to complete every exercise in the appendix! Except maybe the ones that involve files.

5. More things

While the purpose of the previous section was to get you up to speed with a minimal set of features you need to write effective C programs, this section elaborates on features you will need to understand to execute best practices and understand other people's code.

Pernicious Prepositions Part 2: Pertinent Prepositions

Pernicious Prepositions Part 3: Perfidious Prepositions

Every other feature in C

Standard Libraries

File IO

And Beyond!

Consult the internet or appropriate documentation to learn about other libraries, features of C I may not have mentioned, etc.

Appendix A: Exercises

These exercises are well documented elsewhere, so I will merely mention them, in approximate order of difficulty, and leave it up to you to look them up, specify them precisely, and attempt them: Hello World, repeat the user's name back to them, BMI calculator, FizzBuzz, 
