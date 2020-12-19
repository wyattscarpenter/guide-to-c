"most experts' idea of a beginner resource is a monograph" --sol fire__exit

"What does my program do
Besides fill memory?" --Jonathan Blow

1. Introduction

1.1. Preamble

Hello and welcome to the weird and wacky world of C programming. This is a guide to C programming for the complete beginner. Special attention has been made to cover C as of C18 (ISO/IEC 9899:2018), the most recent standard C at time of writing (AD 2020), without any focus on explaining the idiosyncrasies of previous versions of C. This attention is despite my own personal unfamiliarity with more recent C features-- I will not explain any. Come to think of it, I've only once used a C feature that was introduced this millennium. But, point is, I'm not going to explain C features that are no longer used.

This is not a complete overview of the C language. In fact, near the end of the guide I will advise you to read an entirely different, much more comprehensive & canonical, but much less beginner-friendly book about C. This guide is meant to teach you a usable subset of C and a useful, and in some ways extremely straightforward, understanding of what it is you are doing when you program.

1.2. Disclaimer

I release this work into the public domain under CC0. If you use it or find it useful in some way, I would appreciate credit and notice, but that request is not legally binding because, as I just said, I release this work into the public domain under CC0.

THE TEXT IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS TEXT INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS TEXT. ALL NAMES, VARIABLES, AND STRINGS IN THIS TEXT ARE PURELY FICTITIOUS. ANY RESEMBLANCE TO ACTUAL PROGRAMS, LIVING OR DEAD, IS ENTIRELY COINCIDENTAL.

1.3. Forgetting Everything

The first step to learning C, if you haven't learned any low-level programming languages before, is to completely forget any knowledge you have about programming. Once you learn C, you will be able to relate it to other programming languages you might know in a productive way, but while you are learning C any similarities you notice will simply trip you up. Some would argue that you should also forget your preconceptions about the English language to productively learn C, but unfortunately I can't advise that as I need you to read the rest of this guide.

1.4. Chekhov's Gnu: Installing a C Compiler

1.4.1 Basic Computer Literacy

Unfortunately, you need a certain amount of computer savvy to even start writing a C program, so in this section I will give you a brief crash course. A computer is a machine that manipulates data. A computer runs routines called "programs" to manipulate data. Modern computers, when on, are constantly running complicated programs called "operating systems" (OSs) that manage the hardware of the computer and allow other programs to run. Data on computers are stored in persistent chunks known as "files", apparently by analogy to a filing cabinet. A file could store something like a picture, a text document, a collection of other information, a program, or anything really. The operating system manages how files are stored on the hardware, providing an abstract interface so programs can "write to" files without having to worry about how they are stored. Files can have names, and are usually named something relevant, followed by . and then a "file extension", a sequence of letters specifying the type of data in the file. Examples: pictureofcat.jpg, mystory.txt, program.exe

Files in modern operating systems can be arranged into groups called "folders" or "directories". These directories can also store directories as well. This means you can often store files in a nested structure of folders, of arbitrary depth. Folders have no file extension, because they are not files. To specify a particular file in this nested structure, one usually writes the names of the folders to get to the file, and then the file name, separated by slashes (eg, "this/is/a/path/myfile.txt")-- this is called a file path. The entire collection of files and directories on your computer, as well as the particular technology that organizes it for you in your OS, is called your file system.

There are as yet two major modalities for interacting with computers. Graphical, in which the user is presented with pictures representing the organization of the data on the system, and textual, in which the user is presented with text representing the organization of the data on the system. Graphical is often easier to use for novice users, so it is present in all modern operating systems. However, it is often easier to get things done in a textual user interface, so textual is also present.

