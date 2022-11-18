+++
date = "2012-11-08"
title = "Relationship between Machine Code and other Languages"
math = "true"
categories = "test"
+++

## Machine Code

~~~~
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
~~~~


In Machine Code, each command/instruction is made up of two elements, an Op-Code and Operand(s).

These commands are ran directly by the processor in a machine, without any kind of conversion or compilation of code, therefore machine code is the fastest executable code that can be written for a computer.

An example of machine code is 1203, where the 1 is the Op-Code, and the 203 is the Operand (which loads register 2 with bit pattern found at 03).

The trade-off with machine code being the fastest executable code that can be written for a computer is that it is also the hardest for a person to understand, as opposed to higher-level languages, which are described below.

## Assembly Code

~~~~
ADDI    R2, R0  -> R0
MOV     R4      -> [R0]
ADDI    R2, R0  -> R0
~~~~

Assembly language is a language directly related to machine code, in that it is a direct symbolic translation of machine code, as in, there is no complex processing needed to translate assembly code into machine code.

The conversion is a straight one-to-one mnemonic to machine code translation (except for some complex assembler instructions, which are extended).

The example shown in the previous paragraph would be written as:

~~~~
MOV [03] -> R2
~~~~

Where 1 = MOV, 2 = register to, 03 = memory address from.


# Higher Level Languages
High level languages, such as Java and arguably C, were developed in order to make programming syntax easier to understand, because they allow further abstraction from machine code, by allowing constructs such as objects, fields, data types and methods as opposed to addressing memory addresses and registers directly.

## Java

~~~~
public static void main(String[] args) {
    if(args.length == 1) {
        if(args[0] != null && args[0].length() == 0) {
            System.out.print(" is in the language");
            } else {
                new Main(args[0]);
            }
        }
    }
}
~~~~

Java, allows the programmer to use intuitive names for objects and data containers (such as integers, arrays and lists), which makes programming a lot easier with regards to usability, although it is less efficient at run-time than lower level languages, as conversion and optimization (compilation or interpretation) is needed.

Conversion to change the code into low level machine, or assembly language, is called compilation and in the case of Java, the code is first compiled into byte-code (a *.class file), which is platform independent, before it is run on a Java Virtual Machine (JVM), which is specific to the architecture of the actual machine. Using a JVM works by using just-in-time compilation (where code is compiled as needed), which compiles the program from byte-code to native machine code.

The higher level the language is, the larger and more complex the compiler for it will be and will probably include complex ‘garbage collection’, which Java includes. An example of the difference between the levels of abstraction could be that 1 line of Java code could result in hundreds or thousands of machine code instructions.

If a computer was supplied with no compilers and could only be programmed in machine code, tools would need to be built to program the machine in a practical amount of time, because if the only language capable of being written was machine code, few complex programs could be written that could be understood by the coder, or any input/output to be given by user.

To make writing of code easier, an assembler would need to be built to allow the programming of the machine in assembly language; this would translate the assembler mnemonics into OpCodes and Operands. Being able to program in assembly language will allow a programmer to develop simple programs easily, to carry out tasks and solve problems, namely those that involve logic and arithmetic. In order to allow easier coding of programs, a compiler for a higher level programming language such as Java would be useful, but assembly is not abstracted enough to program a compiler for Java, whose compiler and virtual machine are very complex, so another step would be needed first.

A compiler for C would not be as complex, because it shares many similarities with assembler and is written for use with a lightweight compiler. The compilers explained would allow a basis on which we can develop more high-level compilers, as we are increasing the level of abstraction applied to the computer and its machine code. As, once the assembler has been built, the OpCodes need not be remembered, and once a C compiler is built, more sophisticated data structures and memory management is possible, and less knowledge of the registers and memory addresses is needed, adding to the level of abstraction.

## C

~~~~
while(!kbhit())
{
    outportb (0x378,0xFF);
    On_Screen_ini();
    data = inportb (0x378)
    for (bit=0;bit<8;bit++)
    {
        if((data&1)==0x00)
        {
            printf("cage%d",i);
        }
    }
    On_Screen(int s)
    {
        gotoxy(s*9+1,20);
        printf("CAGE%d",s);
    }
}
~~~~

C is a language, which requires minimal processing and parsing at runtime because most of its instructions map efficiently to assembler anyway. The jump from machine code to assembler is small, as they share many of the same instructions and are just presented in a different form, the jump from assembler to C is a little bigger, with a compiler around 100kb in size (Tiny C Compiler written in x86 assembler), but it allows a jump up in abstraction and is nowhere near as large as a JVM or Java Compiler, which needs a large amount of pre-processing.

Many Java Virtual Machines are built with the C language (the official JVM is written in C++), so if a Java Virtual Machine for this computer was to be built, it would allow for pre-compiled Java code to be run on this machine, from the byte-code, which would enable this machine to run very complex codes and perform advanced tasks, using object-orientation and complex data types alongside primitive data types, it also allows the using of massive libraries of methods and procedures, this enables the machine, which could only originally be programmed in machine code and assembly, to perform elaborate tasks.