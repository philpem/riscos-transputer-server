# RISC OS Transputer Server (iserver)

This is an improved version of Inmos Iserver 1.41 (the Occam Toolset Host File Server), which was ported to RISC OS by Richard Crook at SJ Research. It was later released on Stardot as part of a collection of Nexus Networking source code files.

Notably this version of IServer has the option of opening a WIMP window which contains a terminal console. It also supports parsing the messages from the Inmos Occam Toolset compiler and passing them to RISC OS editors via the Throwback mechanism.

I've taken Richard's work, cleaned it up and got it building and working using standard libraries. It previously used a set of headers which seemed to be either SJ-internal or provided privately by Acorn (they resemble the Hdr headers in the RISC OS Open source release).


## Compiler and libraries

For the C compiler, use [Acorn C/C++ 5](https://www.4corn.co.uk/articles/acornc5/). Remember to apply the two updates.

The following libraries are required:

  - RISC_OSLib: https://www.4corn.co.uk/archive/Acorn_Developer_Site/reference/risc_oslib.arc
  - OSLib 6.70: https://web.archive.org/web/20050111103709/http://ro-oslib.sourceforge.net/670/index.html

You will need to build RISC_OSLib from source.

The Makefile expects to find RISC_OSLib in `RISC_OSLib:` (`RISC_OSLib$Path`) and OSLib in `OSLib:`.

