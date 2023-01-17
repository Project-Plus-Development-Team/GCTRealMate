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

## Basic Use
Drag a \*.txt file in the format specified by the file specification over the GCTRM executable. This will output a \*.GCT file with the same name as the \*.txt file.
  
**NOTE: GCTRM only works on Windows 10/11 at the moment. More platforms to follow.**
