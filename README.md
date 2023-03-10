# GCTRealMate
GCTRealMate AKA GCTRM is a command line program that operates as a sort of [macro assembler](https://en.wikipedia.org/wiki/Assembly_language#Assembler) from formatted Gecko and PPC ASM formats living in a text file to Gecko Codes output into a standard [GCT file](https://mariokartwii.com/showthread.php?tid=37).

## Command Line Options
| Option         |     Use                                                  |
| -------------- | -------------------------------------------------------- |
| -g, -G         | Output the converted codeset to a text file codeset.txt. |
| -*             | Output the codeset with * before each code.              |
| -l, -L         | Output the code names into a log file.                   |
| -q, -Q         | Keep console open after running.                         |
| -p, -P, -c, -C | Unimplemented switches                                   |

## File Specification
The \*.txt file you write should be formatted in a way that GCTRM can understand; this section will cover the syntax you should use, seperated into subsections based on need.

### Gecko Codes
Gecko codes are the most simple way to code using GCTRM; simply append an asterisk before each line of Gecko.  
  
Example:  
\* E0000000 80008000  
\* 225664EC 00000000  

### PPC ASM
There are currently 4 ways to embed ASM into a codeset:

#### Hook Calls
Hook calls are useful when you want to run some code at a location, then return back to that location and continue running intended code.  
  
Example:  
HOOK @ $80044A34  
{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lwz r4, 0x8(r3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cmpwi r8, 0x1  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bne- %END%  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lis r12, 0x8059  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;stw r15, -0x7D08(r12)  
}
#### Code Calls
Code calls are useful when you want to run some code at a location, replacing the code that exists at that address and the number of lines that follow it.
  
Example:  
CODE @ $80001198  
{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addi r6, r7, 0x4C  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mr r3, r7  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addi r4, r7, 0x34  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addi r5, r7, 0x38  
}
#### Pulse Calls
Pulse calls are useful when you need to run some code every frame, without a distinction as to where you're injecting it.
  
Example:  
PULSE  
{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lis r3,0x8020  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ori r3,r3,0x0984  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;icbi r0,r3  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lis r3,0x801E  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ori r3,r3,0x9A2C  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;icbi r0,r3  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;isync  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;blr  
}
#### op Calls
op calls are useful when you want to overwrite a single line of code with another line of code.  
Example:  
op bge- 0x14C 	@ $80055444
### Macros
Macros work in a way that is similar to C-type functions. You provide the name of the macro, arguments, and within the macro body you provide a code that utilizes those arguments.  
  
Example:  
.macro ModuleCmd(\<module\>, \<cmd\>)  
{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lwz r3, 0xD8(r31)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lwz r3, \<module\>(r3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lwz r12, 0x0(r3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lwz r12, \<cmd\>(r12)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mtctr r12  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bctrl  
}  
### Aliases
An alias is essentially a C-type variable. You provide the name of the alias and the value to the right of the name after an equal sign symbol. You can utilize logic and arthimetic symbols within the alias to further give it meaning. Aliases are also definable within macros, code calls, and hook calls.  
  
Example 1:  
.alias Teeter_Loc = 0x80546120  
  
Example 2:  
.macro LoadAddress(\<arg1\>,\<arg2\>)  
{  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.alias temp_Hi = \<arg2\> / 0x10000  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.alias temp_Lo = \<arg2\> & 0xFFFF  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lis \<arg1\>, temp_Hi  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ori \<arg1\>,\<arg1\>, temp_Lo  
}  
### Include Other Files
To include other ASM files akin to including headers in C, follow \'.include\' with the path to the ASM file.
  
Example:  
.include ExampleFolder/Example.asm

## Basic Use
Drag a \*.txt file in the format specified by the file specification over the GCTRM executable. This will output a \*.GCT file with the same name as the \*.txt file.
  
**NOTE: GCTRM only works on Windows 10/11 at the moment. More platforms to follow.**
