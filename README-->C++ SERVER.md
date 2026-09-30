*** BRIEF OVERVIEW ***
This is a project made in C++. It is a C++ coded server that incorporates a 3-way TCP handshake, stores clients data in a memory matrix and is able to be retrieve and sent back to client.
this C++ coded server follow the TCP/IP protocol structure and listens on IPv4 connections
It talks to the kernel and manages hardware resources as any other fully functioning server would like where data is placed in the RAM

*** THINGS I LEARNED ALONG THE WAY ***
along the way i learned interesting things about how computers work and way software and hardware work in union togather.
I learned the use of pointers, creating a database via arrays and madstering the use of structs and declaration structures.
there is a massive difference between the deceiving size_t and ssize_t. Lets understand size_t and ssize_t; 1st; size_t is unsigned while ssize_t is signed; size_t measures the size of data (like a string's length), while ssize_t tracks the amount of data successfully moved during an operation with the ability to report an error if something goes wrong. If you're using size_t in a section of code that uses -1 as an answer then size_t will flip out of order and glitch into a very large number which will translates into the amount of bytes in RAM and since this is a very very large and ugly number it will turn into a buffer overflow in RAM and will corrupt any data slots near its blast radius.

*** EXPLINATION OF THE DATABASES MATHEMATICAL FORMULA ***
for:
int row = db.total_writes / 5;
int col = db.total_writes % 5;

HERE IS A VISUAL REPRESENTATION OF WHAT THE DATABASE WOULD LOOK LIKE:

         Col 0   Col 1   Col 2   Col 3   Col 4
Row 0:  [ 0 ]   [ 1 ]   [ 2 ]   [ 3 ]   [ 4 ]
Row 1:  [ 5 ]   [ 6 ]   [ 7 ]   [ 8 ]   [ 9 ]
Row 2:  [10 ]   [11 ]   [12 ]   [13 ]   [14 ]
Row 3:  [15 ]   [16 ]   [17 ]   [18 ]   [19 ]
Row 4:  [20 ]   [21 ]   [22 ]   [23 ]   [24 ]

lets say we wanted to put the data in 6, we figure the rows and columns by doing some math;
rows = 6/5 = 1.2, take away 0.2 then the number 6 is in row 1
columns = 6%5 (how many times can 6 go into 5? well it can't so its the whole 1 piece) = 1 so number 6 is in column 1
