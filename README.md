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
There are currently 3 ways to embed ASM into a codeset:

#### Hook Calls

#### Code Calls

#### Pulse Calls
Pulse calls are useful when you need to run some code every frame, without a distinction as to where you're injecting it.
  
Example:  
PULSE  
{  
	&nbsp;&nbsp;&nbsp;lis r3,0x8020  
	&nbsp;&nbsp;&nbsp;ori r3,r3,0x0984  
	&nbsp;&nbsp;&nbsp;icbi r0,r3  
	&nbsp;&nbsp;&nbsp;lis r3,0x801E  
	&nbsp;&nbsp;&nbsp;ori r3,r3,0x9A2C  
	&nbsp;&nbsp;&nbsp;icbi r0,r3  
	&nbsp;&nbsp;&nbsp;isync  
	&nbsp;&nbsp;&nbsp;blr  
}  
### Include Other Files
To include other ASM files akin to including headers in C, follow \'.include\' with the path to the ASM file.

.include ExampleFolder/Example.asm

## Basic Use
Drag a \*.txt file in the format specified by the file specification over the GCTRM executable. This will output a \*.GCT file with the same name as the \*.txt file.
  
**NOTE: GCTRM only works on Windows 10/11 at the moment. More platforms to follow.**