The main way to interact with a computer textually is the command line; in this program, you type a line (or lines) of text representing a command to the computer, and then the computer executes your command and (if you are lucky) tells you the result. The command line on Microsoft Windows is cmd.exe (to run this hold down the key on your keyboard that looks like the windows logo and press r. This will bring up the Run program. Then type cmd and press enter. This will run the cmd program) the command line on GNU/Linux is GNU Bash (how to run this varies by version of GNU/Linux, but it generally involves opening a "terminal" or "terminal emulator" program). cmd is short for "command" and bash is short for "Bourne Again SHell". ("Shell" is another name for a way to interact with a computer. Don't worry about the other part of the name.)

On the command line, you will typically see a "command prompt" prompting you to type a command. This prompt usually consists of the directory you are in (the current working directory) and a character like $ or >. You can then type commands. The simplest form of command begins with the name of a command, and is followed by arguments/options/flags provided to the command. The conventions of command lines are complex enough to warrant their own guide, as they are really programming languages in their own right, but here are some of the most important concepts:

cd - "Change Directory" - Changes the directory you are in to the one specified, like so:

/home/you/example$ cd ..
/home/you/$ cd example
/home/you/example$ cd .
/home/you/example$ cd ~
/home/you/$ cd /home/you/example
/home/you/example$

Note that . is short for the current directory, .. for the directory containing the current directory, ~ for your home directory, and that file paths to change to can be given in absolute form (starting at /, the "root directory") or in relative form (from the current directory). This example interaction is for GNU/Linux, but cd works similarly in cmd. The biggest difference is that ~ isn't a valid shortcut. Also, there is no real root directory; the thing most analogous to a "root directory" is the root directory of the C drive, "C:\". Also, all the slashes are backwards (you can still elect to use forward slashes, though).

ls (or in cmd, "dir") - List Directory - Lists the contents of the current or specified directory, like so:

/home/you/$ ls
example
/home/you/$ ls example
yet-more-example

mkdir - creates a directory with the specified name, eg:

/home/you/$ ls
example
/home/you/$ mkdir anewdir
/home/you/$ ls
example anewdir

cp (or "copy" in cmd) - copies the first location specified to the second location specified, eg:

/home/you/example/yet-more-example$ ls
from.txt
/home/you/example/yet-more-example$ cp from.txt to.txt
/home/you/example/yet-more-example$ ls
from.txt to.txt

mv (or "move" in cmd) - copies and then deletes the old file, thus "moving" it from one place to another:

/home/you/example/yet-more-example$ ls
from.txt
/home/you/example/yet-more-example$ mv from.txt to.txt
/home/you/example/yet-more-example$ ls
to.txt

There are many more commands. Commands can be built in to the command line program, or they can be specified by files on the computer that represent programs for the command line to invoke. When the name of a command is given to the command line to invoke, it first searches its builtins, then its path. The path is a list of folders it looks for commands in, in order. You may have to look up how to edit the path later, if you want to add a compiler to your path. In cmd, the current directory is searched before the path (which creates several hilarious security problems), so you can invoke a program in the current directory by typing, say,

foo

whereas in Bash you have to type

./foo

The best way to find more information about operating systems, programs, and computers is to use the internet, a network of computers interchanging data over a series of tubes. You can use a web browser program to access a search engine such as google to search over internet webpages (which are really just complicated text documents other computers are offering you on the internet) to find the information you need. The information you find that way is likely more up-to-date and accurate than I could make this guide. It's rather roundabout that you need to use a complicated program on your computer to ask another computer to ask other computers for information about your own computer, but since you probably got this guide over the internet it shouldn't present too much of a difficulty.

1.4.2 What is a compiler?

A C program is a text document on your computer. You will need to install a program on your computer to transform C programs into files that the computer can execute ("executables"). The most popular such program is GCC, which is also Free, so I suggest you use it. Google how to install gcc on your operating system and do that. This may take you some time to figure out; I suggest you treat this as a task of comparable difficulty to writing a complete program. On Ubuntu this may be as simple as running `sudo apt install gcc` on the command line, and on Windows this may be as "simple" as installing Bash for Windows and running `sudo apt install gcc` on the Bash command line. It doesn't really matter what compiler you install or how you install it, so long as it works. You are free to make substitutions so long as you can get something working. Some other c compilers are clang, tcc, and cl. You could also use https://repl.it/languages/c online.

Once you have a compiler, you should be able to run "cc" (stands for "c compiler") on the command line and observe the message from your compiler complaining that you haven't provided a c program. Note that the message from your compiler complaining it can't find a c program looks much different than the message your command line will give you if it can't find your compiler, even though both are complaints about not finding something. If that doesn't work, maybe your compiler didn't take the conventional step of setting the name "cc" to refer to it, so you should trying running your compiler directly. Wherever you installed your compiler from should have instructions about this, so I won't go into it-- though, likely you should cd into the directory containing the compiler executable and/or invoke it by name. Again, remember that this is the hardest part, so don't get discouraged.

2. The Von Neumann Architecture

2.1. Memory: All Alone in the Moonlight

A computer is, essentially, an enormous number of tiny electrical switches set to either on or off. The entire purpose of a computer program is to manipulate the switches to form a particular pattern in a judicious manner. On is known as 1 and off is known as 0. In this way, you can conceive of the computer as a big bank of 1s and 0s scattered around in a big line or grid, like so:

01001000011001010110110001101100011011110010000001100001011011100110010000100000011101110110010101101100011000110110111101101101011001010010000001110100011011110010000001110100011010000110010100100000011101110110010101101001011100100110010000100000011000010110111001100100001000000111011101100001011000110110101101111001001000000111011101101111011100100110110001100100001000000110111101100110001000000100001100100000011100000111001001101111011001110111001001100001011011010110110101101001011011100110011100101110001000000101010001101000011010010111001100100000011010010111001100100000011000010010000001100111011101010110100101100100011001010010000001110100011011110010000001000011001000000111000001110010011011110110011101110010011000010110110101101101011010010110111001100111001000000110011001101111011100100010000001110100011010000110010100100000011000110110111101101101011100000110110001100101011101000110010100100000011000100110010101100111011010010110111001101110011001010111001000101110...

Each 1 or 0 here is a digit. Since each digit can have one of two values, 0 or 1, they are referred to as "binary digits" or "bits" for short. "Binary" comes from the latin word "binarius" meaning "consisting of two". You are probably familiar with a different type of digit, the conventional sort of digit in our society, which can have one of ten values, from 0 to 9. Those are referred to as "decimal digits" from the latin "decimalis" meaning "consisting of ten". There are other sorts of digits, and they can all be used to represent the same underlying values.

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

At this point, with a bit of reflection, it should become apparent to you that we can add bytes. As simply as we might 72 and 101, we could add the bytes holding those values and receive the value 173. You can do all sorts of arithmetic operations like this. If you want to deal with numbers larger than 255, you can treat several consecutive bytes as representing parts of the same number. For instance, two bytes containing 0x48 and 0x65 might be treated as two parts of one number containing 0x4865.

Naturally, the question arises: if I have two bytes that represent the value 0x4865 by being 0x48 and 0x65, should the 0x48 come before the 0x65 or after? The answer is obviously that 0x48 should come first and then 0x65, but computer scientists are bad so they usually make the 0x65 come first and then the 0x48. That is, on most modern machines the string of two bytes we might write out as 6548 represents the integer 0x4865. I find this personally disappointing, but it is ultimately an arbitrary decision of ordering. Machines on which the string of two bytes we might write out as 6548 represents the integer 0x6548 are known as "big-endian" because they start eating the big end of the integer first. Most modern machines are little-endian. Knowing about this is probably useless to you right now, but it will be useful to you later, probably once you've forgotten about it.

The most common type of integer in C programs is 4 bytes long, which is good enough for many purposes.

2.3. Numbers 2: The Numbering

The more mathematically inclined among you may have noticed that, technically, an integer can be negative. This is correct. Most integers stored on computers are "signed" meaning they can be either positive or negative (ie they may be thought of as having a + or - sign in front of them if they are negative) and are stored in something called "two's complement". The details of this are not important right now, as evidenced by the fact that I can't remember them off the top of my head, but suffice to say that some of the possible values of the integer are relegated to representing negative numbers instead.

(I've just remembered that to negate a number in two's complement, you invert all the bits and then add one. The reason it's implemented this way, by the way, is so that the same addition circuit can be used for both signed and unsigned numbers.)

If you don't want negative values, you can choose to use "unsigned integers", which use their full range of values to represent numbers greater than or equal to 0.

It's important to note that the bytes themselves do not store information about what type of thing they represent. If we encounter the string of four bytes FFFFFFFF and chose to interpret them as a signed integer, we will interpret them as representing -1. If we chose to interpret them as an unsigned integer, we will interpret them as representing 0xFFFFFFFF.

Similar to the various types of integers, there are various ways to store a non-integer number (ie a rational number) on a computer. One of the most popular is called "floating point" and again we won't get into the implementation details.

2.4. Addresses

An important note, though you may find its purpose obscure at this point, is that we number all the bytes on your computer. If you wish to refer to a specific byte in memory, you can refer to it by number, its "address". Addresses are usually stored as an unsigned integer eight bytes long.

Now, this is well and good, but you might notice that typically you have more than one program running on your computer. What if they both try to reference the same byte by number? They would probably interfere with each other. Since we typically don't want that, each program on your computer gets to live in its own "virtual memory" where the addresses it references are mapped to unique addresses in the underlying machine. If you don't understand this, don't worry, the whole point is that you and your program can pretend, for addressing purposes, that you're only running one program on the machine.

2.5. Instructions

You're almost ready to write a C program and have a vague idea of what you're doing. You only need to understand one more concept: machine instructions.

Keep in mind that everything that follows is a vast oversimplification and a mere illustration of a concept. It does not demonstrate any real instruction set.

The general way a computer operates is that a list of bytes is interpreted as a series of instructions to the machine. The computer goes down this list in order, executing each instruction in turn.

For instance, let us suppose, completely arbitrarily, that the byte 48 represents an instruction called ADD, and so the byte string 48 56C6C6F20616E64 2077656C636F6D65 20746F2074686520 represents the command to the computer "store into the 4-byte integer at starting at byte 0x56C6C6F20616E64 the result of adding the 4-byte integers starting at bytes 0x2077656C636F6D65 and 0x20746F2074686520". There are similar instructions for subtraction, multiplication, etc.

That covers all the mundane operations, as it were. The other important type of instruction controls program flow. For instance, we might imagine that the byte 77 is a JUMP (or "JMP") instruction, so that 77 6569726420616E64 means "when you encounter this instruction, go immediately to the list of instructions beginning at byte 0x6569726420616E64 and 
begin executing that instead". We might also imagine a sort of conditional jump instruction represented by the byte 78, such that 78 7761636B7920776F 726C64206F662043 means "when you encounter this instruction, if the value of the 4-byte integer starting at byte 0x7761636B7920776F is zero, go immediately to the list of instructions beginning at byte 0x726C64206F662043 and begin executing that instead". Some investment of thought should reveal that even this crude and clunky instruction set of our imagination would allow us to make computer programs of arbitrary purpose and complexity.

But, further, consider one more instruction, which I present to you not as a strictly necessary addition to our set but rather something that will make later concepts easier. Imagine an instruction GO-SUBROUTINE (or "GOSUB") represented by byte 20, and an instruction RETURN (or "RET") represented by byte 73, such that 20 70726F6772616D6D 696E672E20546869 represents the command "Begin executing the instructions beginning at address 696E672E20546869. When you hit the instruction 73 XXXXXXXXXXXXXXXX, store the the value at XXXXXXXXXXXXXXXX into 70726F6772616D6D, then come back here and begin executing again, starting with the instruction right after this command". I fear this paragraph may be too poorly written to be useful, and it omits some crucial details, but this is an important functionality of computer languages: we like to call out to other code, have it calculate some value, and then come back to our original code and use the value. Indeed, this is the primary form of abstraction in a computer, as it essentially allows you to write your own machine instructions. You can set up a complex chain of instructions at a certain place in memory, and in a single instruction, as it were, in your primary chain of instructions invoke the complex chain of instructions and then return to your primary chain of instructions.

3. A C Program

3.1. Declarations

So anyway, time to write a C program. Somewhere convenient on your computer, create the file myprogram.c. Now open myprogram.c in a text editor, such as notepad.

Let us begin by marking our territory. 

Type "int i;" into the text document. Having recovered from the overwhelming feeling of power surging from your fingertips, let us now analyze what you just did. That's a declaration. This tells the compiler, "mark an area of memory as containing an integer (of whatever size you feel appropriate so long as it's at least 2 bytes (but all modern compilers will give you a 4 byte integer here)) and let me use the name i to refer to the integer". Except in certain circumstances that we will discuss later, the compiler, in its infinite grace and wisdom, will additionally do you a favor here and set the bytes in this area of memory with 0s for you. If you want to specify a different value that the compiler should initialize this area with, you may write, for example, "int i = 1;" instead. You can even write "int i = 0;" to redundantly specify that you want an the compiler to set the area to 0.

Now type "int j = 2+2;" This is extremely similar to the above example, I just want to draw your attention to the fact that the compiler, infinite grace etc, will evaluate 2+2 while compiling and set the value of j to 4 to start out with.

Note that each of these declarations ends with a semicolon. That's an important feature of C. Semicolons all over the place. Each declaration ends with a semicolon.

3.2. Functions

Now that you have learned how to get the computer to store arbitrary values for you, let us turn to the question of how you might get the computer to perform calculations for you.

It is important to note that at the "top level" of nesting in a C file (outside of functions), only declarations can happen; the program can't perform any actions. You are only allowed to specify things, not change things over time.. To specify some behavior for your program, it has to happen in a function. C functions, also called procedures, are inspired by mathematical functions, and they can take in values as input (referred to as "input", "arguments", or "parameters") and evaluate to a value ("output", "return value"). Type out this trivial function:

int doublei(int integertodouble){
  int i;
  i = integertodouble * 2
  return i;
}

This tells the compiler to create a function named "doublei" that takes one argument, an int integertodouble. We have chosen the name "doublei" because we want to double an integer. When we put doublei(someint) somewhere in our code, when the program reaches that point, it will execute the body of the function, in this case just "return integertodouble * 2;", and evaluate to the specified return value.

The program will copy someint into a special temporary part of memory called a "stack frame" that we have set up for this invocation of the function. The location of memory wherein the copy resides will be known by the name "integertodouble". (Furthermore, we have specified that integertodouble is an int, so if the previous steps try to copy an incompatible type of data into this memory, the compiler will warn us of our mistake.) The program will then multiply 2 and integertodouble and store that value into the local variable i. It will then "return" the value stored in i, meaning that wherever we put doublei(someint) it will be as though we had put in the resulting value instead. The compiler may optimize away any number of the steps I have just described if it is sure it will be able to get the same result in fewer steps. The "int" in front of the function tells us that the value returned will be an int.

Since we might want to call a function from a function (imagine: int quadruplei(int integertoquadruple){return doublei(i) * 2;}), we need to be able to store an arbitrary amount of information about function calls, as functions can call functions, and so forth, and we need to know what function we're in, what its arguments were, where to return to, and any data local to the function. To do this, we have a "stack" of some large size somewhere in memory. This stack is a collection of this information that we add to and take from. We only expect to put things on, or take things off, the "top" (most recently added part) of the stack, which is why we envision it as a stack of something. Any time we make a function call, the following data is put on the stack and called a "stack frame": the address that function will return to when it is finished, the arguments to the function (in our example, integertodouble), and any data local to the function (in our example, this means enough memory is set aside in the stack frame for an int, and this location in memory is called i).

Once the function returns (jumps to the stored return address), the data is removed from the stack-- conceptually. As it turns out, we don't want to spend time clearing the stack each time, since we're just going to overwrite it next time, so the data is just left in memory and ignored, as we regard the stack properly as the stack without that last stack frame, making the previous stack frame the top frame now.

Similar to how we don't want to spend time clearing the frame we're done with, we don't want to spend time clearing the data from the frame we're about to use. This leads to some interesting errors. When we call our function, instead of the compiler graciously setting int i to 0 in our example here, it instead marks the memory as belonging to i, but leaves the memory in whatever state we found it, containing whatever data was already in those bits. If we had not immediately assigned to it, i could've contained any value. This is known as an uninitialized variable, and, much like the spanish inquisition, no one ever expects it, causing many headaches. So, remember: if a variable lives on the stack, it will not automatically be initialized to 0.

This concept of a "call stack" is kind of an abstract, informal one, and various machines implement it various ways. In fact, the C standard makes no mention of a stack, and only specifies the lifetime of pieces of data.

Illustration of the concept of a call stack:

        0x23948577 <- local int i, filled with whatever garbage was already there
        0x00000002 <- value of argument passed to doublei, the integer 2, copied to the stack
frame2: 0x12345678 <- return address for doublei to return to
        0x00000002 <- value of argument, the integer 2, copied to the stack
frame1: 0x12345678 <- return address for quadruplei to return to

We see here the call stack as the function quadruplei has just called the function double i, but before doublei has assigned anything to i.

Note that the call stack does NOT contain the instructions that are to be performed on the these pieces of data. It is merely a data structure the actual program uses for bookkeeping.

3.4. Statements

There is another type of instruction in C besides declarations. They are "statements", and they do things. For instance, "2+2;" would instruct the machine to compute the result of adding 2 to 2, "doublei(someint);" would compute the result of doubling someint, etc. Now, you might notice that it's not very useful to compute 2+2 in a vacuum. The program will compute a value of 4 and immediately move on, unless the value is stored somewhere. This leads us to contemplation of "assignment", setting a declared variable equal to a value. Assignment in a statement is written like assignment in a declaration, but without the type specifier. For example, once we have a declared variable i somewhere, we can write "i = doublei(someint);" to set the value of the memory referred to by i to whatever result we get out of that invocation of doublei. Fun fact: assignment returns the value assigned, so "x = 2" both sets x to 2 and returns 2. This can be very useful in loops, which we will get to later.

Statements can only be placed inside functions. They execute from top to bottom in the function.

You might notice that statements and declarations often look very similar. This is because they can both contain "expressions" like 2+2. Expressions can only be placed outside of functions (in global declarations) if they involve only compile time constants, that is, values that are known at compile time and known not to change, like a literal 2. Otherwise they can only be used inside of functions (in statements and local declarations)).

