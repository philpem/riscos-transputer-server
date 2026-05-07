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

## Modes of operation

### Iserver mode

Iserver is the canonical (or at least the most recent) Inmos Transputer host fileserver.If you want to run the Inmos D700 TDS, D7205 Occam toolkit or D7214 ANSI C toolkit, this is the mode you want to use. Its usage and protocol are documented in the [S708 User Guide (72-OEK-227-01)](https://www.bitsavers.org/components/inmos/transputer/manuals/72_OEK_227_01_S708_User_Guide.pdf) in sections 6 and 7. This document also gives some good background information on creating images Iserver can load into the Transputer network.

The `-SL` parameter accepts a link name: this is not used by the RISC OS Iserver.


### Afserver mode

Afserver (Alien File Server) is documented in ["The Afserver V1.5" by Inmos](https://transputer.net/prog/afserver/afserver.pdf). Its main use is as a host server for the 3L Ltd. Parallel C and Fortran compilers.

It's been integrated into this version of Iserver as a separate mode. Iserver is used to boot the Transputer, then it hands control over to the Afserver core to run the file-server protocol.

The Afserver mode is enabled with the `-SF` command. For instance, 3L's Worm tool can be run with the command:

```
*iserver -SF -SB IDEFS::7.$.tc2v2.b4.worm /?
Booting root transputer...ok
Worm V1.1, transputer network analyser

usage: worm       count network nodes
  or   worm/c     output configuration file
  or   worm/f     full information on each node

Copyright (C) 3L Ltd, 1989

*iserver -SF -SB IDEFS::4.$.tc2v2.b4.worm /f
Booting root transputer...ok
one processor found
processor ROOT type=T800 25.0MHz, 2.0 penalties, 0 hops 4096K
   links to HOST[0],-------,-------,-------
```

Note that the "`/f`" is a command-line parameter to Worm.

The only supported Afserver command line option is `-:o`, which sets the Afserver option flag value:

```
| set the Option flag to 1
iserver -SF -SZ -:o 1 -SB IDEFS::4.$.tc2v2.b4.tc
```

The default `o` (alien option) is 1, which works for Parallel C. Option 0 will cause Parallel C to hang.

The other Afserver options, per the Inmos document linked above, are specific to the DOS and VAX/VMS versions and are thus not implemented. In most cases they have direct equivalents, e.g. `-:e` is equivalent to `-SE`.
