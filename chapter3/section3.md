# mac：flex&bison

$ lex -h

Usage: flex \[OPTIONS\] \[FILE\]...

Generates programs that perform pattern-matching on text.

Table Compression:

-Ca, --align      trade off larger tables for better memory alignment

-Ce, --ecs        construct equivalence classes

-Cf               do not compress tables; use -f representation

-CF               do not compress tables; use -F representation

-Cm, --meta-ecs   construct meta-equivalence classes

-Cr, --read       use read\(\) instead of stdio for scanner input

-f, --full        generate fast, large scanner. Same as -Cfr

-F, --fast        use alternate table representation. Same as -CFr

-Cem              default compression \(same as --ecs --meta-ecs\)

Debugging:

-d, --debug             enable debug mode in scanner

-b, --backup            write backing-up information to lex.backup

$ yacc -h

GNU bison generates parsers for LALR\(1\) grammars.



Usage: /Library/Developer/CommandLineTools/usr/bin/bison \[OPTION\]... FILE



If a long option shows an argument as mandatory, then it is mandatory

for the equivalent short option also.  Similarly for optional arguments.



Operation modes:

  -h, --help                 display this help and exit

  -V, --version              output version information and exit

      --print-localedir      output directory containing locale-dependent data

  -y, --yacc                 emulate POSIX yacc



Parser:

  -S, --skeleton=FILE        specify the skeleton to use

  -t, --debug                instrument the parser for debugging