3.5. Compiling

So how do we make a program that runs all of these statements and such? Define a special function, main:

int main(void){}

This is pretty much the simplest form main can take. When you compile your program by running cc myprogram.c, and then run the executable that your compiler produces, you will be running main. This form of main takes no arguments, as indicated by the keyword void provided in the arguments list. Main returns an int because C programs return a value from 0-255 to indicate whether they have encountered an error, with 0 indicating no error and other numbers indicating specific errors defined by the program in question. C programs return 0 by default, unless otherwise specified (to specify otherwise, use a return statement like "return 1;").

So, for instance, you could write a program

int i = 1;
int j = 2+2;
int doublei(int integertodouble){
  return integertodouble * 2;
}
int main(void) {
  return doublei(j);
}

then compile it at the command line ("cc myprogram.c"), run it (probably with the command "./a.out"), and check the return value ("echo $?", a command which prints out (echos) the special value $?, the return value of the command which was last run).

Now you can write any program you want, so long as you don't need any input and you only need one byte of output!

(Shortly, I will tell you how to do more sophisticated input and output.)

3.6. Exercise 1: Celsius to Fahrenheit

If you would like to pretend that what you have learned so far can be of use, try programming a simple Celsius to Fahrenheit converter. Some tips: You won't be able to output a number below 0 or above 255. You can write "int c = 20;" and change the right hand side directly in order to calculate different fahrenheits from different celsiuses. To convert from Celsius to Fahrenheit, you have to multiply by 9/5 and add 32. Due to the way integer division is implemented, you should multiply BEFORE you divide. (If / is used between two ints in C, the result is also truncated to an int, so 9/5 would become 1. Even more alarmingly, 5/9 would become 0.)

