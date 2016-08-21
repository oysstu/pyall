pyall
=====
* created:       August 2016
* by:            p.kennedy@fugro.com
* description:   python module to read an XTF sonar file
* notes:         See main at end of script for example how to use this
* based on ALL file version 
* version 1.00
* developed for Python version 3.4 

Done
====
* Basic read loop is in place.  reads datagram header and skips to next record.  THis is quick
P = 80 P Position record decode in place
X = 88 X Depth record decode in place.  Beam data is in a list of depths[], alongtrack[] etc instead of a beam object.  This may be more efficient?
Tested X reader by making a waterfall image.  Results look good


2Do
===
D = 68
E = 69
F = 70
G = 71
H = 72
I = 73
J = 74
K = 75
L = 76
M = 77
N = 78
O = 79
Q = 81
R = 82
S = 83
T = 84
U = 85
V = 86
W = 87
Y = 89

Example Reader
-------
```
    r = ALLReader("C:/development/all/sampledata/EM2040/GeoFocusEM2040400kHzdual-Rx0.5degx1degPitchStabilised.all")
    start_time = time.time() # time the process

    while r.moreData():
        # read a datagram.  If we support it, return the datagram type and aclass for that datagram
        # The user then needs to call the read() method for the class to undertake a fileread and binary decode.  This keeps the read super quick.
        TypeOfDatagram, datagram = r.readDatagram()

        if TypeOfDatagram == 'P':
            datagram.read()
            print ("Lat: %.5f Lon: %.5f" % (datagram.Latitude, datagram.Longitude))

        if TypeOfDatagram == 'X':
            datagram.read()

    print("Read Duration: %.3f seconds" % (time.time() - start_time)) # print the processing time. It is handy to keep an eye on processing performance.

    r.rewind()
    print("Complete reading ALL file :-)")
    r.close()    
    # see the test code in main() at the end of pyall for more details.  Have Fun
```
```
Data Type Conversions from ALL to Python::     
    * ALL types to python struct types
    * signed char = 1 byte = "b"
    * unsigned char = 1 byte = "B"
    * XTFWORD = signed int 2 bytes = h
    * XTFWORD = UNsigned int 2 bytes = H (for unipolar data)
    * DWORD = unsigned int 4 bytes = "L"
    * short = short integer 2 bytes = "h"
    * char = 1 byte = "c"
```