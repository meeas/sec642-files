#!/usr/bin/env python
# 2011-11-01 Joshua Wright, jwright@hasborg.com
import sys
import os

def usage():
    print >>sys.stderr, """
xor2files: XOR the contents of two files together, up to the length of the
smaller of the two files.

Usage: xor2files [file1] [file2] [output file]
"""

if len(sys.argv) != 4:
    usage()
    sys.exit(0)

else:

    file1 = open(sys.argv[1], "rb")
    fc1 = file1.read()
    file1.close()

    file2 = open(sys.argv[2], "rb")
    fc2 = file2.read()
    file2.close()

    ofile = open(sys.argv[3], "wb")

    if len(fc1) < len(fc2):
        length = len(fc1)
    else:
        length = len(fc2)

    ofc=""
    for i in xrange(0,length):
        ofc = "".join( [ ofc, chr(ord(fc1[i])^ord(fc2[i])) ] )

    ofile.write(ofc)
    ofile.close()