4. Things Such As These

4.1. Cunning Conjunctions

At this point, since I need to explain the manipulation of input and output, I should explain logical, or "boolean" (named after famous logician George Boole) functions and operators, but to be honest I don't really feel like going into it in depth. Here's a quick version.

Consider this: "true" and "false", those hallowed truth values, are two values. Ergo, they can be contained in one bit. Therefore, you can treat one byte as eight truth values. As you will remember from everyday life or your logic classes (here I pretend you have an undergraduate degree in computer science or philosophy but still don't know how to program in C) true AND false = false and true OR false = true.

C implements the "bitwise" boolean operators thusly so that each of the bits of two integers are respectively applied to each other:

x & y will evaluate to a third value where each bit of x is ANDed against each bit of y,

x | y will evaluate to a third value where each bit of x is ORed against each bit of y,

x ^ y will evaluate to a third value where each bit of x is XORed against each bit of y.

These operators are very useful for some operations, like when you must set "flags" where each bit of a particular position is set appropriately. But these operations are not much use for the common man such as you or I. So, we encounter "logical values" on bytes. Imagine: a byte (or other integer type) with all bits 0 is false (0), any other byte is true (1). With this convention established, we can set entire bytes to represent true or false without mucking about with individual bits. There is a second set of operators for these:

x && y evaluates to true if both are nonzero,

x || y evaluates to true if either is nonzero,

x ^^ y evaluates to true if exactly one is nonzero.

These functions return 1 or 0, the canonical true and false values. You can specify your own bools by typing eg "bool b = true;", but this requires advanced C features, so right now it's best to think of truth values as a special interpretation of bytes and ints.

4.2. Pernicious Prepositions

Oh boy I guess it's time to teach you flow control statements, huh? These are statements that control the flow of program execution, ie which command is currently being executed. There are quite a few of them, but you only really need one, the rest are variations for convenience. I'm going to teach you two.

4.2.1

If executes a block of statements if the "predicate" variable supplied to it evaluates to true when execution reaches that point in the code.

if (something){
  dosomemorethings();
}

In the above example, something is checked, and if it is true, dosomemorethings is executed. If it is false, we don't execute dosomemorethings. Then we proceed.

I will also teach you the else and else if variant of if, by further example:

if (something){
  dosomemorethings();
} else {
  somestuff();
}

In the above example, something is checked, and if it is true, dosomemorethings is executed. If it is false, somestuff is executed. Then we proceed.

if (something){
  dosomemorethings();
} else if (otherthing){
  somestuff();
}

In the above example, something is checked, and if it is true, dosomemorethings is executed. If it is false, otherthing is checked, and if it is true, somestuff is executed. Then we proceed. As you may notice, this is equivalent to nesting an if into an else like

if (something){
  dosomemorethings();
} else {
  if (otherthing){
    somestuff();
  }
}

You can also have code like

if (something){
  dosomemorethings();
} else if (otherthing){
  somestuff();
} else {
  yetmorethings();
}

Or code with an arbitrary number of else ifs in there.

4.2.2

While repeats a block of code while the predicate supplied to it is true.

while (something) {
  dosomemorethings();
}

In the above example, something is checked, and if it is true, dosomemorethings is executed. Then flow returns to the top of the block, something is checked, and if it is true, dosomemorethings is executed... so on until we check and find something is false, at which point we ignore the code block and proceed past the whole while loop.

It's useful to note that within a while loop, the keyword "continue" will go back up to the top of the while statement (to begin the check-execute cycle again) and the keyword "break" will exit the while loop (ignoring all checks and proceeding with the subsequent code).

There are other keywords that control the flow of program execution, (ie, create loops or check conditions) but frankly they are merely elaborations on these two concepts so they aren't necessary and I will treat them later.

4.3. Comparators

Have you ever looked at two values and wondered how they compare? Well wonder no more! C provides several built-in comparators to compare simple things like numbers. == is in infix operator that tells you if two things are equal. That is, x==y returns true if x is equal to y. Similarly, x>y returns true if x is greater than y, x<y returns true if x is less than y, x>=y returns true if x is greater than or equal to y, x<=y returns true if x is less than or equal to y, and x!=y returns true if x is inequal to y. They return false otherwise.

These comparators can be very useful in the predicates of if and while statements. For example:

while (x!=stopcodon){
  x=processnextunit();
}

4.4. Arrays

Oh thank god we got to arrays. These are easy to explain. In C, much as in life, oftentimes one needs a large number of related values in a list. What better way to store these than right next to each other? An array in C is a list of identical values that are all contiguous (laid out so they touch each other) in memory. You can declare an array like

int x[5];

which gives you 5 contiguous ints, starting at the address held by the variable x.

int x[];

gives you an array of an undetermined length, which is most useful when you want to specify the values in the array directly, which you can do:

int x[] = {2,4,6,8,7000};

You can access any element of this array by typing x[somenumber], where somenumber is the offset from x of the element. The first element has an offset of 0, the second element is 1, etc. Note that this means the nth, last element (say, the fifth element) will be at offset n-1 (say, offset 4).

This is because-- oh man I didn't explain pointers yet did I?

4.5. Pointers

You can store the address of values in memory as values in memory. Remember that we number each byte in memory, so if we want to store the location of an important piece of information, and then look the information up later by its location, we can. This is called storing a pointer and dereferencing, respectively, but those are dumb names for these things so we're just going to talk about addresses and lookups here.

Say you have a location in memory known as x, and that location contains an address of a point in memory (which, remember, is essentially an int with certain machine-specific constraints). You can manipulate the address by manipulating x. Say you type "x = 2;". Now x will contain the address 2. If you type you type "x = x + 4;", x will contain the address 4 above the address x used to hold (note that "4 above" is in whatever units of memory the address is supposed to refer to. If ints, this will be the same as adding 16 bytes to the address!). That's all well and good. But now for the important part: you can look up the value at the address stored in x and use it in your code. To use a value stored at the address stored in x, you simply type "*x" into your code. Note that this is like a funky unary multiplication operation. This may seem confusing, but in the defense of the creators of C, they didn't do a very good job designing this language.

So, if x contains 4, and the integer starting at byte 4 is 69, "*x" will evaluate to 69 wherever it is used. You can also type "*x = 42" to set the value of the memory at the address stored in x to 42, or what have you.

People typically find pointers uh I mean addresses very hard to understand. I think this is because it's typically explained terribly (eg, calling them "pointers" instead of "addresses", referring to the action where you look up the value at a pointer as "dereferencing" instead of "referencing" because "it's no longer a reference to something, guys, it's the real thing now :)" even though the action of referring to another thing is called a reference in every other situation in the english language) but if you still find it hard to understand, feel free to read this section again.

Oh yeah, the type system keeps track of what variables are addresses and what types of things they refer to, which is nice; it would be burdensome to keep track yourself, and storing an integer into a variable that's supposed to contain an address is usually useless, because on modern computers you usually don't know exactly what numerical address anything will be at at program run time. So, there's a special form of declaration for variables that hold addresses. Since addresses are used by typing "*x", addresses are declared by typing

int *x;

which means that once you look up the address specified by x, you will find an int.

Array access, "x[49]" is defined as "*(x+49)".

By convention, there is a special address, 0, at which nothing can be stored. If you wish to indicate that an address does not indicate any valid value, you may set the address to 0. The address is then known as a "null pointer" ("null" means 0).

Humorously, the "0" I just described here is not always numerically the value 0 (machines are wacky, yo), but the C standard specifies that "x=0" will always set the address of x to the aforementioned special value, so you don't have to worry about this distinction. Everywhere else in this document and in C, 0 will be 0 as you expect.

4.6. Strings

The byte data type is called "char" because it can be used to store characters, as discussed in section 2. In fact, this data type is not guaranteed to store a byte, simply to store enough data to hold a character. On all modern computing systems a char is exactly an 8-bit byte though.

You can type a literal char into your program using single quotes, ie:

char lastalphabeticalletter = 'z';

Naturally, text is a string of characters. This should suggest to your brilliant mind that an array is the most promising way to store strings. Correct!

However, in C we often wish to deal with strings of characters of an indeterminate number (ie the user might type "dog" or "sesquipedalian", and we want to handle both cases correctly), and the length of arrays is not stored anywhere in C at program run time. Therefore, a convention is established that strings are "null-terminated" ("null" means 0); when you want to indicate the end of a string, you make the char after the end of the string store the value 0 (in ASCII this is known as a NUL). All functions that deal with strings respect this convention. Pro tip: remember to terminate your strings.

You can type a literal string into your program using double quotes, ie:

char *magic = "xyzzy";

Note that this makes a certain portion of memory contain 6 chars: 'x', 'y', 'z', 'z', 'y', and a char set to 0, which can be written in C as '\0', and then sets the memory known as magic to hold the address of the first 'x'.

4.7. Arguments to Main

You might notice that programs you can run from the command line can take arguments. That is, not only can you run "ls", you can run "ls -l" to get ls to operate in a certain way.

By well-entrenched convention of operating systems, your C program can look at all the text it receives from the command line! Instead of defining a main with no arguments, as we did before, define a main that takes two arguments: the count of command line arguments supplied, and the address of a list of addresses of strings that are the arguments:

int main(int argc, char **argv){}

The count is traditionally named "argc" for "argument count", and the address of address is traditionally named "argv" for "argument vector", from the mathematical concept of a vector (ie, a list).

You might be a little confused at the concept of an address of a list of addresses of strings, but consider: each command line argument (like "-l") is a string. In C, a string is the address of the first char. The operating system will break apart the command (like "ls -l") for you, and give you a list of strings. This list also has to be variable length, so its length is indicated by argc. argv[0] contains the name of your program (like "ls" in the "ls -l" example above). argv[argc], the element of argv after the last argument, is always a null pointer. Here is a simple program that iterates over each character of each argument:

int main(int argc, char **argv){
  int i = 0;
  while(i < argc){
    char c = argv[i][0];
    while(c){
      c = argv[i][j];
    }
    i = i + 1;
}

Here is a second variant that uses the null pointer instead of argc to determine the end of argv. Recall that a null pointer will evaluate as 0 in a while condition:

int main(int argc, char **argv){
  int i = 0;
  while(argv[i]){
    char c = argv[i][0];
    while(c){
      c = argv[i][j];
    }
    i = i + 1;
}

Using a null pointer to signal the end of lists of lists is a common pattern and very useful. It's also unambiguous: a pointer to an empty list is different than a null pointer. If this is not immediately apparent, try to demonstrate it to yourself.

4.7.1 Exercise 1.1: Celsius to Fahrenheit

Try making your Celsius to Fahrenheit accept an argument from the command line to specify a number to convert. Note that your argument will be a string, so you will have to write a function to convert a string of textual digits to an integer. Try not to use any libraries for this; it will be good practice of your skills so far. Note also that you won't be able to output strings yet; this will be addressed shortly.

4.8. C preprocessor

The C programming language has an initial stage of "preprocessing" that replaces certain bits of text with other bits of text, because sometimes that is useful. The C preprocessor (or "cpp", not to be confused with the "cpp" that is C Plus Plus, which is a dumb tacked-on addition to C) is a dumb tacked-on addition to C. Text in programming languages is "parsed", ie its structure is understood by the computer, early in the compilation process. During this process, the text is broken into "tokens", the semantic units the compiler operates on. The preprocessor is in charge of tokenization, and does its other funky business before and after tokenization.

4.8.1 Comments

If you want to include a note in your program for other programmers to read (but not for the compiler to see), you can type "//" the double slash and everything after it, up to the end of the line, will be ignored by the compiler. Similarly, you can type /* to begin a multi-line comment and */ to end one, for example

/* all this text
will be ignored */

Comments don't "nest", so 

/*/**/*/

looks to the compiler like a comment /*/**/ followed by an illegal token */, which produces a syntax error.

Note that commented text will still separate other tokens in the program: "x/*foo*/x" is not the same as "xx", it's the same as "x x".

4.8.2 #include

Sometimes you want to include the text of other files in your file. Say you have written a function, like our doubling function doublei from before. Say, in fact, that you have written a whole family of doubling functions, like doublef, doublec, etc, and you have stored these functions in a file named doubling.c. How might you use those functions in a different program? Your brilliant mind probably immediately conjures the answer: "I could copy the entire text of doubling.c into my new program. In this way, the compiler will make the new program contain the functions of the doubling.c" Correct! So long as doubling.c does not contain a main function (which would mean copying the text of doubling.c into the new program would produce a program with two main functions, an error) this is how you would do it. Luckily, C contains a facility to do this for you conveniently, a preprocessor directive called "#include". 

#include "doubling.c"

copies the entire text of doubling.c into your program in place of that line. Note that we use quotes here to indicate that the file we wish to include may be in the same directory as our program, and we should search there before searching our computer's other directories. If you wish to search only the system directories of your computer for the file, use angle brackets (ie <doubling.c>).

I will not be explaining header files in-depth at this time, except to note that they are valid C programs that end in .h and simply contain function headers like "int doublei(int integertodouble);", which let your program know what functions are in another file that will be hooked up later in the compilation process. Most includes in most C programs use header files, but they are just a somewhat more roundabout way of doing what we have just described with our inclusion of C files.

Note that when a C file is included, any preprocessor directives it contains will be evaluated, including its includes. Therefore, circular includes can pose a huge problem. Don't do that.

4.8.2 #define

The most important feature of the C preprocessor is that these transformations all apply at compile time, when your source code is being transformed into an executable that will eventually be run.

The second most important feature of the cpp is that it is unbound by many of the rules of C syntax and semantics.

For example, let's say you have made the == = mistake too many times, and want to replace those symbols with the word "is" and "set". Immediately, you run into some inconvenience. There are various types in C that can automatically be compared with ==, but since you the user have very limited ability to write functions that take various types, best practice suggests you're going to have to write several functions, along the lines of "int is_i(double x, double y){return x==y;} int is_f(double x, double y){return x==y;}". And set() is even worse: the intuitive int set_i(int x, int y){return x=y;} won't work at all, because the x and y inside the function are local variables, and won't change the values in the original context. Your best bet would be a series of functions like int set_i(int *x, int *y){return *x=*y;} and hope that the compiler will optimize out the overhead usually associated with dealing with pointers.

But there's an easier way.

#define set =
#define is ==

This will replace all instances of the identifier "set" in your code with =, and all instances of the identifier "is" in your code with ==, and let the compiler compile the resulting code as regular C code.

In a way, the preprocessor can be used to work around weaknesses in C syntax. However, there is no standard tool to work around weaknesses in the preprocessor's syntax.

The following code will be reworked to be an illustrative example when I get around to it.

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

A defined thing is known as a macro, as the token replaced by the directive can be transformed into arbitrarily long (ie "macro", from the greek word for "long", μακρός) amounts of code.

4.7.3 #if, #ifdef, #ifndef, #undef, #else, #elif, #endif

It's rather hard to come up with simple motivating examples for using the C preprocessor to #define things, because using the C preprocessor in simple cases is usually poor style, as it makes your code less readable for other programmers, who are more familiar with the C programming language than with your bespoke personal C macros.

In real life, the main purpose for #define is to define platform-specific constants that have to be known at compile time. For instance, suppose that on Windows you need to specify a variable to have the value 1 and for linux the value 2. You don't want to do a lot of mucking around and guessing at run time, so it's much simpler to define a macro. But the macro must be defined a different way for each operating system. How might we go about this?

Well, when compiling for Windows, compilers will typically #define _WIN32 as 1 when compiling for windows, and __linux__ as 1 when compiling for linux. Therefore, we can write the code

#if _WIN32
#define something 1
#endif

#if __linux__
#define something 2
#endif

The "#if [...] #endif" functions like "if{[...]}", but it is evaluated by the compiler while compiling. Something will be defined correctly.

Say we aren't sure if _WIN32 is going to be defined as a literal 1 or something else, and we just want to know if it's defined as anything. In this case we could write

#ifdef _WIN32
#define whatever 1000
#endif

This would excute the body if _WIN32 is defined at all, even as 0.

Similarly you can use #ifndef to check if something isn't defined. You can use #undef to remove a definition.

This is mostly useful in the case I have just described, but also is quite useful to create headers that guard against multiple inclusion, as we have discussed in the previous section. Most header files will be contained in some code like this:

#ifndef MYPROGRAM_VERY_SPECIAL_SYMBOL
#define MYPROGRAM_VERY_SPECIAL_SYMBOL
[rest of header file]
#endif

In this way, including the header file a second time will have no effect.

Rest assured that #else and #elif work just like else and else if.

4.8.4 Others

Here are some other preprocessor directives.

#error allows you to trigger custom compiler errors in case your code detects something is wrong while in the preprocessing stage.

#pragma: from the greek πρᾶγμᾰ (meaning "a thing done" or "a fact") #pragma allows you to specify various additional commands to the compiler that the creators of C didn't think of in time to make them real preprocessor commands. I have never used a single one of these in my code, but I have SEEN them used sometimes. For instance, #pragma once in a header file is like wrapping your header in the ifndef guard discussed earlier to include it only once.

4.8. Input-Output (Standard IO)

Oh thank god I can finally explain how C programs actually are supposed to do input and output (IO). This is pretty much the most important part of a workaday program, most of which are concerned with transforming input data to output data in some way. For instance, a program that takes a number in Celsius and outputs it as Fahrenheit. It was very difficult to pretend you could write a useful program without this, but unfortunately I needed to explain strings and the cpp first, because io uses strings and requires a library. The system you're programming on should define a series of standard system-specific input-output functions in stdio.h. Therefore, you must type

#include <stdio.h>

at the top of your file to do io. It's possible to define your own io functions out of lower-level primitives, but the way you arrange those primitives would be system-dependent anyway, so you might as well just use the system library.

Producing output is often called "printing" because back in the day output would go directly to a printer which would print it out. Taking in input is often called "reading" because it's like your program is reading the input :)

The three most important io functions are puts, getchar, and printf. Puts (put string) prints out a string you supply to it, and then a newline, to a magical place called standard output. getchar (get character) reads a character from standard input. Note the asymmetry between these, due to the details of managing space. Input comes from a "stream" of data, and output goes to a "stream" of data. The point of defining these things as "streams" is so we can make someone else worry about how they actually work and just shove all our data in there as we please. So each stream may take or give an unbounded amount of data. When printing, this is a great convenience: we can print our variable length strings, no problem. However, we can only read in one character at a time, for if we tried to read in all characters at once (say, into an array of 100 characters) we might quickly run out of space. Therefore a good practice in C programs is usually to read a fixed amount of input (such as one character. Though, it can be more efficient to read more than one character.) at a time, and to print results as soon as you can, so to avoid having to manage excessive amounts of memory.

If, god forbid, the output stream should run out of space, puts will return the non-zero constant EOF. People almost never check this.

If, as is inevitable, the input stream should run out of data, getchar will return the non-zero constant EOF. People almost always check this, and write neat loops like

int c;
while(c = getchar() != EOF){
  //do some stuff with c
}

Note that c is an int even though it deals with textual characters. Funny, isn't it?

Now the printf is more subtil than any function of the field which the LORD God had made. But it is important if you want to print out anything besides strings. It takes a string as a "format" and a variable number of other arguments depending on what the format specifies. Printf prints the format string, but whenever it encounters a percent sign ("%") in the string, it prints the next argument to printf. How it formats the data in the argument is specified by a handful of symbols after the %. I can't be bothered to remember most of these specifiers other than %d, which prints an integer as a decimal number.

printf("Here is your number: %d\n", 10); //prints "Here is your number: 10", then a newline.

Printf does all its formatting at runtime, and covers a lot of cases, so it is a fairly hefty function and occasionally you will put the wrong specifiers in and become confused and/or crash your program and/or introduce serious security flaws. For these reasons, I caution against using a printf when a puts will do.

Unlike puts, you will have to specify your own newlines using "\n".

(I recenly wrote a generic compile-time print function. I'm very proud of it, but it's exceptionally arcane so unlike foreach (below) I won't go into details. I will link it here, though https://github.com/wyattscarpenter/print.h)

Even though it's not part of C, I should mention that on the bash command line, you can specify files to go into input or output like so:

programyourerunning <inputfile.txt >outputfile.txt

Note that the angle bracket bits are not arguments to your program. You will not see them in argv! They are processed by bash and never seen by your program.

You can append to a file instead of overwrite it with >>

programyourerunning <inputfile.txt >>outputfile.txt

You can remember this because if you use ">>", the file will contain two outputs (">"), the previous and the present.

You can also "pipe" the output of one program into another, which fittingly uses the pipe character as punctuation:

programyourerunning | anotherprogram

There's a hilarious limitation of Bash, such that

myprogram <file.txt >file.txt

selects the same file as input and output, probably wanting to process a file and rewrite it in place. But selecting a file as output immediately truncates it to zero length, meaning that the input stream is now zero length, and the program gets no data to work with. This erases file.txt.

While this is dumb and scary, you can easily work around it by installing the sponge command (it's found in the moreutils package on ubuntu, sudo apt install moreutils) and working like so:

myprogram <file.txt | sponge file.txt

Sponge soaks up input and writes it to the file given as an argument.

On that note, if you're new to the command line environment, you're probably reasonably scared that rm deletes your files irreversibly (though you can muck about with other tools to recover them, somehow). In practice, this turns out not to be a problem, because you become very used to only rming files you are sure you want to rm, you make periodic backups, and you version control. But, you may wish to use the trash command (sudo apt install trash-cli; man trash) instead, and maybe even alias rm to trash. But I digress.

If you don't specify an output file, output will generally be printed to the command line. If you don't specify an input file, input will be solicited from the command line. Try this, for example:

#include <stdio.h>

int main(void) {
  int c = getchar();
  printf("%c", c);
  return 0;
}

Now you should have enough knowledge to complete every exercise in the appendix! Except maybe the ones that involve files.

5. More things

While the purpose of the previous section was to get you up to speed with a minimal set of features you need to write effective C programs, this section touches briefly on features you will need to understand to execute best practices and understand other people's code.

5.1. Data types

You already know about int, but keep in mind char, unsigned char, unsigned int, long long int, float, double (double precision float), and long double. Keep in mind also void, the type that is used for functions that don't return and pointers with no associated type. For a more comprehensive listing please see https://en.wikipedia.org/wiki/C_data_types

5.2. Structures (to guide Interpretation of Computer Programs)

You can compose datatypes together into more complex data using structures, also known as structs. A struct is just a bunch of primitve data types glued together. You can specify and use them like so:

struct whatever{
  int somedata;
  float otherdata;
  char awholestringofdata[50];
};
struct whatever w = {27, 26.2, "example"};
printf("%s", w.awholestringofdata);

They're pretty cool.

5.3. Standard Libraries

There are a lot of libraries standardized with C and which came with your compiler. They are often quite useful. Here are a few of my favorites. You can #include these in your own code and read up about them online.

5.3.1. stdlib.h

A variety of miscellaneous functions, some useful, this library is most notable for containing malloc (memory allocate), which allows you to allocate memory outside of the "stack" we discussed. This is quite useful when you need it, but causes problems as it's often tricky to keep track of if and when you should release the memory (by calling another function in stdlib, free). You could run out of memory or introduce crashes, bugs, or security issues this way. Other cool functions in this library are rand, bsearch, qsort, abs, and atoi.

5.3.2. File IO in stdio.h

I consider it bad style to manipulate a file directly if instead you could process streams passed to your program via stdin and output into streams going out to stdout. This is because this method has no "side effects", that is, all the data is intentionally specified by the user, with no risk of modifying a file they didn't intend.

However, this only works for a simple class of programs, and sometimes you have to explicitly manipulate a file. Facilities for doing this are contained in stdio, the library you have already been using for io.

5.3.3. string.h

This has useful functions for dealing with c strings.

5.4. Pernicious Prepositions Part 2: Pertinent Prepositions

There are many cases where you'd like to always run a while loop once, and then check at the end of the loop for the while condition. You can to that with a do-while loop, like so:

do {
  statements = 1;
  likethis();
  etc();
} while(something);

5.5. Pernicious Prepositions Part 3: Perfidious Prepositions

You might have noticed that every time I write a while loop in these examples I tend to write something like
int i = 0;
while(i<max) {
  //do something
  i++;
}
However, it's pretty easy to forget the int i = 0; and the i++; in here, and ideally i would be scoped to the inside of the loop rather than the outside. C has a convenience just for this:
for(int i = 0; i < max; i++){
  //do something
}
This is equivalent to the while loop above, it's just a rearrangement and rescoping as I described. You can work out for yourself how a for loop could be transformed into a while loop, I'm sure.

Cute fact 1:
All parts of the for loop are optional-- or, I should say, several parts are allowed to be the null statement ";", which does nothing-- so you can do cute things like write
for(;;){
  //whatever
}
to write infinite loops, much like while(1). I've always found while(1) more intuitive, though, especially if I here pronounce 1 as "true".

You can even do dumb stuff like write for(;;); to make an infinite loop that does nothing.

Cute fact 2:
We usually uses for loops straight-forwardly, but you can get pretty tricky with them, especially if you remember how to initialize multiple variables per declaration and/or if you remember how to use the comma operator. For example,

for(int i = 0, j = 10; i<max && j>max; i++,j++){
  //something hard to understand
}

I cannot usually recommend doing this, since it's usually confusing, but sometimes it's the best way to do something.

Cute fact 3:

Actually, the thing we most usually want to do with a for loop in C is iterate over an array. You will write a LOT of code like

for(int i = 0; i < length; i++){
  //blah blah blah
}

To the extent that you will begin cursing C for not having a foreach loop for some sort.

If you're foolhardy, like me, you can define a foreach macro, like the one detailed in https://wyattscarpenter.github.io/blog/foreach.txt

But that is an advanced technique, which you should only consider later.

5.6. Function Pointers

Functions live in memory, so they have a memory address, so you can pass them to other functions, which is cool. However, type decorations in C are pretty crazy, so the syntax is rather hairy. For example,

#include <stdio.h>
int f(int (*g)(int i)){ //function that takes a function that takes an int
  return (*g)(20);
}
int g(int i){ //function that takes an int
  return i+10;
}
int main() {
  printf("%d",f(g)); //prints 30
}

5.7. Some More Important Keywords

When you get curious, you can look up the following c keywords to learn some more useful features: sizeof, typeof, offsetof, static, union.

5.8. Compiling, Linking, Headers, Object Files, Make

Eventually, you'll want to write a program with multiple files. This will necessitate you learn about C's compilation model, which is overcomplicated, and involves compiling each file separately and then sticking them all together at the end, using "header files", "object files", and a "linker", in approximately that order. This is such a headache that I usually recommend just writing programs in a single file. This is such a headache that a mini programming language, make, has been invented to deal with orchestrating the whole thing (this is what a makefile is. if you see one in a directory of c code, you can usually just type make into the command line and then make will take care of compiling the c code, which is nice.)

5.9. Every Other Feature in C

It's occurred to me that in order to write a more comprehensive guide I would have to spend a lot more time at this than I really care to. Having given you a solid basis for understanding what programs are, I now direct you to other guides to C, such as K&R's The C Programming Language or whatever books happen to be on hand, if you'd like to learn more obscure C features. The rest of this guide will deal with a smattering of advanced topics.

6. Advanced Topics

6.1. Style

I will address both typographical style-- how the code is formatted-- and semantic style-- how the code is formulated, in this section.

6.1.1. Typographical Style

In this guide, I have tried to subtly indoctrinate you into my typographical style of writing code simply by writing all of my sample code in my style. Unlike most reasoned thinkers, I think style is important and there is one objectively correct style (though naturally I admit it is possible that my style is not the objectively correct one). Style can make your code easier or hard to understand. For instance, it is a point of contention whether the braces in a function should be written

void foo(void) {
  //note the same line
}

or

void foo(void)
{
  //note the next line
}

In my youth, I favored the second version because I liked the symmetry of the {} pair. However, as a more experienced programmer I have realized: 1) the top line and bottom line of the function are still symmetrically indented in the first version 2) the second version wastes an entire line for a single, uninteresting character. This may not seem like a lot, but a significant portion of programming is getting all your code on a single screen and staring at it until you understand why it's broken. So it's very helpful to have compact code, and this is an easy tradeoff to make.

Other typographic style points make your code read more or less like it actually works. For instance, even though many people like to write int* c;, on the hunch that they are declaring an int pointer, not a "thing-that-dereferences-to-int", int* c, d; declares an int pointer c and an int d. To avoid confusion you must write int *c. This also makes some sense because there's no way to declare an int[] arr (that's just a parse error) you have to declare an int arr[], so you might as well get used to the way these derived types are declared.

Hopefully, those examples are instructive.

6.1.2. Semantic Style

Some semantic styles are strictly better than others, and this is referred to as writing better code. You can can usually measure this in outcomes like speed, size, etc of the resulting executable. See also my best-selling programming self-help book "Just Write Better Code LOL".

Some styles do not lead to a better execution of your program but do made your program easier to read. For instance, one good rule is: do not do more than three things in a single line of code.

For instance, for some reason a common implementation of strcpy is

char *strcpy(char *dest, const char *src){
  char *start = dest;
  while(*dest++ = *src++);
  return start;
}

The while loop with an empty body here does-- well, it's arguable, but I'd say it's more than 3 things. I might replace it with

while(*src){
  *dest = *src;
  dest++;
  src++;
}

(well, actually, I'm a fun guy who thinks *x++ is pretty slick so I might replace it with while(*src){*dest++ = *src++;})

These arrangements are going to produce equivalent code on all but the dumbest compilers, so you might as well pick the one that makes the most sense to read. You will have to read code often.

Occasionally, you will have to choose between more understandable code and faster-executing code. This is a tradeoff, and there are many schools of thought here, but I find the best thing to do is usually to do it the faster way and then leave a comment explaining what you did.

6.2. TUIs, GUIs, Games

So far I have taught you how to make classic C programs that are invoked from the command line to process streams of data and manipulate files. But there are other modalities of interface. I don't have the time or expertise to explain them here, but I will give you some advice at where to look if you want to make a program with one.

If you use a text editor from the command line like nano, you're using an interactive TUI, or Text User Interface. The state of the art for creating these seems to be a library known as ncurses. 

Most applications these days are Graphical User Interfaces, or GUIs. The state of the art libraries for producing these seem to be Qt and GTK, but honestly I've used a lot of bad GUIs made in Qt and GTK, and those libraries seem overcomplex, so I recommend using IUP (Portable User Interface). All of these GUI libraries are cross-platform, so they work on Windows and Linux and Mac.

I've recently been convinced of a good style of gui framework called immediate-mode guis (imgui), which I think are the future. The top imgui in the world as I write is called Dear ImGui and you can find it at https://github.com/ocornut/imgui. Look at how nice it is to declare ui widgets? Very nice. However, Dear ImGui's in C++, so you have a couple options: switch to C++, "write C in C++" ie making your project technically c++ so you can use the Dear ImGui library but still just only using C constructs, use a foreign function interface from C to C++ (whatever that means), use a C wrapper to Dear ImGui like https://github.com/cimgui/cimgui, or (AND THIS IS THE ONE I RECOMMEND) use Nuklear https://github.com/Immediate-Mode-UI/Nuklear a single-header ANSI C immediate mode cross-platform GUI library licensed under public domain.

If you've played a video game, you may have noticed that they look nothing like GUIs. They also have their own libraries. A popular cross-platform one is SDL (Simple DirectMedia Layer).

All of these are more trouble than they are worth, in my opinion.

6.3. Version Control

You may have noticed that sometimes you edit a program, then you return another day and edit it again, then you return a third day and decide that your second edit was erroneous and want to undo it but whoops! your editor's undo history is gone, and you have no choice but to rewrite the program using your hazy memory as a guide. Many such cases.

What you need is a program that manages versions of your program, a "version control system" (VCS). The most popular one is git (sudo apt install git). Git is a pretty complex tool but simple use of it is somewhat simple. I basically only remember six commands:

git init #starts a new git repo

git commit -am "a summary of what you did" -m "further details about what you did, if need be" #commits all the changes you made, with the supplied message
#on your first run, git commit will give you an error and tell you to identify yourself. I never remember those commands, because git tells me the right commands every time.

git status #see what changes are not yet reverted

git log #see a log of commits

git pull #pull changes from a remote repository, if you have one

git push #push changes to a remote repository, if you have one

#there are commands to set the code to a certain state in history, but these are finicky enough that I always look them up to make sure I'm doing the right thing

6.4. Dynamic Array, Hash Map, and STB

One thing you that will occur to you, as you start to write real programs, is how often you want a dynamic array (an array (eg int[] = {1,2,3,4}) that can grow and shrink as you put more objects into it, for an unknown number of objects) or a hashmap (essentially, an array, possible dynamic, that is indexed by some function on data that is not just an integer offset, the point being to create a system of key-value pairs; an example use case is counting the frequency of words in a file: it is quite useful to have a structure that maps strings to ints (the ints being the frequencies) and a flat array with searching doesn't always cut it for large amounts of data). It will be instructive for you to write these for yourself a couple times, and trying to avoid having to write these things will make you a better programmer. Also, you will become more proficient in hunting down memory leaks & other errors. However, eventually you may wish to use a library for this, and other things, and stb.h is a pretty good one. https://github.com/nothings/stb/blob/master/stb.h

6.5. Licensing

Unfortunately, computer programs are copyrightable, which means that your computer programs, and the computer programs of others, are ensnared in fiddly webs of legal restrictions that you need to take a community college course to really understand. (Many software developers don't understand it, but think they do, leading to an amusing variety of folk copyright rituals!) Basically, no one can legally use, reuse, or copy anyone else's program without permission.

There are a variety of software licenses one can apply to one's code. Free ("open source", "libre") software licenses make the code free as in freedom, giving others rights to use them automatically, under some stipulations. Unfortunately, there are so many of these licenses that many projects are ensnared in fiddly webs of legal restrictions that you need to take a community college course to really understand.

Personally, I just put all my code into the public domain using the CC0 license, as this allows maximal freedom for others. I recommend you do this also, but be aware that as you do this you are giving a gift to the world, and can't get mad if they rip off your code, sell it without telling you, or even make a modification and copyright their changed version.

6.6. And Beyond!

Consult the internet or appropriate documentation to learn about other libraries, features of C I may not have mentioned, etc.

However, note also that given your current knowledge, the best way to learn more is to start making programs!

Appendix A: Exercises

These exercises are well documented elsewhere, so I will merely mention them, in approximate order of difficulty, and leave it up to you to look them up, specify them precisely, and attempt them: Hello World, repeat the user's name back to them, BMI calculator, FizzBuzz, nim, list primes or nth prime, list fibonacci numbers or nth fibonacci numbers, list pi or nth digit of pi or search for a substring of digits within pi, small text adventure or other amusing diversion.