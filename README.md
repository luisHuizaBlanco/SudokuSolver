Sudoku solver via DPLL

By Luis Huiza Blanco

tested on: snappy1.cims.nyu.edu

compile just typing make to use the included Makefile

argument structure: 

./sudoku (optionally -v for verbose mode) rc=v ... [values for filled spaces in the sudoku board (format row => r column => c value => v : rc=v)]

numbers given in rc=v must each be single digits on range 1-9, or it will have an error

example:

./sudoku 11=4 12=2 14=8 15=7 16=5 17=3 19=6 21=3 22=9 23=8 25=4 26=6 27=7 29=1 33=5 37=8 39=2 41=9 43=6 44=4 45=5 56=1 58=8 61=1 62=4 67=6 75=2 76=4 77=5 78=3 82=3 84=1 85=8 88=6 93=9 97=4

should output:

SUCCESS
 4  2  1  8  7  5  3  9  6
 3  9  8  2  4  6  7  5  1
 6  7  5  9  1  3  8  4  2
 9  8  6  4  5  2  1  7  3
 7  5  2  3  6  1  9  8  4
 1  4  3  7  9  8  6  2  5
 8  1  7  6  2  4  5  3  9
 5  3  4  1  8  9  2  6  7
 2  6  9  5  3  7  4  1  8

or

./sudoku -v 11=4 12=2 14=8 15=7 16=5 17=3 19=6 21=3 22=9 23=8 25=4 26=6 27=7 29=1 33=5 37=8 39=2 41=9 43=6 44=4 45=5 56=1 58=8 61=1 62=4 67=6 75=2 76=4 77=5 78=3 82=3 84=1 85=8 88=6 93=9 97=4

will give you:

n4_r1_c1
n2_r1_c2
n8_r1_c4
n7_r1_c5
n5_r1_c6
n3_r1_c7
n6_r1_c9
n3_r2_c1
n9_r2_c2
n8_r2_c3
n4_r2_c5
n6_r2_c6
n7_r2_c7
n1_r2_c9
n5_r3_c3
n8_r3_c7
n2_r3_c9
n9_r4_c1
n6_r4_c3
n4_r4_c4
n5_r4_c5
n1_r5_c6
n8_r5_c8
n1_r6_c1
n4_r6_c2
n6_r6_c7
n2_r7_c5
n4_r7_c6
n5_r7_c7
n3_r7_c8
n3_r8_c2
n1_r8_c4
n8_r8_c5
n6_r8_c8
n9_r9_c3
n4_r9_c7
!n1_r1_c1 !n2_r1_c1
!n1_r1_c1 !n3_r1_c1
!n1_r1_c1 !n4_r1_c1
!n1_r1_c1 !n5_r1_c1
!n1_r1_c1 !n6_r1_c1
!n1_r1_c1 !n7_r1_c1
!n1_r1_c1 !n8_r1_c1
!n1_r1_c1 !n9_r1_c1
!n1_r1_c1 !n1_r1_c2
!n1_r1_c1 !n1_r1_c3
!n1_r1_c1 !n1_r1_c4
!n1_r1_c1 !n1_r1_c5
!n1_r1_c1 !n1_r1_c6
!n1_r1_c1 !n1_r1_c7
!n1_r1_c1 !n1_r1_c8
!n1_r1_c1 !n1_r1_c9
!n1_r1_c1 !n1_r2_c1
!n1_r1_c1 !n1_r3_c1
!n1_r1_c1 !n1_r4_c1
!n1_r1_c1 !n1_r5_c1
!n1_r1_c1 !n1_r6_c1
!n1_r1_c1 !n1_r7_c1
!n1_r1_c1 !n1_r8_c1
!n1_r1_c1 !n1_r9_c1
!n1_r1_c1 !n1_r2_c2
!n1_r1_c1 !n1_r2_c3
!n1_r1_c1 !n1_r3_c2
!n1_r1_c1 !n1_r3_c3
!n2_r1_c1 !n3_r1_c1
!n2_r1_c1 !n4_r1_c1
!n2_r1_c1 !n5_r1_c1
!n2_r1_c1 !n6_r1_c1
!n2_r1_c1 !n7_r1_c1
!n2_r1_c1 !n8_r1_c1
!n2_r1_c1 !n9_r1_c1
!n2_r1_c1 !n2_r1_c2
!n2_r1_c1 !n2_r1_c3
!n2_r1_c1 !n2_r1_c4
!n2_r1_c1 !n2_r1_c5
!n2_r1_c1 !n2_r1_c6
!n2_r1_c1 !n2_r1_c7
!n2_r1_c1 !n2_r1_c8
!n2_r1_c1 !n2_r1_c9
!n2_r1_c1 !n2_r2_c1
!n2_r1_c1 !n2_r3_c1
!n2_r1_c1 !n2_r4_c1
!n2_r1_c1 !n2_r5_c1
!n2_r1_c1 !n2_r6_c1
!n2_r1_c1 !n2_r7_c1
!n2_r1_c1 !n2_r8_c1
!n2_r1_c1 !n2_r9_c1
!n2_r1_c1 !n2_r2_c2
!n2_r1_c1 !n2_r2_c3
!n2_r1_c1 !n2_r3_c2
!n2_r1_c1 !n2_r3_c3
!n3_r1_c1 !n4_r1_c1
!n3_r1_c1 !n5_r1_c1
!n3_r1_c1 !n6_r1_c1
!n3_r1_c1 !n7_r1_c1
!n3_r1_c1 !n8_r1_c1
!n3_r1_c1 !n9_r1_c1
!n3_r1_c1 !n3_r1_c2
!n3_r1_c1 !n3_r1_c3
!n3_r1_c1 !n3_r1_c4
!n3_r1_c1 !n3_r1_c5
!n3_r1_c1 !n3_r1_c6
!n3_r1_c1 !n3_r1_c7
!n3_r1_c1 !n3_r1_c8
!n3_r1_c1 !n3_r1_c9
!n3_r1_c1 !n3_r2_c1
!n3_r1_c1 !n3_r3_c1
!n3_r1_c1 !n3_r4_c1
!n3_r1_c1 !n3_r5_c1
!n3_r1_c1 !n3_r6_c1
!n3_r1_c1 !n3_r7_c1
!n3_r1_c1 !n3_r8_c1
!n3_r1_c1 !n3_r9_c1
!n3_r1_c1 !n3_r2_c2
!n3_r1_c1 !n3_r2_c3
!n3_r1_c1 !n3_r3_c2
!n3_r1_c1 !n3_r3_c3
!n4_r1_c1 !n5_r1_c1
!n4_r1_c1 !n6_r1_c1
!n4_r1_c1 !n7_r1_c1
!n4_r1_c1 !n8_r1_c1
!n4_r1_c1 !n9_r1_c1
!n4_r1_c1 !n4_r1_c2
!n4_r1_c1 !n4_r1_c3
!n4_r1_c1 !n4_r1_c4
!n4_r1_c1 !n4_r1_c5
!n4_r1_c1 !n4_r1_c6
!n4_r1_c1 !n4_r1_c7
!n4_r1_c1 !n4_r1_c8
!n4_r1_c1 !n4_r1_c9
!n4_r1_c1 !n4_r2_c1
!n4_r1_c1 !n4_r3_c1
!n4_r1_c1 !n4_r4_c1
!n4_r1_c1 !n4_r5_c1
!n4_r1_c1 !n4_r6_c1
!n4_r1_c1 !n4_r7_c1
!n4_r1_c1 !n4_r8_c1
!n4_r1_c1 !n4_r9_c1
!n4_r1_c1 !n4_r2_c2
!n4_r1_c1 !n4_r2_c3
!n4_r1_c1 !n4_r3_c2
!n4_r1_c1 !n4_r3_c3
!n5_r1_c1 !n6_r1_c1
!n5_r1_c1 !n7_r1_c1
!n5_r1_c1 !n8_r1_c1
!n5_r1_c1 !n9_r1_c1
!n5_r1_c1 !n5_r1_c2
!n5_r1_c1 !n5_r1_c3
!n5_r1_c1 !n5_r1_c4
!n5_r1_c1 !n5_r1_c5
!n5_r1_c1 !n5_r1_c6
!n5_r1_c1 !n5_r1_c7
!n5_r1_c1 !n5_r1_c8
!n5_r1_c1 !n5_r1_c9
!n5_r1_c1 !n5_r2_c1
!n5_r1_c1 !n5_r3_c1
!n5_r1_c1 !n5_r4_c1
!n5_r1_c1 !n5_r5_c1
!n5_r1_c1 !n5_r6_c1
!n5_r1_c1 !n5_r7_c1
!n5_r1_c1 !n5_r8_c1
!n5_r1_c1 !n5_r9_c1
!n5_r1_c1 !n5_r2_c2
!n5_r1_c1 !n5_r2_c3
!n5_r1_c1 !n5_r3_c2
!n5_r1_c1 !n5_r3_c3
!n6_r1_c1 !n7_r1_c1
!n6_r1_c1 !n8_r1_c1
!n6_r1_c1 !n9_r1_c1
!n6_r1_c1 !n6_r1_c2
!n6_r1_c1 !n6_r1_c3
!n6_r1_c1 !n6_r1_c4
!n6_r1_c1 !n6_r1_c5
!n6_r1_c1 !n6_r1_c6
!n6_r1_c1 !n6_r1_c7
!n6_r1_c1 !n6_r1_c8
!n6_r1_c1 !n6_r1_c9
!n6_r1_c1 !n6_r2_c1
!n6_r1_c1 !n6_r3_c1
!n6_r1_c1 !n6_r4_c1
!n6_r1_c1 !n6_r5_c1
!n6_r1_c1 !n6_r6_c1
!n6_r1_c1 !n6_r7_c1
!n6_r1_c1 !n6_r8_c1
!n6_r1_c1 !n6_r9_c1
!n6_r1_c1 !n6_r2_c2
!n6_r1_c1 !n6_r2_c3
!n6_r1_c1 !n6_r3_c2
!n6_r1_c1 !n6_r3_c3
!n7_r1_c1 !n8_r1_c1
!n7_r1_c1 !n9_r1_c1
!n7_r1_c1 !n7_r1_c2
!n7_r1_c1 !n7_r1_c3
!n7_r1_c1 !n7_r1_c4
!n7_r1_c1 !n7_r1_c5
!n7_r1_c1 !n7_r1_c6
!n7_r1_c1 !n7_r1_c7
!n7_r1_c1 !n7_r1_c8
!n7_r1_c1 !n7_r1_c9
!n7_r1_c1 !n7_r2_c1
!n7_r1_c1 !n7_r3_c1
!n7_r1_c1 !n7_r4_c1
!n7_r1_c1 !n7_r5_c1
!n7_r1_c1 !n7_r6_c1
!n7_r1_c1 !n7_r7_c1
!n7_r1_c1 !n7_r8_c1
!n7_r1_c1 !n7_r9_c1
!n7_r1_c1 !n7_r2_c2
!n7_r1_c1 !n7_r2_c3
!n7_r1_c1 !n7_r3_c2
!n7_r1_c1 !n7_r3_c3
!n8_r1_c1 !n9_r1_c1
!n8_r1_c1 !n8_r1_c2
!n8_r1_c1 !n8_r1_c3
!n8_r1_c1 !n8_r1_c4
!n8_r1_c1 !n8_r1_c5
!n8_r1_c1 !n8_r1_c6
!n8_r1_c1 !n8_r1_c7
!n8_r1_c1 !n8_r1_c8
!n8_r1_c1 !n8_r1_c9
!n8_r1_c1 !n8_r2_c1
!n8_r1_c1 !n8_r3_c1
!n8_r1_c1 !n8_r4_c1
!n8_r1_c1 !n8_r5_c1
!n8_r1_c1 !n8_r6_c1
!n8_r1_c1 !n8_r7_c1
!n8_r1_c1 !n8_r8_c1
!n8_r1_c1 !n8_r9_c1
!n8_r1_c1 !n8_r2_c2
!n8_r1_c1 !n8_r2_c3
!n8_r1_c1 !n8_r3_c2
!n8_r1_c1 !n8_r3_c3
!n9_r1_c1 !n9_r1_c2
!n9_r1_c1 !n9_r1_c3
!n9_r1_c1 !n9_r1_c4
!n9_r1_c1 !n9_r1_c5
!n9_r1_c1 !n9_r1_c6
!n9_r1_c1 !n9_r1_c7
!n9_r1_c1 !n9_r1_c8
!n9_r1_c1 !n9_r1_c9
!n9_r1_c1 !n9_r2_c1
!n9_r1_c1 !n9_r3_c1
!n9_r1_c1 !n9_r4_c1
!n9_r1_c1 !n9_r5_c1
!n9_r1_c1 !n9_r6_c1
!n9_r1_c1 !n9_r7_c1
!n9_r1_c1 !n9_r8_c1
!n9_r1_c1 !n9_r9_c1
!n9_r1_c1 !n9_r2_c2
!n9_r1_c1 !n9_r2_c3
!n9_r1_c1 !n9_r3_c2
!n9_r1_c1 !n9_r3_c3
n1_r1_c1 n2_r1_c1 n3_r1_c1 n4_r1_c1 n5_r1_c1 n6_r1_c1 n7_r1_c1 n8_r1_c1 n9_r1_c1
!n1_r1_c2 !n2_r1_c2
!n1_r1_c2 !n3_r1_c2
!n1_r1_c2 !n4_r1_c2
!n1_r1_c2 !n5_r1_c2
!n1_r1_c2 !n6_r1_c2
!n1_r1_c2 !n7_r1_c2
!n1_r1_c2 !n8_r1_c2
!n1_r1_c2 !n9_r1_c2
!n1_r1_c2 !n1_r1_c3
!n1_r1_c2 !n1_r1_c4
!n1_r1_c2 !n1_r1_c5
!n1_r1_c2 !n1_r1_c6
!n1_r1_c2 !n1_r1_c7
!n1_r1_c2 !n1_r1_c8
!n1_r1_c2 !n1_r1_c9
!n1_r1_c2 !n1_r2_c2
!n1_r1_c2 !n1_r3_c2
!n1_r1_c2 !n1_r4_c2
!n1_r1_c2 !n1_r5_c2
!n1_r1_c2 !n1_r6_c2
!n1_r1_c2 !n1_r7_c2
!n1_r1_c2 !n1_r8_c2
!n1_r1_c2 !n1_r9_c2
!n1_r1_c2 !n1_r2_c1
!n1_r1_c2 !n1_r2_c3
!n1_r1_c2 !n1_r3_c1
!n1_r1_c2 !n1_r3_c3
!n2_r1_c2 !n3_r1_c2
!n2_r1_c2 !n4_r1_c2
!n2_r1_c2 !n5_r1_c2
!n2_r1_c2 !n6_r1_c2
!n2_r1_c2 !n7_r1_c2
!n2_r1_c2 !n8_r1_c2
!n2_r1_c2 !n9_r1_c2
!n2_r1_c2 !n2_r1_c3
!n2_r1_c2 !n2_r1_c4
!n2_r1_c2 !n2_r1_c5
!n2_r1_c2 !n2_r1_c6
!n2_r1_c2 !n2_r1_c7
!n2_r1_c2 !n2_r1_c8
!n2_r1_c2 !n2_r1_c9
!n2_r1_c2 !n2_r2_c2
!n2_r1_c2 !n2_r3_c2
!n2_r1_c2 !n2_r4_c2
!n2_r1_c2 !n2_r5_c2
!n2_r1_c2 !n2_r6_c2
!n2_r1_c2 !n2_r7_c2
!n2_r1_c2 !n2_r8_c2
!n2_r1_c2 !n2_r9_c2
!n2_r1_c2 !n2_r2_c1
!n2_r1_c2 !n2_r2_c3
!n2_r1_c2 !n2_r3_c1
!n2_r1_c2 !n2_r3_c3
!n3_r1_c2 !n4_r1_c2
!n3_r1_c2 !n5_r1_c2
!n3_r1_c2 !n6_r1_c2
!n3_r1_c2 !n7_r1_c2
!n3_r1_c2 !n8_r1_c2
!n3_r1_c2 !n9_r1_c2
!n3_r1_c2 !n3_r1_c3
!n3_r1_c2 !n3_r1_c4
!n3_r1_c2 !n3_r1_c5
!n3_r1_c2 !n3_r1_c6
!n3_r1_c2 !n3_r1_c7
!n3_r1_c2 !n3_r1_c8
!n3_r1_c2 !n3_r1_c9
!n3_r1_c2 !n3_r2_c2
!n3_r1_c2 !n3_r3_c2
!n3_r1_c2 !n3_r4_c2
!n3_r1_c2 !n3_r5_c2
!n3_r1_c2 !n3_r6_c2
!n3_r1_c2 !n3_r7_c2
!n3_r1_c2 !n3_r8_c2
!n3_r1_c2 !n3_r9_c2
!n3_r1_c2 !n3_r2_c1
!n3_r1_c2 !n3_r2_c3
!n3_r1_c2 !n3_r3_c1
!n3_r1_c2 !n3_r3_c3
!n4_r1_c2 !n5_r1_c2
!n4_r1_c2 !n6_r1_c2
!n4_r1_c2 !n7_r1_c2
!n4_r1_c2 !n8_r1_c2
!n4_r1_c2 !n9_r1_c2
!n4_r1_c2 !n4_r1_c3
!n4_r1_c2 !n4_r1_c4
!n4_r1_c2 !n4_r1_c5
!n4_r1_c2 !n4_r1_c6
!n4_r1_c2 !n4_r1_c7
!n4_r1_c2 !n4_r1_c8
!n4_r1_c2 !n4_r1_c9
!n4_r1_c2 !n4_r2_c2
!n4_r1_c2 !n4_r3_c2
!n4_r1_c2 !n4_r4_c2
!n4_r1_c2 !n4_r5_c2
!n4_r1_c2 !n4_r6_c2
!n4_r1_c2 !n4_r7_c2
!n4_r1_c2 !n4_r8_c2
!n4_r1_c2 !n4_r9_c2
!n4_r1_c2 !n4_r2_c1
!n4_r1_c2 !n4_r2_c3
!n4_r1_c2 !n4_r3_c1
!n4_r1_c2 !n4_r3_c3
!n5_r1_c2 !n6_r1_c2
!n5_r1_c2 !n7_r1_c2
!n5_r1_c2 !n8_r1_c2
!n5_r1_c2 !n9_r1_c2
!n5_r1_c2 !n5_r1_c3
!n5_r1_c2 !n5_r1_c4
!n5_r1_c2 !n5_r1_c5
!n5_r1_c2 !n5_r1_c6
!n5_r1_c2 !n5_r1_c7
!n5_r1_c2 !n5_r1_c8
!n5_r1_c2 !n5_r1_c9
!n5_r1_c2 !n5_r2_c2
!n5_r1_c2 !n5_r3_c2
!n5_r1_c2 !n5_r4_c2
!n5_r1_c2 !n5_r5_c2
!n5_r1_c2 !n5_r6_c2
!n5_r1_c2 !n5_r7_c2
!n5_r1_c2 !n5_r8_c2
!n5_r1_c2 !n5_r9_c2
!n5_r1_c2 !n5_r2_c1
!n5_r1_c2 !n5_r2_c3
!n5_r1_c2 !n5_r3_c1
!n5_r1_c2 !n5_r3_c3
!n6_r1_c2 !n7_r1_c2
!n6_r1_c2 !n8_r1_c2
!n6_r1_c2 !n9_r1_c2
!n6_r1_c2 !n6_r1_c3
!n6_r1_c2 !n6_r1_c4
!n6_r1_c2 !n6_r1_c5
!n6_r1_c2 !n6_r1_c6
!n6_r1_c2 !n6_r1_c7
!n6_r1_c2 !n6_r1_c8
!n6_r1_c2 !n6_r1_c9
!n6_r1_c2 !n6_r2_c2
!n6_r1_c2 !n6_r3_c2
!n6_r1_c2 !n6_r4_c2
!n6_r1_c2 !n6_r5_c2
!n6_r1_c2 !n6_r6_c2
!n6_r1_c2 !n6_r7_c2
!n6_r1_c2 !n6_r8_c2
!n6_r1_c2 !n6_r9_c2
!n6_r1_c2 !n6_r2_c1
!n6_r1_c2 !n6_r2_c3
!n6_r1_c2 !n6_r3_c1
!n6_r1_c2 !n6_r3_c3
!n7_r1_c2 !n8_r1_c2
!n7_r1_c2 !n9_r1_c2
!n7_r1_c2 !n7_r1_c3
!n7_r1_c2 !n7_r1_c4
!n7_r1_c2 !n7_r1_c5
!n7_r1_c2 !n7_r1_c6
!n7_r1_c2 !n7_r1_c7
!n7_r1_c2 !n7_r1_c8
!n7_r1_c2 !n7_r1_c9
!n7_r1_c2 !n7_r2_c2
!n7_r1_c2 !n7_r3_c2
!n7_r1_c2 !n7_r4_c2
!n7_r1_c2 !n7_r5_c2
!n7_r1_c2 !n7_r6_c2
!n7_r1_c2 !n7_r7_c2
!n7_r1_c2 !n7_r8_c2
!n7_r1_c2 !n7_r9_c2
!n7_r1_c2 !n7_r2_c1
!n7_r1_c2 !n7_r2_c3
!n7_r1_c2 !n7_r3_c1
!n7_r1_c2 !n7_r3_c3
!n8_r1_c2 !n9_r1_c2
!n8_r1_c2 !n8_r1_c3
!n8_r1_c2 !n8_r1_c4
!n8_r1_c2 !n8_r1_c5
!n8_r1_c2 !n8_r1_c6
!n8_r1_c2 !n8_r1_c7
!n8_r1_c2 !n8_r1_c8
!n8_r1_c2 !n8_r1_c9
!n8_r1_c2 !n8_r2_c2
!n8_r1_c2 !n8_r3_c2
!n8_r1_c2 !n8_r4_c2
!n8_r1_c2 !n8_r5_c2
!n8_r1_c2 !n8_r6_c2
!n8_r1_c2 !n8_r7_c2
!n8_r1_c2 !n8_r8_c2
!n8_r1_c2 !n8_r9_c2
!n8_r1_c2 !n8_r2_c1
!n8_r1_c2 !n8_r2_c3
!n8_r1_c2 !n8_r3_c1
!n8_r1_c2 !n8_r3_c3
!n9_r1_c2 !n9_r1_c3
!n9_r1_c2 !n9_r1_c4
!n9_r1_c2 !n9_r1_c5
!n9_r1_c2 !n9_r1_c6
!n9_r1_c2 !n9_r1_c7
!n9_r1_c2 !n9_r1_c8
!n9_r1_c2 !n9_r1_c9
!n9_r1_c2 !n9_r2_c2
!n9_r1_c2 !n9_r3_c2
!n9_r1_c2 !n9_r4_c2
!n9_r1_c2 !n9_r5_c2
!n9_r1_c2 !n9_r6_c2
!n9_r1_c2 !n9_r7_c2
!n9_r1_c2 !n9_r8_c2
!n9_r1_c2 !n9_r9_c2
!n9_r1_c2 !n9_r2_c1
!n9_r1_c2 !n9_r2_c3
!n9_r1_c2 !n9_r3_c1
!n9_r1_c2 !n9_r3_c3
n1_r1_c2 n2_r1_c2 n3_r1_c2 n4_r1_c2 n5_r1_c2 n6_r1_c2 n7_r1_c2 n8_r1_c2 n9_r1_c2
!n1_r1_c3 !n2_r1_c3
!n1_r1_c3 !n3_r1_c3
!n1_r1_c3 !n4_r1_c3
!n1_r1_c3 !n5_r1_c3
!n1_r1_c3 !n6_r1_c3
!n1_r1_c3 !n7_r1_c3
!n1_r1_c3 !n8_r1_c3
!n1_r1_c3 !n9_r1_c3
!n1_r1_c3 !n1_r1_c4
!n1_r1_c3 !n1_r1_c5
!n1_r1_c3 !n1_r1_c6
!n1_r1_c3 !n1_r1_c7
!n1_r1_c3 !n1_r1_c8
!n1_r1_c3 !n1_r1_c9
!n1_r1_c3 !n1_r2_c3
!n1_r1_c3 !n1_r3_c3
!n1_r1_c3 !n1_r4_c3
!n1_r1_c3 !n1_r5_c3
!n1_r1_c3 !n1_r6_c3
!n1_r1_c3 !n1_r7_c3
!n1_r1_c3 !n1_r8_c3
!n1_r1_c3 !n1_r9_c3
!n1_r1_c3 !n1_r2_c1
!n1_r1_c3 !n1_r2_c2
!n1_r1_c3 !n1_r3_c1
!n1_r1_c3 !n1_r3_c2
!n2_r1_c3 !n3_r1_c3
!n2_r1_c3 !n4_r1_c3
!n2_r1_c3 !n5_r1_c3
!n2_r1_c3 !n6_r1_c3
!n2_r1_c3 !n7_r1_c3
!n2_r1_c3 !n8_r1_c3
!n2_r1_c3 !n9_r1_c3
!n2_r1_c3 !n2_r1_c4
!n2_r1_c3 !n2_r1_c5
!n2_r1_c3 !n2_r1_c6
!n2_r1_c3 !n2_r1_c7
!n2_r1_c3 !n2_r1_c8
!n2_r1_c3 !n2_r1_c9
!n2_r1_c3 !n2_r2_c3
!n2_r1_c3 !n2_r3_c3
!n2_r1_c3 !n2_r4_c3
!n2_r1_c3 !n2_r5_c3
!n2_r1_c3 !n2_r6_c3
!n2_r1_c3 !n2_r7_c3
!n2_r1_c3 !n2_r8_c3
!n2_r1_c3 !n2_r9_c3
!n2_r1_c3 !n2_r2_c1
!n2_r1_c3 !n2_r2_c2
!n2_r1_c3 !n2_r3_c1
!n2_r1_c3 !n2_r3_c2
!n3_r1_c3 !n4_r1_c3
!n3_r1_c3 !n5_r1_c3
!n3_r1_c3 !n6_r1_c3
!n3_r1_c3 !n7_r1_c3
!n3_r1_c3 !n8_r1_c3
!n3_r1_c3 !n9_r1_c3
!n3_r1_c3 !n3_r1_c4
!n3_r1_c3 !n3_r1_c5
!n3_r1_c3 !n3_r1_c6
!n3_r1_c3 !n3_r1_c7
!n3_r1_c3 !n3_r1_c8
!n3_r1_c3 !n3_r1_c9
!n3_r1_c3 !n3_r2_c3
!n3_r1_c3 !n3_r3_c3
!n3_r1_c3 !n3_r4_c3
!n3_r1_c3 !n3_r5_c3
!n3_r1_c3 !n3_r6_c3
!n3_r1_c3 !n3_r7_c3
!n3_r1_c3 !n3_r8_c3
!n3_r1_c3 !n3_r9_c3
!n3_r1_c3 !n3_r2_c1
!n3_r1_c3 !n3_r2_c2
!n3_r1_c3 !n3_r3_c1
!n3_r1_c3 !n3_r3_c2
!n4_r1_c3 !n5_r1_c3
!n4_r1_c3 !n6_r1_c3
!n4_r1_c3 !n7_r1_c3
!n4_r1_c3 !n8_r1_c3
!n4_r1_c3 !n9_r1_c3
!n4_r1_c3 !n4_r1_c4
!n4_r1_c3 !n4_r1_c5
!n4_r1_c3 !n4_r1_c6
!n4_r1_c3 !n4_r1_c7
!n4_r1_c3 !n4_r1_c8
!n4_r1_c3 !n4_r1_c9
!n4_r1_c3 !n4_r2_c3
!n4_r1_c3 !n4_r3_c3
!n4_r1_c3 !n4_r4_c3
!n4_r1_c3 !n4_r5_c3
!n4_r1_c3 !n4_r6_c3
!n4_r1_c3 !n4_r7_c3
!n4_r1_c3 !n4_r8_c3
!n4_r1_c3 !n4_r9_c3
!n4_r1_c3 !n4_r2_c1
!n4_r1_c3 !n4_r2_c2
!n4_r1_c3 !n4_r3_c1
!n4_r1_c3 !n4_r3_c2
!n5_r1_c3 !n6_r1_c3
!n5_r1_c3 !n7_r1_c3
!n5_r1_c3 !n8_r1_c3
!n5_r1_c3 !n9_r1_c3
!n5_r1_c3 !n5_r1_c4
!n5_r1_c3 !n5_r1_c5
!n5_r1_c3 !n5_r1_c6
!n5_r1_c3 !n5_r1_c7
!n5_r1_c3 !n5_r1_c8
!n5_r1_c3 !n5_r1_c9
!n5_r1_c3 !n5_r2_c3
!n5_r1_c3 !n5_r3_c3
!n5_r1_c3 !n5_r4_c3
!n5_r1_c3 !n5_r5_c3
!n5_r1_c3 !n5_r6_c3
!n5_r1_c3 !n5_r7_c3
!n5_r1_c3 !n5_r8_c3
!n5_r1_c3 !n5_r9_c3
!n5_r1_c3 !n5_r2_c1
!n5_r1_c3 !n5_r2_c2
!n5_r1_c3 !n5_r3_c1
!n5_r1_c3 !n5_r3_c2
!n6_r1_c3 !n7_r1_c3
!n6_r1_c3 !n8_r1_c3
!n6_r1_c3 !n9_r1_c3
!n6_r1_c3 !n6_r1_c4
!n6_r1_c3 !n6_r1_c5
!n6_r1_c3 !n6_r1_c6
!n6_r1_c3 !n6_r1_c7
!n6_r1_c3 !n6_r1_c8
!n6_r1_c3 !n6_r1_c9
!n6_r1_c3 !n6_r2_c3
!n6_r1_c3 !n6_r3_c3
!n6_r1_c3 !n6_r4_c3
!n6_r1_c3 !n6_r5_c3
!n6_r1_c3 !n6_r6_c3
!n6_r1_c3 !n6_r7_c3
!n6_r1_c3 !n6_r8_c3
!n6_r1_c3 !n6_r9_c3
!n6_r1_c3 !n6_r2_c1
!n6_r1_c3 !n6_r2_c2
!n6_r1_c3 !n6_r3_c1
!n6_r1_c3 !n6_r3_c2
!n7_r1_c3 !n8_r1_c3
!n7_r1_c3 !n9_r1_c3
!n7_r1_c3 !n7_r1_c4
!n7_r1_c3 !n7_r1_c5
!n7_r1_c3 !n7_r1_c6
!n7_r1_c3 !n7_r1_c7
!n7_r1_c3 !n7_r1_c8
!n7_r1_c3 !n7_r1_c9
!n7_r1_c3 !n7_r2_c3
!n7_r1_c3 !n7_r3_c3
!n7_r1_c3 !n7_r4_c3
!n7_r1_c3 !n7_r5_c3
!n7_r1_c3 !n7_r6_c3
!n7_r1_c3 !n7_r7_c3
!n7_r1_c3 !n7_r8_c3
!n7_r1_c3 !n7_r9_c3
!n7_r1_c3 !n7_r2_c1
!n7_r1_c3 !n7_r2_c2
!n7_r1_c3 !n7_r3_c1
!n7_r1_c3 !n7_r3_c2
!n8_r1_c3 !n9_r1_c3
!n8_r1_c3 !n8_r1_c4
!n8_r1_c3 !n8_r1_c5
!n8_r1_c3 !n8_r1_c6
!n8_r1_c3 !n8_r1_c7
!n8_r1_c3 !n8_r1_c8
!n8_r1_c3 !n8_r1_c9
!n8_r1_c3 !n8_r2_c3
!n8_r1_c3 !n8_r3_c3
!n8_r1_c3 !n8_r4_c3
!n8_r1_c3 !n8_r5_c3
!n8_r1_c3 !n8_r6_c3
!n8_r1_c3 !n8_r7_c3
!n8_r1_c3 !n8_r8_c3
!n8_r1_c3 !n8_r9_c3
!n8_r1_c3 !n8_r2_c1
!n8_r1_c3 !n8_r2_c2
!n8_r1_c3 !n8_r3_c1
!n8_r1_c3 !n8_r3_c2
!n9_r1_c3 !n9_r1_c4
!n9_r1_c3 !n9_r1_c5
!n9_r1_c3 !n9_r1_c6
!n9_r1_c3 !n9_r1_c7
!n9_r1_c3 !n9_r1_c8
!n9_r1_c3 !n9_r1_c9
!n9_r1_c3 !n9_r2_c3
!n9_r1_c3 !n9_r3_c3
!n9_r1_c3 !n9_r4_c3
!n9_r1_c3 !n9_r5_c3
!n9_r1_c3 !n9_r6_c3
!n9_r1_c3 !n9_r7_c3
!n9_r1_c3 !n9_r8_c3
!n9_r1_c3 !n9_r9_c3
!n9_r1_c3 !n9_r2_c1
!n9_r1_c3 !n9_r2_c2
!n9_r1_c3 !n9_r3_c1
!n9_r1_c3 !n9_r3_c2
n1_r1_c3 n2_r1_c3 n3_r1_c3 n4_r1_c3 n5_r1_c3 n6_r1_c3 n7_r1_c3 n8_r1_c3 n9_r1_c3
!n1_r1_c4 !n2_r1_c4
!n1_r1_c4 !n3_r1_c4
!n1_r1_c4 !n4_r1_c4
!n1_r1_c4 !n5_r1_c4
!n1_r1_c4 !n6_r1_c4
!n1_r1_c4 !n7_r1_c4
!n1_r1_c4 !n8_r1_c4
!n1_r1_c4 !n9_r1_c4
!n1_r1_c4 !n1_r1_c5
!n1_r1_c4 !n1_r1_c6
!n1_r1_c4 !n1_r1_c7
!n1_r1_c4 !n1_r1_c8
!n1_r1_c4 !n1_r1_c9
!n1_r1_c4 !n1_r2_c4
!n1_r1_c4 !n1_r3_c4
!n1_r1_c4 !n1_r4_c4
!n1_r1_c4 !n1_r5_c4
!n1_r1_c4 !n1_r6_c4
!n1_r1_c4 !n1_r7_c4
!n1_r1_c4 !n1_r8_c4
!n1_r1_c4 !n1_r9_c4
!n1_r1_c4 !n1_r2_c5
!n1_r1_c4 !n1_r2_c6
!n1_r1_c4 !n1_r3_c5
!n1_r1_c4 !n1_r3_c6
!n2_r1_c4 !n3_r1_c4
!n2_r1_c4 !n4_r1_c4
!n2_r1_c4 !n5_r1_c4
!n2_r1_c4 !n6_r1_c4
!n2_r1_c4 !n7_r1_c4
!n2_r1_c4 !n8_r1_c4
!n2_r1_c4 !n9_r1_c4
!n2_r1_c4 !n2_r1_c5
!n2_r1_c4 !n2_r1_c6
!n2_r1_c4 !n2_r1_c7
!n2_r1_c4 !n2_r1_c8
!n2_r1_c4 !n2_r1_c9
!n2_r1_c4 !n2_r2_c4
!n2_r1_c4 !n2_r3_c4
!n2_r1_c4 !n2_r4_c4
!n2_r1_c4 !n2_r5_c4
!n2_r1_c4 !n2_r6_c4
!n2_r1_c4 !n2_r7_c4
!n2_r1_c4 !n2_r8_c4
!n2_r1_c4 !n2_r9_c4
!n2_r1_c4 !n2_r2_c5
!n2_r1_c4 !n2_r2_c6
!n2_r1_c4 !n2_r3_c5
!n2_r1_c4 !n2_r3_c6
!n3_r1_c4 !n4_r1_c4
!n3_r1_c4 !n5_r1_c4
!n3_r1_c4 !n6_r1_c4
!n3_r1_c4 !n7_r1_c4
!n3_r1_c4 !n8_r1_c4
!n3_r1_c4 !n9_r1_c4
!n3_r1_c4 !n3_r1_c5
!n3_r1_c4 !n3_r1_c6
!n3_r1_c4 !n3_r1_c7
!n3_r1_c4 !n3_r1_c8
!n3_r1_c4 !n3_r1_c9
!n3_r1_c4 !n3_r2_c4
!n3_r1_c4 !n3_r3_c4
!n3_r1_c4 !n3_r4_c4
!n3_r1_c4 !n3_r5_c4
!n3_r1_c4 !n3_r6_c4
!n3_r1_c4 !n3_r7_c4
!n3_r1_c4 !n3_r8_c4
!n3_r1_c4 !n3_r9_c4
!n3_r1_c4 !n3_r2_c5
!n3_r1_c4 !n3_r2_c6
!n3_r1_c4 !n3_r3_c5
!n3_r1_c4 !n3_r3_c6
!n4_r1_c4 !n5_r1_c4
!n4_r1_c4 !n6_r1_c4
!n4_r1_c4 !n7_r1_c4
!n4_r1_c4 !n8_r1_c4
!n4_r1_c4 !n9_r1_c4
!n4_r1_c4 !n4_r1_c5
!n4_r1_c4 !n4_r1_c6
!n4_r1_c4 !n4_r1_c7
!n4_r1_c4 !n4_r1_c8
!n4_r1_c4 !n4_r1_c9
!n4_r1_c4 !n4_r2_c4
!n4_r1_c4 !n4_r3_c4
!n4_r1_c4 !n4_r4_c4
!n4_r1_c4 !n4_r5_c4
!n4_r1_c4 !n4_r6_c4
!n4_r1_c4 !n4_r7_c4
!n4_r1_c4 !n4_r8_c4
!n4_r1_c4 !n4_r9_c4
!n4_r1_c4 !n4_r2_c5
!n4_r1_c4 !n4_r2_c6
!n4_r1_c4 !n4_r3_c5
!n4_r1_c4 !n4_r3_c6
!n5_r1_c4 !n6_r1_c4
!n5_r1_c4 !n7_r1_c4
!n5_r1_c4 !n8_r1_c4
!n5_r1_c4 !n9_r1_c4
!n5_r1_c4 !n5_r1_c5
!n5_r1_c4 !n5_r1_c6
!n5_r1_c4 !n5_r1_c7
!n5_r1_c4 !n5_r1_c8
!n5_r1_c4 !n5_r1_c9
!n5_r1_c4 !n5_r2_c4
!n5_r1_c4 !n5_r3_c4
!n5_r1_c4 !n5_r4_c4
!n5_r1_c4 !n5_r5_c4
!n5_r1_c4 !n5_r6_c4
!n5_r1_c4 !n5_r7_c4
!n5_r1_c4 !n5_r8_c4
!n5_r1_c4 !n5_r9_c4
!n5_r1_c4 !n5_r2_c5
!n5_r1_c4 !n5_r2_c6
!n5_r1_c4 !n5_r3_c5
!n5_r1_c4 !n5_r3_c6
!n6_r1_c4 !n7_r1_c4
!n6_r1_c4 !n8_r1_c4
!n6_r1_c4 !n9_r1_c4
!n6_r1_c4 !n6_r1_c5
!n6_r1_c4 !n6_r1_c6
!n6_r1_c4 !n6_r1_c7
!n6_r1_c4 !n6_r1_c8
!n6_r1_c4 !n6_r1_c9
!n6_r1_c4 !n6_r2_c4
!n6_r1_c4 !n6_r3_c4
!n6_r1_c4 !n6_r4_c4
!n6_r1_c4 !n6_r5_c4
!n6_r1_c4 !n6_r6_c4
!n6_r1_c4 !n6_r7_c4
!n6_r1_c4 !n6_r8_c4
!n6_r1_c4 !n6_r9_c4
!n6_r1_c4 !n6_r2_c5
!n6_r1_c4 !n6_r2_c6
!n6_r1_c4 !n6_r3_c5
!n6_r1_c4 !n6_r3_c6
!n7_r1_c4 !n8_r1_c4
!n7_r1_c4 !n9_r1_c4
!n7_r1_c4 !n7_r1_c5
!n7_r1_c4 !n7_r1_c6
!n7_r1_c4 !n7_r1_c7
!n7_r1_c4 !n7_r1_c8
!n7_r1_c4 !n7_r1_c9
!n7_r1_c4 !n7_r2_c4
!n7_r1_c4 !n7_r3_c4
!n7_r1_c4 !n7_r4_c4
!n7_r1_c4 !n7_r5_c4
!n7_r1_c4 !n7_r6_c4
!n7_r1_c4 !n7_r7_c4
!n7_r1_c4 !n7_r8_c4
!n7_r1_c4 !n7_r9_c4
!n7_r1_c4 !n7_r2_c5
!n7_r1_c4 !n7_r2_c6
!n7_r1_c4 !n7_r3_c5
!n7_r1_c4 !n7_r3_c6
!n8_r1_c4 !n9_r1_c4
!n8_r1_c4 !n8_r1_c5
!n8_r1_c4 !n8_r1_c6
!n8_r1_c4 !n8_r1_c7
!n8_r1_c4 !n8_r1_c8
!n8_r1_c4 !n8_r1_c9
!n8_r1_c4 !n8_r2_c4
!n8_r1_c4 !n8_r3_c4
!n8_r1_c4 !n8_r4_c4
!n8_r1_c4 !n8_r5_c4
!n8_r1_c4 !n8_r6_c4
!n8_r1_c4 !n8_r7_c4
!n8_r1_c4 !n8_r8_c4
!n8_r1_c4 !n8_r9_c4
!n8_r1_c4 !n8_r2_c5
!n8_r1_c4 !n8_r2_c6
!n8_r1_c4 !n8_r3_c5
!n8_r1_c4 !n8_r3_c6
!n9_r1_c4 !n9_r1_c5
!n9_r1_c4 !n9_r1_c6
!n9_r1_c4 !n9_r1_c7
!n9_r1_c4 !n9_r1_c8
!n9_r1_c4 !n9_r1_c9
!n9_r1_c4 !n9_r2_c4
!n9_r1_c4 !n9_r3_c4
!n9_r1_c4 !n9_r4_c4
!n9_r1_c4 !n9_r5_c4
!n9_r1_c4 !n9_r6_c4
!n9_r1_c4 !n9_r7_c4
!n9_r1_c4 !n9_r8_c4
!n9_r1_c4 !n9_r9_c4
!n9_r1_c4 !n9_r2_c5
!n9_r1_c4 !n9_r2_c6
!n9_r1_c4 !n9_r3_c5
!n9_r1_c4 !n9_r3_c6
n1_r1_c4 n2_r1_c4 n3_r1_c4 n4_r1_c4 n5_r1_c4 n6_r1_c4 n7_r1_c4 n8_r1_c4 n9_r1_c4
!n1_r1_c5 !n2_r1_c5
!n1_r1_c5 !n3_r1_c5
!n1_r1_c5 !n4_r1_c5
!n1_r1_c5 !n5_r1_c5
!n1_r1_c5 !n6_r1_c5
!n1_r1_c5 !n7_r1_c5
!n1_r1_c5 !n8_r1_c5
!n1_r1_c5 !n9_r1_c5
!n1_r1_c5 !n1_r1_c6
!n1_r1_c5 !n1_r1_c7
!n1_r1_c5 !n1_r1_c8
!n1_r1_c5 !n1_r1_c9
!n1_r1_c5 !n1_r2_c5
!n1_r1_c5 !n1_r3_c5
!n1_r1_c5 !n1_r4_c5
!n1_r1_c5 !n1_r5_c5
!n1_r1_c5 !n1_r6_c5
!n1_r1_c5 !n1_r7_c5
!n1_r1_c5 !n1_r8_c5
!n1_r1_c5 !n1_r9_c5
!n1_r1_c5 !n1_r2_c4
!n1_r1_c5 !n1_r2_c6
!n1_r1_c5 !n1_r3_c4
!n1_r1_c5 !n1_r3_c6
!n2_r1_c5 !n3_r1_c5
!n2_r1_c5 !n4_r1_c5
!n2_r1_c5 !n5_r1_c5
!n2_r1_c5 !n6_r1_c5
!n2_r1_c5 !n7_r1_c5
!n2_r1_c5 !n8_r1_c5
!n2_r1_c5 !n9_r1_c5
!n2_r1_c5 !n2_r1_c6
!n2_r1_c5 !n2_r1_c7
!n2_r1_c5 !n2_r1_c8
!n2_r1_c5 !n2_r1_c9
!n2_r1_c5 !n2_r2_c5
!n2_r1_c5 !n2_r3_c5
!n2_r1_c5 !n2_r4_c5
!n2_r1_c5 !n2_r5_c5
!n2_r1_c5 !n2_r6_c5
!n2_r1_c5 !n2_r7_c5
!n2_r1_c5 !n2_r8_c5
!n2_r1_c5 !n2_r9_c5
!n2_r1_c5 !n2_r2_c4
!n2_r1_c5 !n2_r2_c6
!n2_r1_c5 !n2_r3_c4
!n2_r1_c5 !n2_r3_c6
!n3_r1_c5 !n4_r1_c5
!n3_r1_c5 !n5_r1_c5
!n3_r1_c5 !n6_r1_c5
!n3_r1_c5 !n7_r1_c5
!n3_r1_c5 !n8_r1_c5
!n3_r1_c5 !n9_r1_c5
!n3_r1_c5 !n3_r1_c6
!n3_r1_c5 !n3_r1_c7
!n3_r1_c5 !n3_r1_c8
!n3_r1_c5 !n3_r1_c9
!n3_r1_c5 !n3_r2_c5
!n3_r1_c5 !n3_r3_c5
!n3_r1_c5 !n3_r4_c5
!n3_r1_c5 !n3_r5_c5
!n3_r1_c5 !n3_r6_c5
!n3_r1_c5 !n3_r7_c5
!n3_r1_c5 !n3_r8_c5
!n3_r1_c5 !n3_r9_c5
!n3_r1_c5 !n3_r2_c4
!n3_r1_c5 !n3_r2_c6
!n3_r1_c5 !n3_r3_c4
!n3_r1_c5 !n3_r3_c6
!n4_r1_c5 !n5_r1_c5
!n4_r1_c5 !n6_r1_c5
!n4_r1_c5 !n7_r1_c5
!n4_r1_c5 !n8_r1_c5
!n4_r1_c5 !n9_r1_c5
!n4_r1_c5 !n4_r1_c6
!n4_r1_c5 !n4_r1_c7
!n4_r1_c5 !n4_r1_c8
!n4_r1_c5 !n4_r1_c9
!n4_r1_c5 !n4_r2_c5
!n4_r1_c5 !n4_r3_c5
!n4_r1_c5 !n4_r4_c5
!n4_r1_c5 !n4_r5_c5
!n4_r1_c5 !n4_r6_c5
!n4_r1_c5 !n4_r7_c5
!n4_r1_c5 !n4_r8_c5
!n4_r1_c5 !n4_r9_c5
!n4_r1_c5 !n4_r2_c4
!n4_r1_c5 !n4_r2_c6
!n4_r1_c5 !n4_r3_c4
!n4_r1_c5 !n4_r3_c6
!n5_r1_c5 !n6_r1_c5
!n5_r1_c5 !n7_r1_c5
!n5_r1_c5 !n8_r1_c5
!n5_r1_c5 !n9_r1_c5
!n5_r1_c5 !n5_r1_c6
!n5_r1_c5 !n5_r1_c7
!n5_r1_c5 !n5_r1_c8
!n5_r1_c5 !n5_r1_c9
!n5_r1_c5 !n5_r2_c5
!n5_r1_c5 !n5_r3_c5
!n5_r1_c5 !n5_r4_c5
!n5_r1_c5 !n5_r5_c5
!n5_r1_c5 !n5_r6_c5
!n5_r1_c5 !n5_r7_c5
!n5_r1_c5 !n5_r8_c5
!n5_r1_c5 !n5_r9_c5
!n5_r1_c5 !n5_r2_c4
!n5_r1_c5 !n5_r2_c6
!n5_r1_c5 !n5_r3_c4
!n5_r1_c5 !n5_r3_c6
!n6_r1_c5 !n7_r1_c5
!n6_r1_c5 !n8_r1_c5
!n6_r1_c5 !n9_r1_c5
!n6_r1_c5 !n6_r1_c6
!n6_r1_c5 !n6_r1_c7
!n6_r1_c5 !n6_r1_c8
!n6_r1_c5 !n6_r1_c9
!n6_r1_c5 !n6_r2_c5
!n6_r1_c5 !n6_r3_c5
!n6_r1_c5 !n6_r4_c5
!n6_r1_c5 !n6_r5_c5
!n6_r1_c5 !n6_r6_c5
!n6_r1_c5 !n6_r7_c5
!n6_r1_c5 !n6_r8_c5
!n6_r1_c5 !n6_r9_c5
!n6_r1_c5 !n6_r2_c4
!n6_r1_c5 !n6_r2_c6
!n6_r1_c5 !n6_r3_c4
!n6_r1_c5 !n6_r3_c6
!n7_r1_c5 !n8_r1_c5
!n7_r1_c5 !n9_r1_c5
!n7_r1_c5 !n7_r1_c6
!n7_r1_c5 !n7_r1_c7
!n7_r1_c5 !n7_r1_c8
!n7_r1_c5 !n7_r1_c9
!n7_r1_c5 !n7_r2_c5
!n7_r1_c5 !n7_r3_c5
!n7_r1_c5 !n7_r4_c5
!n7_r1_c5 !n7_r5_c5
!n7_r1_c5 !n7_r6_c5
!n7_r1_c5 !n7_r7_c5
!n7_r1_c5 !n7_r8_c5
!n7_r1_c5 !n7_r9_c5
!n7_r1_c5 !n7_r2_c4
!n7_r1_c5 !n7_r2_c6
!n7_r1_c5 !n7_r3_c4
!n7_r1_c5 !n7_r3_c6
!n8_r1_c5 !n9_r1_c5
!n8_r1_c5 !n8_r1_c6
!n8_r1_c5 !n8_r1_c7
!n8_r1_c5 !n8_r1_c8
!n8_r1_c5 !n8_r1_c9
!n8_r1_c5 !n8_r2_c5
!n8_r1_c5 !n8_r3_c5
!n8_r1_c5 !n8_r4_c5
!n8_r1_c5 !n8_r5_c5
!n8_r1_c5 !n8_r6_c5
!n8_r1_c5 !n8_r7_c5
!n8_r1_c5 !n8_r8_c5
!n8_r1_c5 !n8_r9_c5
!n8_r1_c5 !n8_r2_c4
!n8_r1_c5 !n8_r2_c6
!n8_r1_c5 !n8_r3_c4
!n8_r1_c5 !n8_r3_c6
!n9_r1_c5 !n9_r1_c6
!n9_r1_c5 !n9_r1_c7
!n9_r1_c5 !n9_r1_c8
!n9_r1_c5 !n9_r1_c9
!n9_r1_c5 !n9_r2_c5
!n9_r1_c5 !n9_r3_c5
!n9_r1_c5 !n9_r4_c5
!n9_r1_c5 !n9_r5_c5
!n9_r1_c5 !n9_r6_c5
!n9_r1_c5 !n9_r7_c5
!n9_r1_c5 !n9_r8_c5
!n9_r1_c5 !n9_r9_c5
!n9_r1_c5 !n9_r2_c4
!n9_r1_c5 !n9_r2_c6
!n9_r1_c5 !n9_r3_c4
!n9_r1_c5 !n9_r3_c6
n1_r1_c5 n2_r1_c5 n3_r1_c5 n4_r1_c5 n5_r1_c5 n6_r1_c5 n7_r1_c5 n8_r1_c5 n9_r1_c5
!n1_r1_c6 !n2_r1_c6
!n1_r1_c6 !n3_r1_c6
!n1_r1_c6 !n4_r1_c6
!n1_r1_c6 !n5_r1_c6
!n1_r1_c6 !n6_r1_c6
!n1_r1_c6 !n7_r1_c6
!n1_r1_c6 !n8_r1_c6
!n1_r1_c6 !n9_r1_c6
!n1_r1_c6 !n1_r1_c7
!n1_r1_c6 !n1_r1_c8
!n1_r1_c6 !n1_r1_c9
!n1_r1_c6 !n1_r2_c6
!n1_r1_c6 !n1_r3_c6
!n1_r1_c6 !n1_r4_c6
!n1_r1_c6 !n1_r5_c6
!n1_r1_c6 !n1_r6_c6
!n1_r1_c6 !n1_r7_c6
!n1_r1_c6 !n1_r8_c6
!n1_r1_c6 !n1_r9_c6
!n1_r1_c6 !n1_r2_c4
!n1_r1_c6 !n1_r2_c5
!n1_r1_c6 !n1_r3_c4
!n1_r1_c6 !n1_r3_c5
!n2_r1_c6 !n3_r1_c6
!n2_r1_c6 !n4_r1_c6
!n2_r1_c6 !n5_r1_c6
!n2_r1_c6 !n6_r1_c6
!n2_r1_c6 !n7_r1_c6
!n2_r1_c6 !n8_r1_c6
!n2_r1_c6 !n9_r1_c6
!n2_r1_c6 !n2_r1_c7
!n2_r1_c6 !n2_r1_c8
!n2_r1_c6 !n2_r1_c9
!n2_r1_c6 !n2_r2_c6
!n2_r1_c6 !n2_r3_c6
!n2_r1_c6 !n2_r4_c6
!n2_r1_c6 !n2_r5_c6
!n2_r1_c6 !n2_r6_c6
!n2_r1_c6 !n2_r7_c6
!n2_r1_c6 !n2_r8_c6
!n2_r1_c6 !n2_r9_c6
!n2_r1_c6 !n2_r2_c4
!n2_r1_c6 !n2_r2_c5
!n2_r1_c6 !n2_r3_c4
!n2_r1_c6 !n2_r3_c5
!n3_r1_c6 !n4_r1_c6
!n3_r1_c6 !n5_r1_c6
!n3_r1_c6 !n6_r1_c6
!n3_r1_c6 !n7_r1_c6
!n3_r1_c6 !n8_r1_c6
!n3_r1_c6 !n9_r1_c6
!n3_r1_c6 !n3_r1_c7
!n3_r1_c6 !n3_r1_c8
!n3_r1_c6 !n3_r1_c9
!n3_r1_c6 !n3_r2_c6
!n3_r1_c6 !n3_r3_c6
!n3_r1_c6 !n3_r4_c6
!n3_r1_c6 !n3_r5_c6
!n3_r1_c6 !n3_r6_c6
!n3_r1_c6 !n3_r7_c6
!n3_r1_c6 !n3_r8_c6
!n3_r1_c6 !n3_r9_c6
!n3_r1_c6 !n3_r2_c4
!n3_r1_c6 !n3_r2_c5
!n3_r1_c6 !n3_r3_c4
!n3_r1_c6 !n3_r3_c5
!n4_r1_c6 !n5_r1_c6
!n4_r1_c6 !n6_r1_c6
!n4_r1_c6 !n7_r1_c6
!n4_r1_c6 !n8_r1_c6
!n4_r1_c6 !n9_r1_c6
!n4_r1_c6 !n4_r1_c7
!n4_r1_c6 !n4_r1_c8
!n4_r1_c6 !n4_r1_c9
!n4_r1_c6 !n4_r2_c6
!n4_r1_c6 !n4_r3_c6
!n4_r1_c6 !n4_r4_c6
!n4_r1_c6 !n4_r5_c6
!n4_r1_c6 !n4_r6_c6
!n4_r1_c6 !n4_r7_c6
!n4_r1_c6 !n4_r8_c6
!n4_r1_c6 !n4_r9_c6
!n4_r1_c6 !n4_r2_c4
!n4_r1_c6 !n4_r2_c5
!n4_r1_c6 !n4_r3_c4
!n4_r1_c6 !n4_r3_c5
!n5_r1_c6 !n6_r1_c6
!n5_r1_c6 !n7_r1_c6
!n5_r1_c6 !n8_r1_c6
!n5_r1_c6 !n9_r1_c6
!n5_r1_c6 !n5_r1_c7
!n5_r1_c6 !n5_r1_c8
!n5_r1_c6 !n5_r1_c9
!n5_r1_c6 !n5_r2_c6
!n5_r1_c6 !n5_r3_c6
!n5_r1_c6 !n5_r4_c6
!n5_r1_c6 !n5_r5_c6
!n5_r1_c6 !n5_r6_c6
!n5_r1_c6 !n5_r7_c6
!n5_r1_c6 !n5_r8_c6
!n5_r1_c6 !n5_r9_c6
!n5_r1_c6 !n5_r2_c4
!n5_r1_c6 !n5_r2_c5
!n5_r1_c6 !n5_r3_c4
!n5_r1_c6 !n5_r3_c5
!n6_r1_c6 !n7_r1_c6
!n6_r1_c6 !n8_r1_c6
!n6_r1_c6 !n9_r1_c6
!n6_r1_c6 !n6_r1_c7
!n6_r1_c6 !n6_r1_c8
!n6_r1_c6 !n6_r1_c9
!n6_r1_c6 !n6_r2_c6
!n6_r1_c6 !n6_r3_c6
!n6_r1_c6 !n6_r4_c6
!n6_r1_c6 !n6_r5_c6
!n6_r1_c6 !n6_r6_c6
!n6_r1_c6 !n6_r7_c6
!n6_r1_c6 !n6_r8_c6
!n6_r1_c6 !n6_r9_c6
!n6_r1_c6 !n6_r2_c4
!n6_r1_c6 !n6_r2_c5
!n6_r1_c6 !n6_r3_c4
!n6_r1_c6 !n6_r3_c5
!n7_r1_c6 !n8_r1_c6
!n7_r1_c6 !n9_r1_c6
!n7_r1_c6 !n7_r1_c7
!n7_r1_c6 !n7_r1_c8
!n7_r1_c6 !n7_r1_c9
!n7_r1_c6 !n7_r2_c6
!n7_r1_c6 !n7_r3_c6
!n7_r1_c6 !n7_r4_c6
!n7_r1_c6 !n7_r5_c6
!n7_r1_c6 !n7_r6_c6
!n7_r1_c6 !n7_r7_c6
!n7_r1_c6 !n7_r8_c6
!n7_r1_c6 !n7_r9_c6
!n7_r1_c6 !n7_r2_c4
!n7_r1_c6 !n7_r2_c5
!n7_r1_c6 !n7_r3_c4
!n7_r1_c6 !n7_r3_c5
!n8_r1_c6 !n9_r1_c6
!n8_r1_c6 !n8_r1_c7
!n8_r1_c6 !n8_r1_c8
!n8_r1_c6 !n8_r1_c9
!n8_r1_c6 !n8_r2_c6
!n8_r1_c6 !n8_r3_c6
!n8_r1_c6 !n8_r4_c6
!n8_r1_c6 !n8_r5_c6
!n8_r1_c6 !n8_r6_c6
!n8_r1_c6 !n8_r7_c6
!n8_r1_c6 !n8_r8_c6
!n8_r1_c6 !n8_r9_c6
!n8_r1_c6 !n8_r2_c4
!n8_r1_c6 !n8_r2_c5
!n8_r1_c6 !n8_r3_c4
!n8_r1_c6 !n8_r3_c5
!n9_r1_c6 !n9_r1_c7
!n9_r1_c6 !n9_r1_c8
!n9_r1_c6 !n9_r1_c9
!n9_r1_c6 !n9_r2_c6
!n9_r1_c6 !n9_r3_c6
!n9_r1_c6 !n9_r4_c6
!n9_r1_c6 !n9_r5_c6
!n9_r1_c6 !n9_r6_c6
!n9_r1_c6 !n9_r7_c6
!n9_r1_c6 !n9_r8_c6
!n9_r1_c6 !n9_r9_c6
!n9_r1_c6 !n9_r2_c4
!n9_r1_c6 !n9_r2_c5
!n9_r1_c6 !n9_r3_c4
!n9_r1_c6 !n9_r3_c5
n1_r1_c6 n2_r1_c6 n3_r1_c6 n4_r1_c6 n5_r1_c6 n6_r1_c6 n7_r1_c6 n8_r1_c6 n9_r1_c6
!n1_r1_c7 !n2_r1_c7
!n1_r1_c7 !n3_r1_c7
!n1_r1_c7 !n4_r1_c7
!n1_r1_c7 !n5_r1_c7
!n1_r1_c7 !n6_r1_c7
!n1_r1_c7 !n7_r1_c7
!n1_r1_c7 !n8_r1_c7
!n1_r1_c7 !n9_r1_c7
!n1_r1_c7 !n1_r1_c8
!n1_r1_c7 !n1_r1_c9
!n1_r1_c7 !n1_r2_c7
!n1_r1_c7 !n1_r3_c7
!n1_r1_c7 !n1_r4_c7
!n1_r1_c7 !n1_r5_c7
!n1_r1_c7 !n1_r6_c7
!n1_r1_c7 !n1_r7_c7
!n1_r1_c7 !n1_r8_c7
!n1_r1_c7 !n1_r9_c7
!n1_r1_c7 !n1_r2_c8
!n1_r1_c7 !n1_r2_c9
!n1_r1_c7 !n1_r3_c8
!n1_r1_c7 !n1_r3_c9
!n2_r1_c7 !n3_r1_c7
!n2_r1_c7 !n4_r1_c7
!n2_r1_c7 !n5_r1_c7
!n2_r1_c7 !n6_r1_c7
!n2_r1_c7 !n7_r1_c7
!n2_r1_c7 !n8_r1_c7
!n2_r1_c7 !n9_r1_c7
!n2_r1_c7 !n2_r1_c8
!n2_r1_c7 !n2_r1_c9
!n2_r1_c7 !n2_r2_c7
!n2_r1_c7 !n2_r3_c7
!n2_r1_c7 !n2_r4_c7
!n2_r1_c7 !n2_r5_c7
!n2_r1_c7 !n2_r6_c7
!n2_r1_c7 !n2_r7_c7
!n2_r1_c7 !n2_r8_c7
!n2_r1_c7 !n2_r9_c7
!n2_r1_c7 !n2_r2_c8
!n2_r1_c7 !n2_r2_c9
!n2_r1_c7 !n2_r3_c8
!n2_r1_c7 !n2_r3_c9
!n3_r1_c7 !n4_r1_c7
!n3_r1_c7 !n5_r1_c7
!n3_r1_c7 !n6_r1_c7
!n3_r1_c7 !n7_r1_c7
!n3_r1_c7 !n8_r1_c7
!n3_r1_c7 !n9_r1_c7
!n3_r1_c7 !n3_r1_c8
!n3_r1_c7 !n3_r1_c9
!n3_r1_c7 !n3_r2_c7
!n3_r1_c7 !n3_r3_c7
!n3_r1_c7 !n3_r4_c7
!n3_r1_c7 !n3_r5_c7
!n3_r1_c7 !n3_r6_c7
!n3_r1_c7 !n3_r7_c7
!n3_r1_c7 !n3_r8_c7
!n3_r1_c7 !n3_r9_c7
!n3_r1_c7 !n3_r2_c8
!n3_r1_c7 !n3_r2_c9
!n3_r1_c7 !n3_r3_c8
!n3_r1_c7 !n3_r3_c9
!n4_r1_c7 !n5_r1_c7
!n4_r1_c7 !n6_r1_c7
!n4_r1_c7 !n7_r1_c7
!n4_r1_c7 !n8_r1_c7
!n4_r1_c7 !n9_r1_c7
!n4_r1_c7 !n4_r1_c8
!n4_r1_c7 !n4_r1_c9
!n4_r1_c7 !n4_r2_c7
!n4_r1_c7 !n4_r3_c7
!n4_r1_c7 !n4_r4_c7
!n4_r1_c7 !n4_r5_c7
!n4_r1_c7 !n4_r6_c7
!n4_r1_c7 !n4_r7_c7
!n4_r1_c7 !n4_r8_c7
!n4_r1_c7 !n4_r9_c7
!n4_r1_c7 !n4_r2_c8
!n4_r1_c7 !n4_r2_c9
!n4_r1_c7 !n4_r3_c8
!n4_r1_c7 !n4_r3_c9
!n5_r1_c7 !n6_r1_c7
!n5_r1_c7 !n7_r1_c7
!n5_r1_c7 !n8_r1_c7
!n5_r1_c7 !n9_r1_c7
!n5_r1_c7 !n5_r1_c8
!n5_r1_c7 !n5_r1_c9
!n5_r1_c7 !n5_r2_c7
!n5_r1_c7 !n5_r3_c7
!n5_r1_c7 !n5_r4_c7
!n5_r1_c7 !n5_r5_c7
!n5_r1_c7 !n5_r6_c7
!n5_r1_c7 !n5_r7_c7
!n5_r1_c7 !n5_r8_c7
!n5_r1_c7 !n5_r9_c7
!n5_r1_c7 !n5_r2_c8
!n5_r1_c7 !n5_r2_c9
!n5_r1_c7 !n5_r3_c8
!n5_r1_c7 !n5_r3_c9
!n6_r1_c7 !n7_r1_c7
!n6_r1_c7 !n8_r1_c7
!n6_r1_c7 !n9_r1_c7
!n6_r1_c7 !n6_r1_c8
!n6_r1_c7 !n6_r1_c9
!n6_r1_c7 !n6_r2_c7
!n6_r1_c7 !n6_r3_c7
!n6_r1_c7 !n6_r4_c7
!n6_r1_c7 !n6_r5_c7
!n6_r1_c7 !n6_r6_c7
!n6_r1_c7 !n6_r7_c7
!n6_r1_c7 !n6_r8_c7
!n6_r1_c7 !n6_r9_c7
!n6_r1_c7 !n6_r2_c8
!n6_r1_c7 !n6_r2_c9
!n6_r1_c7 !n6_r3_c8
!n6_r1_c7 !n6_r3_c9
!n7_r1_c7 !n8_r1_c7
!n7_r1_c7 !n9_r1_c7
!n7_r1_c7 !n7_r1_c8
!n7_r1_c7 !n7_r1_c9
!n7_r1_c7 !n7_r2_c7
!n7_r1_c7 !n7_r3_c7
!n7_r1_c7 !n7_r4_c7
!n7_r1_c7 !n7_r5_c7
!n7_r1_c7 !n7_r6_c7
!n7_r1_c7 !n7_r7_c7
!n7_r1_c7 !n7_r8_c7
!n7_r1_c7 !n7_r9_c7
!n7_r1_c7 !n7_r2_c8
!n7_r1_c7 !n7_r2_c9
!n7_r1_c7 !n7_r3_c8
!n7_r1_c7 !n7_r3_c9
!n8_r1_c7 !n9_r1_c7
!n8_r1_c7 !n8_r1_c8
!n8_r1_c7 !n8_r1_c9
!n8_r1_c7 !n8_r2_c7
!n8_r1_c7 !n8_r3_c7
!n8_r1_c7 !n8_r4_c7
!n8_r1_c7 !n8_r5_c7
!n8_r1_c7 !n8_r6_c7
!n8_r1_c7 !n8_r7_c7
!n8_r1_c7 !n8_r8_c7
!n8_r1_c7 !n8_r9_c7
!n8_r1_c7 !n8_r2_c8
!n8_r1_c7 !n8_r2_c9
!n8_r1_c7 !n8_r3_c8
!n8_r1_c7 !n8_r3_c9
!n9_r1_c7 !n9_r1_c8
!n9_r1_c7 !n9_r1_c9
!n9_r1_c7 !n9_r2_c7
!n9_r1_c7 !n9_r3_c7
!n9_r1_c7 !n9_r4_c7
!n9_r1_c7 !n9_r5_c7
!n9_r1_c7 !n9_r6_c7
!n9_r1_c7 !n9_r7_c7
!n9_r1_c7 !n9_r8_c7
!n9_r1_c7 !n9_r9_c7
!n9_r1_c7 !n9_r2_c8
!n9_r1_c7 !n9_r2_c9
!n9_r1_c7 !n9_r3_c8
!n9_r1_c7 !n9_r3_c9
n1_r1_c7 n2_r1_c7 n3_r1_c7 n4_r1_c7 n5_r1_c7 n6_r1_c7 n7_r1_c7 n8_r1_c7 n9_r1_c7
!n1_r1_c8 !n2_r1_c8
!n1_r1_c8 !n3_r1_c8
!n1_r1_c8 !n4_r1_c8
!n1_r1_c8 !n5_r1_c8
!n1_r1_c8 !n6_r1_c8
!n1_r1_c8 !n7_r1_c8
!n1_r1_c8 !n8_r1_c8
!n1_r1_c8 !n9_r1_c8
!n1_r1_c8 !n1_r1_c9
!n1_r1_c8 !n1_r2_c8
!n1_r1_c8 !n1_r3_c8
!n1_r1_c8 !n1_r4_c8
!n1_r1_c8 !n1_r5_c8
!n1_r1_c8 !n1_r6_c8
!n1_r1_c8 !n1_r7_c8
!n1_r1_c8 !n1_r8_c8
!n1_r1_c8 !n1_r9_c8
!n1_r1_c8 !n1_r2_c7
!n1_r1_c8 !n1_r2_c9
!n1_r1_c8 !n1_r3_c7
!n1_r1_c8 !n1_r3_c9
!n2_r1_c8 !n3_r1_c8
!n2_r1_c8 !n4_r1_c8
!n2_r1_c8 !n5_r1_c8
!n2_r1_c8 !n6_r1_c8
!n2_r1_c8 !n7_r1_c8
!n2_r1_c8 !n8_r1_c8
!n2_r1_c8 !n9_r1_c8
!n2_r1_c8 !n2_r1_c9
!n2_r1_c8 !n2_r2_c8
!n2_r1_c8 !n2_r3_c8
!n2_r1_c8 !n2_r4_c8
!n2_r1_c8 !n2_r5_c8
!n2_r1_c8 !n2_r6_c8
!n2_r1_c8 !n2_r7_c8
!n2_r1_c8 !n2_r8_c8
!n2_r1_c8 !n2_r9_c8
!n2_r1_c8 !n2_r2_c7
!n2_r1_c8 !n2_r2_c9
!n2_r1_c8 !n2_r3_c7
!n2_r1_c8 !n2_r3_c9
!n3_r1_c8 !n4_r1_c8
!n3_r1_c8 !n5_r1_c8
!n3_r1_c8 !n6_r1_c8
!n3_r1_c8 !n7_r1_c8
!n3_r1_c8 !n8_r1_c8
!n3_r1_c8 !n9_r1_c8
!n3_r1_c8 !n3_r1_c9
!n3_r1_c8 !n3_r2_c8
!n3_r1_c8 !n3_r3_c8
!n3_r1_c8 !n3_r4_c8
!n3_r1_c8 !n3_r5_c8
!n3_r1_c8 !n3_r6_c8
!n3_r1_c8 !n3_r7_c8
!n3_r1_c8 !n3_r8_c8
!n3_r1_c8 !n3_r9_c8
!n3_r1_c8 !n3_r2_c7
!n3_r1_c8 !n3_r2_c9
!n3_r1_c8 !n3_r3_c7
!n3_r1_c8 !n3_r3_c9
!n4_r1_c8 !n5_r1_c8
!n4_r1_c8 !n6_r1_c8
!n4_r1_c8 !n7_r1_c8
!n4_r1_c8 !n8_r1_c8
!n4_r1_c8 !n9_r1_c8
!n4_r1_c8 !n4_r1_c9
!n4_r1_c8 !n4_r2_c8
!n4_r1_c8 !n4_r3_c8
!n4_r1_c8 !n4_r4_c8
!n4_r1_c8 !n4_r5_c8
!n4_r1_c8 !n4_r6_c8
!n4_r1_c8 !n4_r7_c8
!n4_r1_c8 !n4_r8_c8
!n4_r1_c8 !n4_r9_c8
!n4_r1_c8 !n4_r2_c7
!n4_r1_c8 !n4_r2_c9
!n4_r1_c8 !n4_r3_c7
!n4_r1_c8 !n4_r3_c9
!n5_r1_c8 !n6_r1_c8
!n5_r1_c8 !n7_r1_c8
!n5_r1_c8 !n8_r1_c8
!n5_r1_c8 !n9_r1_c8
!n5_r1_c8 !n5_r1_c9
!n5_r1_c8 !n5_r2_c8
!n5_r1_c8 !n5_r3_c8
!n5_r1_c8 !n5_r4_c8
!n5_r1_c8 !n5_r5_c8
!n5_r1_c8 !n5_r6_c8
!n5_r1_c8 !n5_r7_c8
!n5_r1_c8 !n5_r8_c8
!n5_r1_c8 !n5_r9_c8
!n5_r1_c8 !n5_r2_c7
!n5_r1_c8 !n5_r2_c9
!n5_r1_c8 !n5_r3_c7
!n5_r1_c8 !n5_r3_c9
!n6_r1_c8 !n7_r1_c8
!n6_r1_c8 !n8_r1_c8
!n6_r1_c8 !n9_r1_c8
!n6_r1_c8 !n6_r1_c9
!n6_r1_c8 !n6_r2_c8
!n6_r1_c8 !n6_r3_c8
!n6_r1_c8 !n6_r4_c8
!n6_r1_c8 !n6_r5_c8
!n6_r1_c8 !n6_r6_c8
!n6_r1_c8 !n6_r7_c8
!n6_r1_c8 !n6_r8_c8
!n6_r1_c8 !n6_r9_c8
!n6_r1_c8 !n6_r2_c7
!n6_r1_c8 !n6_r2_c9
!n6_r1_c8 !n6_r3_c7
!n6_r1_c8 !n6_r3_c9
!n7_r1_c8 !n8_r1_c8
!n7_r1_c8 !n9_r1_c8
!n7_r1_c8 !n7_r1_c9
!n7_r1_c8 !n7_r2_c8
!n7_r1_c8 !n7_r3_c8
!n7_r1_c8 !n7_r4_c8
!n7_r1_c8 !n7_r5_c8
!n7_r1_c8 !n7_r6_c8
!n7_r1_c8 !n7_r7_c8
!n7_r1_c8 !n7_r8_c8
!n7_r1_c8 !n7_r9_c8
!n7_r1_c8 !n7_r2_c7
!n7_r1_c8 !n7_r2_c9
!n7_r1_c8 !n7_r3_c7
!n7_r1_c8 !n7_r3_c9
!n8_r1_c8 !n9_r1_c8
!n8_r1_c8 !n8_r1_c9
!n8_r1_c8 !n8_r2_c8
!n8_r1_c8 !n8_r3_c8
!n8_r1_c8 !n8_r4_c8
!n8_r1_c8 !n8_r5_c8
!n8_r1_c8 !n8_r6_c8
!n8_r1_c8 !n8_r7_c8
!n8_r1_c8 !n8_r8_c8
!n8_r1_c8 !n8_r9_c8
!n8_r1_c8 !n8_r2_c7
!n8_r1_c8 !n8_r2_c9
!n8_r1_c8 !n8_r3_c7
!n8_r1_c8 !n8_r3_c9
!n9_r1_c8 !n9_r1_c9
!n9_r1_c8 !n9_r2_c8
!n9_r1_c8 !n9_r3_c8
!n9_r1_c8 !n9_r4_c8
!n9_r1_c8 !n9_r5_c8
!n9_r1_c8 !n9_r6_c8
!n9_r1_c8 !n9_r7_c8
!n9_r1_c8 !n9_r8_c8
!n9_r1_c8 !n9_r9_c8
!n9_r1_c8 !n9_r2_c7
!n9_r1_c8 !n9_r2_c9
!n9_r1_c8 !n9_r3_c7
!n9_r1_c8 !n9_r3_c9
n1_r1_c8 n2_r1_c8 n3_r1_c8 n4_r1_c8 n5_r1_c8 n6_r1_c8 n7_r1_c8 n8_r1_c8 n9_r1_c8
!n1_r1_c9 !n2_r1_c9
!n1_r1_c9 !n3_r1_c9
!n1_r1_c9 !n4_r1_c9
!n1_r1_c9 !n5_r1_c9
!n1_r1_c9 !n6_r1_c9
!n1_r1_c9 !n7_r1_c9
!n1_r1_c9 !n8_r1_c9
!n1_r1_c9 !n9_r1_c9
!n1_r1_c9 !n1_r2_c9
!n1_r1_c9 !n1_r3_c9
!n1_r1_c9 !n1_r4_c9
!n1_r1_c9 !n1_r5_c9
!n1_r1_c9 !n1_r6_c9
!n1_r1_c9 !n1_r7_c9
!n1_r1_c9 !n1_r8_c9
!n1_r1_c9 !n1_r9_c9
!n1_r1_c9 !n1_r2_c7
!n1_r1_c9 !n1_r2_c8
!n1_r1_c9 !n1_r3_c7
!n1_r1_c9 !n1_r3_c8
!n2_r1_c9 !n3_r1_c9
!n2_r1_c9 !n4_r1_c9
!n2_r1_c9 !n5_r1_c9
!n2_r1_c9 !n6_r1_c9
!n2_r1_c9 !n7_r1_c9
!n2_r1_c9 !n8_r1_c9
!n2_r1_c9 !n9_r1_c9
!n2_r1_c9 !n2_r2_c9
!n2_r1_c9 !n2_r3_c9
!n2_r1_c9 !n2_r4_c9
!n2_r1_c9 !n2_r5_c9
!n2_r1_c9 !n2_r6_c9
!n2_r1_c9 !n2_r7_c9
!n2_r1_c9 !n2_r8_c9
!n2_r1_c9 !n2_r9_c9
!n2_r1_c9 !n2_r2_c7
!n2_r1_c9 !n2_r2_c8
!n2_r1_c9 !n2_r3_c7
!n2_r1_c9 !n2_r3_c8
!n3_r1_c9 !n4_r1_c9
!n3_r1_c9 !n5_r1_c9
!n3_r1_c9 !n6_r1_c9
!n3_r1_c9 !n7_r1_c9
!n3_r1_c9 !n8_r1_c9
!n3_r1_c9 !n9_r1_c9
!n3_r1_c9 !n3_r2_c9
!n3_r1_c9 !n3_r3_c9
!n3_r1_c9 !n3_r4_c9
!n3_r1_c9 !n3_r5_c9
!n3_r1_c9 !n3_r6_c9
!n3_r1_c9 !n3_r7_c9
!n3_r1_c9 !n3_r8_c9
!n3_r1_c9 !n3_r9_c9
!n3_r1_c9 !n3_r2_c7
!n3_r1_c9 !n3_r2_c8
!n3_r1_c9 !n3_r3_c7
!n3_r1_c9 !n3_r3_c8
!n4_r1_c9 !n5_r1_c9
!n4_r1_c9 !n6_r1_c9
!n4_r1_c9 !n7_r1_c9
!n4_r1_c9 !n8_r1_c9
!n4_r1_c9 !n9_r1_c9
!n4_r1_c9 !n4_r2_c9
!n4_r1_c9 !n4_r3_c9
!n4_r1_c9 !n4_r4_c9
!n4_r1_c9 !n4_r5_c9
!n4_r1_c9 !n4_r6_c9
!n4_r1_c9 !n4_r7_c9
!n4_r1_c9 !n4_r8_c9
!n4_r1_c9 !n4_r9_c9
!n4_r1_c9 !n4_r2_c7
!n4_r1_c9 !n4_r2_c8
!n4_r1_c9 !n4_r3_c7
!n4_r1_c9 !n4_r3_c8
!n5_r1_c9 !n6_r1_c9
!n5_r1_c9 !n7_r1_c9
!n5_r1_c9 !n8_r1_c9
!n5_r1_c9 !n9_r1_c9
!n5_r1_c9 !n5_r2_c9
!n5_r1_c9 !n5_r3_c9
!n5_r1_c9 !n5_r4_c9
!n5_r1_c9 !n5_r5_c9
!n5_r1_c9 !n5_r6_c9
!n5_r1_c9 !n5_r7_c9
!n5_r1_c9 !n5_r8_c9
!n5_r1_c9 !n5_r9_c9
!n5_r1_c9 !n5_r2_c7
!n5_r1_c9 !n5_r2_c8
!n5_r1_c9 !n5_r3_c7
!n5_r1_c9 !n5_r3_c8
!n6_r1_c9 !n7_r1_c9
!n6_r1_c9 !n8_r1_c9
!n6_r1_c9 !n9_r1_c9
!n6_r1_c9 !n6_r2_c9
!n6_r1_c9 !n6_r3_c9
!n6_r1_c9 !n6_r4_c9
!n6_r1_c9 !n6_r5_c9
!n6_r1_c9 !n6_r6_c9
!n6_r1_c9 !n6_r7_c9
!n6_r1_c9 !n6_r8_c9
!n6_r1_c9 !n6_r9_c9
!n6_r1_c9 !n6_r2_c7
!n6_r1_c9 !n6_r2_c8
!n6_r1_c9 !n6_r3_c7
!n6_r1_c9 !n6_r3_c8
!n7_r1_c9 !n8_r1_c9
!n7_r1_c9 !n9_r1_c9
!n7_r1_c9 !n7_r2_c9
!n7_r1_c9 !n7_r3_c9
!n7_r1_c9 !n7_r4_c9
!n7_r1_c9 !n7_r5_c9
!n7_r1_c9 !n7_r6_c9
!n7_r1_c9 !n7_r7_c9
!n7_r1_c9 !n7_r8_c9
!n7_r1_c9 !n7_r9_c9
!n7_r1_c9 !n7_r2_c7
!n7_r1_c9 !n7_r2_c8
!n7_r1_c9 !n7_r3_c7
!n7_r1_c9 !n7_r3_c8
!n8_r1_c9 !n9_r1_c9
!n8_r1_c9 !n8_r2_c9
!n8_r1_c9 !n8_r3_c9
!n8_r1_c9 !n8_r4_c9
!n8_r1_c9 !n8_r5_c9
!n8_r1_c9 !n8_r6_c9
!n8_r1_c9 !n8_r7_c9
!n8_r1_c9 !n8_r8_c9
!n8_r1_c9 !n8_r9_c9
!n8_r1_c9 !n8_r2_c7
!n8_r1_c9 !n8_r2_c8
!n8_r1_c9 !n8_r3_c7
!n8_r1_c9 !n8_r3_c8
!n9_r1_c9 !n9_r2_c9
!n9_r1_c9 !n9_r3_c9
!n9_r1_c9 !n9_r4_c9
!n9_r1_c9 !n9_r5_c9
!n9_r1_c9 !n9_r6_c9
!n9_r1_c9 !n9_r7_c9
!n9_r1_c9 !n9_r8_c9
!n9_r1_c9 !n9_r9_c9
!n9_r1_c9 !n9_r2_c7
!n9_r1_c9 !n9_r2_c8
!n9_r1_c9 !n9_r3_c7
!n9_r1_c9 !n9_r3_c8
n1_r1_c9 n2_r1_c9 n3_r1_c9 n4_r1_c9 n5_r1_c9 n6_r1_c9 n7_r1_c9 n8_r1_c9 n9_r1_c9
!n1_r2_c1 !n2_r2_c1
!n1_r2_c1 !n3_r2_c1
!n1_r2_c1 !n4_r2_c1
!n1_r2_c1 !n5_r2_c1
!n1_r2_c1 !n6_r2_c1
!n1_r2_c1 !n7_r2_c1
!n1_r2_c1 !n8_r2_c1
!n1_r2_c1 !n9_r2_c1
!n1_r2_c1 !n1_r2_c2
!n1_r2_c1 !n1_r2_c3
!n1_r2_c1 !n1_r2_c4
!n1_r2_c1 !n1_r2_c5
!n1_r2_c1 !n1_r2_c6
!n1_r2_c1 !n1_r2_c7
!n1_r2_c1 !n1_r2_c8
!n1_r2_c1 !n1_r2_c9
!n1_r2_c1 !n1_r3_c1
!n1_r2_c1 !n1_r4_c1
!n1_r2_c1 !n1_r5_c1
!n1_r2_c1 !n1_r6_c1
!n1_r2_c1 !n1_r7_c1
!n1_r2_c1 !n1_r8_c1
!n1_r2_c1 !n1_r9_c1
!n1_r2_c1 !n1_r1_c2
!n1_r2_c1 !n1_r1_c3
!n1_r2_c1 !n1_r3_c2
!n1_r2_c1 !n1_r3_c3
!n2_r2_c1 !n3_r2_c1
!n2_r2_c1 !n4_r2_c1
!n2_r2_c1 !n5_r2_c1
!n2_r2_c1 !n6_r2_c1
!n2_r2_c1 !n7_r2_c1
!n2_r2_c1 !n8_r2_c1
!n2_r2_c1 !n9_r2_c1
!n2_r2_c1 !n2_r2_c2
!n2_r2_c1 !n2_r2_c3
!n2_r2_c1 !n2_r2_c4
!n2_r2_c1 !n2_r2_c5
!n2_r2_c1 !n2_r2_c6
!n2_r2_c1 !n2_r2_c7
!n2_r2_c1 !n2_r2_c8
!n2_r2_c1 !n2_r2_c9
!n2_r2_c1 !n2_r3_c1
!n2_r2_c1 !n2_r4_c1
!n2_r2_c1 !n2_r5_c1
!n2_r2_c1 !n2_r6_c1
!n2_r2_c1 !n2_r7_c1
!n2_r2_c1 !n2_r8_c1
!n2_r2_c1 !n2_r9_c1
!n2_r2_c1 !n2_r1_c2
!n2_r2_c1 !n2_r1_c3
!n2_r2_c1 !n2_r3_c2
!n2_r2_c1 !n2_r3_c3
!n3_r2_c1 !n4_r2_c1
!n3_r2_c1 !n5_r2_c1
!n3_r2_c1 !n6_r2_c1
!n3_r2_c1 !n7_r2_c1
!n3_r2_c1 !n8_r2_c1
!n3_r2_c1 !n9_r2_c1
!n3_r2_c1 !n3_r2_c2
!n3_r2_c1 !n3_r2_c3
!n3_r2_c1 !n3_r2_c4
!n3_r2_c1 !n3_r2_c5
!n3_r2_c1 !n3_r2_c6
!n3_r2_c1 !n3_r2_c7
!n3_r2_c1 !n3_r2_c8
!n3_r2_c1 !n3_r2_c9
!n3_r2_c1 !n3_r3_c1
!n3_r2_c1 !n3_r4_c1
!n3_r2_c1 !n3_r5_c1
!n3_r2_c1 !n3_r6_c1
!n3_r2_c1 !n3_r7_c1
!n3_r2_c1 !n3_r8_c1
!n3_r2_c1 !n3_r9_c1
!n3_r2_c1 !n3_r1_c2
!n3_r2_c1 !n3_r1_c3
!n3_r2_c1 !n3_r3_c2
!n3_r2_c1 !n3_r3_c3
!n4_r2_c1 !n5_r2_c1
!n4_r2_c1 !n6_r2_c1
!n4_r2_c1 !n7_r2_c1
!n4_r2_c1 !n8_r2_c1
!n4_r2_c1 !n9_r2_c1
!n4_r2_c1 !n4_r2_c2
!n4_r2_c1 !n4_r2_c3
!n4_r2_c1 !n4_r2_c4
!n4_r2_c1 !n4_r2_c5
!n4_r2_c1 !n4_r2_c6
!n4_r2_c1 !n4_r2_c7
!n4_r2_c1 !n4_r2_c8
!n4_r2_c1 !n4_r2_c9
!n4_r2_c1 !n4_r3_c1
!n4_r2_c1 !n4_r4_c1
!n4_r2_c1 !n4_r5_c1
!n4_r2_c1 !n4_r6_c1
!n4_r2_c1 !n4_r7_c1
!n4_r2_c1 !n4_r8_c1
!n4_r2_c1 !n4_r9_c1
!n4_r2_c1 !n4_r1_c2
!n4_r2_c1 !n4_r1_c3
!n4_r2_c1 !n4_r3_c2
!n4_r2_c1 !n4_r3_c3
!n5_r2_c1 !n6_r2_c1
!n5_r2_c1 !n7_r2_c1
!n5_r2_c1 !n8_r2_c1
!n5_r2_c1 !n9_r2_c1
!n5_r2_c1 !n5_r2_c2
!n5_r2_c1 !n5_r2_c3
!n5_r2_c1 !n5_r2_c4
!n5_r2_c1 !n5_r2_c5
!n5_r2_c1 !n5_r2_c6
!n5_r2_c1 !n5_r2_c7
!n5_r2_c1 !n5_r2_c8
!n5_r2_c1 !n5_r2_c9
!n5_r2_c1 !n5_r3_c1
!n5_r2_c1 !n5_r4_c1
!n5_r2_c1 !n5_r5_c1
!n5_r2_c1 !n5_r6_c1
!n5_r2_c1 !n5_r7_c1
!n5_r2_c1 !n5_r8_c1
!n5_r2_c1 !n5_r9_c1
!n5_r2_c1 !n5_r1_c2
!n5_r2_c1 !n5_r1_c3
!n5_r2_c1 !n5_r3_c2
!n5_r2_c1 !n5_r3_c3
!n6_r2_c1 !n7_r2_c1
!n6_r2_c1 !n8_r2_c1
!n6_r2_c1 !n9_r2_c1
!n6_r2_c1 !n6_r2_c2
!n6_r2_c1 !n6_r2_c3
!n6_r2_c1 !n6_r2_c4
!n6_r2_c1 !n6_r2_c5
!n6_r2_c1 !n6_r2_c6
!n6_r2_c1 !n6_r2_c7
!n6_r2_c1 !n6_r2_c8
!n6_r2_c1 !n6_r2_c9
!n6_r2_c1 !n6_r3_c1
!n6_r2_c1 !n6_r4_c1
!n6_r2_c1 !n6_r5_c1
!n6_r2_c1 !n6_r6_c1
!n6_r2_c1 !n6_r7_c1
!n6_r2_c1 !n6_r8_c1
!n6_r2_c1 !n6_r9_c1
!n6_r2_c1 !n6_r1_c2
!n6_r2_c1 !n6_r1_c3
!n6_r2_c1 !n6_r3_c2
!n6_r2_c1 !n6_r3_c3
!n7_r2_c1 !n8_r2_c1
!n7_r2_c1 !n9_r2_c1
!n7_r2_c1 !n7_r2_c2
!n7_r2_c1 !n7_r2_c3
!n7_r2_c1 !n7_r2_c4
!n7_r2_c1 !n7_r2_c5
!n7_r2_c1 !n7_r2_c6
!n7_r2_c1 !n7_r2_c7
!n7_r2_c1 !n7_r2_c8
!n7_r2_c1 !n7_r2_c9
!n7_r2_c1 !n7_r3_c1
!n7_r2_c1 !n7_r4_c1
!n7_r2_c1 !n7_r5_c1
!n7_r2_c1 !n7_r6_c1
!n7_r2_c1 !n7_r7_c1
!n7_r2_c1 !n7_r8_c1
!n7_r2_c1 !n7_r9_c1
!n7_r2_c1 !n7_r1_c2
!n7_r2_c1 !n7_r1_c3
!n7_r2_c1 !n7_r3_c2
!n7_r2_c1 !n7_r3_c3
!n8_r2_c1 !n9_r2_c1
!n8_r2_c1 !n8_r2_c2
!n8_r2_c1 !n8_r2_c3
!n8_r2_c1 !n8_r2_c4
!n8_r2_c1 !n8_r2_c5
!n8_r2_c1 !n8_r2_c6
!n8_r2_c1 !n8_r2_c7
!n8_r2_c1 !n8_r2_c8
!n8_r2_c1 !n8_r2_c9
!n8_r2_c1 !n8_r3_c1
!n8_r2_c1 !n8_r4_c1
!n8_r2_c1 !n8_r5_c1
!n8_r2_c1 !n8_r6_c1
!n8_r2_c1 !n8_r7_c1
!n8_r2_c1 !n8_r8_c1
!n8_r2_c1 !n8_r9_c1
!n8_r2_c1 !n8_r1_c2
!n8_r2_c1 !n8_r1_c3
!n8_r2_c1 !n8_r3_c2
!n8_r2_c1 !n8_r3_c3
!n9_r2_c1 !n9_r2_c2
!n9_r2_c1 !n9_r2_c3
!n9_r2_c1 !n9_r2_c4
!n9_r2_c1 !n9_r2_c5
!n9_r2_c1 !n9_r2_c6
!n9_r2_c1 !n9_r2_c7
!n9_r2_c1 !n9_r2_c8
!n9_r2_c1 !n9_r2_c9
!n9_r2_c1 !n9_r3_c1
!n9_r2_c1 !n9_r4_c1
!n9_r2_c1 !n9_r5_c1
!n9_r2_c1 !n9_r6_c1
!n9_r2_c1 !n9_r7_c1
!n9_r2_c1 !n9_r8_c1
!n9_r2_c1 !n9_r9_c1
!n9_r2_c1 !n9_r1_c2
!n9_r2_c1 !n9_r1_c3
!n9_r2_c1 !n9_r3_c2
!n9_r2_c1 !n9_r3_c3
n1_r2_c1 n2_r2_c1 n3_r2_c1 n4_r2_c1 n5_r2_c1 n6_r2_c1 n7_r2_c1 n8_r2_c1 n9_r2_c1
!n1_r2_c2 !n2_r2_c2
!n1_r2_c2 !n3_r2_c2
!n1_r2_c2 !n4_r2_c2
!n1_r2_c2 !n5_r2_c2
!n1_r2_c2 !n6_r2_c2
!n1_r2_c2 !n7_r2_c2
!n1_r2_c2 !n8_r2_c2
!n1_r2_c2 !n9_r2_c2
!n1_r2_c2 !n1_r2_c3
!n1_r2_c2 !n1_r2_c4
!n1_r2_c2 !n1_r2_c5
!n1_r2_c2 !n1_r2_c6
!n1_r2_c2 !n1_r2_c7
!n1_r2_c2 !n1_r2_c8
!n1_r2_c2 !n1_r2_c9
!n1_r2_c2 !n1_r3_c2
!n1_r2_c2 !n1_r4_c2
!n1_r2_c2 !n1_r5_c2
!n1_r2_c2 !n1_r6_c2
!n1_r2_c2 !n1_r7_c2
!n1_r2_c2 !n1_r8_c2
!n1_r2_c2 !n1_r9_c2
!n1_r2_c2 !n1_r1_c1
!n1_r2_c2 !n1_r1_c3
!n1_r2_c2 !n1_r3_c1
!n1_r2_c2 !n1_r3_c3
!n2_r2_c2 !n3_r2_c2
!n2_r2_c2 !n4_r2_c2
!n2_r2_c2 !n5_r2_c2
!n2_r2_c2 !n6_r2_c2
!n2_r2_c2 !n7_r2_c2
!n2_r2_c2 !n8_r2_c2
!n2_r2_c2 !n9_r2_c2
!n2_r2_c2 !n2_r2_c3
!n2_r2_c2 !n2_r2_c4
!n2_r2_c2 !n2_r2_c5
!n2_r2_c2 !n2_r2_c6
!n2_r2_c2 !n2_r2_c7
!n2_r2_c2 !n2_r2_c8
!n2_r2_c2 !n2_r2_c9
!n2_r2_c2 !n2_r3_c2
!n2_r2_c2 !n2_r4_c2
!n2_r2_c2 !n2_r5_c2
!n2_r2_c2 !n2_r6_c2
!n2_r2_c2 !n2_r7_c2
!n2_r2_c2 !n2_r8_c2
!n2_r2_c2 !n2_r9_c2
!n2_r2_c2 !n2_r1_c1
!n2_r2_c2 !n2_r1_c3
!n2_r2_c2 !n2_r3_c1
!n2_r2_c2 !n2_r3_c3
!n3_r2_c2 !n4_r2_c2
!n3_r2_c2 !n5_r2_c2
!n3_r2_c2 !n6_r2_c2
!n3_r2_c2 !n7_r2_c2
!n3_r2_c2 !n8_r2_c2
!n3_r2_c2 !n9_r2_c2
!n3_r2_c2 !n3_r2_c3
!n3_r2_c2 !n3_r2_c4
!n3_r2_c2 !n3_r2_c5
!n3_r2_c2 !n3_r2_c6
!n3_r2_c2 !n3_r2_c7
!n3_r2_c2 !n3_r2_c8
!n3_r2_c2 !n3_r2_c9
!n3_r2_c2 !n3_r3_c2
!n3_r2_c2 !n3_r4_c2
!n3_r2_c2 !n3_r5_c2
!n3_r2_c2 !n3_r6_c2
!n3_r2_c2 !n3_r7_c2
!n3_r2_c2 !n3_r8_c2
!n3_r2_c2 !n3_r9_c2
!n3_r2_c2 !n3_r1_c1
!n3_r2_c2 !n3_r1_c3
!n3_r2_c2 !n3_r3_c1
!n3_r2_c2 !n3_r3_c3
!n4_r2_c2 !n5_r2_c2
!n4_r2_c2 !n6_r2_c2
!n4_r2_c2 !n7_r2_c2
!n4_r2_c2 !n8_r2_c2
!n4_r2_c2 !n9_r2_c2
!n4_r2_c2 !n4_r2_c3
!n4_r2_c2 !n4_r2_c4
!n4_r2_c2 !n4_r2_c5
!n4_r2_c2 !n4_r2_c6
!n4_r2_c2 !n4_r2_c7
!n4_r2_c2 !n4_r2_c8
!n4_r2_c2 !n4_r2_c9
!n4_r2_c2 !n4_r3_c2
!n4_r2_c2 !n4_r4_c2
!n4_r2_c2 !n4_r5_c2
!n4_r2_c2 !n4_r6_c2
!n4_r2_c2 !n4_r7_c2
!n4_r2_c2 !n4_r8_c2
!n4_r2_c2 !n4_r9_c2
!n4_r2_c2 !n4_r1_c1
!n4_r2_c2 !n4_r1_c3
!n4_r2_c2 !n4_r3_c1
!n4_r2_c2 !n4_r3_c3
!n5_r2_c2 !n6_r2_c2
!n5_r2_c2 !n7_r2_c2
!n5_r2_c2 !n8_r2_c2
!n5_r2_c2 !n9_r2_c2
!n5_r2_c2 !n5_r2_c3
!n5_r2_c2 !n5_r2_c4
!n5_r2_c2 !n5_r2_c5
!n5_r2_c2 !n5_r2_c6
!n5_r2_c2 !n5_r2_c7
!n5_r2_c2 !n5_r2_c8
!n5_r2_c2 !n5_r2_c9
!n5_r2_c2 !n5_r3_c2
!n5_r2_c2 !n5_r4_c2
!n5_r2_c2 !n5_r5_c2
!n5_r2_c2 !n5_r6_c2
!n5_r2_c2 !n5_r7_c2
!n5_r2_c2 !n5_r8_c2
!n5_r2_c2 !n5_r9_c2
!n5_r2_c2 !n5_r1_c1
!n5_r2_c2 !n5_r1_c3
!n5_r2_c2 !n5_r3_c1
!n5_r2_c2 !n5_r3_c3
!n6_r2_c2 !n7_r2_c2
!n6_r2_c2 !n8_r2_c2
!n6_r2_c2 !n9_r2_c2
!n6_r2_c2 !n6_r2_c3
!n6_r2_c2 !n6_r2_c4
!n6_r2_c2 !n6_r2_c5
!n6_r2_c2 !n6_r2_c6
!n6_r2_c2 !n6_r2_c7
!n6_r2_c2 !n6_r2_c8
!n6_r2_c2 !n6_r2_c9
!n6_r2_c2 !n6_r3_c2
!n6_r2_c2 !n6_r4_c2
!n6_r2_c2 !n6_r5_c2
!n6_r2_c2 !n6_r6_c2
!n6_r2_c2 !n6_r7_c2
!n6_r2_c2 !n6_r8_c2
!n6_r2_c2 !n6_r9_c2
!n6_r2_c2 !n6_r1_c1
!n6_r2_c2 !n6_r1_c3
!n6_r2_c2 !n6_r3_c1
!n6_r2_c2 !n6_r3_c3
!n7_r2_c2 !n8_r2_c2
!n7_r2_c2 !n9_r2_c2
!n7_r2_c2 !n7_r2_c3
!n7_r2_c2 !n7_r2_c4
!n7_r2_c2 !n7_r2_c5
!n7_r2_c2 !n7_r2_c6
!n7_r2_c2 !n7_r2_c7
!n7_r2_c2 !n7_r2_c8
!n7_r2_c2 !n7_r2_c9
!n7_r2_c2 !n7_r3_c2
!n7_r2_c2 !n7_r4_c2
!n7_r2_c2 !n7_r5_c2
!n7_r2_c2 !n7_r6_c2
!n7_r2_c2 !n7_r7_c2
!n7_r2_c2 !n7_r8_c2
!n7_r2_c2 !n7_r9_c2
!n7_r2_c2 !n7_r1_c1
!n7_r2_c2 !n7_r1_c3
!n7_r2_c2 !n7_r3_c1
!n7_r2_c2 !n7_r3_c3
!n8_r2_c2 !n9_r2_c2
!n8_r2_c2 !n8_r2_c3
!n8_r2_c2 !n8_r2_c4
!n8_r2_c2 !n8_r2_c5
!n8_r2_c2 !n8_r2_c6
!n8_r2_c2 !n8_r2_c7
!n8_r2_c2 !n8_r2_c8
!n8_r2_c2 !n8_r2_c9
!n8_r2_c2 !n8_r3_c2
!n8_r2_c2 !n8_r4_c2
!n8_r2_c2 !n8_r5_c2
!n8_r2_c2 !n8_r6_c2
!n8_r2_c2 !n8_r7_c2
!n8_r2_c2 !n8_r8_c2
!n8_r2_c2 !n8_r9_c2
!n8_r2_c2 !n8_r1_c1
!n8_r2_c2 !n8_r1_c3
!n8_r2_c2 !n8_r3_c1
!n8_r2_c2 !n8_r3_c3
!n9_r2_c2 !n9_r2_c3
!n9_r2_c2 !n9_r2_c4
!n9_r2_c2 !n9_r2_c5
!n9_r2_c2 !n9_r2_c6
!n9_r2_c2 !n9_r2_c7
!n9_r2_c2 !n9_r2_c8
!n9_r2_c2 !n9_r2_c9
!n9_r2_c2 !n9_r3_c2
!n9_r2_c2 !n9_r4_c2
!n9_r2_c2 !n9_r5_c2
!n9_r2_c2 !n9_r6_c2
!n9_r2_c2 !n9_r7_c2
!n9_r2_c2 !n9_r8_c2
!n9_r2_c2 !n9_r9_c2
!n9_r2_c2 !n9_r1_c1
!n9_r2_c2 !n9_r1_c3
!n9_r2_c2 !n9_r3_c1
!n9_r2_c2 !n9_r3_c3
n1_r2_c2 n2_r2_c2 n3_r2_c2 n4_r2_c2 n5_r2_c2 n6_r2_c2 n7_r2_c2 n8_r2_c2 n9_r2_c2
!n1_r2_c3 !n2_r2_c3
!n1_r2_c3 !n3_r2_c3
!n1_r2_c3 !n4_r2_c3
!n1_r2_c3 !n5_r2_c3
!n1_r2_c3 !n6_r2_c3
!n1_r2_c3 !n7_r2_c3
!n1_r2_c3 !n8_r2_c3
!n1_r2_c3 !n9_r2_c3
!n1_r2_c3 !n1_r2_c4
!n1_r2_c3 !n1_r2_c5
!n1_r2_c3 !n1_r2_c6
!n1_r2_c3 !n1_r2_c7
!n1_r2_c3 !n1_r2_c8
!n1_r2_c3 !n1_r2_c9
!n1_r2_c3 !n1_r3_c3
!n1_r2_c3 !n1_r4_c3
!n1_r2_c3 !n1_r5_c3
!n1_r2_c3 !n1_r6_c3
!n1_r2_c3 !n1_r7_c3
!n1_r2_c3 !n1_r8_c3
!n1_r2_c3 !n1_r9_c3
!n1_r2_c3 !n1_r1_c1
!n1_r2_c3 !n1_r1_c2
!n1_r2_c3 !n1_r3_c1
!n1_r2_c3 !n1_r3_c2
!n2_r2_c3 !n3_r2_c3
!n2_r2_c3 !n4_r2_c3
!n2_r2_c3 !n5_r2_c3
!n2_r2_c3 !n6_r2_c3
!n2_r2_c3 !n7_r2_c3
!n2_r2_c3 !n8_r2_c3
!n2_r2_c3 !n9_r2_c3
!n2_r2_c3 !n2_r2_c4
!n2_r2_c3 !n2_r2_c5
!n2_r2_c3 !n2_r2_c6
!n2_r2_c3 !n2_r2_c7
!n2_r2_c3 !n2_r2_c8
!n2_r2_c3 !n2_r2_c9
!n2_r2_c3 !n2_r3_c3
!n2_r2_c3 !n2_r4_c3
!n2_r2_c3 !n2_r5_c3
!n2_r2_c3 !n2_r6_c3
!n2_r2_c3 !n2_r7_c3
!n2_r2_c3 !n2_r8_c3
!n2_r2_c3 !n2_r9_c3
!n2_r2_c3 !n2_r1_c1
!n2_r2_c3 !n2_r1_c2
!n2_r2_c3 !n2_r3_c1
!n2_r2_c3 !n2_r3_c2
!n3_r2_c3 !n4_r2_c3
!n3_r2_c3 !n5_r2_c3
!n3_r2_c3 !n6_r2_c3
!n3_r2_c3 !n7_r2_c3
!n3_r2_c3 !n8_r2_c3
!n3_r2_c3 !n9_r2_c3
!n3_r2_c3 !n3_r2_c4
!n3_r2_c3 !n3_r2_c5
!n3_r2_c3 !n3_r2_c6
!n3_r2_c3 !n3_r2_c7
!n3_r2_c3 !n3_r2_c8
!n3_r2_c3 !n3_r2_c9
!n3_r2_c3 !n3_r3_c3
!n3_r2_c3 !n3_r4_c3
!n3_r2_c3 !n3_r5_c3
!n3_r2_c3 !n3_r6_c3
!n3_r2_c3 !n3_r7_c3
!n3_r2_c3 !n3_r8_c3
!n3_r2_c3 !n3_r9_c3
!n3_r2_c3 !n3_r1_c1
!n3_r2_c3 !n3_r1_c2
!n3_r2_c3 !n3_r3_c1
!n3_r2_c3 !n3_r3_c2
!n4_r2_c3 !n5_r2_c3
!n4_r2_c3 !n6_r2_c3
!n4_r2_c3 !n7_r2_c3
!n4_r2_c3 !n8_r2_c3
!n4_r2_c3 !n9_r2_c3
!n4_r2_c3 !n4_r2_c4
!n4_r2_c3 !n4_r2_c5
!n4_r2_c3 !n4_r2_c6
!n4_r2_c3 !n4_r2_c7
!n4_r2_c3 !n4_r2_c8
!n4_r2_c3 !n4_r2_c9
!n4_r2_c3 !n4_r3_c3
!n4_r2_c3 !n4_r4_c3
!n4_r2_c3 !n4_r5_c3
!n4_r2_c3 !n4_r6_c3
!n4_r2_c3 !n4_r7_c3
!n4_r2_c3 !n4_r8_c3
!n4_r2_c3 !n4_r9_c3
!n4_r2_c3 !n4_r1_c1
!n4_r2_c3 !n4_r1_c2
!n4_r2_c3 !n4_r3_c1
!n4_r2_c3 !n4_r3_c2
!n5_r2_c3 !n6_r2_c3
!n5_r2_c3 !n7_r2_c3
!n5_r2_c3 !n8_r2_c3
!n5_r2_c3 !n9_r2_c3
!n5_r2_c3 !n5_r2_c4
!n5_r2_c3 !n5_r2_c5
!n5_r2_c3 !n5_r2_c6
!n5_r2_c3 !n5_r2_c7
!n5_r2_c3 !n5_r2_c8
!n5_r2_c3 !n5_r2_c9
!n5_r2_c3 !n5_r3_c3
!n5_r2_c3 !n5_r4_c3
!n5_r2_c3 !n5_r5_c3
!n5_r2_c3 !n5_r6_c3
!n5_r2_c3 !n5_r7_c3
!n5_r2_c3 !n5_r8_c3
!n5_r2_c3 !n5_r9_c3
!n5_r2_c3 !n5_r1_c1
!n5_r2_c3 !n5_r1_c2
!n5_r2_c3 !n5_r3_c1
!n5_r2_c3 !n5_r3_c2
!n6_r2_c3 !n7_r2_c3
!n6_r2_c3 !n8_r2_c3
!n6_r2_c3 !n9_r2_c3
!n6_r2_c3 !n6_r2_c4
!n6_r2_c3 !n6_r2_c5
!n6_r2_c3 !n6_r2_c6
!n6_r2_c3 !n6_r2_c7
!n6_r2_c3 !n6_r2_c8
!n6_r2_c3 !n6_r2_c9
!n6_r2_c3 !n6_r3_c3
!n6_r2_c3 !n6_r4_c3
!n6_r2_c3 !n6_r5_c3
!n6_r2_c3 !n6_r6_c3
!n6_r2_c3 !n6_r7_c3
!n6_r2_c3 !n6_r8_c3
!n6_r2_c3 !n6_r9_c3
!n6_r2_c3 !n6_r1_c1
!n6_r2_c3 !n6_r1_c2
!n6_r2_c3 !n6_r3_c1
!n6_r2_c3 !n6_r3_c2
!n7_r2_c3 !n8_r2_c3
!n7_r2_c3 !n9_r2_c3
!n7_r2_c3 !n7_r2_c4
!n7_r2_c3 !n7_r2_c5
!n7_r2_c3 !n7_r2_c6
!n7_r2_c3 !n7_r2_c7
!n7_r2_c3 !n7_r2_c8
!n7_r2_c3 !n7_r2_c9
!n7_r2_c3 !n7_r3_c3
!n7_r2_c3 !n7_r4_c3
!n7_r2_c3 !n7_r5_c3
!n7_r2_c3 !n7_r6_c3
!n7_r2_c3 !n7_r7_c3
!n7_r2_c3 !n7_r8_c3
!n7_r2_c3 !n7_r9_c3
!n7_r2_c3 !n7_r1_c1
!n7_r2_c3 !n7_r1_c2
!n7_r2_c3 !n7_r3_c1
!n7_r2_c3 !n7_r3_c2
!n8_r2_c3 !n9_r2_c3
!n8_r2_c3 !n8_r2_c4
!n8_r2_c3 !n8_r2_c5
!n8_r2_c3 !n8_r2_c6
!n8_r2_c3 !n8_r2_c7
!n8_r2_c3 !n8_r2_c8
!n8_r2_c3 !n8_r2_c9
!n8_r2_c3 !n8_r3_c3
!n8_r2_c3 !n8_r4_c3
!n8_r2_c3 !n8_r5_c3
!n8_r2_c3 !n8_r6_c3
!n8_r2_c3 !n8_r7_c3
!n8_r2_c3 !n8_r8_c3
!n8_r2_c3 !n8_r9_c3
!n8_r2_c3 !n8_r1_c1
!n8_r2_c3 !n8_r1_c2
!n8_r2_c3 !n8_r3_c1
!n8_r2_c3 !n8_r3_c2
!n9_r2_c3 !n9_r2_c4
!n9_r2_c3 !n9_r2_c5
!n9_r2_c3 !n9_r2_c6
!n9_r2_c3 !n9_r2_c7
!n9_r2_c3 !n9_r2_c8
!n9_r2_c3 !n9_r2_c9
!n9_r2_c3 !n9_r3_c3
!n9_r2_c3 !n9_r4_c3
!n9_r2_c3 !n9_r5_c3
!n9_r2_c3 !n9_r6_c3
!n9_r2_c3 !n9_r7_c3
!n9_r2_c3 !n9_r8_c3
!n9_r2_c3 !n9_r9_c3
!n9_r2_c3 !n9_r1_c1
!n9_r2_c3 !n9_r1_c2
!n9_r2_c3 !n9_r3_c1
!n9_r2_c3 !n9_r3_c2
n1_r2_c3 n2_r2_c3 n3_r2_c3 n4_r2_c3 n5_r2_c3 n6_r2_c3 n7_r2_c3 n8_r2_c3 n9_r2_c3
!n1_r2_c4 !n2_r2_c4
!n1_r2_c4 !n3_r2_c4
!n1_r2_c4 !n4_r2_c4
!n1_r2_c4 !n5_r2_c4
!n1_r2_c4 !n6_r2_c4
!n1_r2_c4 !n7_r2_c4
!n1_r2_c4 !n8_r2_c4
!n1_r2_c4 !n9_r2_c4
!n1_r2_c4 !n1_r2_c5
!n1_r2_c4 !n1_r2_c6
!n1_r2_c4 !n1_r2_c7
!n1_r2_c4 !n1_r2_c8
!n1_r2_c4 !n1_r2_c9
!n1_r2_c4 !n1_r3_c4
!n1_r2_c4 !n1_r4_c4
!n1_r2_c4 !n1_r5_c4
!n1_r2_c4 !n1_r6_c4
!n1_r2_c4 !n1_r7_c4
!n1_r2_c4 !n1_r8_c4
!n1_r2_c4 !n1_r9_c4
!n1_r2_c4 !n1_r1_c5
!n1_r2_c4 !n1_r1_c6
!n1_r2_c4 !n1_r3_c5
!n1_r2_c4 !n1_r3_c6
!n2_r2_c4 !n3_r2_c4
!n2_r2_c4 !n4_r2_c4
!n2_r2_c4 !n5_r2_c4
!n2_r2_c4 !n6_r2_c4
!n2_r2_c4 !n7_r2_c4
!n2_r2_c4 !n8_r2_c4
!n2_r2_c4 !n9_r2_c4
!n2_r2_c4 !n2_r2_c5
!n2_r2_c4 !n2_r2_c6
!n2_r2_c4 !n2_r2_c7
!n2_r2_c4 !n2_r2_c8
!n2_r2_c4 !n2_r2_c9
!n2_r2_c4 !n2_r3_c4
!n2_r2_c4 !n2_r4_c4
!n2_r2_c4 !n2_r5_c4
!n2_r2_c4 !n2_r6_c4
!n2_r2_c4 !n2_r7_c4
!n2_r2_c4 !n2_r8_c4
!n2_r2_c4 !n2_r9_c4
!n2_r2_c4 !n2_r1_c5
!n2_r2_c4 !n2_r1_c6
!n2_r2_c4 !n2_r3_c5
!n2_r2_c4 !n2_r3_c6
!n3_r2_c4 !n4_r2_c4
!n3_r2_c4 !n5_r2_c4
!n3_r2_c4 !n6_r2_c4
!n3_r2_c4 !n7_r2_c4
!n3_r2_c4 !n8_r2_c4
!n3_r2_c4 !n9_r2_c4
!n3_r2_c4 !n3_r2_c5
!n3_r2_c4 !n3_r2_c6
!n3_r2_c4 !n3_r2_c7
!n3_r2_c4 !n3_r2_c8
!n3_r2_c4 !n3_r2_c9
!n3_r2_c4 !n3_r3_c4
!n3_r2_c4 !n3_r4_c4
!n3_r2_c4 !n3_r5_c4
!n3_r2_c4 !n3_r6_c4
!n3_r2_c4 !n3_r7_c4
!n3_r2_c4 !n3_r8_c4
!n3_r2_c4 !n3_r9_c4
!n3_r2_c4 !n3_r1_c5
!n3_r2_c4 !n3_r1_c6
!n3_r2_c4 !n3_r3_c5
!n3_r2_c4 !n3_r3_c6
!n4_r2_c4 !n5_r2_c4
!n4_r2_c4 !n6_r2_c4
!n4_r2_c4 !n7_r2_c4
!n4_r2_c4 !n8_r2_c4
!n4_r2_c4 !n9_r2_c4
!n4_r2_c4 !n4_r2_c5
!n4_r2_c4 !n4_r2_c6
!n4_r2_c4 !n4_r2_c7
!n4_r2_c4 !n4_r2_c8
!n4_r2_c4 !n4_r2_c9
!n4_r2_c4 !n4_r3_c4
!n4_r2_c4 !n4_r4_c4
!n4_r2_c4 !n4_r5_c4
!n4_r2_c4 !n4_r6_c4
!n4_r2_c4 !n4_r7_c4
!n4_r2_c4 !n4_r8_c4
!n4_r2_c4 !n4_r9_c4
!n4_r2_c4 !n4_r1_c5
!n4_r2_c4 !n4_r1_c6
!n4_r2_c4 !n4_r3_c5
!n4_r2_c4 !n4_r3_c6
!n5_r2_c4 !n6_r2_c4
!n5_r2_c4 !n7_r2_c4
!n5_r2_c4 !n8_r2_c4
!n5_r2_c4 !n9_r2_c4
!n5_r2_c4 !n5_r2_c5
!n5_r2_c4 !n5_r2_c6
!n5_r2_c4 !n5_r2_c7
!n5_r2_c4 !n5_r2_c8
!n5_r2_c4 !n5_r2_c9
!n5_r2_c4 !n5_r3_c4
!n5_r2_c4 !n5_r4_c4
!n5_r2_c4 !n5_r5_c4
!n5_r2_c4 !n5_r6_c4
!n5_r2_c4 !n5_r7_c4
!n5_r2_c4 !n5_r8_c4
!n5_r2_c4 !n5_r9_c4
!n5_r2_c4 !n5_r1_c5
!n5_r2_c4 !n5_r1_c6
!n5_r2_c4 !n5_r3_c5
!n5_r2_c4 !n5_r3_c6
!n6_r2_c4 !n7_r2_c4
!n6_r2_c4 !n8_r2_c4
!n6_r2_c4 !n9_r2_c4
!n6_r2_c4 !n6_r2_c5
!n6_r2_c4 !n6_r2_c6
!n6_r2_c4 !n6_r2_c7
!n6_r2_c4 !n6_r2_c8
!n6_r2_c4 !n6_r2_c9
!n6_r2_c4 !n6_r3_c4
!n6_r2_c4 !n6_r4_c4
!n6_r2_c4 !n6_r5_c4
!n6_r2_c4 !n6_r6_c4
!n6_r2_c4 !n6_r7_c4
!n6_r2_c4 !n6_r8_c4
!n6_r2_c4 !n6_r9_c4
!n6_r2_c4 !n6_r1_c5
!n6_r2_c4 !n6_r1_c6
!n6_r2_c4 !n6_r3_c5
!n6_r2_c4 !n6_r3_c6
!n7_r2_c4 !n8_r2_c4
!n7_r2_c4 !n9_r2_c4
!n7_r2_c4 !n7_r2_c5
!n7_r2_c4 !n7_r2_c6
!n7_r2_c4 !n7_r2_c7
!n7_r2_c4 !n7_r2_c8
!n7_r2_c4 !n7_r2_c9
!n7_r2_c4 !n7_r3_c4
!n7_r2_c4 !n7_r4_c4
!n7_r2_c4 !n7_r5_c4
!n7_r2_c4 !n7_r6_c4
!n7_r2_c4 !n7_r7_c4
!n7_r2_c4 !n7_r8_c4
!n7_r2_c4 !n7_r9_c4
!n7_r2_c4 !n7_r1_c5
!n7_r2_c4 !n7_r1_c6
!n7_r2_c4 !n7_r3_c5
!n7_r2_c4 !n7_r3_c6
!n8_r2_c4 !n9_r2_c4
!n8_r2_c4 !n8_r2_c5
!n8_r2_c4 !n8_r2_c6
!n8_r2_c4 !n8_r2_c7
!n8_r2_c4 !n8_r2_c8
!n8_r2_c4 !n8_r2_c9
!n8_r2_c4 !n8_r3_c4
!n8_r2_c4 !n8_r4_c4
!n8_r2_c4 !n8_r5_c4
!n8_r2_c4 !n8_r6_c4
!n8_r2_c4 !n8_r7_c4
!n8_r2_c4 !n8_r8_c4
!n8_r2_c4 !n8_r9_c4
!n8_r2_c4 !n8_r1_c5
!n8_r2_c4 !n8_r1_c6
!n8_r2_c4 !n8_r3_c5
!n8_r2_c4 !n8_r3_c6
!n9_r2_c4 !n9_r2_c5
!n9_r2_c4 !n9_r2_c6
!n9_r2_c4 !n9_r2_c7
!n9_r2_c4 !n9_r2_c8
!n9_r2_c4 !n9_r2_c9
!n9_r2_c4 !n9_r3_c4
!n9_r2_c4 !n9_r4_c4
!n9_r2_c4 !n9_r5_c4
!n9_r2_c4 !n9_r6_c4
!n9_r2_c4 !n9_r7_c4
!n9_r2_c4 !n9_r8_c4
!n9_r2_c4 !n9_r9_c4
!n9_r2_c4 !n9_r1_c5
!n9_r2_c4 !n9_r1_c6
!n9_r2_c4 !n9_r3_c5
!n9_r2_c4 !n9_r3_c6
n1_r2_c4 n2_r2_c4 n3_r2_c4 n4_r2_c4 n5_r2_c4 n6_r2_c4 n7_r2_c4 n8_r2_c4 n9_r2_c4
!n1_r2_c5 !n2_r2_c5
!n1_r2_c5 !n3_r2_c5
!n1_r2_c5 !n4_r2_c5
!n1_r2_c5 !n5_r2_c5
!n1_r2_c5 !n6_r2_c5
!n1_r2_c5 !n7_r2_c5
!n1_r2_c5 !n8_r2_c5
!n1_r2_c5 !n9_r2_c5
!n1_r2_c5 !n1_r2_c6
!n1_r2_c5 !n1_r2_c7
!n1_r2_c5 !n1_r2_c8
!n1_r2_c5 !n1_r2_c9
!n1_r2_c5 !n1_r3_c5
!n1_r2_c5 !n1_r4_c5
!n1_r2_c5 !n1_r5_c5
!n1_r2_c5 !n1_r6_c5
!n1_r2_c5 !n1_r7_c5
!n1_r2_c5 !n1_r8_c5
!n1_r2_c5 !n1_r9_c5
!n1_r2_c5 !n1_r1_c4
!n1_r2_c5 !n1_r1_c6
!n1_r2_c5 !n1_r3_c4
!n1_r2_c5 !n1_r3_c6
!n2_r2_c5 !n3_r2_c5
!n2_r2_c5 !n4_r2_c5
!n2_r2_c5 !n5_r2_c5
!n2_r2_c5 !n6_r2_c5
!n2_r2_c5 !n7_r2_c5
!n2_r2_c5 !n8_r2_c5
!n2_r2_c5 !n9_r2_c5
!n2_r2_c5 !n2_r2_c6
!n2_r2_c5 !n2_r2_c7
!n2_r2_c5 !n2_r2_c8
!n2_r2_c5 !n2_r2_c9
!n2_r2_c5 !n2_r3_c5
!n2_r2_c5 !n2_r4_c5
!n2_r2_c5 !n2_r5_c5
!n2_r2_c5 !n2_r6_c5
!n2_r2_c5 !n2_r7_c5
!n2_r2_c5 !n2_r8_c5
!n2_r2_c5 !n2_r9_c5
!n2_r2_c5 !n2_r1_c4
!n2_r2_c5 !n2_r1_c6
!n2_r2_c5 !n2_r3_c4
!n2_r2_c5 !n2_r3_c6
!n3_r2_c5 !n4_r2_c5
!n3_r2_c5 !n5_r2_c5
!n3_r2_c5 !n6_r2_c5
!n3_r2_c5 !n7_r2_c5
!n3_r2_c5 !n8_r2_c5
!n3_r2_c5 !n9_r2_c5
!n3_r2_c5 !n3_r2_c6
!n3_r2_c5 !n3_r2_c7
!n3_r2_c5 !n3_r2_c8
!n3_r2_c5 !n3_r2_c9
!n3_r2_c5 !n3_r3_c5
!n3_r2_c5 !n3_r4_c5
!n3_r2_c5 !n3_r5_c5
!n3_r2_c5 !n3_r6_c5
!n3_r2_c5 !n3_r7_c5
!n3_r2_c5 !n3_r8_c5
!n3_r2_c5 !n3_r9_c5
!n3_r2_c5 !n3_r1_c4
!n3_r2_c5 !n3_r1_c6
!n3_r2_c5 !n3_r3_c4
!n3_r2_c5 !n3_r3_c6
!n4_r2_c5 !n5_r2_c5
!n4_r2_c5 !n6_r2_c5
!n4_r2_c5 !n7_r2_c5
!n4_r2_c5 !n8_r2_c5
!n4_r2_c5 !n9_r2_c5
!n4_r2_c5 !n4_r2_c6
!n4_r2_c5 !n4_r2_c7
!n4_r2_c5 !n4_r2_c8
!n4_r2_c5 !n4_r2_c9
!n4_r2_c5 !n4_r3_c5
!n4_r2_c5 !n4_r4_c5
!n4_r2_c5 !n4_r5_c5
!n4_r2_c5 !n4_r6_c5
!n4_r2_c5 !n4_r7_c5
!n4_r2_c5 !n4_r8_c5
!n4_r2_c5 !n4_r9_c5
!n4_r2_c5 !n4_r1_c4
!n4_r2_c5 !n4_r1_c6
!n4_r2_c5 !n4_r3_c4
!n4_r2_c5 !n4_r3_c6
!n5_r2_c5 !n6_r2_c5
!n5_r2_c5 !n7_r2_c5
!n5_r2_c5 !n8_r2_c5
!n5_r2_c5 !n9_r2_c5
!n5_r2_c5 !n5_r2_c6
!n5_r2_c5 !n5_r2_c7
!n5_r2_c5 !n5_r2_c8
!n5_r2_c5 !n5_r2_c9
!n5_r2_c5 !n5_r3_c5
!n5_r2_c5 !n5_r4_c5
!n5_r2_c5 !n5_r5_c5
!n5_r2_c5 !n5_r6_c5
!n5_r2_c5 !n5_r7_c5
!n5_r2_c5 !n5_r8_c5
!n5_r2_c5 !n5_r9_c5
!n5_r2_c5 !n5_r1_c4
!n5_r2_c5 !n5_r1_c6
!n5_r2_c5 !n5_r3_c4
!n5_r2_c5 !n5_r3_c6
!n6_r2_c5 !n7_r2_c5
!n6_r2_c5 !n8_r2_c5
!n6_r2_c5 !n9_r2_c5
!n6_r2_c5 !n6_r2_c6
!n6_r2_c5 !n6_r2_c7
!n6_r2_c5 !n6_r2_c8
!n6_r2_c5 !n6_r2_c9
!n6_r2_c5 !n6_r3_c5
!n6_r2_c5 !n6_r4_c5
!n6_r2_c5 !n6_r5_c5
!n6_r2_c5 !n6_r6_c5
!n6_r2_c5 !n6_r7_c5
!n6_r2_c5 !n6_r8_c5
!n6_r2_c5 !n6_r9_c5
!n6_r2_c5 !n6_r1_c4
!n6_r2_c5 !n6_r1_c6
!n6_r2_c5 !n6_r3_c4
!n6_r2_c5 !n6_r3_c6
!n7_r2_c5 !n8_r2_c5
!n7_r2_c5 !n9_r2_c5
!n7_r2_c5 !n7_r2_c6
!n7_r2_c5 !n7_r2_c7
!n7_r2_c5 !n7_r2_c8
!n7_r2_c5 !n7_r2_c9
!n7_r2_c5 !n7_r3_c5
!n7_r2_c5 !n7_r4_c5
!n7_r2_c5 !n7_r5_c5
!n7_r2_c5 !n7_r6_c5
!n7_r2_c5 !n7_r7_c5
!n7_r2_c5 !n7_r8_c5
!n7_r2_c5 !n7_r9_c5
!n7_r2_c5 !n7_r1_c4
!n7_r2_c5 !n7_r1_c6
!n7_r2_c5 !n7_r3_c4
!n7_r2_c5 !n7_r3_c6
!n8_r2_c5 !n9_r2_c5
!n8_r2_c5 !n8_r2_c6
!n8_r2_c5 !n8_r2_c7
!n8_r2_c5 !n8_r2_c8
!n8_r2_c5 !n8_r2_c9
!n8_r2_c5 !n8_r3_c5
!n8_r2_c5 !n8_r4_c5
!n8_r2_c5 !n8_r5_c5
!n8_r2_c5 !n8_r6_c5
!n8_r2_c5 !n8_r7_c5
!n8_r2_c5 !n8_r8_c5
!n8_r2_c5 !n8_r9_c5
!n8_r2_c5 !n8_r1_c4
!n8_r2_c5 !n8_r1_c6
!n8_r2_c5 !n8_r3_c4
!n8_r2_c5 !n8_r3_c6
!n9_r2_c5 !n9_r2_c6
!n9_r2_c5 !n9_r2_c7
!n9_r2_c5 !n9_r2_c8
!n9_r2_c5 !n9_r2_c9
!n9_r2_c5 !n9_r3_c5
!n9_r2_c5 !n9_r4_c5
!n9_r2_c5 !n9_r5_c5
!n9_r2_c5 !n9_r6_c5
!n9_r2_c5 !n9_r7_c5
!n9_r2_c5 !n9_r8_c5
!n9_r2_c5 !n9_r9_c5
!n9_r2_c5 !n9_r1_c4
!n9_r2_c5 !n9_r1_c6
!n9_r2_c5 !n9_r3_c4
!n9_r2_c5 !n9_r3_c6
n1_r2_c5 n2_r2_c5 n3_r2_c5 n4_r2_c5 n5_r2_c5 n6_r2_c5 n7_r2_c5 n8_r2_c5 n9_r2_c5
!n1_r2_c6 !n2_r2_c6
!n1_r2_c6 !n3_r2_c6
!n1_r2_c6 !n4_r2_c6
!n1_r2_c6 !n5_r2_c6
!n1_r2_c6 !n6_r2_c6
!n1_r2_c6 !n7_r2_c6
!n1_r2_c6 !n8_r2_c6
!n1_r2_c6 !n9_r2_c6
!n1_r2_c6 !n1_r2_c7
!n1_r2_c6 !n1_r2_c8
!n1_r2_c6 !n1_r2_c9
!n1_r2_c6 !n1_r3_c6
!n1_r2_c6 !n1_r4_c6
!n1_r2_c6 !n1_r5_c6
!n1_r2_c6 !n1_r6_c6
!n1_r2_c6 !n1_r7_c6
!n1_r2_c6 !n1_r8_c6
!n1_r2_c6 !n1_r9_c6
!n1_r2_c6 !n1_r1_c4
!n1_r2_c6 !n1_r1_c5
!n1_r2_c6 !n1_r3_c4
!n1_r2_c6 !n1_r3_c5
!n2_r2_c6 !n3_r2_c6
!n2_r2_c6 !n4_r2_c6
!n2_r2_c6 !n5_r2_c6
!n2_r2_c6 !n6_r2_c6
!n2_r2_c6 !n7_r2_c6
!n2_r2_c6 !n8_r2_c6
!n2_r2_c6 !n9_r2_c6
!n2_r2_c6 !n2_r2_c7
!n2_r2_c6 !n2_r2_c8
!n2_r2_c6 !n2_r2_c9
!n2_r2_c6 !n2_r3_c6
!n2_r2_c6 !n2_r4_c6
!n2_r2_c6 !n2_r5_c6
!n2_r2_c6 !n2_r6_c6
!n2_r2_c6 !n2_r7_c6
!n2_r2_c6 !n2_r8_c6
!n2_r2_c6 !n2_r9_c6
!n2_r2_c6 !n2_r1_c4
!n2_r2_c6 !n2_r1_c5
!n2_r2_c6 !n2_r3_c4
!n2_r2_c6 !n2_r3_c5
!n3_r2_c6 !n4_r2_c6
!n3_r2_c6 !n5_r2_c6
!n3_r2_c6 !n6_r2_c6
!n3_r2_c6 !n7_r2_c6
!n3_r2_c6 !n8_r2_c6
!n3_r2_c6 !n9_r2_c6
!n3_r2_c6 !n3_r2_c7
!n3_r2_c6 !n3_r2_c8
!n3_r2_c6 !n3_r2_c9
!n3_r2_c6 !n3_r3_c6
!n3_r2_c6 !n3_r4_c6
!n3_r2_c6 !n3_r5_c6
!n3_r2_c6 !n3_r6_c6
!n3_r2_c6 !n3_r7_c6
!n3_r2_c6 !n3_r8_c6
!n3_r2_c6 !n3_r9_c6
!n3_r2_c6 !n3_r1_c4
!n3_r2_c6 !n3_r1_c5
!n3_r2_c6 !n3_r3_c4
!n3_r2_c6 !n3_r3_c5
!n4_r2_c6 !n5_r2_c6
!n4_r2_c6 !n6_r2_c6
!n4_r2_c6 !n7_r2_c6
!n4_r2_c6 !n8_r2_c6
!n4_r2_c6 !n9_r2_c6
!n4_r2_c6 !n4_r2_c7
!n4_r2_c6 !n4_r2_c8
!n4_r2_c6 !n4_r2_c9
!n4_r2_c6 !n4_r3_c6
!n4_r2_c6 !n4_r4_c6
!n4_r2_c6 !n4_r5_c6
!n4_r2_c6 !n4_r6_c6
!n4_r2_c6 !n4_r7_c6
!n4_r2_c6 !n4_r8_c6
!n4_r2_c6 !n4_r9_c6
!n4_r2_c6 !n4_r1_c4
!n4_r2_c6 !n4_r1_c5
!n4_r2_c6 !n4_r3_c4
!n4_r2_c6 !n4_r3_c5
!n5_r2_c6 !n6_r2_c6
!n5_r2_c6 !n7_r2_c6
!n5_r2_c6 !n8_r2_c6
!n5_r2_c6 !n9_r2_c6
!n5_r2_c6 !n5_r2_c7
!n5_r2_c6 !n5_r2_c8
!n5_r2_c6 !n5_r2_c9
!n5_r2_c6 !n5_r3_c6
!n5_r2_c6 !n5_r4_c6
!n5_r2_c6 !n5_r5_c6
!n5_r2_c6 !n5_r6_c6
!n5_r2_c6 !n5_r7_c6
!n5_r2_c6 !n5_r8_c6
!n5_r2_c6 !n5_r9_c6
!n5_r2_c6 !n5_r1_c4
!n5_r2_c6 !n5_r1_c5
!n5_r2_c6 !n5_r3_c4
!n5_r2_c6 !n5_r3_c5
!n6_r2_c6 !n7_r2_c6
!n6_r2_c6 !n8_r2_c6
!n6_r2_c6 !n9_r2_c6
!n6_r2_c6 !n6_r2_c7
!n6_r2_c6 !n6_r2_c8
!n6_r2_c6 !n6_r2_c9
!n6_r2_c6 !n6_r3_c6
!n6_r2_c6 !n6_r4_c6
!n6_r2_c6 !n6_r5_c6
!n6_r2_c6 !n6_r6_c6
!n6_r2_c6 !n6_r7_c6
!n6_r2_c6 !n6_r8_c6
!n6_r2_c6 !n6_r9_c6
!n6_r2_c6 !n6_r1_c4
!n6_r2_c6 !n6_r1_c5
!n6_r2_c6 !n6_r3_c4
!n6_r2_c6 !n6_r3_c5
!n7_r2_c6 !n8_r2_c6
!n7_r2_c6 !n9_r2_c6
!n7_r2_c6 !n7_r2_c7
!n7_r2_c6 !n7_r2_c8
!n7_r2_c6 !n7_r2_c9
!n7_r2_c6 !n7_r3_c6
!n7_r2_c6 !n7_r4_c6
!n7_r2_c6 !n7_r5_c6
!n7_r2_c6 !n7_r6_c6
!n7_r2_c6 !n7_r7_c6
!n7_r2_c6 !n7_r8_c6
!n7_r2_c6 !n7_r9_c6
!n7_r2_c6 !n7_r1_c4
!n7_r2_c6 !n7_r1_c5
!n7_r2_c6 !n7_r3_c4
!n7_r2_c6 !n7_r3_c5
!n8_r2_c6 !n9_r2_c6
!n8_r2_c6 !n8_r2_c7
!n8_r2_c6 !n8_r2_c8
!n8_r2_c6 !n8_r2_c9
!n8_r2_c6 !n8_r3_c6
!n8_r2_c6 !n8_r4_c6
!n8_r2_c6 !n8_r5_c6
!n8_r2_c6 !n8_r6_c6
!n8_r2_c6 !n8_r7_c6
!n8_r2_c6 !n8_r8_c6
!n8_r2_c6 !n8_r9_c6
!n8_r2_c6 !n8_r1_c4
!n8_r2_c6 !n8_r1_c5
!n8_r2_c6 !n8_r3_c4
!n8_r2_c6 !n8_r3_c5
!n9_r2_c6 !n9_r2_c7
!n9_r2_c6 !n9_r2_c8
!n9_r2_c6 !n9_r2_c9
!n9_r2_c6 !n9_r3_c6
!n9_r2_c6 !n9_r4_c6
!n9_r2_c6 !n9_r5_c6
!n9_r2_c6 !n9_r6_c6
!n9_r2_c6 !n9_r7_c6
!n9_r2_c6 !n9_r8_c6
!n9_r2_c6 !n9_r9_c6
!n9_r2_c6 !n9_r1_c4
!n9_r2_c6 !n9_r1_c5
!n9_r2_c6 !n9_r3_c4
!n9_r2_c6 !n9_r3_c5
n1_r2_c6 n2_r2_c6 n3_r2_c6 n4_r2_c6 n5_r2_c6 n6_r2_c6 n7_r2_c6 n8_r2_c6 n9_r2_c6
!n1_r2_c7 !n2_r2_c7
!n1_r2_c7 !n3_r2_c7
!n1_r2_c7 !n4_r2_c7
!n1_r2_c7 !n5_r2_c7
!n1_r2_c7 !n6_r2_c7
!n1_r2_c7 !n7_r2_c7
!n1_r2_c7 !n8_r2_c7
!n1_r2_c7 !n9_r2_c7
!n1_r2_c7 !n1_r2_c8
!n1_r2_c7 !n1_r2_c9
!n1_r2_c7 !n1_r3_c7
!n1_r2_c7 !n1_r4_c7
!n1_r2_c7 !n1_r5_c7
!n1_r2_c7 !n1_r6_c7
!n1_r2_c7 !n1_r7_c7
!n1_r2_c7 !n1_r8_c7
!n1_r2_c7 !n1_r9_c7
!n1_r2_c7 !n1_r1_c8
!n1_r2_c7 !n1_r1_c9
!n1_r2_c7 !n1_r3_c8
!n1_r2_c7 !n1_r3_c9
!n2_r2_c7 !n3_r2_c7
!n2_r2_c7 !n4_r2_c7
!n2_r2_c7 !n5_r2_c7
!n2_r2_c7 !n6_r2_c7
!n2_r2_c7 !n7_r2_c7
!n2_r2_c7 !n8_r2_c7
!n2_r2_c7 !n9_r2_c7
!n2_r2_c7 !n2_r2_c8
!n2_r2_c7 !n2_r2_c9
!n2_r2_c7 !n2_r3_c7
!n2_r2_c7 !n2_r4_c7
!n2_r2_c7 !n2_r5_c7
!n2_r2_c7 !n2_r6_c7
!n2_r2_c7 !n2_r7_c7
!n2_r2_c7 !n2_r8_c7
!n2_r2_c7 !n2_r9_c7
!n2_r2_c7 !n2_r1_c8
!n2_r2_c7 !n2_r1_c9
!n2_r2_c7 !n2_r3_c8
!n2_r2_c7 !n2_r3_c9
!n3_r2_c7 !n4_r2_c7
!n3_r2_c7 !n5_r2_c7
!n3_r2_c7 !n6_r2_c7
!n3_r2_c7 !n7_r2_c7
!n3_r2_c7 !n8_r2_c7
!n3_r2_c7 !n9_r2_c7
!n3_r2_c7 !n3_r2_c8
!n3_r2_c7 !n3_r2_c9
!n3_r2_c7 !n3_r3_c7
!n3_r2_c7 !n3_r4_c7
!n3_r2_c7 !n3_r5_c7
!n3_r2_c7 !n3_r6_c7
!n3_r2_c7 !n3_r7_c7
!n3_r2_c7 !n3_r8_c7
!n3_r2_c7 !n3_r9_c7
!n3_r2_c7 !n3_r1_c8
!n3_r2_c7 !n3_r1_c9
!n3_r2_c7 !n3_r3_c8
!n3_r2_c7 !n3_r3_c9
!n4_r2_c7 !n5_r2_c7
!n4_r2_c7 !n6_r2_c7
!n4_r2_c7 !n7_r2_c7
!n4_r2_c7 !n8_r2_c7
!n4_r2_c7 !n9_r2_c7
!n4_r2_c7 !n4_r2_c8
!n4_r2_c7 !n4_r2_c9
!n4_r2_c7 !n4_r3_c7
!n4_r2_c7 !n4_r4_c7
!n4_r2_c7 !n4_r5_c7
!n4_r2_c7 !n4_r6_c7
!n4_r2_c7 !n4_r7_c7
!n4_r2_c7 !n4_r8_c7
!n4_r2_c7 !n4_r9_c7
!n4_r2_c7 !n4_r1_c8
!n4_r2_c7 !n4_r1_c9
!n4_r2_c7 !n4_r3_c8
!n4_r2_c7 !n4_r3_c9
!n5_r2_c7 !n6_r2_c7
!n5_r2_c7 !n7_r2_c7
!n5_r2_c7 !n8_r2_c7
!n5_r2_c7 !n9_r2_c7
!n5_r2_c7 !n5_r2_c8
!n5_r2_c7 !n5_r2_c9
!n5_r2_c7 !n5_r3_c7
!n5_r2_c7 !n5_r4_c7
!n5_r2_c7 !n5_r5_c7
!n5_r2_c7 !n5_r6_c7
!n5_r2_c7 !n5_r7_c7
!n5_r2_c7 !n5_r8_c7
!n5_r2_c7 !n5_r9_c7
!n5_r2_c7 !n5_r1_c8
!n5_r2_c7 !n5_r1_c9
!n5_r2_c7 !n5_r3_c8
!n5_r2_c7 !n5_r3_c9
!n6_r2_c7 !n7_r2_c7
!n6_r2_c7 !n8_r2_c7
!n6_r2_c7 !n9_r2_c7
!n6_r2_c7 !n6_r2_c8
!n6_r2_c7 !n6_r2_c9
!n6_r2_c7 !n6_r3_c7
!n6_r2_c7 !n6_r4_c7
!n6_r2_c7 !n6_r5_c7
!n6_r2_c7 !n6_r6_c7
!n6_r2_c7 !n6_r7_c7
!n6_r2_c7 !n6_r8_c7
!n6_r2_c7 !n6_r9_c7
!n6_r2_c7 !n6_r1_c8
!n6_r2_c7 !n6_r1_c9
!n6_r2_c7 !n6_r3_c8
!n6_r2_c7 !n6_r3_c9
!n7_r2_c7 !n8_r2_c7
!n7_r2_c7 !n9_r2_c7
!n7_r2_c7 !n7_r2_c8
!n7_r2_c7 !n7_r2_c9
!n7_r2_c7 !n7_r3_c7
!n7_r2_c7 !n7_r4_c7
!n7_r2_c7 !n7_r5_c7
!n7_r2_c7 !n7_r6_c7
!n7_r2_c7 !n7_r7_c7
!n7_r2_c7 !n7_r8_c7
!n7_r2_c7 !n7_r9_c7
!n7_r2_c7 !n7_r1_c8
!n7_r2_c7 !n7_r1_c9
!n7_r2_c7 !n7_r3_c8
!n7_r2_c7 !n7_r3_c9
!n8_r2_c7 !n9_r2_c7
!n8_r2_c7 !n8_r2_c8
!n8_r2_c7 !n8_r2_c9
!n8_r2_c7 !n8_r3_c7
!n8_r2_c7 !n8_r4_c7
!n8_r2_c7 !n8_r5_c7
!n8_r2_c7 !n8_r6_c7
!n8_r2_c7 !n8_r7_c7
!n8_r2_c7 !n8_r8_c7
!n8_r2_c7 !n8_r9_c7
!n8_r2_c7 !n8_r1_c8
!n8_r2_c7 !n8_r1_c9
!n8_r2_c7 !n8_r3_c8
!n8_r2_c7 !n8_r3_c9
!n9_r2_c7 !n9_r2_c8
!n9_r2_c7 !n9_r2_c9
!n9_r2_c7 !n9_r3_c7
!n9_r2_c7 !n9_r4_c7
!n9_r2_c7 !n9_r5_c7
!n9_r2_c7 !n9_r6_c7
!n9_r2_c7 !n9_r7_c7
!n9_r2_c7 !n9_r8_c7
!n9_r2_c7 !n9_r9_c7
!n9_r2_c7 !n9_r1_c8
!n9_r2_c7 !n9_r1_c9
!n9_r2_c7 !n9_r3_c8
!n9_r2_c7 !n9_r3_c9
n1_r2_c7 n2_r2_c7 n3_r2_c7 n4_r2_c7 n5_r2_c7 n6_r2_c7 n7_r2_c7 n8_r2_c7 n9_r2_c7
!n1_r2_c8 !n2_r2_c8
!n1_r2_c8 !n3_r2_c8
!n1_r2_c8 !n4_r2_c8
!n1_r2_c8 !n5_r2_c8
!n1_r2_c8 !n6_r2_c8
!n1_r2_c8 !n7_r2_c8
!n1_r2_c8 !n8_r2_c8
!n1_r2_c8 !n9_r2_c8
!n1_r2_c8 !n1_r2_c9
!n1_r2_c8 !n1_r3_c8
!n1_r2_c8 !n1_r4_c8
!n1_r2_c8 !n1_r5_c8
!n1_r2_c8 !n1_r6_c8
!n1_r2_c8 !n1_r7_c8
!n1_r2_c8 !n1_r8_c8
!n1_r2_c8 !n1_r9_c8
!n1_r2_c8 !n1_r1_c7
!n1_r2_c8 !n1_r1_c9
!n1_r2_c8 !n1_r3_c7
!n1_r2_c8 !n1_r3_c9
!n2_r2_c8 !n3_r2_c8
!n2_r2_c8 !n4_r2_c8
!n2_r2_c8 !n5_r2_c8
!n2_r2_c8 !n6_r2_c8
!n2_r2_c8 !n7_r2_c8
!n2_r2_c8 !n8_r2_c8
!n2_r2_c8 !n9_r2_c8
!n2_r2_c8 !n2_r2_c9
!n2_r2_c8 !n2_r3_c8
!n2_r2_c8 !n2_r4_c8
!n2_r2_c8 !n2_r5_c8
!n2_r2_c8 !n2_r6_c8
!n2_r2_c8 !n2_r7_c8
!n2_r2_c8 !n2_r8_c8
!n2_r2_c8 !n2_r9_c8
!n2_r2_c8 !n2_r1_c7
!n2_r2_c8 !n2_r1_c9
!n2_r2_c8 !n2_r3_c7
!n2_r2_c8 !n2_r3_c9
!n3_r2_c8 !n4_r2_c8
!n3_r2_c8 !n5_r2_c8
!n3_r2_c8 !n6_r2_c8
!n3_r2_c8 !n7_r2_c8
!n3_r2_c8 !n8_r2_c8
!n3_r2_c8 !n9_r2_c8
!n3_r2_c8 !n3_r2_c9
!n3_r2_c8 !n3_r3_c8
!n3_r2_c8 !n3_r4_c8
!n3_r2_c8 !n3_r5_c8
!n3_r2_c8 !n3_r6_c8
!n3_r2_c8 !n3_r7_c8
!n3_r2_c8 !n3_r8_c8
!n3_r2_c8 !n3_r9_c8
!n3_r2_c8 !n3_r1_c7
!n3_r2_c8 !n3_r1_c9
!n3_r2_c8 !n3_r3_c7
!n3_r2_c8 !n3_r3_c9
!n4_r2_c8 !n5_r2_c8
!n4_r2_c8 !n6_r2_c8
!n4_r2_c8 !n7_r2_c8
!n4_r2_c8 !n8_r2_c8
!n4_r2_c8 !n9_r2_c8
!n4_r2_c8 !n4_r2_c9
!n4_r2_c8 !n4_r3_c8
!n4_r2_c8 !n4_r4_c8
!n4_r2_c8 !n4_r5_c8
!n4_r2_c8 !n4_r6_c8
!n4_r2_c8 !n4_r7_c8
!n4_r2_c8 !n4_r8_c8
!n4_r2_c8 !n4_r9_c8
!n4_r2_c8 !n4_r1_c7
!n4_r2_c8 !n4_r1_c9
!n4_r2_c8 !n4_r3_c7
!n4_r2_c8 !n4_r3_c9
!n5_r2_c8 !n6_r2_c8
!n5_r2_c8 !n7_r2_c8
!n5_r2_c8 !n8_r2_c8
!n5_r2_c8 !n9_r2_c8
!n5_r2_c8 !n5_r2_c9
!n5_r2_c8 !n5_r3_c8
!n5_r2_c8 !n5_r4_c8
!n5_r2_c8 !n5_r5_c8
!n5_r2_c8 !n5_r6_c8
!n5_r2_c8 !n5_r7_c8
!n5_r2_c8 !n5_r8_c8
!n5_r2_c8 !n5_r9_c8
!n5_r2_c8 !n5_r1_c7
!n5_r2_c8 !n5_r1_c9
!n5_r2_c8 !n5_r3_c7
!n5_r2_c8 !n5_r3_c9
!n6_r2_c8 !n7_r2_c8
!n6_r2_c8 !n8_r2_c8
!n6_r2_c8 !n9_r2_c8
!n6_r2_c8 !n6_r2_c9
!n6_r2_c8 !n6_r3_c8
!n6_r2_c8 !n6_r4_c8
!n6_r2_c8 !n6_r5_c8
!n6_r2_c8 !n6_r6_c8
!n6_r2_c8 !n6_r7_c8
!n6_r2_c8 !n6_r8_c8
!n6_r2_c8 !n6_r9_c8
!n6_r2_c8 !n6_r1_c7
!n6_r2_c8 !n6_r1_c9
!n6_r2_c8 !n6_r3_c7
!n6_r2_c8 !n6_r3_c9
!n7_r2_c8 !n8_r2_c8
!n7_r2_c8 !n9_r2_c8
!n7_r2_c8 !n7_r2_c9
!n7_r2_c8 !n7_r3_c8
!n7_r2_c8 !n7_r4_c8
!n7_r2_c8 !n7_r5_c8
!n7_r2_c8 !n7_r6_c8
!n7_r2_c8 !n7_r7_c8
!n7_r2_c8 !n7_r8_c8
!n7_r2_c8 !n7_r9_c8
!n7_r2_c8 !n7_r1_c7
!n7_r2_c8 !n7_r1_c9
!n7_r2_c8 !n7_r3_c7
!n7_r2_c8 !n7_r3_c9
!n8_r2_c8 !n9_r2_c8
!n8_r2_c8 !n8_r2_c9
!n8_r2_c8 !n8_r3_c8
!n8_r2_c8 !n8_r4_c8
!n8_r2_c8 !n8_r5_c8
!n8_r2_c8 !n8_r6_c8
!n8_r2_c8 !n8_r7_c8
!n8_r2_c8 !n8_r8_c8
!n8_r2_c8 !n8_r9_c8
!n8_r2_c8 !n8_r1_c7
!n8_r2_c8 !n8_r1_c9
!n8_r2_c8 !n8_r3_c7
!n8_r2_c8 !n8_r3_c9
!n9_r2_c8 !n9_r2_c9
!n9_r2_c8 !n9_r3_c8
!n9_r2_c8 !n9_r4_c8
!n9_r2_c8 !n9_r5_c8
!n9_r2_c8 !n9_r6_c8
!n9_r2_c8 !n9_r7_c8
!n9_r2_c8 !n9_r8_c8
!n9_r2_c8 !n9_r9_c8
!n9_r2_c8 !n9_r1_c7
!n9_r2_c8 !n9_r1_c9
!n9_r2_c8 !n9_r3_c7
!n9_r2_c8 !n9_r3_c9
n1_r2_c8 n2_r2_c8 n3_r2_c8 n4_r2_c8 n5_r2_c8 n6_r2_c8 n7_r2_c8 n8_r2_c8 n9_r2_c8
!n1_r2_c9 !n2_r2_c9
!n1_r2_c9 !n3_r2_c9
!n1_r2_c9 !n4_r2_c9
!n1_r2_c9 !n5_r2_c9
!n1_r2_c9 !n6_r2_c9
!n1_r2_c9 !n7_r2_c9
!n1_r2_c9 !n8_r2_c9
!n1_r2_c9 !n9_r2_c9
!n1_r2_c9 !n1_r3_c9
!n1_r2_c9 !n1_r4_c9
!n1_r2_c9 !n1_r5_c9
!n1_r2_c9 !n1_r6_c9
!n1_r2_c9 !n1_r7_c9
!n1_r2_c9 !n1_r8_c9
!n1_r2_c9 !n1_r9_c9
!n1_r2_c9 !n1_r1_c7
!n1_r2_c9 !n1_r1_c8
!n1_r2_c9 !n1_r3_c7
!n1_r2_c9 !n1_r3_c8
!n2_r2_c9 !n3_r2_c9
!n2_r2_c9 !n4_r2_c9
!n2_r2_c9 !n5_r2_c9
!n2_r2_c9 !n6_r2_c9
!n2_r2_c9 !n7_r2_c9
!n2_r2_c9 !n8_r2_c9
!n2_r2_c9 !n9_r2_c9
!n2_r2_c9 !n2_r3_c9
!n2_r2_c9 !n2_r4_c9
!n2_r2_c9 !n2_r5_c9
!n2_r2_c9 !n2_r6_c9
!n2_r2_c9 !n2_r7_c9
!n2_r2_c9 !n2_r8_c9
!n2_r2_c9 !n2_r9_c9
!n2_r2_c9 !n2_r1_c7
!n2_r2_c9 !n2_r1_c8
!n2_r2_c9 !n2_r3_c7
!n2_r2_c9 !n2_r3_c8
!n3_r2_c9 !n4_r2_c9
!n3_r2_c9 !n5_r2_c9
!n3_r2_c9 !n6_r2_c9
!n3_r2_c9 !n7_r2_c9
!n3_r2_c9 !n8_r2_c9
!n3_r2_c9 !n9_r2_c9
!n3_r2_c9 !n3_r3_c9
!n3_r2_c9 !n3_r4_c9
!n3_r2_c9 !n3_r5_c9
!n3_r2_c9 !n3_r6_c9
!n3_r2_c9 !n3_r7_c9
!n3_r2_c9 !n3_r8_c9
!n3_r2_c9 !n3_r9_c9
!n3_r2_c9 !n3_r1_c7
!n3_r2_c9 !n3_r1_c8
!n3_r2_c9 !n3_r3_c7
!n3_r2_c9 !n3_r3_c8
!n4_r2_c9 !n5_r2_c9
!n4_r2_c9 !n6_r2_c9
!n4_r2_c9 !n7_r2_c9
!n4_r2_c9 !n8_r2_c9
!n4_r2_c9 !n9_r2_c9
!n4_r2_c9 !n4_r3_c9
!n4_r2_c9 !n4_r4_c9
!n4_r2_c9 !n4_r5_c9
!n4_r2_c9 !n4_r6_c9
!n4_r2_c9 !n4_r7_c9
!n4_r2_c9 !n4_r8_c9
!n4_r2_c9 !n4_r9_c9
!n4_r2_c9 !n4_r1_c7
!n4_r2_c9 !n4_r1_c8
!n4_r2_c9 !n4_r3_c7
!n4_r2_c9 !n4_r3_c8
!n5_r2_c9 !n6_r2_c9
!n5_r2_c9 !n7_r2_c9
!n5_r2_c9 !n8_r2_c9
!n5_r2_c9 !n9_r2_c9
!n5_r2_c9 !n5_r3_c9
!n5_r2_c9 !n5_r4_c9
!n5_r2_c9 !n5_r5_c9
!n5_r2_c9 !n5_r6_c9
!n5_r2_c9 !n5_r7_c9
!n5_r2_c9 !n5_r8_c9
!n5_r2_c9 !n5_r9_c9
!n5_r2_c9 !n5_r1_c7
!n5_r2_c9 !n5_r1_c8
!n5_r2_c9 !n5_r3_c7
!n5_r2_c9 !n5_r3_c8
!n6_r2_c9 !n7_r2_c9
!n6_r2_c9 !n8_r2_c9
!n6_r2_c9 !n9_r2_c9
!n6_r2_c9 !n6_r3_c9
!n6_r2_c9 !n6_r4_c9
!n6_r2_c9 !n6_r5_c9
!n6_r2_c9 !n6_r6_c9
!n6_r2_c9 !n6_r7_c9
!n6_r2_c9 !n6_r8_c9
!n6_r2_c9 !n6_r9_c9
!n6_r2_c9 !n6_r1_c7
!n6_r2_c9 !n6_r1_c8
!n6_r2_c9 !n6_r3_c7
!n6_r2_c9 !n6_r3_c8
!n7_r2_c9 !n8_r2_c9
!n7_r2_c9 !n9_r2_c9
!n7_r2_c9 !n7_r3_c9
!n7_r2_c9 !n7_r4_c9
!n7_r2_c9 !n7_r5_c9
!n7_r2_c9 !n7_r6_c9
!n7_r2_c9 !n7_r7_c9
!n7_r2_c9 !n7_r8_c9
!n7_r2_c9 !n7_r9_c9
!n7_r2_c9 !n7_r1_c7
!n7_r2_c9 !n7_r1_c8
!n7_r2_c9 !n7_r3_c7
!n7_r2_c9 !n7_r3_c8
!n8_r2_c9 !n9_r2_c9
!n8_r2_c9 !n8_r3_c9
!n8_r2_c9 !n8_r4_c9
!n8_r2_c9 !n8_r5_c9
!n8_r2_c9 !n8_r6_c9
!n8_r2_c9 !n8_r7_c9
!n8_r2_c9 !n8_r8_c9
!n8_r2_c9 !n8_r9_c9
!n8_r2_c9 !n8_r1_c7
!n8_r2_c9 !n8_r1_c8
!n8_r2_c9 !n8_r3_c7
!n8_r2_c9 !n8_r3_c8
!n9_r2_c9 !n9_r3_c9
!n9_r2_c9 !n9_r4_c9
!n9_r2_c9 !n9_r5_c9
!n9_r2_c9 !n9_r6_c9
!n9_r2_c9 !n9_r7_c9
!n9_r2_c9 !n9_r8_c9
!n9_r2_c9 !n9_r9_c9
!n9_r2_c9 !n9_r1_c7
!n9_r2_c9 !n9_r1_c8
!n9_r2_c9 !n9_r3_c7
!n9_r2_c9 !n9_r3_c8
n1_r2_c9 n2_r2_c9 n3_r2_c9 n4_r2_c9 n5_r2_c9 n6_r2_c9 n7_r2_c9 n8_r2_c9 n9_r2_c9
!n1_r3_c1 !n2_r3_c1
!n1_r3_c1 !n3_r3_c1
!n1_r3_c1 !n4_r3_c1
!n1_r3_c1 !n5_r3_c1
!n1_r3_c1 !n6_r3_c1
!n1_r3_c1 !n7_r3_c1
!n1_r3_c1 !n8_r3_c1
!n1_r3_c1 !n9_r3_c1
!n1_r3_c1 !n1_r3_c2
!n1_r3_c1 !n1_r3_c3
!n1_r3_c1 !n1_r3_c4
!n1_r3_c1 !n1_r3_c5
!n1_r3_c1 !n1_r3_c6
!n1_r3_c1 !n1_r3_c7
!n1_r3_c1 !n1_r3_c8
!n1_r3_c1 !n1_r3_c9
!n1_r3_c1 !n1_r4_c1
!n1_r3_c1 !n1_r5_c1
!n1_r3_c1 !n1_r6_c1
!n1_r3_c1 !n1_r7_c1
!n1_r3_c1 !n1_r8_c1
!n1_r3_c1 !n1_r9_c1
!n1_r3_c1 !n1_r1_c2
!n1_r3_c1 !n1_r1_c3
!n1_r3_c1 !n1_r2_c2
!n1_r3_c1 !n1_r2_c3
!n2_r3_c1 !n3_r3_c1
!n2_r3_c1 !n4_r3_c1
!n2_r3_c1 !n5_r3_c1
!n2_r3_c1 !n6_r3_c1
!n2_r3_c1 !n7_r3_c1
!n2_r3_c1 !n8_r3_c1
!n2_r3_c1 !n9_r3_c1
!n2_r3_c1 !n2_r3_c2
!n2_r3_c1 !n2_r3_c3
!n2_r3_c1 !n2_r3_c4
!n2_r3_c1 !n2_r3_c5
!n2_r3_c1 !n2_r3_c6
!n2_r3_c1 !n2_r3_c7
!n2_r3_c1 !n2_r3_c8
!n2_r3_c1 !n2_r3_c9
!n2_r3_c1 !n2_r4_c1
!n2_r3_c1 !n2_r5_c1
!n2_r3_c1 !n2_r6_c1
!n2_r3_c1 !n2_r7_c1
!n2_r3_c1 !n2_r8_c1
!n2_r3_c1 !n2_r9_c1
!n2_r3_c1 !n2_r1_c2
!n2_r3_c1 !n2_r1_c3
!n2_r3_c1 !n2_r2_c2
!n2_r3_c1 !n2_r2_c3
!n3_r3_c1 !n4_r3_c1
!n3_r3_c1 !n5_r3_c1
!n3_r3_c1 !n6_r3_c1
!n3_r3_c1 !n7_r3_c1
!n3_r3_c1 !n8_r3_c1
!n3_r3_c1 !n9_r3_c1
!n3_r3_c1 !n3_r3_c2
!n3_r3_c1 !n3_r3_c3
!n3_r3_c1 !n3_r3_c4
!n3_r3_c1 !n3_r3_c5
!n3_r3_c1 !n3_r3_c6
!n3_r3_c1 !n3_r3_c7
!n3_r3_c1 !n3_r3_c8
!n3_r3_c1 !n3_r3_c9
!n3_r3_c1 !n3_r4_c1
!n3_r3_c1 !n3_r5_c1
!n3_r3_c1 !n3_r6_c1
!n3_r3_c1 !n3_r7_c1
!n3_r3_c1 !n3_r8_c1
!n3_r3_c1 !n3_r9_c1
!n3_r3_c1 !n3_r1_c2
!n3_r3_c1 !n3_r1_c3
!n3_r3_c1 !n3_r2_c2
!n3_r3_c1 !n3_r2_c3
!n4_r3_c1 !n5_r3_c1
!n4_r3_c1 !n6_r3_c1
!n4_r3_c1 !n7_r3_c1
!n4_r3_c1 !n8_r3_c1
!n4_r3_c1 !n9_r3_c1
!n4_r3_c1 !n4_r3_c2
!n4_r3_c1 !n4_r3_c3
!n4_r3_c1 !n4_r3_c4
!n4_r3_c1 !n4_r3_c5
!n4_r3_c1 !n4_r3_c6
!n4_r3_c1 !n4_r3_c7
!n4_r3_c1 !n4_r3_c8
!n4_r3_c1 !n4_r3_c9
!n4_r3_c1 !n4_r4_c1
!n4_r3_c1 !n4_r5_c1
!n4_r3_c1 !n4_r6_c1
!n4_r3_c1 !n4_r7_c1
!n4_r3_c1 !n4_r8_c1
!n4_r3_c1 !n4_r9_c1
!n4_r3_c1 !n4_r1_c2
!n4_r3_c1 !n4_r1_c3
!n4_r3_c1 !n4_r2_c2
!n4_r3_c1 !n4_r2_c3
!n5_r3_c1 !n6_r3_c1
!n5_r3_c1 !n7_r3_c1
!n5_r3_c1 !n8_r3_c1
!n5_r3_c1 !n9_r3_c1
!n5_r3_c1 !n5_r3_c2
!n5_r3_c1 !n5_r3_c3
!n5_r3_c1 !n5_r3_c4
!n5_r3_c1 !n5_r3_c5
!n5_r3_c1 !n5_r3_c6
!n5_r3_c1 !n5_r3_c7
!n5_r3_c1 !n5_r3_c8
!n5_r3_c1 !n5_r3_c9
!n5_r3_c1 !n5_r4_c1
!n5_r3_c1 !n5_r5_c1
!n5_r3_c1 !n5_r6_c1
!n5_r3_c1 !n5_r7_c1
!n5_r3_c1 !n5_r8_c1
!n5_r3_c1 !n5_r9_c1
!n5_r3_c1 !n5_r1_c2
!n5_r3_c1 !n5_r1_c3
!n5_r3_c1 !n5_r2_c2
!n5_r3_c1 !n5_r2_c3
!n6_r3_c1 !n7_r3_c1
!n6_r3_c1 !n8_r3_c1
!n6_r3_c1 !n9_r3_c1
!n6_r3_c1 !n6_r3_c2
!n6_r3_c1 !n6_r3_c3
!n6_r3_c1 !n6_r3_c4
!n6_r3_c1 !n6_r3_c5
!n6_r3_c1 !n6_r3_c6
!n6_r3_c1 !n6_r3_c7
!n6_r3_c1 !n6_r3_c8
!n6_r3_c1 !n6_r3_c9
!n6_r3_c1 !n6_r4_c1
!n6_r3_c1 !n6_r5_c1
!n6_r3_c1 !n6_r6_c1
!n6_r3_c1 !n6_r7_c1
!n6_r3_c1 !n6_r8_c1
!n6_r3_c1 !n6_r9_c1
!n6_r3_c1 !n6_r1_c2
!n6_r3_c1 !n6_r1_c3
!n6_r3_c1 !n6_r2_c2
!n6_r3_c1 !n6_r2_c3
!n7_r3_c1 !n8_r3_c1
!n7_r3_c1 !n9_r3_c1
!n7_r3_c1 !n7_r3_c2
!n7_r3_c1 !n7_r3_c3
!n7_r3_c1 !n7_r3_c4
!n7_r3_c1 !n7_r3_c5
!n7_r3_c1 !n7_r3_c6
!n7_r3_c1 !n7_r3_c7
!n7_r3_c1 !n7_r3_c8
!n7_r3_c1 !n7_r3_c9
!n7_r3_c1 !n7_r4_c1
!n7_r3_c1 !n7_r5_c1
!n7_r3_c1 !n7_r6_c1
!n7_r3_c1 !n7_r7_c1
!n7_r3_c1 !n7_r8_c1
!n7_r3_c1 !n7_r9_c1
!n7_r3_c1 !n7_r1_c2
!n7_r3_c1 !n7_r1_c3
!n7_r3_c1 !n7_r2_c2
!n7_r3_c1 !n7_r2_c3
!n8_r3_c1 !n9_r3_c1
!n8_r3_c1 !n8_r3_c2
!n8_r3_c1 !n8_r3_c3
!n8_r3_c1 !n8_r3_c4
!n8_r3_c1 !n8_r3_c5
!n8_r3_c1 !n8_r3_c6
!n8_r3_c1 !n8_r3_c7
!n8_r3_c1 !n8_r3_c8
!n8_r3_c1 !n8_r3_c9
!n8_r3_c1 !n8_r4_c1
!n8_r3_c1 !n8_r5_c1
!n8_r3_c1 !n8_r6_c1
!n8_r3_c1 !n8_r7_c1
!n8_r3_c1 !n8_r8_c1
!n8_r3_c1 !n8_r9_c1
!n8_r3_c1 !n8_r1_c2
!n8_r3_c1 !n8_r1_c3
!n8_r3_c1 !n8_r2_c2
!n8_r3_c1 !n8_r2_c3
!n9_r3_c1 !n9_r3_c2
!n9_r3_c1 !n9_r3_c3
!n9_r3_c1 !n9_r3_c4
!n9_r3_c1 !n9_r3_c5
!n9_r3_c1 !n9_r3_c6
!n9_r3_c1 !n9_r3_c7
!n9_r3_c1 !n9_r3_c8
!n9_r3_c1 !n9_r3_c9
!n9_r3_c1 !n9_r4_c1
!n9_r3_c1 !n9_r5_c1
!n9_r3_c1 !n9_r6_c1
!n9_r3_c1 !n9_r7_c1
!n9_r3_c1 !n9_r8_c1
!n9_r3_c1 !n9_r9_c1
!n9_r3_c1 !n9_r1_c2
!n9_r3_c1 !n9_r1_c3
!n9_r3_c1 !n9_r2_c2
!n9_r3_c1 !n9_r2_c3
n1_r3_c1 n2_r3_c1 n3_r3_c1 n4_r3_c1 n5_r3_c1 n6_r3_c1 n7_r3_c1 n8_r3_c1 n9_r3_c1
!n1_r3_c2 !n2_r3_c2
!n1_r3_c2 !n3_r3_c2
!n1_r3_c2 !n4_r3_c2
!n1_r3_c2 !n5_r3_c2
!n1_r3_c2 !n6_r3_c2
!n1_r3_c2 !n7_r3_c2
!n1_r3_c2 !n8_r3_c2
!n1_r3_c2 !n9_r3_c2
!n1_r3_c2 !n1_r3_c3
!n1_r3_c2 !n1_r3_c4
!n1_r3_c2 !n1_r3_c5
!n1_r3_c2 !n1_r3_c6
!n1_r3_c2 !n1_r3_c7
!n1_r3_c2 !n1_r3_c8
!n1_r3_c2 !n1_r3_c9
!n1_r3_c2 !n1_r4_c2
!n1_r3_c2 !n1_r5_c2
!n1_r3_c2 !n1_r6_c2
!n1_r3_c2 !n1_r7_c2
!n1_r3_c2 !n1_r8_c2
!n1_r3_c2 !n1_r9_c2
!n1_r3_c2 !n1_r1_c1
!n1_r3_c2 !n1_r1_c3
!n1_r3_c2 !n1_r2_c1
!n1_r3_c2 !n1_r2_c3
!n2_r3_c2 !n3_r3_c2
!n2_r3_c2 !n4_r3_c2
!n2_r3_c2 !n5_r3_c2
!n2_r3_c2 !n6_r3_c2
!n2_r3_c2 !n7_r3_c2
!n2_r3_c2 !n8_r3_c2
!n2_r3_c2 !n9_r3_c2
!n2_r3_c2 !n2_r3_c3
!n2_r3_c2 !n2_r3_c4
!n2_r3_c2 !n2_r3_c5
!n2_r3_c2 !n2_r3_c6
!n2_r3_c2 !n2_r3_c7
!n2_r3_c2 !n2_r3_c8
!n2_r3_c2 !n2_r3_c9
!n2_r3_c2 !n2_r4_c2
!n2_r3_c2 !n2_r5_c2
!n2_r3_c2 !n2_r6_c2
!n2_r3_c2 !n2_r7_c2
!n2_r3_c2 !n2_r8_c2
!n2_r3_c2 !n2_r9_c2
!n2_r3_c2 !n2_r1_c1
!n2_r3_c2 !n2_r1_c3
!n2_r3_c2 !n2_r2_c1
!n2_r3_c2 !n2_r2_c3
!n3_r3_c2 !n4_r3_c2
!n3_r3_c2 !n5_r3_c2
!n3_r3_c2 !n6_r3_c2
!n3_r3_c2 !n7_r3_c2
!n3_r3_c2 !n8_r3_c2
!n3_r3_c2 !n9_r3_c2
!n3_r3_c2 !n3_r3_c3
!n3_r3_c2 !n3_r3_c4
!n3_r3_c2 !n3_r3_c5
!n3_r3_c2 !n3_r3_c6
!n3_r3_c2 !n3_r3_c7
!n3_r3_c2 !n3_r3_c8
!n3_r3_c2 !n3_r3_c9
!n3_r3_c2 !n3_r4_c2
!n3_r3_c2 !n3_r5_c2
!n3_r3_c2 !n3_r6_c2
!n3_r3_c2 !n3_r7_c2
!n3_r3_c2 !n3_r8_c2
!n3_r3_c2 !n3_r9_c2
!n3_r3_c2 !n3_r1_c1
!n3_r3_c2 !n3_r1_c3
!n3_r3_c2 !n3_r2_c1
!n3_r3_c2 !n3_r2_c3
!n4_r3_c2 !n5_r3_c2
!n4_r3_c2 !n6_r3_c2
!n4_r3_c2 !n7_r3_c2
!n4_r3_c2 !n8_r3_c2
!n4_r3_c2 !n9_r3_c2
!n4_r3_c2 !n4_r3_c3
!n4_r3_c2 !n4_r3_c4
!n4_r3_c2 !n4_r3_c5
!n4_r3_c2 !n4_r3_c6
!n4_r3_c2 !n4_r3_c7
!n4_r3_c2 !n4_r3_c8
!n4_r3_c2 !n4_r3_c9
!n4_r3_c2 !n4_r4_c2
!n4_r3_c2 !n4_r5_c2
!n4_r3_c2 !n4_r6_c2
!n4_r3_c2 !n4_r7_c2
!n4_r3_c2 !n4_r8_c2
!n4_r3_c2 !n4_r9_c2
!n4_r3_c2 !n4_r1_c1
!n4_r3_c2 !n4_r1_c3
!n4_r3_c2 !n4_r2_c1
!n4_r3_c2 !n4_r2_c3
!n5_r3_c2 !n6_r3_c2
!n5_r3_c2 !n7_r3_c2
!n5_r3_c2 !n8_r3_c2
!n5_r3_c2 !n9_r3_c2
!n5_r3_c2 !n5_r3_c3
!n5_r3_c2 !n5_r3_c4
!n5_r3_c2 !n5_r3_c5
!n5_r3_c2 !n5_r3_c6
!n5_r3_c2 !n5_r3_c7
!n5_r3_c2 !n5_r3_c8
!n5_r3_c2 !n5_r3_c9
!n5_r3_c2 !n5_r4_c2
!n5_r3_c2 !n5_r5_c2
!n5_r3_c2 !n5_r6_c2
!n5_r3_c2 !n5_r7_c2
!n5_r3_c2 !n5_r8_c2
!n5_r3_c2 !n5_r9_c2
!n5_r3_c2 !n5_r1_c1
!n5_r3_c2 !n5_r1_c3
!n5_r3_c2 !n5_r2_c1
!n5_r3_c2 !n5_r2_c3
!n6_r3_c2 !n7_r3_c2
!n6_r3_c2 !n8_r3_c2
!n6_r3_c2 !n9_r3_c2
!n6_r3_c2 !n6_r3_c3
!n6_r3_c2 !n6_r3_c4
!n6_r3_c2 !n6_r3_c5
!n6_r3_c2 !n6_r3_c6
!n6_r3_c2 !n6_r3_c7
!n6_r3_c2 !n6_r3_c8
!n6_r3_c2 !n6_r3_c9
!n6_r3_c2 !n6_r4_c2
!n6_r3_c2 !n6_r5_c2
!n6_r3_c2 !n6_r6_c2
!n6_r3_c2 !n6_r7_c2
!n6_r3_c2 !n6_r8_c2
!n6_r3_c2 !n6_r9_c2
!n6_r3_c2 !n6_r1_c1
!n6_r3_c2 !n6_r1_c3
!n6_r3_c2 !n6_r2_c1
!n6_r3_c2 !n6_r2_c3
!n7_r3_c2 !n8_r3_c2
!n7_r3_c2 !n9_r3_c2
!n7_r3_c2 !n7_r3_c3
!n7_r3_c2 !n7_r3_c4
!n7_r3_c2 !n7_r3_c5
!n7_r3_c2 !n7_r3_c6
!n7_r3_c2 !n7_r3_c7
!n7_r3_c2 !n7_r3_c8
!n7_r3_c2 !n7_r3_c9
!n7_r3_c2 !n7_r4_c2
!n7_r3_c2 !n7_r5_c2
!n7_r3_c2 !n7_r6_c2
!n7_r3_c2 !n7_r7_c2
!n7_r3_c2 !n7_r8_c2
!n7_r3_c2 !n7_r9_c2
!n7_r3_c2 !n7_r1_c1
!n7_r3_c2 !n7_r1_c3
!n7_r3_c2 !n7_r2_c1
!n7_r3_c2 !n7_r2_c3
!n8_r3_c2 !n9_r3_c2
!n8_r3_c2 !n8_r3_c3
!n8_r3_c2 !n8_r3_c4
!n8_r3_c2 !n8_r3_c5
!n8_r3_c2 !n8_r3_c6
!n8_r3_c2 !n8_r3_c7
!n8_r3_c2 !n8_r3_c8
!n8_r3_c2 !n8_r3_c9
!n8_r3_c2 !n8_r4_c2
!n8_r3_c2 !n8_r5_c2
!n8_r3_c2 !n8_r6_c2
!n8_r3_c2 !n8_r7_c2
!n8_r3_c2 !n8_r8_c2
!n8_r3_c2 !n8_r9_c2
!n8_r3_c2 !n8_r1_c1
!n8_r3_c2 !n8_r1_c3
!n8_r3_c2 !n8_r2_c1
!n8_r3_c2 !n8_r2_c3
!n9_r3_c2 !n9_r3_c3
!n9_r3_c2 !n9_r3_c4
!n9_r3_c2 !n9_r3_c5
!n9_r3_c2 !n9_r3_c6
!n9_r3_c2 !n9_r3_c7
!n9_r3_c2 !n9_r3_c8
!n9_r3_c2 !n9_r3_c9
!n9_r3_c2 !n9_r4_c2
!n9_r3_c2 !n9_r5_c2
!n9_r3_c2 !n9_r6_c2
!n9_r3_c2 !n9_r7_c2
!n9_r3_c2 !n9_r8_c2
!n9_r3_c2 !n9_r9_c2
!n9_r3_c2 !n9_r1_c1
!n9_r3_c2 !n9_r1_c3
!n9_r3_c2 !n9_r2_c1
!n9_r3_c2 !n9_r2_c3
n1_r3_c2 n2_r3_c2 n3_r3_c2 n4_r3_c2 n5_r3_c2 n6_r3_c2 n7_r3_c2 n8_r3_c2 n9_r3_c2
!n1_r3_c3 !n2_r3_c3
!n1_r3_c3 !n3_r3_c3
!n1_r3_c3 !n4_r3_c3
!n1_r3_c3 !n5_r3_c3
!n1_r3_c3 !n6_r3_c3
!n1_r3_c3 !n7_r3_c3
!n1_r3_c3 !n8_r3_c3
!n1_r3_c3 !n9_r3_c3
!n1_r3_c3 !n1_r3_c4
!n1_r3_c3 !n1_r3_c5
!n1_r3_c3 !n1_r3_c6
!n1_r3_c3 !n1_r3_c7
!n1_r3_c3 !n1_r3_c8
!n1_r3_c3 !n1_r3_c9
!n1_r3_c3 !n1_r4_c3
!n1_r3_c3 !n1_r5_c3
!n1_r3_c3 !n1_r6_c3
!n1_r3_c3 !n1_r7_c3
!n1_r3_c3 !n1_r8_c3
!n1_r3_c3 !n1_r9_c3
!n1_r3_c3 !n1_r1_c1
!n1_r3_c3 !n1_r1_c2
!n1_r3_c3 !n1_r2_c1
!n1_r3_c3 !n1_r2_c2
!n2_r3_c3 !n3_r3_c3
!n2_r3_c3 !n4_r3_c3
!n2_r3_c3 !n5_r3_c3
!n2_r3_c3 !n6_r3_c3
!n2_r3_c3 !n7_r3_c3
!n2_r3_c3 !n8_r3_c3
!n2_r3_c3 !n9_r3_c3
!n2_r3_c3 !n2_r3_c4
!n2_r3_c3 !n2_r3_c5
!n2_r3_c3 !n2_r3_c6
!n2_r3_c3 !n2_r3_c7
!n2_r3_c3 !n2_r3_c8
!n2_r3_c3 !n2_r3_c9
!n2_r3_c3 !n2_r4_c3
!n2_r3_c3 !n2_r5_c3
!n2_r3_c3 !n2_r6_c3
!n2_r3_c3 !n2_r7_c3
!n2_r3_c3 !n2_r8_c3
!n2_r3_c3 !n2_r9_c3
!n2_r3_c3 !n2_r1_c1
!n2_r3_c3 !n2_r1_c2
!n2_r3_c3 !n2_r2_c1
!n2_r3_c3 !n2_r2_c2
!n3_r3_c3 !n4_r3_c3
!n3_r3_c3 !n5_r3_c3
!n3_r3_c3 !n6_r3_c3
!n3_r3_c3 !n7_r3_c3
!n3_r3_c3 !n8_r3_c3
!n3_r3_c3 !n9_r3_c3
!n3_r3_c3 !n3_r3_c4
!n3_r3_c3 !n3_r3_c5
!n3_r3_c3 !n3_r3_c6
!n3_r3_c3 !n3_r3_c7
!n3_r3_c3 !n3_r3_c8
!n3_r3_c3 !n3_r3_c9
!n3_r3_c3 !n3_r4_c3
!n3_r3_c3 !n3_r5_c3
!n3_r3_c3 !n3_r6_c3
!n3_r3_c3 !n3_r7_c3
!n3_r3_c3 !n3_r8_c3
!n3_r3_c3 !n3_r9_c3
!n3_r3_c3 !n3_r1_c1
!n3_r3_c3 !n3_r1_c2
!n3_r3_c3 !n3_r2_c1
!n3_r3_c3 !n3_r2_c2
!n4_r3_c3 !n5_r3_c3
!n4_r3_c3 !n6_r3_c3
!n4_r3_c3 !n7_r3_c3
!n4_r3_c3 !n8_r3_c3
!n4_r3_c3 !n9_r3_c3
!n4_r3_c3 !n4_r3_c4
!n4_r3_c3 !n4_r3_c5
!n4_r3_c3 !n4_r3_c6
!n4_r3_c3 !n4_r3_c7
!n4_r3_c3 !n4_r3_c8
!n4_r3_c3 !n4_r3_c9
!n4_r3_c3 !n4_r4_c3
!n4_r3_c3 !n4_r5_c3
!n4_r3_c3 !n4_r6_c3
!n4_r3_c3 !n4_r7_c3
!n4_r3_c3 !n4_r8_c3
!n4_r3_c3 !n4_r9_c3
!n4_r3_c3 !n4_r1_c1
!n4_r3_c3 !n4_r1_c2
!n4_r3_c3 !n4_r2_c1
!n4_r3_c3 !n4_r2_c2
!n5_r3_c3 !n6_r3_c3
!n5_r3_c3 !n7_r3_c3
!n5_r3_c3 !n8_r3_c3
!n5_r3_c3 !n9_r3_c3
!n5_r3_c3 !n5_r3_c4
!n5_r3_c3 !n5_r3_c5
!n5_r3_c3 !n5_r3_c6
!n5_r3_c3 !n5_r3_c7
!n5_r3_c3 !n5_r3_c8
!n5_r3_c3 !n5_r3_c9
!n5_r3_c3 !n5_r4_c3
!n5_r3_c3 !n5_r5_c3
!n5_r3_c3 !n5_r6_c3
!n5_r3_c3 !n5_r7_c3
!n5_r3_c3 !n5_r8_c3
!n5_r3_c3 !n5_r9_c3
!n5_r3_c3 !n5_r1_c1
!n5_r3_c3 !n5_r1_c2
!n5_r3_c3 !n5_r2_c1
!n5_r3_c3 !n5_r2_c2
!n6_r3_c3 !n7_r3_c3
!n6_r3_c3 !n8_r3_c3
!n6_r3_c3 !n9_r3_c3
!n6_r3_c3 !n6_r3_c4
!n6_r3_c3 !n6_r3_c5
!n6_r3_c3 !n6_r3_c6
!n6_r3_c3 !n6_r3_c7
!n6_r3_c3 !n6_r3_c8
!n6_r3_c3 !n6_r3_c9
!n6_r3_c3 !n6_r4_c3
!n6_r3_c3 !n6_r5_c3
!n6_r3_c3 !n6_r6_c3
!n6_r3_c3 !n6_r7_c3
!n6_r3_c3 !n6_r8_c3
!n6_r3_c3 !n6_r9_c3
!n6_r3_c3 !n6_r1_c1
!n6_r3_c3 !n6_r1_c2
!n6_r3_c3 !n6_r2_c1
!n6_r3_c3 !n6_r2_c2
!n7_r3_c3 !n8_r3_c3
!n7_r3_c3 !n9_r3_c3
!n7_r3_c3 !n7_r3_c4
!n7_r3_c3 !n7_r3_c5
!n7_r3_c3 !n7_r3_c6
!n7_r3_c3 !n7_r3_c7
!n7_r3_c3 !n7_r3_c8
!n7_r3_c3 !n7_r3_c9
!n7_r3_c3 !n7_r4_c3
!n7_r3_c3 !n7_r5_c3
!n7_r3_c3 !n7_r6_c3
!n7_r3_c3 !n7_r7_c3
!n7_r3_c3 !n7_r8_c3
!n7_r3_c3 !n7_r9_c3
!n7_r3_c3 !n7_r1_c1
!n7_r3_c3 !n7_r1_c2
!n7_r3_c3 !n7_r2_c1
!n7_r3_c3 !n7_r2_c2
!n8_r3_c3 !n9_r3_c3
!n8_r3_c3 !n8_r3_c4
!n8_r3_c3 !n8_r3_c5
!n8_r3_c3 !n8_r3_c6
!n8_r3_c3 !n8_r3_c7
!n8_r3_c3 !n8_r3_c8
!n8_r3_c3 !n8_r3_c9
!n8_r3_c3 !n8_r4_c3
!n8_r3_c3 !n8_r5_c3
!n8_r3_c3 !n8_r6_c3
!n8_r3_c3 !n8_r7_c3
!n8_r3_c3 !n8_r8_c3
!n8_r3_c3 !n8_r9_c3
!n8_r3_c3 !n8_r1_c1
!n8_r3_c3 !n8_r1_c2
!n8_r3_c3 !n8_r2_c1
!n8_r3_c3 !n8_r2_c2
!n9_r3_c3 !n9_r3_c4
!n9_r3_c3 !n9_r3_c5
!n9_r3_c3 !n9_r3_c6
!n9_r3_c3 !n9_r3_c7
!n9_r3_c3 !n9_r3_c8
!n9_r3_c3 !n9_r3_c9
!n9_r3_c3 !n9_r4_c3
!n9_r3_c3 !n9_r5_c3
!n9_r3_c3 !n9_r6_c3
!n9_r3_c3 !n9_r7_c3
!n9_r3_c3 !n9_r8_c3
!n9_r3_c3 !n9_r9_c3
!n9_r3_c3 !n9_r1_c1
!n9_r3_c3 !n9_r1_c2
!n9_r3_c3 !n9_r2_c1
!n9_r3_c3 !n9_r2_c2
n1_r3_c3 n2_r3_c3 n3_r3_c3 n4_r3_c3 n5_r3_c3 n6_r3_c3 n7_r3_c3 n8_r3_c3 n9_r3_c3
!n1_r3_c4 !n2_r3_c4
!n1_r3_c4 !n3_r3_c4
!n1_r3_c4 !n4_r3_c4
!n1_r3_c4 !n5_r3_c4
!n1_r3_c4 !n6_r3_c4
!n1_r3_c4 !n7_r3_c4
!n1_r3_c4 !n8_r3_c4
!n1_r3_c4 !n9_r3_c4
!n1_r3_c4 !n1_r3_c5
!n1_r3_c4 !n1_r3_c6
!n1_r3_c4 !n1_r3_c7
!n1_r3_c4 !n1_r3_c8
!n1_r3_c4 !n1_r3_c9
!n1_r3_c4 !n1_r4_c4
!n1_r3_c4 !n1_r5_c4
!n1_r3_c4 !n1_r6_c4
!n1_r3_c4 !n1_r7_c4
!n1_r3_c4 !n1_r8_c4
!n1_r3_c4 !n1_r9_c4
!n1_r3_c4 !n1_r1_c5
!n1_r3_c4 !n1_r1_c6
!n1_r3_c4 !n1_r2_c5
!n1_r3_c4 !n1_r2_c6
!n2_r3_c4 !n3_r3_c4
!n2_r3_c4 !n4_r3_c4
!n2_r3_c4 !n5_r3_c4
!n2_r3_c4 !n6_r3_c4
!n2_r3_c4 !n7_r3_c4
!n2_r3_c4 !n8_r3_c4
!n2_r3_c4 !n9_r3_c4
!n2_r3_c4 !n2_r3_c5
!n2_r3_c4 !n2_r3_c6
!n2_r3_c4 !n2_r3_c7
!n2_r3_c4 !n2_r3_c8
!n2_r3_c4 !n2_r3_c9
!n2_r3_c4 !n2_r4_c4
!n2_r3_c4 !n2_r5_c4
!n2_r3_c4 !n2_r6_c4
!n2_r3_c4 !n2_r7_c4
!n2_r3_c4 !n2_r8_c4
!n2_r3_c4 !n2_r9_c4
!n2_r3_c4 !n2_r1_c5
!n2_r3_c4 !n2_r1_c6
!n2_r3_c4 !n2_r2_c5
!n2_r3_c4 !n2_r2_c6
!n3_r3_c4 !n4_r3_c4
!n3_r3_c4 !n5_r3_c4
!n3_r3_c4 !n6_r3_c4
!n3_r3_c4 !n7_r3_c4
!n3_r3_c4 !n8_r3_c4
!n3_r3_c4 !n9_r3_c4
!n3_r3_c4 !n3_r3_c5
!n3_r3_c4 !n3_r3_c6
!n3_r3_c4 !n3_r3_c7
!n3_r3_c4 !n3_r3_c8
!n3_r3_c4 !n3_r3_c9
!n3_r3_c4 !n3_r4_c4
!n3_r3_c4 !n3_r5_c4
!n3_r3_c4 !n3_r6_c4
!n3_r3_c4 !n3_r7_c4
!n3_r3_c4 !n3_r8_c4
!n3_r3_c4 !n3_r9_c4
!n3_r3_c4 !n3_r1_c5
!n3_r3_c4 !n3_r1_c6
!n3_r3_c4 !n3_r2_c5
!n3_r3_c4 !n3_r2_c6
!n4_r3_c4 !n5_r3_c4
!n4_r3_c4 !n6_r3_c4
!n4_r3_c4 !n7_r3_c4
!n4_r3_c4 !n8_r3_c4
!n4_r3_c4 !n9_r3_c4
!n4_r3_c4 !n4_r3_c5
!n4_r3_c4 !n4_r3_c6
!n4_r3_c4 !n4_r3_c7
!n4_r3_c4 !n4_r3_c8
!n4_r3_c4 !n4_r3_c9
!n4_r3_c4 !n4_r4_c4
!n4_r3_c4 !n4_r5_c4
!n4_r3_c4 !n4_r6_c4
!n4_r3_c4 !n4_r7_c4
!n4_r3_c4 !n4_r8_c4
!n4_r3_c4 !n4_r9_c4
!n4_r3_c4 !n4_r1_c5
!n4_r3_c4 !n4_r1_c6
!n4_r3_c4 !n4_r2_c5
!n4_r3_c4 !n4_r2_c6
!n5_r3_c4 !n6_r3_c4
!n5_r3_c4 !n7_r3_c4
!n5_r3_c4 !n8_r3_c4
!n5_r3_c4 !n9_r3_c4
!n5_r3_c4 !n5_r3_c5
!n5_r3_c4 !n5_r3_c6
!n5_r3_c4 !n5_r3_c7
!n5_r3_c4 !n5_r3_c8
!n5_r3_c4 !n5_r3_c9
!n5_r3_c4 !n5_r4_c4
!n5_r3_c4 !n5_r5_c4
!n5_r3_c4 !n5_r6_c4
!n5_r3_c4 !n5_r7_c4
!n5_r3_c4 !n5_r8_c4
!n5_r3_c4 !n5_r9_c4
!n5_r3_c4 !n5_r1_c5
!n5_r3_c4 !n5_r1_c6
!n5_r3_c4 !n5_r2_c5
!n5_r3_c4 !n5_r2_c6
!n6_r3_c4 !n7_r3_c4
!n6_r3_c4 !n8_r3_c4
!n6_r3_c4 !n9_r3_c4
!n6_r3_c4 !n6_r3_c5
!n6_r3_c4 !n6_r3_c6
!n6_r3_c4 !n6_r3_c7
!n6_r3_c4 !n6_r3_c8
!n6_r3_c4 !n6_r3_c9
!n6_r3_c4 !n6_r4_c4
!n6_r3_c4 !n6_r5_c4
!n6_r3_c4 !n6_r6_c4
!n6_r3_c4 !n6_r7_c4
!n6_r3_c4 !n6_r8_c4
!n6_r3_c4 !n6_r9_c4
!n6_r3_c4 !n6_r1_c5
!n6_r3_c4 !n6_r1_c6
!n6_r3_c4 !n6_r2_c5
!n6_r3_c4 !n6_r2_c6
!n7_r3_c4 !n8_r3_c4
!n7_r3_c4 !n9_r3_c4
!n7_r3_c4 !n7_r3_c5
!n7_r3_c4 !n7_r3_c6
!n7_r3_c4 !n7_r3_c7
!n7_r3_c4 !n7_r3_c8
!n7_r3_c4 !n7_r3_c9
!n7_r3_c4 !n7_r4_c4
!n7_r3_c4 !n7_r5_c4
!n7_r3_c4 !n7_r6_c4
!n7_r3_c4 !n7_r7_c4
!n7_r3_c4 !n7_r8_c4
!n7_r3_c4 !n7_r9_c4
!n7_r3_c4 !n7_r1_c5
!n7_r3_c4 !n7_r1_c6
!n7_r3_c4 !n7_r2_c5
!n7_r3_c4 !n7_r2_c6
!n8_r3_c4 !n9_r3_c4
!n8_r3_c4 !n8_r3_c5
!n8_r3_c4 !n8_r3_c6
!n8_r3_c4 !n8_r3_c7
!n8_r3_c4 !n8_r3_c8
!n8_r3_c4 !n8_r3_c9
!n8_r3_c4 !n8_r4_c4
!n8_r3_c4 !n8_r5_c4
!n8_r3_c4 !n8_r6_c4
!n8_r3_c4 !n8_r7_c4
!n8_r3_c4 !n8_r8_c4
!n8_r3_c4 !n8_r9_c4
!n8_r3_c4 !n8_r1_c5
!n8_r3_c4 !n8_r1_c6
!n8_r3_c4 !n8_r2_c5
!n8_r3_c4 !n8_r2_c6
!n9_r3_c4 !n9_r3_c5
!n9_r3_c4 !n9_r3_c6
!n9_r3_c4 !n9_r3_c7
!n9_r3_c4 !n9_r3_c8
!n9_r3_c4 !n9_r3_c9
!n9_r3_c4 !n9_r4_c4
!n9_r3_c4 !n9_r5_c4
!n9_r3_c4 !n9_r6_c4
!n9_r3_c4 !n9_r7_c4
!n9_r3_c4 !n9_r8_c4
!n9_r3_c4 !n9_r9_c4
!n9_r3_c4 !n9_r1_c5
!n9_r3_c4 !n9_r1_c6
!n9_r3_c4 !n9_r2_c5
!n9_r3_c4 !n9_r2_c6
n1_r3_c4 n2_r3_c4 n3_r3_c4 n4_r3_c4 n5_r3_c4 n6_r3_c4 n7_r3_c4 n8_r3_c4 n9_r3_c4
!n1_r3_c5 !n2_r3_c5
!n1_r3_c5 !n3_r3_c5
!n1_r3_c5 !n4_r3_c5
!n1_r3_c5 !n5_r3_c5
!n1_r3_c5 !n6_r3_c5
!n1_r3_c5 !n7_r3_c5
!n1_r3_c5 !n8_r3_c5
!n1_r3_c5 !n9_r3_c5
!n1_r3_c5 !n1_r3_c6
!n1_r3_c5 !n1_r3_c7
!n1_r3_c5 !n1_r3_c8
!n1_r3_c5 !n1_r3_c9
!n1_r3_c5 !n1_r4_c5
!n1_r3_c5 !n1_r5_c5
!n1_r3_c5 !n1_r6_c5
!n1_r3_c5 !n1_r7_c5
!n1_r3_c5 !n1_r8_c5
!n1_r3_c5 !n1_r9_c5
!n1_r3_c5 !n1_r1_c4
!n1_r3_c5 !n1_r1_c6
!n1_r3_c5 !n1_r2_c4
!n1_r3_c5 !n1_r2_c6
!n2_r3_c5 !n3_r3_c5
!n2_r3_c5 !n4_r3_c5
!n2_r3_c5 !n5_r3_c5
!n2_r3_c5 !n6_r3_c5
!n2_r3_c5 !n7_r3_c5
!n2_r3_c5 !n8_r3_c5
!n2_r3_c5 !n9_r3_c5
!n2_r3_c5 !n2_r3_c6
!n2_r3_c5 !n2_r3_c7
!n2_r3_c5 !n2_r3_c8
!n2_r3_c5 !n2_r3_c9
!n2_r3_c5 !n2_r4_c5
!n2_r3_c5 !n2_r5_c5
!n2_r3_c5 !n2_r6_c5
!n2_r3_c5 !n2_r7_c5
!n2_r3_c5 !n2_r8_c5
!n2_r3_c5 !n2_r9_c5
!n2_r3_c5 !n2_r1_c4
!n2_r3_c5 !n2_r1_c6
!n2_r3_c5 !n2_r2_c4
!n2_r3_c5 !n2_r2_c6
!n3_r3_c5 !n4_r3_c5
!n3_r3_c5 !n5_r3_c5
!n3_r3_c5 !n6_r3_c5
!n3_r3_c5 !n7_r3_c5
!n3_r3_c5 !n8_r3_c5
!n3_r3_c5 !n9_r3_c5
!n3_r3_c5 !n3_r3_c6
!n3_r3_c5 !n3_r3_c7
!n3_r3_c5 !n3_r3_c8
!n3_r3_c5 !n3_r3_c9
!n3_r3_c5 !n3_r4_c5
!n3_r3_c5 !n3_r5_c5
!n3_r3_c5 !n3_r6_c5
!n3_r3_c5 !n3_r7_c5
!n3_r3_c5 !n3_r8_c5
!n3_r3_c5 !n3_r9_c5
!n3_r3_c5 !n3_r1_c4
!n3_r3_c5 !n3_r1_c6
!n3_r3_c5 !n3_r2_c4
!n3_r3_c5 !n3_r2_c6
!n4_r3_c5 !n5_r3_c5
!n4_r3_c5 !n6_r3_c5
!n4_r3_c5 !n7_r3_c5
!n4_r3_c5 !n8_r3_c5
!n4_r3_c5 !n9_r3_c5
!n4_r3_c5 !n4_r3_c6
!n4_r3_c5 !n4_r3_c7
!n4_r3_c5 !n4_r3_c8
!n4_r3_c5 !n4_r3_c9
!n4_r3_c5 !n4_r4_c5
!n4_r3_c5 !n4_r5_c5
!n4_r3_c5 !n4_r6_c5
!n4_r3_c5 !n4_r7_c5
!n4_r3_c5 !n4_r8_c5
!n4_r3_c5 !n4_r9_c5
!n4_r3_c5 !n4_r1_c4
!n4_r3_c5 !n4_r1_c6
!n4_r3_c5 !n4_r2_c4
!n4_r3_c5 !n4_r2_c6
!n5_r3_c5 !n6_r3_c5
!n5_r3_c5 !n7_r3_c5
!n5_r3_c5 !n8_r3_c5
!n5_r3_c5 !n9_r3_c5
!n5_r3_c5 !n5_r3_c6
!n5_r3_c5 !n5_r3_c7
!n5_r3_c5 !n5_r3_c8
!n5_r3_c5 !n5_r3_c9
!n5_r3_c5 !n5_r4_c5
!n5_r3_c5 !n5_r5_c5
!n5_r3_c5 !n5_r6_c5
!n5_r3_c5 !n5_r7_c5
!n5_r3_c5 !n5_r8_c5
!n5_r3_c5 !n5_r9_c5
!n5_r3_c5 !n5_r1_c4
!n5_r3_c5 !n5_r1_c6
!n5_r3_c5 !n5_r2_c4
!n5_r3_c5 !n5_r2_c6
!n6_r3_c5 !n7_r3_c5
!n6_r3_c5 !n8_r3_c5
!n6_r3_c5 !n9_r3_c5
!n6_r3_c5 !n6_r3_c6
!n6_r3_c5 !n6_r3_c7
!n6_r3_c5 !n6_r3_c8
!n6_r3_c5 !n6_r3_c9
!n6_r3_c5 !n6_r4_c5
!n6_r3_c5 !n6_r5_c5
!n6_r3_c5 !n6_r6_c5
!n6_r3_c5 !n6_r7_c5
!n6_r3_c5 !n6_r8_c5
!n6_r3_c5 !n6_r9_c5
!n6_r3_c5 !n6_r1_c4
!n6_r3_c5 !n6_r1_c6
!n6_r3_c5 !n6_r2_c4
!n6_r3_c5 !n6_r2_c6
!n7_r3_c5 !n8_r3_c5
!n7_r3_c5 !n9_r3_c5
!n7_r3_c5 !n7_r3_c6
!n7_r3_c5 !n7_r3_c7
!n7_r3_c5 !n7_r3_c8
!n7_r3_c5 !n7_r3_c9
!n7_r3_c5 !n7_r4_c5
!n7_r3_c5 !n7_r5_c5
!n7_r3_c5 !n7_r6_c5
!n7_r3_c5 !n7_r7_c5
!n7_r3_c5 !n7_r8_c5
!n7_r3_c5 !n7_r9_c5
!n7_r3_c5 !n7_r1_c4
!n7_r3_c5 !n7_r1_c6
!n7_r3_c5 !n7_r2_c4
!n7_r3_c5 !n7_r2_c6
!n8_r3_c5 !n9_r3_c5
!n8_r3_c5 !n8_r3_c6
!n8_r3_c5 !n8_r3_c7
!n8_r3_c5 !n8_r3_c8
!n8_r3_c5 !n8_r3_c9
!n8_r3_c5 !n8_r4_c5
!n8_r3_c5 !n8_r5_c5
!n8_r3_c5 !n8_r6_c5
!n8_r3_c5 !n8_r7_c5
!n8_r3_c5 !n8_r8_c5
!n8_r3_c5 !n8_r9_c5
!n8_r3_c5 !n8_r1_c4
!n8_r3_c5 !n8_r1_c6
!n8_r3_c5 !n8_r2_c4
!n8_r3_c5 !n8_r2_c6
!n9_r3_c5 !n9_r3_c6
!n9_r3_c5 !n9_r3_c7
!n9_r3_c5 !n9_r3_c8
!n9_r3_c5 !n9_r3_c9
!n9_r3_c5 !n9_r4_c5
!n9_r3_c5 !n9_r5_c5
!n9_r3_c5 !n9_r6_c5
!n9_r3_c5 !n9_r7_c5
!n9_r3_c5 !n9_r8_c5
!n9_r3_c5 !n9_r9_c5
!n9_r3_c5 !n9_r1_c4
!n9_r3_c5 !n9_r1_c6
!n9_r3_c5 !n9_r2_c4
!n9_r3_c5 !n9_r2_c6
n1_r3_c5 n2_r3_c5 n3_r3_c5 n4_r3_c5 n5_r3_c5 n6_r3_c5 n7_r3_c5 n8_r3_c5 n9_r3_c5
!n1_r3_c6 !n2_r3_c6
!n1_r3_c6 !n3_r3_c6
!n1_r3_c6 !n4_r3_c6
!n1_r3_c6 !n5_r3_c6
!n1_r3_c6 !n6_r3_c6
!n1_r3_c6 !n7_r3_c6
!n1_r3_c6 !n8_r3_c6
!n1_r3_c6 !n9_r3_c6
!n1_r3_c6 !n1_r3_c7
!n1_r3_c6 !n1_r3_c8
!n1_r3_c6 !n1_r3_c9
!n1_r3_c6 !n1_r4_c6
!n1_r3_c6 !n1_r5_c6
!n1_r3_c6 !n1_r6_c6
!n1_r3_c6 !n1_r7_c6
!n1_r3_c6 !n1_r8_c6
!n1_r3_c6 !n1_r9_c6
!n1_r3_c6 !n1_r1_c4
!n1_r3_c6 !n1_r1_c5
!n1_r3_c6 !n1_r2_c4
!n1_r3_c6 !n1_r2_c5
!n2_r3_c6 !n3_r3_c6
!n2_r3_c6 !n4_r3_c6
!n2_r3_c6 !n5_r3_c6
!n2_r3_c6 !n6_r3_c6
!n2_r3_c6 !n7_r3_c6
!n2_r3_c6 !n8_r3_c6
!n2_r3_c6 !n9_r3_c6
!n2_r3_c6 !n2_r3_c7
!n2_r3_c6 !n2_r3_c8
!n2_r3_c6 !n2_r3_c9
!n2_r3_c6 !n2_r4_c6
!n2_r3_c6 !n2_r5_c6
!n2_r3_c6 !n2_r6_c6
!n2_r3_c6 !n2_r7_c6
!n2_r3_c6 !n2_r8_c6
!n2_r3_c6 !n2_r9_c6
!n2_r3_c6 !n2_r1_c4
!n2_r3_c6 !n2_r1_c5
!n2_r3_c6 !n2_r2_c4
!n2_r3_c6 !n2_r2_c5
!n3_r3_c6 !n4_r3_c6
!n3_r3_c6 !n5_r3_c6
!n3_r3_c6 !n6_r3_c6
!n3_r3_c6 !n7_r3_c6
!n3_r3_c6 !n8_r3_c6
!n3_r3_c6 !n9_r3_c6
!n3_r3_c6 !n3_r3_c7
!n3_r3_c6 !n3_r3_c8
!n3_r3_c6 !n3_r3_c9
!n3_r3_c6 !n3_r4_c6
!n3_r3_c6 !n3_r5_c6
!n3_r3_c6 !n3_r6_c6
!n3_r3_c6 !n3_r7_c6
!n3_r3_c6 !n3_r8_c6
!n3_r3_c6 !n3_r9_c6
!n3_r3_c6 !n3_r1_c4
!n3_r3_c6 !n3_r1_c5
!n3_r3_c6 !n3_r2_c4
!n3_r3_c6 !n3_r2_c5
!n4_r3_c6 !n5_r3_c6
!n4_r3_c6 !n6_r3_c6
!n4_r3_c6 !n7_r3_c6
!n4_r3_c6 !n8_r3_c6
!n4_r3_c6 !n9_r3_c6
!n4_r3_c6 !n4_r3_c7
!n4_r3_c6 !n4_r3_c8
!n4_r3_c6 !n4_r3_c9
!n4_r3_c6 !n4_r4_c6
!n4_r3_c6 !n4_r5_c6
!n4_r3_c6 !n4_r6_c6
!n4_r3_c6 !n4_r7_c6
!n4_r3_c6 !n4_r8_c6
!n4_r3_c6 !n4_r9_c6
!n4_r3_c6 !n4_r1_c4
!n4_r3_c6 !n4_r1_c5
!n4_r3_c6 !n4_r2_c4
!n4_r3_c6 !n4_r2_c5
!n5_r3_c6 !n6_r3_c6
!n5_r3_c6 !n7_r3_c6
!n5_r3_c6 !n8_r3_c6
!n5_r3_c6 !n9_r3_c6
!n5_r3_c6 !n5_r3_c7
!n5_r3_c6 !n5_r3_c8
!n5_r3_c6 !n5_r3_c9
!n5_r3_c6 !n5_r4_c6
!n5_r3_c6 !n5_r5_c6
!n5_r3_c6 !n5_r6_c6
!n5_r3_c6 !n5_r7_c6
!n5_r3_c6 !n5_r8_c6
!n5_r3_c6 !n5_r9_c6
!n5_r3_c6 !n5_r1_c4
!n5_r3_c6 !n5_r1_c5
!n5_r3_c6 !n5_r2_c4
!n5_r3_c6 !n5_r2_c5
!n6_r3_c6 !n7_r3_c6
!n6_r3_c6 !n8_r3_c6
!n6_r3_c6 !n9_r3_c6
!n6_r3_c6 !n6_r3_c7
!n6_r3_c6 !n6_r3_c8
!n6_r3_c6 !n6_r3_c9
!n6_r3_c6 !n6_r4_c6
!n6_r3_c6 !n6_r5_c6
!n6_r3_c6 !n6_r6_c6
!n6_r3_c6 !n6_r7_c6
!n6_r3_c6 !n6_r8_c6
!n6_r3_c6 !n6_r9_c6
!n6_r3_c6 !n6_r1_c4
!n6_r3_c6 !n6_r1_c5
!n6_r3_c6 !n6_r2_c4
!n6_r3_c6 !n6_r2_c5
!n7_r3_c6 !n8_r3_c6
!n7_r3_c6 !n9_r3_c6
!n7_r3_c6 !n7_r3_c7
!n7_r3_c6 !n7_r3_c8
!n7_r3_c6 !n7_r3_c9
!n7_r3_c6 !n7_r4_c6
!n7_r3_c6 !n7_r5_c6
!n7_r3_c6 !n7_r6_c6
!n7_r3_c6 !n7_r7_c6
!n7_r3_c6 !n7_r8_c6
!n7_r3_c6 !n7_r9_c6
!n7_r3_c6 !n7_r1_c4
!n7_r3_c6 !n7_r1_c5
!n7_r3_c6 !n7_r2_c4
!n7_r3_c6 !n7_r2_c5
!n8_r3_c6 !n9_r3_c6
!n8_r3_c6 !n8_r3_c7
!n8_r3_c6 !n8_r3_c8
!n8_r3_c6 !n8_r3_c9
!n8_r3_c6 !n8_r4_c6
!n8_r3_c6 !n8_r5_c6
!n8_r3_c6 !n8_r6_c6
!n8_r3_c6 !n8_r7_c6
!n8_r3_c6 !n8_r8_c6
!n8_r3_c6 !n8_r9_c6
!n8_r3_c6 !n8_r1_c4
!n8_r3_c6 !n8_r1_c5
!n8_r3_c6 !n8_r2_c4
!n8_r3_c6 !n8_r2_c5
!n9_r3_c6 !n9_r3_c7
!n9_r3_c6 !n9_r3_c8
!n9_r3_c6 !n9_r3_c9
!n9_r3_c6 !n9_r4_c6
!n9_r3_c6 !n9_r5_c6
!n9_r3_c6 !n9_r6_c6
!n9_r3_c6 !n9_r7_c6
!n9_r3_c6 !n9_r8_c6
!n9_r3_c6 !n9_r9_c6
!n9_r3_c6 !n9_r1_c4
!n9_r3_c6 !n9_r1_c5
!n9_r3_c6 !n9_r2_c4
!n9_r3_c6 !n9_r2_c5
n1_r3_c6 n2_r3_c6 n3_r3_c6 n4_r3_c6 n5_r3_c6 n6_r3_c6 n7_r3_c6 n8_r3_c6 n9_r3_c6
!n1_r3_c7 !n2_r3_c7
!n1_r3_c7 !n3_r3_c7
!n1_r3_c7 !n4_r3_c7
!n1_r3_c7 !n5_r3_c7
!n1_r3_c7 !n6_r3_c7
!n1_r3_c7 !n7_r3_c7
!n1_r3_c7 !n8_r3_c7
!n1_r3_c7 !n9_r3_c7
!n1_r3_c7 !n1_r3_c8
!n1_r3_c7 !n1_r3_c9
!n1_r3_c7 !n1_r4_c7
!n1_r3_c7 !n1_r5_c7
!n1_r3_c7 !n1_r6_c7
!n1_r3_c7 !n1_r7_c7
!n1_r3_c7 !n1_r8_c7
!n1_r3_c7 !n1_r9_c7
!n1_r3_c7 !n1_r1_c8
!n1_r3_c7 !n1_r1_c9
!n1_r3_c7 !n1_r2_c8
!n1_r3_c7 !n1_r2_c9
!n2_r3_c7 !n3_r3_c7
!n2_r3_c7 !n4_r3_c7
!n2_r3_c7 !n5_r3_c7
!n2_r3_c7 !n6_r3_c7
!n2_r3_c7 !n7_r3_c7
!n2_r3_c7 !n8_r3_c7
!n2_r3_c7 !n9_r3_c7
!n2_r3_c7 !n2_r3_c8
!n2_r3_c7 !n2_r3_c9
!n2_r3_c7 !n2_r4_c7
!n2_r3_c7 !n2_r5_c7
!n2_r3_c7 !n2_r6_c7
!n2_r3_c7 !n2_r7_c7
!n2_r3_c7 !n2_r8_c7
!n2_r3_c7 !n2_r9_c7
!n2_r3_c7 !n2_r1_c8
!n2_r3_c7 !n2_r1_c9
!n2_r3_c7 !n2_r2_c8
!n2_r3_c7 !n2_r2_c9
!n3_r3_c7 !n4_r3_c7
!n3_r3_c7 !n5_r3_c7
!n3_r3_c7 !n6_r3_c7
!n3_r3_c7 !n7_r3_c7
!n3_r3_c7 !n8_r3_c7
!n3_r3_c7 !n9_r3_c7
!n3_r3_c7 !n3_r3_c8
!n3_r3_c7 !n3_r3_c9
!n3_r3_c7 !n3_r4_c7
!n3_r3_c7 !n3_r5_c7
!n3_r3_c7 !n3_r6_c7
!n3_r3_c7 !n3_r7_c7
!n3_r3_c7 !n3_r8_c7
!n3_r3_c7 !n3_r9_c7
!n3_r3_c7 !n3_r1_c8
!n3_r3_c7 !n3_r1_c9
!n3_r3_c7 !n3_r2_c8
!n3_r3_c7 !n3_r2_c9
!n4_r3_c7 !n5_r3_c7
!n4_r3_c7 !n6_r3_c7
!n4_r3_c7 !n7_r3_c7
!n4_r3_c7 !n8_r3_c7
!n4_r3_c7 !n9_r3_c7
!n4_r3_c7 !n4_r3_c8
!n4_r3_c7 !n4_r3_c9
!n4_r3_c7 !n4_r4_c7
!n4_r3_c7 !n4_r5_c7
!n4_r3_c7 !n4_r6_c7
!n4_r3_c7 !n4_r7_c7
!n4_r3_c7 !n4_r8_c7
!n4_r3_c7 !n4_r9_c7
!n4_r3_c7 !n4_r1_c8
!n4_r3_c7 !n4_r1_c9
!n4_r3_c7 !n4_r2_c8
!n4_r3_c7 !n4_r2_c9
!n5_r3_c7 !n6_r3_c7
!n5_r3_c7 !n7_r3_c7
!n5_r3_c7 !n8_r3_c7
!n5_r3_c7 !n9_r3_c7
!n5_r3_c7 !n5_r3_c8
!n5_r3_c7 !n5_r3_c9
!n5_r3_c7 !n5_r4_c7
!n5_r3_c7 !n5_r5_c7
!n5_r3_c7 !n5_r6_c7
!n5_r3_c7 !n5_r7_c7
!n5_r3_c7 !n5_r8_c7
!n5_r3_c7 !n5_r9_c7
!n5_r3_c7 !n5_r1_c8
!n5_r3_c7 !n5_r1_c9
!n5_r3_c7 !n5_r2_c8
!n5_r3_c7 !n5_r2_c9
!n6_r3_c7 !n7_r3_c7
!n6_r3_c7 !n8_r3_c7
!n6_r3_c7 !n9_r3_c7
!n6_r3_c7 !n6_r3_c8
!n6_r3_c7 !n6_r3_c9
!n6_r3_c7 !n6_r4_c7
!n6_r3_c7 !n6_r5_c7
!n6_r3_c7 !n6_r6_c7
!n6_r3_c7 !n6_r7_c7
!n6_r3_c7 !n6_r8_c7
!n6_r3_c7 !n6_r9_c7
!n6_r3_c7 !n6_r1_c8
!n6_r3_c7 !n6_r1_c9
!n6_r3_c7 !n6_r2_c8
!n6_r3_c7 !n6_r2_c9
!n7_r3_c7 !n8_r3_c7
!n7_r3_c7 !n9_r3_c7
!n7_r3_c7 !n7_r3_c8
!n7_r3_c7 !n7_r3_c9
!n7_r3_c7 !n7_r4_c7
!n7_r3_c7 !n7_r5_c7
!n7_r3_c7 !n7_r6_c7
!n7_r3_c7 !n7_r7_c7
!n7_r3_c7 !n7_r8_c7
!n7_r3_c7 !n7_r9_c7
!n7_r3_c7 !n7_r1_c8
!n7_r3_c7 !n7_r1_c9
!n7_r3_c7 !n7_r2_c8
!n7_r3_c7 !n7_r2_c9
!n8_r3_c7 !n9_r3_c7
!n8_r3_c7 !n8_r3_c8
!n8_r3_c7 !n8_r3_c9
!n8_r3_c7 !n8_r4_c7
!n8_r3_c7 !n8_r5_c7
!n8_r3_c7 !n8_r6_c7
!n8_r3_c7 !n8_r7_c7
!n8_r3_c7 !n8_r8_c7
!n8_r3_c7 !n8_r9_c7
!n8_r3_c7 !n8_r1_c8
!n8_r3_c7 !n8_r1_c9
!n8_r3_c7 !n8_r2_c8
!n8_r3_c7 !n8_r2_c9
!n9_r3_c7 !n9_r3_c8
!n9_r3_c7 !n9_r3_c9
!n9_r3_c7 !n9_r4_c7
!n9_r3_c7 !n9_r5_c7
!n9_r3_c7 !n9_r6_c7
!n9_r3_c7 !n9_r7_c7
!n9_r3_c7 !n9_r8_c7
!n9_r3_c7 !n9_r9_c7
!n9_r3_c7 !n9_r1_c8
!n9_r3_c7 !n9_r1_c9
!n9_r3_c7 !n9_r2_c8
!n9_r3_c7 !n9_r2_c9
n1_r3_c7 n2_r3_c7 n3_r3_c7 n4_r3_c7 n5_r3_c7 n6_r3_c7 n7_r3_c7 n8_r3_c7 n9_r3_c7
!n1_r3_c8 !n2_r3_c8
!n1_r3_c8 !n3_r3_c8
!n1_r3_c8 !n4_r3_c8
!n1_r3_c8 !n5_r3_c8
!n1_r3_c8 !n6_r3_c8
!n1_r3_c8 !n7_r3_c8
!n1_r3_c8 !n8_r3_c8
!n1_r3_c8 !n9_r3_c8
!n1_r3_c8 !n1_r3_c9
!n1_r3_c8 !n1_r4_c8
!n1_r3_c8 !n1_r5_c8
!n1_r3_c8 !n1_r6_c8
!n1_r3_c8 !n1_r7_c8
!n1_r3_c8 !n1_r8_c8
!n1_r3_c8 !n1_r9_c8
!n1_r3_c8 !n1_r1_c7
!n1_r3_c8 !n1_r1_c9
!n1_r3_c8 !n1_r2_c7
!n1_r3_c8 !n1_r2_c9
!n2_r3_c8 !n3_r3_c8
!n2_r3_c8 !n4_r3_c8
!n2_r3_c8 !n5_r3_c8
!n2_r3_c8 !n6_r3_c8
!n2_r3_c8 !n7_r3_c8
!n2_r3_c8 !n8_r3_c8
!n2_r3_c8 !n9_r3_c8
!n2_r3_c8 !n2_r3_c9
!n2_r3_c8 !n2_r4_c8
!n2_r3_c8 !n2_r5_c8
!n2_r3_c8 !n2_r6_c8
!n2_r3_c8 !n2_r7_c8
!n2_r3_c8 !n2_r8_c8
!n2_r3_c8 !n2_r9_c8
!n2_r3_c8 !n2_r1_c7
!n2_r3_c8 !n2_r1_c9
!n2_r3_c8 !n2_r2_c7
!n2_r3_c8 !n2_r2_c9
!n3_r3_c8 !n4_r3_c8
!n3_r3_c8 !n5_r3_c8
!n3_r3_c8 !n6_r3_c8
!n3_r3_c8 !n7_r3_c8
!n3_r3_c8 !n8_r3_c8
!n3_r3_c8 !n9_r3_c8
!n3_r3_c8 !n3_r3_c9
!n3_r3_c8 !n3_r4_c8
!n3_r3_c8 !n3_r5_c8
!n3_r3_c8 !n3_r6_c8
!n3_r3_c8 !n3_r7_c8
!n3_r3_c8 !n3_r8_c8
!n3_r3_c8 !n3_r9_c8
!n3_r3_c8 !n3_r1_c7
!n3_r3_c8 !n3_r1_c9
!n3_r3_c8 !n3_r2_c7
!n3_r3_c8 !n3_r2_c9
!n4_r3_c8 !n5_r3_c8
!n4_r3_c8 !n6_r3_c8
!n4_r3_c8 !n7_r3_c8
!n4_r3_c8 !n8_r3_c8
!n4_r3_c8 !n9_r3_c8
!n4_r3_c8 !n4_r3_c9
!n4_r3_c8 !n4_r4_c8
!n4_r3_c8 !n4_r5_c8
!n4_r3_c8 !n4_r6_c8
!n4_r3_c8 !n4_r7_c8
!n4_r3_c8 !n4_r8_c8
!n4_r3_c8 !n4_r9_c8
!n4_r3_c8 !n4_r1_c7
!n4_r3_c8 !n4_r1_c9
!n4_r3_c8 !n4_r2_c7
!n4_r3_c8 !n4_r2_c9
!n5_r3_c8 !n6_r3_c8
!n5_r3_c8 !n7_r3_c8
!n5_r3_c8 !n8_r3_c8
!n5_r3_c8 !n9_r3_c8
!n5_r3_c8 !n5_r3_c9
!n5_r3_c8 !n5_r4_c8
!n5_r3_c8 !n5_r5_c8
!n5_r3_c8 !n5_r6_c8
!n5_r3_c8 !n5_r7_c8
!n5_r3_c8 !n5_r8_c8
!n5_r3_c8 !n5_r9_c8
!n5_r3_c8 !n5_r1_c7
!n5_r3_c8 !n5_r1_c9
!n5_r3_c8 !n5_r2_c7
!n5_r3_c8 !n5_r2_c9
!n6_r3_c8 !n7_r3_c8
!n6_r3_c8 !n8_r3_c8
!n6_r3_c8 !n9_r3_c8
!n6_r3_c8 !n6_r3_c9
!n6_r3_c8 !n6_r4_c8
!n6_r3_c8 !n6_r5_c8
!n6_r3_c8 !n6_r6_c8
!n6_r3_c8 !n6_r7_c8
!n6_r3_c8 !n6_r8_c8
!n6_r3_c8 !n6_r9_c8
!n6_r3_c8 !n6_r1_c7
!n6_r3_c8 !n6_r1_c9
!n6_r3_c8 !n6_r2_c7
!n6_r3_c8 !n6_r2_c9
!n7_r3_c8 !n8_r3_c8
!n7_r3_c8 !n9_r3_c8
!n7_r3_c8 !n7_r3_c9
!n7_r3_c8 !n7_r4_c8
!n7_r3_c8 !n7_r5_c8
!n7_r3_c8 !n7_r6_c8
!n7_r3_c8 !n7_r7_c8
!n7_r3_c8 !n7_r8_c8
!n7_r3_c8 !n7_r9_c8
!n7_r3_c8 !n7_r1_c7
!n7_r3_c8 !n7_r1_c9
!n7_r3_c8 !n7_r2_c7
!n7_r3_c8 !n7_r2_c9
!n8_r3_c8 !n9_r3_c8
!n8_r3_c8 !n8_r3_c9
!n8_r3_c8 !n8_r4_c8
!n8_r3_c8 !n8_r5_c8
!n8_r3_c8 !n8_r6_c8
!n8_r3_c8 !n8_r7_c8
!n8_r3_c8 !n8_r8_c8
!n8_r3_c8 !n8_r9_c8
!n8_r3_c8 !n8_r1_c7
!n8_r3_c8 !n8_r1_c9
!n8_r3_c8 !n8_r2_c7
!n8_r3_c8 !n8_r2_c9
!n9_r3_c8 !n9_r3_c9
!n9_r3_c8 !n9_r4_c8
!n9_r3_c8 !n9_r5_c8
!n9_r3_c8 !n9_r6_c8
!n9_r3_c8 !n9_r7_c8
!n9_r3_c8 !n9_r8_c8
!n9_r3_c8 !n9_r9_c8
!n9_r3_c8 !n9_r1_c7
!n9_r3_c8 !n9_r1_c9
!n9_r3_c8 !n9_r2_c7
!n9_r3_c8 !n9_r2_c9
n1_r3_c8 n2_r3_c8 n3_r3_c8 n4_r3_c8 n5_r3_c8 n6_r3_c8 n7_r3_c8 n8_r3_c8 n9_r3_c8
!n1_r3_c9 !n2_r3_c9
!n1_r3_c9 !n3_r3_c9
!n1_r3_c9 !n4_r3_c9
!n1_r3_c9 !n5_r3_c9
!n1_r3_c9 !n6_r3_c9
!n1_r3_c9 !n7_r3_c9
!n1_r3_c9 !n8_r3_c9
!n1_r3_c9 !n9_r3_c9
!n1_r3_c9 !n1_r4_c9
!n1_r3_c9 !n1_r5_c9
!n1_r3_c9 !n1_r6_c9
!n1_r3_c9 !n1_r7_c9
!n1_r3_c9 !n1_r8_c9
!n1_r3_c9 !n1_r9_c9
!n1_r3_c9 !n1_r1_c7
!n1_r3_c9 !n1_r1_c8
!n1_r3_c9 !n1_r2_c7
!n1_r3_c9 !n1_r2_c8
!n2_r3_c9 !n3_r3_c9
!n2_r3_c9 !n4_r3_c9
!n2_r3_c9 !n5_r3_c9
!n2_r3_c9 !n6_r3_c9
!n2_r3_c9 !n7_r3_c9
!n2_r3_c9 !n8_r3_c9
!n2_r3_c9 !n9_r3_c9
!n2_r3_c9 !n2_r4_c9
!n2_r3_c9 !n2_r5_c9
!n2_r3_c9 !n2_r6_c9
!n2_r3_c9 !n2_r7_c9
!n2_r3_c9 !n2_r8_c9
!n2_r3_c9 !n2_r9_c9
!n2_r3_c9 !n2_r1_c7
!n2_r3_c9 !n2_r1_c8
!n2_r3_c9 !n2_r2_c7
!n2_r3_c9 !n2_r2_c8
!n3_r3_c9 !n4_r3_c9
!n3_r3_c9 !n5_r3_c9
!n3_r3_c9 !n6_r3_c9
!n3_r3_c9 !n7_r3_c9
!n3_r3_c9 !n8_r3_c9
!n3_r3_c9 !n9_r3_c9
!n3_r3_c9 !n3_r4_c9
!n3_r3_c9 !n3_r5_c9
!n3_r3_c9 !n3_r6_c9
!n3_r3_c9 !n3_r7_c9
!n3_r3_c9 !n3_r8_c9
!n3_r3_c9 !n3_r9_c9
!n3_r3_c9 !n3_r1_c7
!n3_r3_c9 !n3_r1_c8
!n3_r3_c9 !n3_r2_c7
!n3_r3_c9 !n3_r2_c8
!n4_r3_c9 !n5_r3_c9
!n4_r3_c9 !n6_r3_c9
!n4_r3_c9 !n7_r3_c9
!n4_r3_c9 !n8_r3_c9
!n4_r3_c9 !n9_r3_c9
!n4_r3_c9 !n4_r4_c9
!n4_r3_c9 !n4_r5_c9
!n4_r3_c9 !n4_r6_c9
!n4_r3_c9 !n4_r7_c9
!n4_r3_c9 !n4_r8_c9
!n4_r3_c9 !n4_r9_c9
!n4_r3_c9 !n4_r1_c7
!n4_r3_c9 !n4_r1_c8
!n4_r3_c9 !n4_r2_c7
!n4_r3_c9 !n4_r2_c8
!n5_r3_c9 !n6_r3_c9
!n5_r3_c9 !n7_r3_c9
!n5_r3_c9 !n8_r3_c9
!n5_r3_c9 !n9_r3_c9
!n5_r3_c9 !n5_r4_c9
!n5_r3_c9 !n5_r5_c9
!n5_r3_c9 !n5_r6_c9
!n5_r3_c9 !n5_r7_c9
!n5_r3_c9 !n5_r8_c9
!n5_r3_c9 !n5_r9_c9
!n5_r3_c9 !n5_r1_c7
!n5_r3_c9 !n5_r1_c8
!n5_r3_c9 !n5_r2_c7
!n5_r3_c9 !n5_r2_c8
!n6_r3_c9 !n7_r3_c9
!n6_r3_c9 !n8_r3_c9
!n6_r3_c9 !n9_r3_c9
!n6_r3_c9 !n6_r4_c9
!n6_r3_c9 !n6_r5_c9
!n6_r3_c9 !n6_r6_c9
!n6_r3_c9 !n6_r7_c9
!n6_r3_c9 !n6_r8_c9
!n6_r3_c9 !n6_r9_c9
!n6_r3_c9 !n6_r1_c7
!n6_r3_c9 !n6_r1_c8
!n6_r3_c9 !n6_r2_c7
!n6_r3_c9 !n6_r2_c8
!n7_r3_c9 !n8_r3_c9
!n7_r3_c9 !n9_r3_c9
!n7_r3_c9 !n7_r4_c9
!n7_r3_c9 !n7_r5_c9
!n7_r3_c9 !n7_r6_c9
!n7_r3_c9 !n7_r7_c9
!n7_r3_c9 !n7_r8_c9
!n7_r3_c9 !n7_r9_c9
!n7_r3_c9 !n7_r1_c7
!n7_r3_c9 !n7_r1_c8
!n7_r3_c9 !n7_r2_c7
!n7_r3_c9 !n7_r2_c8
!n8_r3_c9 !n9_r3_c9
!n8_r3_c9 !n8_r4_c9
!n8_r3_c9 !n8_r5_c9
!n8_r3_c9 !n8_r6_c9
!n8_r3_c9 !n8_r7_c9
!n8_r3_c9 !n8_r8_c9
!n8_r3_c9 !n8_r9_c9
!n8_r3_c9 !n8_r1_c7
!n8_r3_c9 !n8_r1_c8
!n8_r3_c9 !n8_r2_c7
!n8_r3_c9 !n8_r2_c8
!n9_r3_c9 !n9_r4_c9
!n9_r3_c9 !n9_r5_c9
!n9_r3_c9 !n9_r6_c9
!n9_r3_c9 !n9_r7_c9
!n9_r3_c9 !n9_r8_c9
!n9_r3_c9 !n9_r9_c9
!n9_r3_c9 !n9_r1_c7
!n9_r3_c9 !n9_r1_c8
!n9_r3_c9 !n9_r2_c7
!n9_r3_c9 !n9_r2_c8
n1_r3_c9 n2_r3_c9 n3_r3_c9 n4_r3_c9 n5_r3_c9 n6_r3_c9 n7_r3_c9 n8_r3_c9 n9_r3_c9
!n1_r4_c1 !n2_r4_c1
!n1_r4_c1 !n3_r4_c1
!n1_r4_c1 !n4_r4_c1
!n1_r4_c1 !n5_r4_c1
!n1_r4_c1 !n6_r4_c1
!n1_r4_c1 !n7_r4_c1
!n1_r4_c1 !n8_r4_c1
!n1_r4_c1 !n9_r4_c1
!n1_r4_c1 !n1_r4_c2
!n1_r4_c1 !n1_r4_c3
!n1_r4_c1 !n1_r4_c4
!n1_r4_c1 !n1_r4_c5
!n1_r4_c1 !n1_r4_c6
!n1_r4_c1 !n1_r4_c7
!n1_r4_c1 !n1_r4_c8
!n1_r4_c1 !n1_r4_c9
!n1_r4_c1 !n1_r5_c1
!n1_r4_c1 !n1_r6_c1
!n1_r4_c1 !n1_r7_c1
!n1_r4_c1 !n1_r8_c1
!n1_r4_c1 !n1_r9_c1
!n1_r4_c1 !n1_r5_c2
!n1_r4_c1 !n1_r5_c3
!n1_r4_c1 !n1_r6_c2
!n1_r4_c1 !n1_r6_c3
!n2_r4_c1 !n3_r4_c1
!n2_r4_c1 !n4_r4_c1
!n2_r4_c1 !n5_r4_c1
!n2_r4_c1 !n6_r4_c1
!n2_r4_c1 !n7_r4_c1
!n2_r4_c1 !n8_r4_c1
!n2_r4_c1 !n9_r4_c1
!n2_r4_c1 !n2_r4_c2
!n2_r4_c1 !n2_r4_c3
!n2_r4_c1 !n2_r4_c4
!n2_r4_c1 !n2_r4_c5
!n2_r4_c1 !n2_r4_c6
!n2_r4_c1 !n2_r4_c7
!n2_r4_c1 !n2_r4_c8
!n2_r4_c1 !n2_r4_c9
!n2_r4_c1 !n2_r5_c1
!n2_r4_c1 !n2_r6_c1
!n2_r4_c1 !n2_r7_c1
!n2_r4_c1 !n2_r8_c1
!n2_r4_c1 !n2_r9_c1
!n2_r4_c1 !n2_r5_c2
!n2_r4_c1 !n2_r5_c3
!n2_r4_c1 !n2_r6_c2
!n2_r4_c1 !n2_r6_c3
!n3_r4_c1 !n4_r4_c1
!n3_r4_c1 !n5_r4_c1
!n3_r4_c1 !n6_r4_c1
!n3_r4_c1 !n7_r4_c1
!n3_r4_c1 !n8_r4_c1
!n3_r4_c1 !n9_r4_c1
!n3_r4_c1 !n3_r4_c2
!n3_r4_c1 !n3_r4_c3
!n3_r4_c1 !n3_r4_c4
!n3_r4_c1 !n3_r4_c5
!n3_r4_c1 !n3_r4_c6
!n3_r4_c1 !n3_r4_c7
!n3_r4_c1 !n3_r4_c8
!n3_r4_c1 !n3_r4_c9
!n3_r4_c1 !n3_r5_c1
!n3_r4_c1 !n3_r6_c1
!n3_r4_c1 !n3_r7_c1
!n3_r4_c1 !n3_r8_c1
!n3_r4_c1 !n3_r9_c1
!n3_r4_c1 !n3_r5_c2
!n3_r4_c1 !n3_r5_c3
!n3_r4_c1 !n3_r6_c2
!n3_r4_c1 !n3_r6_c3
!n4_r4_c1 !n5_r4_c1
!n4_r4_c1 !n6_r4_c1
!n4_r4_c1 !n7_r4_c1
!n4_r4_c1 !n8_r4_c1
!n4_r4_c1 !n9_r4_c1
!n4_r4_c1 !n4_r4_c2
!n4_r4_c1 !n4_r4_c3
!n4_r4_c1 !n4_r4_c4
!n4_r4_c1 !n4_r4_c5
!n4_r4_c1 !n4_r4_c6
!n4_r4_c1 !n4_r4_c7
!n4_r4_c1 !n4_r4_c8
!n4_r4_c1 !n4_r4_c9
!n4_r4_c1 !n4_r5_c1
!n4_r4_c1 !n4_r6_c1
!n4_r4_c1 !n4_r7_c1
!n4_r4_c1 !n4_r8_c1
!n4_r4_c1 !n4_r9_c1
!n4_r4_c1 !n4_r5_c2
!n4_r4_c1 !n4_r5_c3
!n4_r4_c1 !n4_r6_c2
!n4_r4_c1 !n4_r6_c3
!n5_r4_c1 !n6_r4_c1
!n5_r4_c1 !n7_r4_c1
!n5_r4_c1 !n8_r4_c1
!n5_r4_c1 !n9_r4_c1
!n5_r4_c1 !n5_r4_c2
!n5_r4_c1 !n5_r4_c3
!n5_r4_c1 !n5_r4_c4
!n5_r4_c1 !n5_r4_c5
!n5_r4_c1 !n5_r4_c6
!n5_r4_c1 !n5_r4_c7
!n5_r4_c1 !n5_r4_c8
!n5_r4_c1 !n5_r4_c9
!n5_r4_c1 !n5_r5_c1
!n5_r4_c1 !n5_r6_c1
!n5_r4_c1 !n5_r7_c1
!n5_r4_c1 !n5_r8_c1
!n5_r4_c1 !n5_r9_c1
!n5_r4_c1 !n5_r5_c2
!n5_r4_c1 !n5_r5_c3
!n5_r4_c1 !n5_r6_c2
!n5_r4_c1 !n5_r6_c3
!n6_r4_c1 !n7_r4_c1
!n6_r4_c1 !n8_r4_c1
!n6_r4_c1 !n9_r4_c1
!n6_r4_c1 !n6_r4_c2
!n6_r4_c1 !n6_r4_c3
!n6_r4_c1 !n6_r4_c4
!n6_r4_c1 !n6_r4_c5
!n6_r4_c1 !n6_r4_c6
!n6_r4_c1 !n6_r4_c7
!n6_r4_c1 !n6_r4_c8
!n6_r4_c1 !n6_r4_c9
!n6_r4_c1 !n6_r5_c1
!n6_r4_c1 !n6_r6_c1
!n6_r4_c1 !n6_r7_c1
!n6_r4_c1 !n6_r8_c1
!n6_r4_c1 !n6_r9_c1
!n6_r4_c1 !n6_r5_c2
!n6_r4_c1 !n6_r5_c3
!n6_r4_c1 !n6_r6_c2
!n6_r4_c1 !n6_r6_c3
!n7_r4_c1 !n8_r4_c1
!n7_r4_c1 !n9_r4_c1
!n7_r4_c1 !n7_r4_c2
!n7_r4_c1 !n7_r4_c3
!n7_r4_c1 !n7_r4_c4
!n7_r4_c1 !n7_r4_c5
!n7_r4_c1 !n7_r4_c6
!n7_r4_c1 !n7_r4_c7
!n7_r4_c1 !n7_r4_c8
!n7_r4_c1 !n7_r4_c9
!n7_r4_c1 !n7_r5_c1
!n7_r4_c1 !n7_r6_c1
!n7_r4_c1 !n7_r7_c1
!n7_r4_c1 !n7_r8_c1
!n7_r4_c1 !n7_r9_c1
!n7_r4_c1 !n7_r5_c2
!n7_r4_c1 !n7_r5_c3
!n7_r4_c1 !n7_r6_c2
!n7_r4_c1 !n7_r6_c3
!n8_r4_c1 !n9_r4_c1
!n8_r4_c1 !n8_r4_c2
!n8_r4_c1 !n8_r4_c3
!n8_r4_c1 !n8_r4_c4
!n8_r4_c1 !n8_r4_c5
!n8_r4_c1 !n8_r4_c6
!n8_r4_c1 !n8_r4_c7
!n8_r4_c1 !n8_r4_c8
!n8_r4_c1 !n8_r4_c9
!n8_r4_c1 !n8_r5_c1
!n8_r4_c1 !n8_r6_c1
!n8_r4_c1 !n8_r7_c1
!n8_r4_c1 !n8_r8_c1
!n8_r4_c1 !n8_r9_c1
!n8_r4_c1 !n8_r5_c2
!n8_r4_c1 !n8_r5_c3
!n8_r4_c1 !n8_r6_c2
!n8_r4_c1 !n8_r6_c3
!n9_r4_c1 !n9_r4_c2
!n9_r4_c1 !n9_r4_c3
!n9_r4_c1 !n9_r4_c4
!n9_r4_c1 !n9_r4_c5
!n9_r4_c1 !n9_r4_c6
!n9_r4_c1 !n9_r4_c7
!n9_r4_c1 !n9_r4_c8
!n9_r4_c1 !n9_r4_c9
!n9_r4_c1 !n9_r5_c1
!n9_r4_c1 !n9_r6_c1
!n9_r4_c1 !n9_r7_c1
!n9_r4_c1 !n9_r8_c1
!n9_r4_c1 !n9_r9_c1
!n9_r4_c1 !n9_r5_c2
!n9_r4_c1 !n9_r5_c3
!n9_r4_c1 !n9_r6_c2
!n9_r4_c1 !n9_r6_c3
n1_r4_c1 n2_r4_c1 n3_r4_c1 n4_r4_c1 n5_r4_c1 n6_r4_c1 n7_r4_c1 n8_r4_c1 n9_r4_c1
!n1_r4_c2 !n2_r4_c2
!n1_r4_c2 !n3_r4_c2
!n1_r4_c2 !n4_r4_c2
!n1_r4_c2 !n5_r4_c2
!n1_r4_c2 !n6_r4_c2
!n1_r4_c2 !n7_r4_c2
!n1_r4_c2 !n8_r4_c2
!n1_r4_c2 !n9_r4_c2
!n1_r4_c2 !n1_r4_c3
!n1_r4_c2 !n1_r4_c4
!n1_r4_c2 !n1_r4_c5
!n1_r4_c2 !n1_r4_c6
!n1_r4_c2 !n1_r4_c7
!n1_r4_c2 !n1_r4_c8
!n1_r4_c2 !n1_r4_c9
!n1_r4_c2 !n1_r5_c2
!n1_r4_c2 !n1_r6_c2
!n1_r4_c2 !n1_r7_c2
!n1_r4_c2 !n1_r8_c2
!n1_r4_c2 !n1_r9_c2
!n1_r4_c2 !n1_r5_c1
!n1_r4_c2 !n1_r5_c3
!n1_r4_c2 !n1_r6_c1
!n1_r4_c2 !n1_r6_c3
!n2_r4_c2 !n3_r4_c2
!n2_r4_c2 !n4_r4_c2
!n2_r4_c2 !n5_r4_c2
!n2_r4_c2 !n6_r4_c2
!n2_r4_c2 !n7_r4_c2
!n2_r4_c2 !n8_r4_c2
!n2_r4_c2 !n9_r4_c2
!n2_r4_c2 !n2_r4_c3
!n2_r4_c2 !n2_r4_c4
!n2_r4_c2 !n2_r4_c5
!n2_r4_c2 !n2_r4_c6
!n2_r4_c2 !n2_r4_c7
!n2_r4_c2 !n2_r4_c8
!n2_r4_c2 !n2_r4_c9
!n2_r4_c2 !n2_r5_c2
!n2_r4_c2 !n2_r6_c2
!n2_r4_c2 !n2_r7_c2
!n2_r4_c2 !n2_r8_c2
!n2_r4_c2 !n2_r9_c2
!n2_r4_c2 !n2_r5_c1
!n2_r4_c2 !n2_r5_c3
!n2_r4_c2 !n2_r6_c1
!n2_r4_c2 !n2_r6_c3
!n3_r4_c2 !n4_r4_c2
!n3_r4_c2 !n5_r4_c2
!n3_r4_c2 !n6_r4_c2
!n3_r4_c2 !n7_r4_c2
!n3_r4_c2 !n8_r4_c2
!n3_r4_c2 !n9_r4_c2
!n3_r4_c2 !n3_r4_c3
!n3_r4_c2 !n3_r4_c4
!n3_r4_c2 !n3_r4_c5
!n3_r4_c2 !n3_r4_c6
!n3_r4_c2 !n3_r4_c7
!n3_r4_c2 !n3_r4_c8
!n3_r4_c2 !n3_r4_c9
!n3_r4_c2 !n3_r5_c2
!n3_r4_c2 !n3_r6_c2
!n3_r4_c2 !n3_r7_c2
!n3_r4_c2 !n3_r8_c2
!n3_r4_c2 !n3_r9_c2
!n3_r4_c2 !n3_r5_c1
!n3_r4_c2 !n3_r5_c3
!n3_r4_c2 !n3_r6_c1
!n3_r4_c2 !n3_r6_c3
!n4_r4_c2 !n5_r4_c2
!n4_r4_c2 !n6_r4_c2
!n4_r4_c2 !n7_r4_c2
!n4_r4_c2 !n8_r4_c2
!n4_r4_c2 !n9_r4_c2
!n4_r4_c2 !n4_r4_c3
!n4_r4_c2 !n4_r4_c4
!n4_r4_c2 !n4_r4_c5
!n4_r4_c2 !n4_r4_c6
!n4_r4_c2 !n4_r4_c7
!n4_r4_c2 !n4_r4_c8
!n4_r4_c2 !n4_r4_c9
!n4_r4_c2 !n4_r5_c2
!n4_r4_c2 !n4_r6_c2
!n4_r4_c2 !n4_r7_c2
!n4_r4_c2 !n4_r8_c2
!n4_r4_c2 !n4_r9_c2
!n4_r4_c2 !n4_r5_c1
!n4_r4_c2 !n4_r5_c3
!n4_r4_c2 !n4_r6_c1
!n4_r4_c2 !n4_r6_c3
!n5_r4_c2 !n6_r4_c2
!n5_r4_c2 !n7_r4_c2
!n5_r4_c2 !n8_r4_c2
!n5_r4_c2 !n9_r4_c2
!n5_r4_c2 !n5_r4_c3
!n5_r4_c2 !n5_r4_c4
!n5_r4_c2 !n5_r4_c5
!n5_r4_c2 !n5_r4_c6
!n5_r4_c2 !n5_r4_c7
!n5_r4_c2 !n5_r4_c8
!n5_r4_c2 !n5_r4_c9
!n5_r4_c2 !n5_r5_c2
!n5_r4_c2 !n5_r6_c2
!n5_r4_c2 !n5_r7_c2
!n5_r4_c2 !n5_r8_c2
!n5_r4_c2 !n5_r9_c2
!n5_r4_c2 !n5_r5_c1
!n5_r4_c2 !n5_r5_c3
!n5_r4_c2 !n5_r6_c1
!n5_r4_c2 !n5_r6_c3
!n6_r4_c2 !n7_r4_c2
!n6_r4_c2 !n8_r4_c2
!n6_r4_c2 !n9_r4_c2
!n6_r4_c2 !n6_r4_c3
!n6_r4_c2 !n6_r4_c4
!n6_r4_c2 !n6_r4_c5
!n6_r4_c2 !n6_r4_c6
!n6_r4_c2 !n6_r4_c7
!n6_r4_c2 !n6_r4_c8
!n6_r4_c2 !n6_r4_c9
!n6_r4_c2 !n6_r5_c2
!n6_r4_c2 !n6_r6_c2
!n6_r4_c2 !n6_r7_c2
!n6_r4_c2 !n6_r8_c2
!n6_r4_c2 !n6_r9_c2
!n6_r4_c2 !n6_r5_c1
!n6_r4_c2 !n6_r5_c3
!n6_r4_c2 !n6_r6_c1
!n6_r4_c2 !n6_r6_c3
!n7_r4_c2 !n8_r4_c2
!n7_r4_c2 !n9_r4_c2
!n7_r4_c2 !n7_r4_c3
!n7_r4_c2 !n7_r4_c4
!n7_r4_c2 !n7_r4_c5
!n7_r4_c2 !n7_r4_c6
!n7_r4_c2 !n7_r4_c7
!n7_r4_c2 !n7_r4_c8
!n7_r4_c2 !n7_r4_c9
!n7_r4_c2 !n7_r5_c2
!n7_r4_c2 !n7_r6_c2
!n7_r4_c2 !n7_r7_c2
!n7_r4_c2 !n7_r8_c2
!n7_r4_c2 !n7_r9_c2
!n7_r4_c2 !n7_r5_c1
!n7_r4_c2 !n7_r5_c3
!n7_r4_c2 !n7_r6_c1
!n7_r4_c2 !n7_r6_c3
!n8_r4_c2 !n9_r4_c2
!n8_r4_c2 !n8_r4_c3
!n8_r4_c2 !n8_r4_c4
!n8_r4_c2 !n8_r4_c5
!n8_r4_c2 !n8_r4_c6
!n8_r4_c2 !n8_r4_c7
!n8_r4_c2 !n8_r4_c8
!n8_r4_c2 !n8_r4_c9
!n8_r4_c2 !n8_r5_c2
!n8_r4_c2 !n8_r6_c2
!n8_r4_c2 !n8_r7_c2
!n8_r4_c2 !n8_r8_c2
!n8_r4_c2 !n8_r9_c2
!n8_r4_c2 !n8_r5_c1
!n8_r4_c2 !n8_r5_c3
!n8_r4_c2 !n8_r6_c1
!n8_r4_c2 !n8_r6_c3
!n9_r4_c2 !n9_r4_c3
!n9_r4_c2 !n9_r4_c4
!n9_r4_c2 !n9_r4_c5
!n9_r4_c2 !n9_r4_c6
!n9_r4_c2 !n9_r4_c7
!n9_r4_c2 !n9_r4_c8
!n9_r4_c2 !n9_r4_c9
!n9_r4_c2 !n9_r5_c2
!n9_r4_c2 !n9_r6_c2
!n9_r4_c2 !n9_r7_c2
!n9_r4_c2 !n9_r8_c2
!n9_r4_c2 !n9_r9_c2
!n9_r4_c2 !n9_r5_c1
!n9_r4_c2 !n9_r5_c3
!n9_r4_c2 !n9_r6_c1
!n9_r4_c2 !n9_r6_c3
n1_r4_c2 n2_r4_c2 n3_r4_c2 n4_r4_c2 n5_r4_c2 n6_r4_c2 n7_r4_c2 n8_r4_c2 n9_r4_c2
!n1_r4_c3 !n2_r4_c3
!n1_r4_c3 !n3_r4_c3
!n1_r4_c3 !n4_r4_c3
!n1_r4_c3 !n5_r4_c3
!n1_r4_c3 !n6_r4_c3
!n1_r4_c3 !n7_r4_c3
!n1_r4_c3 !n8_r4_c3
!n1_r4_c3 !n9_r4_c3
!n1_r4_c3 !n1_r4_c4
!n1_r4_c3 !n1_r4_c5
!n1_r4_c3 !n1_r4_c6
!n1_r4_c3 !n1_r4_c7
!n1_r4_c3 !n1_r4_c8
!n1_r4_c3 !n1_r4_c9
!n1_r4_c3 !n1_r5_c3
!n1_r4_c3 !n1_r6_c3
!n1_r4_c3 !n1_r7_c3
!n1_r4_c3 !n1_r8_c3
!n1_r4_c3 !n1_r9_c3
!n1_r4_c3 !n1_r5_c1
!n1_r4_c3 !n1_r5_c2
!n1_r4_c3 !n1_r6_c1
!n1_r4_c3 !n1_r6_c2
!n2_r4_c3 !n3_r4_c3
!n2_r4_c3 !n4_r4_c3
!n2_r4_c3 !n5_r4_c3
!n2_r4_c3 !n6_r4_c3
!n2_r4_c3 !n7_r4_c3
!n2_r4_c3 !n8_r4_c3
!n2_r4_c3 !n9_r4_c3
!n2_r4_c3 !n2_r4_c4
!n2_r4_c3 !n2_r4_c5
!n2_r4_c3 !n2_r4_c6
!n2_r4_c3 !n2_r4_c7
!n2_r4_c3 !n2_r4_c8
!n2_r4_c3 !n2_r4_c9
!n2_r4_c3 !n2_r5_c3
!n2_r4_c3 !n2_r6_c3
!n2_r4_c3 !n2_r7_c3
!n2_r4_c3 !n2_r8_c3
!n2_r4_c3 !n2_r9_c3
!n2_r4_c3 !n2_r5_c1
!n2_r4_c3 !n2_r5_c2
!n2_r4_c3 !n2_r6_c1
!n2_r4_c3 !n2_r6_c2
!n3_r4_c3 !n4_r4_c3
!n3_r4_c3 !n5_r4_c3
!n3_r4_c3 !n6_r4_c3
!n3_r4_c3 !n7_r4_c3
!n3_r4_c3 !n8_r4_c3
!n3_r4_c3 !n9_r4_c3
!n3_r4_c3 !n3_r4_c4
!n3_r4_c3 !n3_r4_c5
!n3_r4_c3 !n3_r4_c6
!n3_r4_c3 !n3_r4_c7
!n3_r4_c3 !n3_r4_c8
!n3_r4_c3 !n3_r4_c9
!n3_r4_c3 !n3_r5_c3
!n3_r4_c3 !n3_r6_c3
!n3_r4_c3 !n3_r7_c3
!n3_r4_c3 !n3_r8_c3
!n3_r4_c3 !n3_r9_c3
!n3_r4_c3 !n3_r5_c1
!n3_r4_c3 !n3_r5_c2
!n3_r4_c3 !n3_r6_c1
!n3_r4_c3 !n3_r6_c2
!n4_r4_c3 !n5_r4_c3
!n4_r4_c3 !n6_r4_c3
!n4_r4_c3 !n7_r4_c3
!n4_r4_c3 !n8_r4_c3
!n4_r4_c3 !n9_r4_c3
!n4_r4_c3 !n4_r4_c4
!n4_r4_c3 !n4_r4_c5
!n4_r4_c3 !n4_r4_c6
!n4_r4_c3 !n4_r4_c7
!n4_r4_c3 !n4_r4_c8
!n4_r4_c3 !n4_r4_c9
!n4_r4_c3 !n4_r5_c3
!n4_r4_c3 !n4_r6_c3
!n4_r4_c3 !n4_r7_c3
!n4_r4_c3 !n4_r8_c3
!n4_r4_c3 !n4_r9_c3
!n4_r4_c3 !n4_r5_c1
!n4_r4_c3 !n4_r5_c2
!n4_r4_c3 !n4_r6_c1
!n4_r4_c3 !n4_r6_c2
!n5_r4_c3 !n6_r4_c3
!n5_r4_c3 !n7_r4_c3
!n5_r4_c3 !n8_r4_c3
!n5_r4_c3 !n9_r4_c3
!n5_r4_c3 !n5_r4_c4
!n5_r4_c3 !n5_r4_c5
!n5_r4_c3 !n5_r4_c6
!n5_r4_c3 !n5_r4_c7
!n5_r4_c3 !n5_r4_c8
!n5_r4_c3 !n5_r4_c9
!n5_r4_c3 !n5_r5_c3
!n5_r4_c3 !n5_r6_c3
!n5_r4_c3 !n5_r7_c3
!n5_r4_c3 !n5_r8_c3
!n5_r4_c3 !n5_r9_c3
!n5_r4_c3 !n5_r5_c1
!n5_r4_c3 !n5_r5_c2
!n5_r4_c3 !n5_r6_c1
!n5_r4_c3 !n5_r6_c2
!n6_r4_c3 !n7_r4_c3
!n6_r4_c3 !n8_r4_c3
!n6_r4_c3 !n9_r4_c3
!n6_r4_c3 !n6_r4_c4
!n6_r4_c3 !n6_r4_c5
!n6_r4_c3 !n6_r4_c6
!n6_r4_c3 !n6_r4_c7
!n6_r4_c3 !n6_r4_c8
!n6_r4_c3 !n6_r4_c9
!n6_r4_c3 !n6_r5_c3
!n6_r4_c3 !n6_r6_c3
!n6_r4_c3 !n6_r7_c3
!n6_r4_c3 !n6_r8_c3
!n6_r4_c3 !n6_r9_c3
!n6_r4_c3 !n6_r5_c1
!n6_r4_c3 !n6_r5_c2
!n6_r4_c3 !n6_r6_c1
!n6_r4_c3 !n6_r6_c2
!n7_r4_c3 !n8_r4_c3
!n7_r4_c3 !n9_r4_c3
!n7_r4_c3 !n7_r4_c4
!n7_r4_c3 !n7_r4_c5
!n7_r4_c3 !n7_r4_c6
!n7_r4_c3 !n7_r4_c7
!n7_r4_c3 !n7_r4_c8
!n7_r4_c3 !n7_r4_c9
!n7_r4_c3 !n7_r5_c3
!n7_r4_c3 !n7_r6_c3
!n7_r4_c3 !n7_r7_c3
!n7_r4_c3 !n7_r8_c3
!n7_r4_c3 !n7_r9_c3
!n7_r4_c3 !n7_r5_c1
!n7_r4_c3 !n7_r5_c2
!n7_r4_c3 !n7_r6_c1
!n7_r4_c3 !n7_r6_c2
!n8_r4_c3 !n9_r4_c3
!n8_r4_c3 !n8_r4_c4
!n8_r4_c3 !n8_r4_c5
!n8_r4_c3 !n8_r4_c6
!n8_r4_c3 !n8_r4_c7
!n8_r4_c3 !n8_r4_c8
!n8_r4_c3 !n8_r4_c9
!n8_r4_c3 !n8_r5_c3
!n8_r4_c3 !n8_r6_c3
!n8_r4_c3 !n8_r7_c3
!n8_r4_c3 !n8_r8_c3
!n8_r4_c3 !n8_r9_c3
!n8_r4_c3 !n8_r5_c1
!n8_r4_c3 !n8_r5_c2
!n8_r4_c3 !n8_r6_c1
!n8_r4_c3 !n8_r6_c2
!n9_r4_c3 !n9_r4_c4
!n9_r4_c3 !n9_r4_c5
!n9_r4_c3 !n9_r4_c6
!n9_r4_c3 !n9_r4_c7
!n9_r4_c3 !n9_r4_c8
!n9_r4_c3 !n9_r4_c9
!n9_r4_c3 !n9_r5_c3
!n9_r4_c3 !n9_r6_c3
!n9_r4_c3 !n9_r7_c3
!n9_r4_c3 !n9_r8_c3
!n9_r4_c3 !n9_r9_c3
!n9_r4_c3 !n9_r5_c1
!n9_r4_c3 !n9_r5_c2
!n9_r4_c3 !n9_r6_c1
!n9_r4_c3 !n9_r6_c2
n1_r4_c3 n2_r4_c3 n3_r4_c3 n4_r4_c3 n5_r4_c3 n6_r4_c3 n7_r4_c3 n8_r4_c3 n9_r4_c3
!n1_r4_c4 !n2_r4_c4
!n1_r4_c4 !n3_r4_c4
!n1_r4_c4 !n4_r4_c4
!n1_r4_c4 !n5_r4_c4
!n1_r4_c4 !n6_r4_c4
!n1_r4_c4 !n7_r4_c4
!n1_r4_c4 !n8_r4_c4
!n1_r4_c4 !n9_r4_c4
!n1_r4_c4 !n1_r4_c5
!n1_r4_c4 !n1_r4_c6
!n1_r4_c4 !n1_r4_c7
!n1_r4_c4 !n1_r4_c8
!n1_r4_c4 !n1_r4_c9
!n1_r4_c4 !n1_r5_c4
!n1_r4_c4 !n1_r6_c4
!n1_r4_c4 !n1_r7_c4
!n1_r4_c4 !n1_r8_c4
!n1_r4_c4 !n1_r9_c4
!n1_r4_c4 !n1_r5_c5
!n1_r4_c4 !n1_r5_c6
!n1_r4_c4 !n1_r6_c5
!n1_r4_c4 !n1_r6_c6
!n2_r4_c4 !n3_r4_c4
!n2_r4_c4 !n4_r4_c4
!n2_r4_c4 !n5_r4_c4
!n2_r4_c4 !n6_r4_c4
!n2_r4_c4 !n7_r4_c4
!n2_r4_c4 !n8_r4_c4
!n2_r4_c4 !n9_r4_c4
!n2_r4_c4 !n2_r4_c5
!n2_r4_c4 !n2_r4_c6
!n2_r4_c4 !n2_r4_c7
!n2_r4_c4 !n2_r4_c8
!n2_r4_c4 !n2_r4_c9
!n2_r4_c4 !n2_r5_c4
!n2_r4_c4 !n2_r6_c4
!n2_r4_c4 !n2_r7_c4
!n2_r4_c4 !n2_r8_c4
!n2_r4_c4 !n2_r9_c4
!n2_r4_c4 !n2_r5_c5
!n2_r4_c4 !n2_r5_c6
!n2_r4_c4 !n2_r6_c5
!n2_r4_c4 !n2_r6_c6
!n3_r4_c4 !n4_r4_c4
!n3_r4_c4 !n5_r4_c4
!n3_r4_c4 !n6_r4_c4
!n3_r4_c4 !n7_r4_c4
!n3_r4_c4 !n8_r4_c4
!n3_r4_c4 !n9_r4_c4
!n3_r4_c4 !n3_r4_c5
!n3_r4_c4 !n3_r4_c6
!n3_r4_c4 !n3_r4_c7
!n3_r4_c4 !n3_r4_c8
!n3_r4_c4 !n3_r4_c9
!n3_r4_c4 !n3_r5_c4
!n3_r4_c4 !n3_r6_c4
!n3_r4_c4 !n3_r7_c4
!n3_r4_c4 !n3_r8_c4
!n3_r4_c4 !n3_r9_c4
!n3_r4_c4 !n3_r5_c5
!n3_r4_c4 !n3_r5_c6
!n3_r4_c4 !n3_r6_c5
!n3_r4_c4 !n3_r6_c6
!n4_r4_c4 !n5_r4_c4
!n4_r4_c4 !n6_r4_c4
!n4_r4_c4 !n7_r4_c4
!n4_r4_c4 !n8_r4_c4
!n4_r4_c4 !n9_r4_c4
!n4_r4_c4 !n4_r4_c5
!n4_r4_c4 !n4_r4_c6
!n4_r4_c4 !n4_r4_c7
!n4_r4_c4 !n4_r4_c8
!n4_r4_c4 !n4_r4_c9
!n4_r4_c4 !n4_r5_c4
!n4_r4_c4 !n4_r6_c4
!n4_r4_c4 !n4_r7_c4
!n4_r4_c4 !n4_r8_c4
!n4_r4_c4 !n4_r9_c4
!n4_r4_c4 !n4_r5_c5
!n4_r4_c4 !n4_r5_c6
!n4_r4_c4 !n4_r6_c5
!n4_r4_c4 !n4_r6_c6
!n5_r4_c4 !n6_r4_c4
!n5_r4_c4 !n7_r4_c4
!n5_r4_c4 !n8_r4_c4
!n5_r4_c4 !n9_r4_c4
!n5_r4_c4 !n5_r4_c5
!n5_r4_c4 !n5_r4_c6
!n5_r4_c4 !n5_r4_c7
!n5_r4_c4 !n5_r4_c8
!n5_r4_c4 !n5_r4_c9
!n5_r4_c4 !n5_r5_c4
!n5_r4_c4 !n5_r6_c4
!n5_r4_c4 !n5_r7_c4
!n5_r4_c4 !n5_r8_c4
!n5_r4_c4 !n5_r9_c4
!n5_r4_c4 !n5_r5_c5
!n5_r4_c4 !n5_r5_c6
!n5_r4_c4 !n5_r6_c5
!n5_r4_c4 !n5_r6_c6
!n6_r4_c4 !n7_r4_c4
!n6_r4_c4 !n8_r4_c4
!n6_r4_c4 !n9_r4_c4
!n6_r4_c4 !n6_r4_c5
!n6_r4_c4 !n6_r4_c6
!n6_r4_c4 !n6_r4_c7
!n6_r4_c4 !n6_r4_c8
!n6_r4_c4 !n6_r4_c9
!n6_r4_c4 !n6_r5_c4
!n6_r4_c4 !n6_r6_c4
!n6_r4_c4 !n6_r7_c4
!n6_r4_c4 !n6_r8_c4
!n6_r4_c4 !n6_r9_c4
!n6_r4_c4 !n6_r5_c5
!n6_r4_c4 !n6_r5_c6
!n6_r4_c4 !n6_r6_c5
!n6_r4_c4 !n6_r6_c6
!n7_r4_c4 !n8_r4_c4
!n7_r4_c4 !n9_r4_c4
!n7_r4_c4 !n7_r4_c5
!n7_r4_c4 !n7_r4_c6
!n7_r4_c4 !n7_r4_c7
!n7_r4_c4 !n7_r4_c8
!n7_r4_c4 !n7_r4_c9
!n7_r4_c4 !n7_r5_c4
!n7_r4_c4 !n7_r6_c4
!n7_r4_c4 !n7_r7_c4
!n7_r4_c4 !n7_r8_c4
!n7_r4_c4 !n7_r9_c4
!n7_r4_c4 !n7_r5_c5
!n7_r4_c4 !n7_r5_c6
!n7_r4_c4 !n7_r6_c5
!n7_r4_c4 !n7_r6_c6
!n8_r4_c4 !n9_r4_c4
!n8_r4_c4 !n8_r4_c5
!n8_r4_c4 !n8_r4_c6
!n8_r4_c4 !n8_r4_c7
!n8_r4_c4 !n8_r4_c8
!n8_r4_c4 !n8_r4_c9
!n8_r4_c4 !n8_r5_c4
!n8_r4_c4 !n8_r6_c4
!n8_r4_c4 !n8_r7_c4
!n8_r4_c4 !n8_r8_c4
!n8_r4_c4 !n8_r9_c4
!n8_r4_c4 !n8_r5_c5
!n8_r4_c4 !n8_r5_c6
!n8_r4_c4 !n8_r6_c5
!n8_r4_c4 !n8_r6_c6
!n9_r4_c4 !n9_r4_c5
!n9_r4_c4 !n9_r4_c6
!n9_r4_c4 !n9_r4_c7
!n9_r4_c4 !n9_r4_c8
!n9_r4_c4 !n9_r4_c9
!n9_r4_c4 !n9_r5_c4
!n9_r4_c4 !n9_r6_c4
!n9_r4_c4 !n9_r7_c4
!n9_r4_c4 !n9_r8_c4
!n9_r4_c4 !n9_r9_c4
!n9_r4_c4 !n9_r5_c5
!n9_r4_c4 !n9_r5_c6
!n9_r4_c4 !n9_r6_c5
!n9_r4_c4 !n9_r6_c6
n1_r4_c4 n2_r4_c4 n3_r4_c4 n4_r4_c4 n5_r4_c4 n6_r4_c4 n7_r4_c4 n8_r4_c4 n9_r4_c4
!n1_r4_c5 !n2_r4_c5
!n1_r4_c5 !n3_r4_c5
!n1_r4_c5 !n4_r4_c5
!n1_r4_c5 !n5_r4_c5
!n1_r4_c5 !n6_r4_c5
!n1_r4_c5 !n7_r4_c5
!n1_r4_c5 !n8_r4_c5
!n1_r4_c5 !n9_r4_c5
!n1_r4_c5 !n1_r4_c6
!n1_r4_c5 !n1_r4_c7
!n1_r4_c5 !n1_r4_c8
!n1_r4_c5 !n1_r4_c9
!n1_r4_c5 !n1_r5_c5
!n1_r4_c5 !n1_r6_c5
!n1_r4_c5 !n1_r7_c5
!n1_r4_c5 !n1_r8_c5
!n1_r4_c5 !n1_r9_c5
!n1_r4_c5 !n1_r5_c4
!n1_r4_c5 !n1_r5_c6
!n1_r4_c5 !n1_r6_c4
!n1_r4_c5 !n1_r6_c6
!n2_r4_c5 !n3_r4_c5
!n2_r4_c5 !n4_r4_c5
!n2_r4_c5 !n5_r4_c5
!n2_r4_c5 !n6_r4_c5
!n2_r4_c5 !n7_r4_c5
!n2_r4_c5 !n8_r4_c5
!n2_r4_c5 !n9_r4_c5
!n2_r4_c5 !n2_r4_c6
!n2_r4_c5 !n2_r4_c7
!n2_r4_c5 !n2_r4_c8
!n2_r4_c5 !n2_r4_c9
!n2_r4_c5 !n2_r5_c5
!n2_r4_c5 !n2_r6_c5
!n2_r4_c5 !n2_r7_c5
!n2_r4_c5 !n2_r8_c5
!n2_r4_c5 !n2_r9_c5
!n2_r4_c5 !n2_r5_c4
!n2_r4_c5 !n2_r5_c6
!n2_r4_c5 !n2_r6_c4
!n2_r4_c5 !n2_r6_c6
!n3_r4_c5 !n4_r4_c5
!n3_r4_c5 !n5_r4_c5
!n3_r4_c5 !n6_r4_c5
!n3_r4_c5 !n7_r4_c5
!n3_r4_c5 !n8_r4_c5
!n3_r4_c5 !n9_r4_c5
!n3_r4_c5 !n3_r4_c6
!n3_r4_c5 !n3_r4_c7
!n3_r4_c5 !n3_r4_c8
!n3_r4_c5 !n3_r4_c9
!n3_r4_c5 !n3_r5_c5
!n3_r4_c5 !n3_r6_c5
!n3_r4_c5 !n3_r7_c5
!n3_r4_c5 !n3_r8_c5
!n3_r4_c5 !n3_r9_c5
!n3_r4_c5 !n3_r5_c4
!n3_r4_c5 !n3_r5_c6
!n3_r4_c5 !n3_r6_c4
!n3_r4_c5 !n3_r6_c6
!n4_r4_c5 !n5_r4_c5
!n4_r4_c5 !n6_r4_c5
!n4_r4_c5 !n7_r4_c5
!n4_r4_c5 !n8_r4_c5
!n4_r4_c5 !n9_r4_c5
!n4_r4_c5 !n4_r4_c6
!n4_r4_c5 !n4_r4_c7
!n4_r4_c5 !n4_r4_c8
!n4_r4_c5 !n4_r4_c9
!n4_r4_c5 !n4_r5_c5
!n4_r4_c5 !n4_r6_c5
!n4_r4_c5 !n4_r7_c5
!n4_r4_c5 !n4_r8_c5
!n4_r4_c5 !n4_r9_c5
!n4_r4_c5 !n4_r5_c4
!n4_r4_c5 !n4_r5_c6
!n4_r4_c5 !n4_r6_c4
!n4_r4_c5 !n4_r6_c6
!n5_r4_c5 !n6_r4_c5
!n5_r4_c5 !n7_r4_c5
!n5_r4_c5 !n8_r4_c5
!n5_r4_c5 !n9_r4_c5
!n5_r4_c5 !n5_r4_c6
!n5_r4_c5 !n5_r4_c7
!n5_r4_c5 !n5_r4_c8
!n5_r4_c5 !n5_r4_c9
!n5_r4_c5 !n5_r5_c5
!n5_r4_c5 !n5_r6_c5
!n5_r4_c5 !n5_r7_c5
!n5_r4_c5 !n5_r8_c5
!n5_r4_c5 !n5_r9_c5
!n5_r4_c5 !n5_r5_c4
!n5_r4_c5 !n5_r5_c6
!n5_r4_c5 !n5_r6_c4
!n5_r4_c5 !n5_r6_c6
!n6_r4_c5 !n7_r4_c5
!n6_r4_c5 !n8_r4_c5
!n6_r4_c5 !n9_r4_c5
!n6_r4_c5 !n6_r4_c6
!n6_r4_c5 !n6_r4_c7
!n6_r4_c5 !n6_r4_c8
!n6_r4_c5 !n6_r4_c9
!n6_r4_c5 !n6_r5_c5
!n6_r4_c5 !n6_r6_c5
!n6_r4_c5 !n6_r7_c5
!n6_r4_c5 !n6_r8_c5
!n6_r4_c5 !n6_r9_c5
!n6_r4_c5 !n6_r5_c4
!n6_r4_c5 !n6_r5_c6
!n6_r4_c5 !n6_r6_c4
!n6_r4_c5 !n6_r6_c6
!n7_r4_c5 !n8_r4_c5
!n7_r4_c5 !n9_r4_c5
!n7_r4_c5 !n7_r4_c6
!n7_r4_c5 !n7_r4_c7
!n7_r4_c5 !n7_r4_c8
!n7_r4_c5 !n7_r4_c9
!n7_r4_c5 !n7_r5_c5
!n7_r4_c5 !n7_r6_c5
!n7_r4_c5 !n7_r7_c5
!n7_r4_c5 !n7_r8_c5
!n7_r4_c5 !n7_r9_c5
!n7_r4_c5 !n7_r5_c4
!n7_r4_c5 !n7_r5_c6
!n7_r4_c5 !n7_r6_c4
!n7_r4_c5 !n7_r6_c6
!n8_r4_c5 !n9_r4_c5
!n8_r4_c5 !n8_r4_c6
!n8_r4_c5 !n8_r4_c7
!n8_r4_c5 !n8_r4_c8
!n8_r4_c5 !n8_r4_c9
!n8_r4_c5 !n8_r5_c5
!n8_r4_c5 !n8_r6_c5
!n8_r4_c5 !n8_r7_c5
!n8_r4_c5 !n8_r8_c5
!n8_r4_c5 !n8_r9_c5
!n8_r4_c5 !n8_r5_c4
!n8_r4_c5 !n8_r5_c6
!n8_r4_c5 !n8_r6_c4
!n8_r4_c5 !n8_r6_c6
!n9_r4_c5 !n9_r4_c6
!n9_r4_c5 !n9_r4_c7
!n9_r4_c5 !n9_r4_c8
!n9_r4_c5 !n9_r4_c9
!n9_r4_c5 !n9_r5_c5
!n9_r4_c5 !n9_r6_c5
!n9_r4_c5 !n9_r7_c5
!n9_r4_c5 !n9_r8_c5
!n9_r4_c5 !n9_r9_c5
!n9_r4_c5 !n9_r5_c4
!n9_r4_c5 !n9_r5_c6
!n9_r4_c5 !n9_r6_c4
!n9_r4_c5 !n9_r6_c6
n1_r4_c5 n2_r4_c5 n3_r4_c5 n4_r4_c5 n5_r4_c5 n6_r4_c5 n7_r4_c5 n8_r4_c5 n9_r4_c5
!n1_r4_c6 !n2_r4_c6
!n1_r4_c6 !n3_r4_c6
!n1_r4_c6 !n4_r4_c6
!n1_r4_c6 !n5_r4_c6
!n1_r4_c6 !n6_r4_c6
!n1_r4_c6 !n7_r4_c6
!n1_r4_c6 !n8_r4_c6
!n1_r4_c6 !n9_r4_c6
!n1_r4_c6 !n1_r4_c7
!n1_r4_c6 !n1_r4_c8
!n1_r4_c6 !n1_r4_c9
!n1_r4_c6 !n1_r5_c6
!n1_r4_c6 !n1_r6_c6
!n1_r4_c6 !n1_r7_c6
!n1_r4_c6 !n1_r8_c6
!n1_r4_c6 !n1_r9_c6
!n1_r4_c6 !n1_r5_c4
!n1_r4_c6 !n1_r5_c5
!n1_r4_c6 !n1_r6_c4
!n1_r4_c6 !n1_r6_c5
!n2_r4_c6 !n3_r4_c6
!n2_r4_c6 !n4_r4_c6
!n2_r4_c6 !n5_r4_c6
!n2_r4_c6 !n6_r4_c6
!n2_r4_c6 !n7_r4_c6
!n2_r4_c6 !n8_r4_c6
!n2_r4_c6 !n9_r4_c6
!n2_r4_c6 !n2_r4_c7
!n2_r4_c6 !n2_r4_c8
!n2_r4_c6 !n2_r4_c9
!n2_r4_c6 !n2_r5_c6
!n2_r4_c6 !n2_r6_c6
!n2_r4_c6 !n2_r7_c6
!n2_r4_c6 !n2_r8_c6
!n2_r4_c6 !n2_r9_c6
!n2_r4_c6 !n2_r5_c4
!n2_r4_c6 !n2_r5_c5
!n2_r4_c6 !n2_r6_c4
!n2_r4_c6 !n2_r6_c5
!n3_r4_c6 !n4_r4_c6
!n3_r4_c6 !n5_r4_c6
!n3_r4_c6 !n6_r4_c6
!n3_r4_c6 !n7_r4_c6
!n3_r4_c6 !n8_r4_c6
!n3_r4_c6 !n9_r4_c6
!n3_r4_c6 !n3_r4_c7
!n3_r4_c6 !n3_r4_c8
!n3_r4_c6 !n3_r4_c9
!n3_r4_c6 !n3_r5_c6
!n3_r4_c6 !n3_r6_c6
!n3_r4_c6 !n3_r7_c6
!n3_r4_c6 !n3_r8_c6
!n3_r4_c6 !n3_r9_c6
!n3_r4_c6 !n3_r5_c4
!n3_r4_c6 !n3_r5_c5
!n3_r4_c6 !n3_r6_c4
!n3_r4_c6 !n3_r6_c5
!n4_r4_c6 !n5_r4_c6
!n4_r4_c6 !n6_r4_c6
!n4_r4_c6 !n7_r4_c6
!n4_r4_c6 !n8_r4_c6
!n4_r4_c6 !n9_r4_c6
!n4_r4_c6 !n4_r4_c7
!n4_r4_c6 !n4_r4_c8
!n4_r4_c6 !n4_r4_c9
!n4_r4_c6 !n4_r5_c6
!n4_r4_c6 !n4_r6_c6
!n4_r4_c6 !n4_r7_c6
!n4_r4_c6 !n4_r8_c6
!n4_r4_c6 !n4_r9_c6
!n4_r4_c6 !n4_r5_c4
!n4_r4_c6 !n4_r5_c5
!n4_r4_c6 !n4_r6_c4
!n4_r4_c6 !n4_r6_c5
!n5_r4_c6 !n6_r4_c6
!n5_r4_c6 !n7_r4_c6
!n5_r4_c6 !n8_r4_c6
!n5_r4_c6 !n9_r4_c6
!n5_r4_c6 !n5_r4_c7
!n5_r4_c6 !n5_r4_c8
!n5_r4_c6 !n5_r4_c9
!n5_r4_c6 !n5_r5_c6
!n5_r4_c6 !n5_r6_c6
!n5_r4_c6 !n5_r7_c6
!n5_r4_c6 !n5_r8_c6
!n5_r4_c6 !n5_r9_c6
!n5_r4_c6 !n5_r5_c4
!n5_r4_c6 !n5_r5_c5
!n5_r4_c6 !n5_r6_c4
!n5_r4_c6 !n5_r6_c5
!n6_r4_c6 !n7_r4_c6
!n6_r4_c6 !n8_r4_c6
!n6_r4_c6 !n9_r4_c6
!n6_r4_c6 !n6_r4_c7
!n6_r4_c6 !n6_r4_c8
!n6_r4_c6 !n6_r4_c9
!n6_r4_c6 !n6_r5_c6
!n6_r4_c6 !n6_r6_c6
!n6_r4_c6 !n6_r7_c6
!n6_r4_c6 !n6_r8_c6
!n6_r4_c6 !n6_r9_c6
!n6_r4_c6 !n6_r5_c4
!n6_r4_c6 !n6_r5_c5
!n6_r4_c6 !n6_r6_c4
!n6_r4_c6 !n6_r6_c5
!n7_r4_c6 !n8_r4_c6
!n7_r4_c6 !n9_r4_c6
!n7_r4_c6 !n7_r4_c7
!n7_r4_c6 !n7_r4_c8
!n7_r4_c6 !n7_r4_c9
!n7_r4_c6 !n7_r5_c6
!n7_r4_c6 !n7_r6_c6
!n7_r4_c6 !n7_r7_c6
!n7_r4_c6 !n7_r8_c6
!n7_r4_c6 !n7_r9_c6
!n7_r4_c6 !n7_r5_c4
!n7_r4_c6 !n7_r5_c5
!n7_r4_c6 !n7_r6_c4
!n7_r4_c6 !n7_r6_c5
!n8_r4_c6 !n9_r4_c6
!n8_r4_c6 !n8_r4_c7
!n8_r4_c6 !n8_r4_c8
!n8_r4_c6 !n8_r4_c9
!n8_r4_c6 !n8_r5_c6
!n8_r4_c6 !n8_r6_c6
!n8_r4_c6 !n8_r7_c6
!n8_r4_c6 !n8_r8_c6
!n8_r4_c6 !n8_r9_c6
!n8_r4_c6 !n8_r5_c4
!n8_r4_c6 !n8_r5_c5
!n8_r4_c6 !n8_r6_c4
!n8_r4_c6 !n8_r6_c5
!n9_r4_c6 !n9_r4_c7
!n9_r4_c6 !n9_r4_c8
!n9_r4_c6 !n9_r4_c9
!n9_r4_c6 !n9_r5_c6
!n9_r4_c6 !n9_r6_c6
!n9_r4_c6 !n9_r7_c6
!n9_r4_c6 !n9_r8_c6
!n9_r4_c6 !n9_r9_c6
!n9_r4_c6 !n9_r5_c4
!n9_r4_c6 !n9_r5_c5
!n9_r4_c6 !n9_r6_c4
!n9_r4_c6 !n9_r6_c5
n1_r4_c6 n2_r4_c6 n3_r4_c6 n4_r4_c6 n5_r4_c6 n6_r4_c6 n7_r4_c6 n8_r4_c6 n9_r4_c6
!n1_r4_c7 !n2_r4_c7
!n1_r4_c7 !n3_r4_c7
!n1_r4_c7 !n4_r4_c7
!n1_r4_c7 !n5_r4_c7
!n1_r4_c7 !n6_r4_c7
!n1_r4_c7 !n7_r4_c7
!n1_r4_c7 !n8_r4_c7
!n1_r4_c7 !n9_r4_c7
!n1_r4_c7 !n1_r4_c8
!n1_r4_c7 !n1_r4_c9
!n1_r4_c7 !n1_r5_c7
!n1_r4_c7 !n1_r6_c7
!n1_r4_c7 !n1_r7_c7
!n1_r4_c7 !n1_r8_c7
!n1_r4_c7 !n1_r9_c7
!n1_r4_c7 !n1_r5_c8
!n1_r4_c7 !n1_r5_c9
!n1_r4_c7 !n1_r6_c8
!n1_r4_c7 !n1_r6_c9
!n2_r4_c7 !n3_r4_c7
!n2_r4_c7 !n4_r4_c7
!n2_r4_c7 !n5_r4_c7
!n2_r4_c7 !n6_r4_c7
!n2_r4_c7 !n7_r4_c7
!n2_r4_c7 !n8_r4_c7
!n2_r4_c7 !n9_r4_c7
!n2_r4_c7 !n2_r4_c8
!n2_r4_c7 !n2_r4_c9
!n2_r4_c7 !n2_r5_c7
!n2_r4_c7 !n2_r6_c7
!n2_r4_c7 !n2_r7_c7
!n2_r4_c7 !n2_r8_c7
!n2_r4_c7 !n2_r9_c7
!n2_r4_c7 !n2_r5_c8
!n2_r4_c7 !n2_r5_c9
!n2_r4_c7 !n2_r6_c8
!n2_r4_c7 !n2_r6_c9
!n3_r4_c7 !n4_r4_c7
!n3_r4_c7 !n5_r4_c7
!n3_r4_c7 !n6_r4_c7
!n3_r4_c7 !n7_r4_c7
!n3_r4_c7 !n8_r4_c7
!n3_r4_c7 !n9_r4_c7
!n3_r4_c7 !n3_r4_c8
!n3_r4_c7 !n3_r4_c9
!n3_r4_c7 !n3_r5_c7
!n3_r4_c7 !n3_r6_c7
!n3_r4_c7 !n3_r7_c7
!n3_r4_c7 !n3_r8_c7
!n3_r4_c7 !n3_r9_c7
!n3_r4_c7 !n3_r5_c8
!n3_r4_c7 !n3_r5_c9
!n3_r4_c7 !n3_r6_c8
!n3_r4_c7 !n3_r6_c9
!n4_r4_c7 !n5_r4_c7
!n4_r4_c7 !n6_r4_c7
!n4_r4_c7 !n7_r4_c7
!n4_r4_c7 !n8_r4_c7
!n4_r4_c7 !n9_r4_c7
!n4_r4_c7 !n4_r4_c8
!n4_r4_c7 !n4_r4_c9
!n4_r4_c7 !n4_r5_c7
!n4_r4_c7 !n4_r6_c7
!n4_r4_c7 !n4_r7_c7
!n4_r4_c7 !n4_r8_c7
!n4_r4_c7 !n4_r9_c7
!n4_r4_c7 !n4_r5_c8
!n4_r4_c7 !n4_r5_c9
!n4_r4_c7 !n4_r6_c8
!n4_r4_c7 !n4_r6_c9
!n5_r4_c7 !n6_r4_c7
!n5_r4_c7 !n7_r4_c7
!n5_r4_c7 !n8_r4_c7
!n5_r4_c7 !n9_r4_c7
!n5_r4_c7 !n5_r4_c8
!n5_r4_c7 !n5_r4_c9
!n5_r4_c7 !n5_r5_c7
!n5_r4_c7 !n5_r6_c7
!n5_r4_c7 !n5_r7_c7
!n5_r4_c7 !n5_r8_c7
!n5_r4_c7 !n5_r9_c7
!n5_r4_c7 !n5_r5_c8
!n5_r4_c7 !n5_r5_c9
!n5_r4_c7 !n5_r6_c8
!n5_r4_c7 !n5_r6_c9
!n6_r4_c7 !n7_r4_c7
!n6_r4_c7 !n8_r4_c7
!n6_r4_c7 !n9_r4_c7
!n6_r4_c7 !n6_r4_c8
!n6_r4_c7 !n6_r4_c9
!n6_r4_c7 !n6_r5_c7
!n6_r4_c7 !n6_r6_c7
!n6_r4_c7 !n6_r7_c7
!n6_r4_c7 !n6_r8_c7
!n6_r4_c7 !n6_r9_c7
!n6_r4_c7 !n6_r5_c8
!n6_r4_c7 !n6_r5_c9
!n6_r4_c7 !n6_r6_c8
!n6_r4_c7 !n6_r6_c9
!n7_r4_c7 !n8_r4_c7
!n7_r4_c7 !n9_r4_c7
!n7_r4_c7 !n7_r4_c8
!n7_r4_c7 !n7_r4_c9
!n7_r4_c7 !n7_r5_c7
!n7_r4_c7 !n7_r6_c7
!n7_r4_c7 !n7_r7_c7
!n7_r4_c7 !n7_r8_c7
!n7_r4_c7 !n7_r9_c7
!n7_r4_c7 !n7_r5_c8
!n7_r4_c7 !n7_r5_c9
!n7_r4_c7 !n7_r6_c8
!n7_r4_c7 !n7_r6_c9
!n8_r4_c7 !n9_r4_c7
!n8_r4_c7 !n8_r4_c8
!n8_r4_c7 !n8_r4_c9
!n8_r4_c7 !n8_r5_c7
!n8_r4_c7 !n8_r6_c7
!n8_r4_c7 !n8_r7_c7
!n8_r4_c7 !n8_r8_c7
!n8_r4_c7 !n8_r9_c7
!n8_r4_c7 !n8_r5_c8
!n8_r4_c7 !n8_r5_c9
!n8_r4_c7 !n8_r6_c8
!n8_r4_c7 !n8_r6_c9
!n9_r4_c7 !n9_r4_c8
!n9_r4_c7 !n9_r4_c9
!n9_r4_c7 !n9_r5_c7
!n9_r4_c7 !n9_r6_c7
!n9_r4_c7 !n9_r7_c7
!n9_r4_c7 !n9_r8_c7
!n9_r4_c7 !n9_r9_c7
!n9_r4_c7 !n9_r5_c8
!n9_r4_c7 !n9_r5_c9
!n9_r4_c7 !n9_r6_c8
!n9_r4_c7 !n9_r6_c9
n1_r4_c7 n2_r4_c7 n3_r4_c7 n4_r4_c7 n5_r4_c7 n6_r4_c7 n7_r4_c7 n8_r4_c7 n9_r4_c7
!n1_r4_c8 !n2_r4_c8
!n1_r4_c8 !n3_r4_c8
!n1_r4_c8 !n4_r4_c8
!n1_r4_c8 !n5_r4_c8
!n1_r4_c8 !n6_r4_c8
!n1_r4_c8 !n7_r4_c8
!n1_r4_c8 !n8_r4_c8
!n1_r4_c8 !n9_r4_c8
!n1_r4_c8 !n1_r4_c9
!n1_r4_c8 !n1_r5_c8
!n1_r4_c8 !n1_r6_c8
!n1_r4_c8 !n1_r7_c8
!n1_r4_c8 !n1_r8_c8
!n1_r4_c8 !n1_r9_c8
!n1_r4_c8 !n1_r5_c7
!n1_r4_c8 !n1_r5_c9
!n1_r4_c8 !n1_r6_c7
!n1_r4_c8 !n1_r6_c9
!n2_r4_c8 !n3_r4_c8
!n2_r4_c8 !n4_r4_c8
!n2_r4_c8 !n5_r4_c8
!n2_r4_c8 !n6_r4_c8
!n2_r4_c8 !n7_r4_c8
!n2_r4_c8 !n8_r4_c8
!n2_r4_c8 !n9_r4_c8
!n2_r4_c8 !n2_r4_c9
!n2_r4_c8 !n2_r5_c8
!n2_r4_c8 !n2_r6_c8
!n2_r4_c8 !n2_r7_c8
!n2_r4_c8 !n2_r8_c8
!n2_r4_c8 !n2_r9_c8
!n2_r4_c8 !n2_r5_c7
!n2_r4_c8 !n2_r5_c9
!n2_r4_c8 !n2_r6_c7
!n2_r4_c8 !n2_r6_c9
!n3_r4_c8 !n4_r4_c8
!n3_r4_c8 !n5_r4_c8
!n3_r4_c8 !n6_r4_c8
!n3_r4_c8 !n7_r4_c8
!n3_r4_c8 !n8_r4_c8
!n3_r4_c8 !n9_r4_c8
!n3_r4_c8 !n3_r4_c9
!n3_r4_c8 !n3_r5_c8
!n3_r4_c8 !n3_r6_c8
!n3_r4_c8 !n3_r7_c8
!n3_r4_c8 !n3_r8_c8
!n3_r4_c8 !n3_r9_c8
!n3_r4_c8 !n3_r5_c7
!n3_r4_c8 !n3_r5_c9
!n3_r4_c8 !n3_r6_c7
!n3_r4_c8 !n3_r6_c9
!n4_r4_c8 !n5_r4_c8
!n4_r4_c8 !n6_r4_c8
!n4_r4_c8 !n7_r4_c8
!n4_r4_c8 !n8_r4_c8
!n4_r4_c8 !n9_r4_c8
!n4_r4_c8 !n4_r4_c9
!n4_r4_c8 !n4_r5_c8
!n4_r4_c8 !n4_r6_c8
!n4_r4_c8 !n4_r7_c8
!n4_r4_c8 !n4_r8_c8
!n4_r4_c8 !n4_r9_c8
!n4_r4_c8 !n4_r5_c7
!n4_r4_c8 !n4_r5_c9
!n4_r4_c8 !n4_r6_c7
!n4_r4_c8 !n4_r6_c9
!n5_r4_c8 !n6_r4_c8
!n5_r4_c8 !n7_r4_c8
!n5_r4_c8 !n8_r4_c8
!n5_r4_c8 !n9_r4_c8
!n5_r4_c8 !n5_r4_c9
!n5_r4_c8 !n5_r5_c8
!n5_r4_c8 !n5_r6_c8
!n5_r4_c8 !n5_r7_c8
!n5_r4_c8 !n5_r8_c8
!n5_r4_c8 !n5_r9_c8
!n5_r4_c8 !n5_r5_c7
!n5_r4_c8 !n5_r5_c9
!n5_r4_c8 !n5_r6_c7
!n5_r4_c8 !n5_r6_c9
!n6_r4_c8 !n7_r4_c8
!n6_r4_c8 !n8_r4_c8
!n6_r4_c8 !n9_r4_c8
!n6_r4_c8 !n6_r4_c9
!n6_r4_c8 !n6_r5_c8
!n6_r4_c8 !n6_r6_c8
!n6_r4_c8 !n6_r7_c8
!n6_r4_c8 !n6_r8_c8
!n6_r4_c8 !n6_r9_c8
!n6_r4_c8 !n6_r5_c7
!n6_r4_c8 !n6_r5_c9
!n6_r4_c8 !n6_r6_c7
!n6_r4_c8 !n6_r6_c9
!n7_r4_c8 !n8_r4_c8
!n7_r4_c8 !n9_r4_c8
!n7_r4_c8 !n7_r4_c9
!n7_r4_c8 !n7_r5_c8
!n7_r4_c8 !n7_r6_c8
!n7_r4_c8 !n7_r7_c8
!n7_r4_c8 !n7_r8_c8
!n7_r4_c8 !n7_r9_c8
!n7_r4_c8 !n7_r5_c7
!n7_r4_c8 !n7_r5_c9
!n7_r4_c8 !n7_r6_c7
!n7_r4_c8 !n7_r6_c9
!n8_r4_c8 !n9_r4_c8
!n8_r4_c8 !n8_r4_c9
!n8_r4_c8 !n8_r5_c8
!n8_r4_c8 !n8_r6_c8
!n8_r4_c8 !n8_r7_c8
!n8_r4_c8 !n8_r8_c8
!n8_r4_c8 !n8_r9_c8
!n8_r4_c8 !n8_r5_c7
!n8_r4_c8 !n8_r5_c9
!n8_r4_c8 !n8_r6_c7
!n8_r4_c8 !n8_r6_c9
!n9_r4_c8 !n9_r4_c9
!n9_r4_c8 !n9_r5_c8
!n9_r4_c8 !n9_r6_c8
!n9_r4_c8 !n9_r7_c8
!n9_r4_c8 !n9_r8_c8
!n9_r4_c8 !n9_r9_c8
!n9_r4_c8 !n9_r5_c7
!n9_r4_c8 !n9_r5_c9
!n9_r4_c8 !n9_r6_c7
!n9_r4_c8 !n9_r6_c9
n1_r4_c8 n2_r4_c8 n3_r4_c8 n4_r4_c8 n5_r4_c8 n6_r4_c8 n7_r4_c8 n8_r4_c8 n9_r4_c8
!n1_r4_c9 !n2_r4_c9
!n1_r4_c9 !n3_r4_c9
!n1_r4_c9 !n4_r4_c9
!n1_r4_c9 !n5_r4_c9
!n1_r4_c9 !n6_r4_c9
!n1_r4_c9 !n7_r4_c9
!n1_r4_c9 !n8_r4_c9
!n1_r4_c9 !n9_r4_c9
!n1_r4_c9 !n1_r5_c9
!n1_r4_c9 !n1_r6_c9
!n1_r4_c9 !n1_r7_c9
!n1_r4_c9 !n1_r8_c9
!n1_r4_c9 !n1_r9_c9
!n1_r4_c9 !n1_r5_c7
!n1_r4_c9 !n1_r5_c8
!n1_r4_c9 !n1_r6_c7
!n1_r4_c9 !n1_r6_c8
!n2_r4_c9 !n3_r4_c9
!n2_r4_c9 !n4_r4_c9
!n2_r4_c9 !n5_r4_c9
!n2_r4_c9 !n6_r4_c9
!n2_r4_c9 !n7_r4_c9
!n2_r4_c9 !n8_r4_c9
!n2_r4_c9 !n9_r4_c9
!n2_r4_c9 !n2_r5_c9
!n2_r4_c9 !n2_r6_c9
!n2_r4_c9 !n2_r7_c9
!n2_r4_c9 !n2_r8_c9
!n2_r4_c9 !n2_r9_c9
!n2_r4_c9 !n2_r5_c7
!n2_r4_c9 !n2_r5_c8
!n2_r4_c9 !n2_r6_c7
!n2_r4_c9 !n2_r6_c8
!n3_r4_c9 !n4_r4_c9
!n3_r4_c9 !n5_r4_c9
!n3_r4_c9 !n6_r4_c9
!n3_r4_c9 !n7_r4_c9
!n3_r4_c9 !n8_r4_c9
!n3_r4_c9 !n9_r4_c9
!n3_r4_c9 !n3_r5_c9
!n3_r4_c9 !n3_r6_c9
!n3_r4_c9 !n3_r7_c9
!n3_r4_c9 !n3_r8_c9
!n3_r4_c9 !n3_r9_c9
!n3_r4_c9 !n3_r5_c7
!n3_r4_c9 !n3_r5_c8
!n3_r4_c9 !n3_r6_c7
!n3_r4_c9 !n3_r6_c8
!n4_r4_c9 !n5_r4_c9
!n4_r4_c9 !n6_r4_c9
!n4_r4_c9 !n7_r4_c9
!n4_r4_c9 !n8_r4_c9
!n4_r4_c9 !n9_r4_c9
!n4_r4_c9 !n4_r5_c9
!n4_r4_c9 !n4_r6_c9
!n4_r4_c9 !n4_r7_c9
!n4_r4_c9 !n4_r8_c9
!n4_r4_c9 !n4_r9_c9
!n4_r4_c9 !n4_r5_c7
!n4_r4_c9 !n4_r5_c8
!n4_r4_c9 !n4_r6_c7
!n4_r4_c9 !n4_r6_c8
!n5_r4_c9 !n6_r4_c9
!n5_r4_c9 !n7_r4_c9
!n5_r4_c9 !n8_r4_c9
!n5_r4_c9 !n9_r4_c9
!n5_r4_c9 !n5_r5_c9
!n5_r4_c9 !n5_r6_c9
!n5_r4_c9 !n5_r7_c9
!n5_r4_c9 !n5_r8_c9
!n5_r4_c9 !n5_r9_c9
!n5_r4_c9 !n5_r5_c7
!n5_r4_c9 !n5_r5_c8
!n5_r4_c9 !n5_r6_c7
!n5_r4_c9 !n5_r6_c8
!n6_r4_c9 !n7_r4_c9
!n6_r4_c9 !n8_r4_c9
!n6_r4_c9 !n9_r4_c9
!n6_r4_c9 !n6_r5_c9
!n6_r4_c9 !n6_r6_c9
!n6_r4_c9 !n6_r7_c9
!n6_r4_c9 !n6_r8_c9
!n6_r4_c9 !n6_r9_c9
!n6_r4_c9 !n6_r5_c7
!n6_r4_c9 !n6_r5_c8
!n6_r4_c9 !n6_r6_c7
!n6_r4_c9 !n6_r6_c8
!n7_r4_c9 !n8_r4_c9
!n7_r4_c9 !n9_r4_c9
!n7_r4_c9 !n7_r5_c9
!n7_r4_c9 !n7_r6_c9
!n7_r4_c9 !n7_r7_c9
!n7_r4_c9 !n7_r8_c9
!n7_r4_c9 !n7_r9_c9
!n7_r4_c9 !n7_r5_c7
!n7_r4_c9 !n7_r5_c8
!n7_r4_c9 !n7_r6_c7
!n7_r4_c9 !n7_r6_c8
!n8_r4_c9 !n9_r4_c9
!n8_r4_c9 !n8_r5_c9
!n8_r4_c9 !n8_r6_c9
!n8_r4_c9 !n8_r7_c9
!n8_r4_c9 !n8_r8_c9
!n8_r4_c9 !n8_r9_c9
!n8_r4_c9 !n8_r5_c7
!n8_r4_c9 !n8_r5_c8
!n8_r4_c9 !n8_r6_c7
!n8_r4_c9 !n8_r6_c8
!n9_r4_c9 !n9_r5_c9
!n9_r4_c9 !n9_r6_c9
!n9_r4_c9 !n9_r7_c9
!n9_r4_c9 !n9_r8_c9
!n9_r4_c9 !n9_r9_c9
!n9_r4_c9 !n9_r5_c7
!n9_r4_c9 !n9_r5_c8
!n9_r4_c9 !n9_r6_c7
!n9_r4_c9 !n9_r6_c8
n1_r4_c9 n2_r4_c9 n3_r4_c9 n4_r4_c9 n5_r4_c9 n6_r4_c9 n7_r4_c9 n8_r4_c9 n9_r4_c9
!n1_r5_c1 !n2_r5_c1
!n1_r5_c1 !n3_r5_c1
!n1_r5_c1 !n4_r5_c1
!n1_r5_c1 !n5_r5_c1
!n1_r5_c1 !n6_r5_c1
!n1_r5_c1 !n7_r5_c1
!n1_r5_c1 !n8_r5_c1
!n1_r5_c1 !n9_r5_c1
!n1_r5_c1 !n1_r5_c2
!n1_r5_c1 !n1_r5_c3
!n1_r5_c1 !n1_r5_c4
!n1_r5_c1 !n1_r5_c5
!n1_r5_c1 !n1_r5_c6
!n1_r5_c1 !n1_r5_c7
!n1_r5_c1 !n1_r5_c8
!n1_r5_c1 !n1_r5_c9
!n1_r5_c1 !n1_r6_c1
!n1_r5_c1 !n1_r7_c1
!n1_r5_c1 !n1_r8_c1
!n1_r5_c1 !n1_r9_c1
!n1_r5_c1 !n1_r4_c2
!n1_r5_c1 !n1_r4_c3
!n1_r5_c1 !n1_r6_c2
!n1_r5_c1 !n1_r6_c3
!n2_r5_c1 !n3_r5_c1
!n2_r5_c1 !n4_r5_c1
!n2_r5_c1 !n5_r5_c1
!n2_r5_c1 !n6_r5_c1
!n2_r5_c1 !n7_r5_c1
!n2_r5_c1 !n8_r5_c1
!n2_r5_c1 !n9_r5_c1
!n2_r5_c1 !n2_r5_c2
!n2_r5_c1 !n2_r5_c3
!n2_r5_c1 !n2_r5_c4
!n2_r5_c1 !n2_r5_c5
!n2_r5_c1 !n2_r5_c6
!n2_r5_c1 !n2_r5_c7
!n2_r5_c1 !n2_r5_c8
!n2_r5_c1 !n2_r5_c9
!n2_r5_c1 !n2_r6_c1
!n2_r5_c1 !n2_r7_c1
!n2_r5_c1 !n2_r8_c1
!n2_r5_c1 !n2_r9_c1
!n2_r5_c1 !n2_r4_c2
!n2_r5_c1 !n2_r4_c3
!n2_r5_c1 !n2_r6_c2
!n2_r5_c1 !n2_r6_c3
!n3_r5_c1 !n4_r5_c1
!n3_r5_c1 !n5_r5_c1
!n3_r5_c1 !n6_r5_c1
!n3_r5_c1 !n7_r5_c1
!n3_r5_c1 !n8_r5_c1
!n3_r5_c1 !n9_r5_c1
!n3_r5_c1 !n3_r5_c2
!n3_r5_c1 !n3_r5_c3
!n3_r5_c1 !n3_r5_c4
!n3_r5_c1 !n3_r5_c5
!n3_r5_c1 !n3_r5_c6
!n3_r5_c1 !n3_r5_c7
!n3_r5_c1 !n3_r5_c8
!n3_r5_c1 !n3_r5_c9
!n3_r5_c1 !n3_r6_c1
!n3_r5_c1 !n3_r7_c1
!n3_r5_c1 !n3_r8_c1
!n3_r5_c1 !n3_r9_c1
!n3_r5_c1 !n3_r4_c2
!n3_r5_c1 !n3_r4_c3
!n3_r5_c1 !n3_r6_c2
!n3_r5_c1 !n3_r6_c3
!n4_r5_c1 !n5_r5_c1
!n4_r5_c1 !n6_r5_c1
!n4_r5_c1 !n7_r5_c1
!n4_r5_c1 !n8_r5_c1
!n4_r5_c1 !n9_r5_c1
!n4_r5_c1 !n4_r5_c2
!n4_r5_c1 !n4_r5_c3
!n4_r5_c1 !n4_r5_c4
!n4_r5_c1 !n4_r5_c5
!n4_r5_c1 !n4_r5_c6
!n4_r5_c1 !n4_r5_c7
!n4_r5_c1 !n4_r5_c8
!n4_r5_c1 !n4_r5_c9
!n4_r5_c1 !n4_r6_c1
!n4_r5_c1 !n4_r7_c1
!n4_r5_c1 !n4_r8_c1
!n4_r5_c1 !n4_r9_c1
!n4_r5_c1 !n4_r4_c2
!n4_r5_c1 !n4_r4_c3
!n4_r5_c1 !n4_r6_c2
!n4_r5_c1 !n4_r6_c3
!n5_r5_c1 !n6_r5_c1
!n5_r5_c1 !n7_r5_c1
!n5_r5_c1 !n8_r5_c1
!n5_r5_c1 !n9_r5_c1
!n5_r5_c1 !n5_r5_c2
!n5_r5_c1 !n5_r5_c3
!n5_r5_c1 !n5_r5_c4
!n5_r5_c1 !n5_r5_c5
!n5_r5_c1 !n5_r5_c6
!n5_r5_c1 !n5_r5_c7
!n5_r5_c1 !n5_r5_c8
!n5_r5_c1 !n5_r5_c9
!n5_r5_c1 !n5_r6_c1
!n5_r5_c1 !n5_r7_c1
!n5_r5_c1 !n5_r8_c1
!n5_r5_c1 !n5_r9_c1
!n5_r5_c1 !n5_r4_c2
!n5_r5_c1 !n5_r4_c3
!n5_r5_c1 !n5_r6_c2
!n5_r5_c1 !n5_r6_c3
!n6_r5_c1 !n7_r5_c1
!n6_r5_c1 !n8_r5_c1
!n6_r5_c1 !n9_r5_c1
!n6_r5_c1 !n6_r5_c2
!n6_r5_c1 !n6_r5_c3
!n6_r5_c1 !n6_r5_c4
!n6_r5_c1 !n6_r5_c5
!n6_r5_c1 !n6_r5_c6
!n6_r5_c1 !n6_r5_c7
!n6_r5_c1 !n6_r5_c8
!n6_r5_c1 !n6_r5_c9
!n6_r5_c1 !n6_r6_c1
!n6_r5_c1 !n6_r7_c1
!n6_r5_c1 !n6_r8_c1
!n6_r5_c1 !n6_r9_c1
!n6_r5_c1 !n6_r4_c2
!n6_r5_c1 !n6_r4_c3
!n6_r5_c1 !n6_r6_c2
!n6_r5_c1 !n6_r6_c3
!n7_r5_c1 !n8_r5_c1
!n7_r5_c1 !n9_r5_c1
!n7_r5_c1 !n7_r5_c2
!n7_r5_c1 !n7_r5_c3
!n7_r5_c1 !n7_r5_c4
!n7_r5_c1 !n7_r5_c5
!n7_r5_c1 !n7_r5_c6
!n7_r5_c1 !n7_r5_c7
!n7_r5_c1 !n7_r5_c8
!n7_r5_c1 !n7_r5_c9
!n7_r5_c1 !n7_r6_c1
!n7_r5_c1 !n7_r7_c1
!n7_r5_c1 !n7_r8_c1
!n7_r5_c1 !n7_r9_c1
!n7_r5_c1 !n7_r4_c2
!n7_r5_c1 !n7_r4_c3
!n7_r5_c1 !n7_r6_c2
!n7_r5_c1 !n7_r6_c3
!n8_r5_c1 !n9_r5_c1
!n8_r5_c1 !n8_r5_c2
!n8_r5_c1 !n8_r5_c3
!n8_r5_c1 !n8_r5_c4
!n8_r5_c1 !n8_r5_c5
!n8_r5_c1 !n8_r5_c6
!n8_r5_c1 !n8_r5_c7
!n8_r5_c1 !n8_r5_c8
!n8_r5_c1 !n8_r5_c9
!n8_r5_c1 !n8_r6_c1
!n8_r5_c1 !n8_r7_c1
!n8_r5_c1 !n8_r8_c1
!n8_r5_c1 !n8_r9_c1
!n8_r5_c1 !n8_r4_c2
!n8_r5_c1 !n8_r4_c3
!n8_r5_c1 !n8_r6_c2
!n8_r5_c1 !n8_r6_c3
!n9_r5_c1 !n9_r5_c2
!n9_r5_c1 !n9_r5_c3
!n9_r5_c1 !n9_r5_c4
!n9_r5_c1 !n9_r5_c5
!n9_r5_c1 !n9_r5_c6
!n9_r5_c1 !n9_r5_c7
!n9_r5_c1 !n9_r5_c8
!n9_r5_c1 !n9_r5_c9
!n9_r5_c1 !n9_r6_c1
!n9_r5_c1 !n9_r7_c1
!n9_r5_c1 !n9_r8_c1
!n9_r5_c1 !n9_r9_c1
!n9_r5_c1 !n9_r4_c2
!n9_r5_c1 !n9_r4_c3
!n9_r5_c1 !n9_r6_c2
!n9_r5_c1 !n9_r6_c3
n1_r5_c1 n2_r5_c1 n3_r5_c1 n4_r5_c1 n5_r5_c1 n6_r5_c1 n7_r5_c1 n8_r5_c1 n9_r5_c1
!n1_r5_c2 !n2_r5_c2
!n1_r5_c2 !n3_r5_c2
!n1_r5_c2 !n4_r5_c2
!n1_r5_c2 !n5_r5_c2
!n1_r5_c2 !n6_r5_c2
!n1_r5_c2 !n7_r5_c2
!n1_r5_c2 !n8_r5_c2
!n1_r5_c2 !n9_r5_c2
!n1_r5_c2 !n1_r5_c3
!n1_r5_c2 !n1_r5_c4
!n1_r5_c2 !n1_r5_c5
!n1_r5_c2 !n1_r5_c6
!n1_r5_c2 !n1_r5_c7
!n1_r5_c2 !n1_r5_c8
!n1_r5_c2 !n1_r5_c9
!n1_r5_c2 !n1_r6_c2
!n1_r5_c2 !n1_r7_c2
!n1_r5_c2 !n1_r8_c2
!n1_r5_c2 !n1_r9_c2
!n1_r5_c2 !n1_r4_c1
!n1_r5_c2 !n1_r4_c3
!n1_r5_c2 !n1_r6_c1
!n1_r5_c2 !n1_r6_c3
!n2_r5_c2 !n3_r5_c2
!n2_r5_c2 !n4_r5_c2
!n2_r5_c2 !n5_r5_c2
!n2_r5_c2 !n6_r5_c2
!n2_r5_c2 !n7_r5_c2
!n2_r5_c2 !n8_r5_c2
!n2_r5_c2 !n9_r5_c2
!n2_r5_c2 !n2_r5_c3
!n2_r5_c2 !n2_r5_c4
!n2_r5_c2 !n2_r5_c5
!n2_r5_c2 !n2_r5_c6
!n2_r5_c2 !n2_r5_c7
!n2_r5_c2 !n2_r5_c8
!n2_r5_c2 !n2_r5_c9
!n2_r5_c2 !n2_r6_c2
!n2_r5_c2 !n2_r7_c2
!n2_r5_c2 !n2_r8_c2
!n2_r5_c2 !n2_r9_c2
!n2_r5_c2 !n2_r4_c1
!n2_r5_c2 !n2_r4_c3
!n2_r5_c2 !n2_r6_c1
!n2_r5_c2 !n2_r6_c3
!n3_r5_c2 !n4_r5_c2
!n3_r5_c2 !n5_r5_c2
!n3_r5_c2 !n6_r5_c2
!n3_r5_c2 !n7_r5_c2
!n3_r5_c2 !n8_r5_c2
!n3_r5_c2 !n9_r5_c2
!n3_r5_c2 !n3_r5_c3
!n3_r5_c2 !n3_r5_c4
!n3_r5_c2 !n3_r5_c5
!n3_r5_c2 !n3_r5_c6
!n3_r5_c2 !n3_r5_c7
!n3_r5_c2 !n3_r5_c8
!n3_r5_c2 !n3_r5_c9
!n3_r5_c2 !n3_r6_c2
!n3_r5_c2 !n3_r7_c2
!n3_r5_c2 !n3_r8_c2
!n3_r5_c2 !n3_r9_c2
!n3_r5_c2 !n3_r4_c1
!n3_r5_c2 !n3_r4_c3
!n3_r5_c2 !n3_r6_c1
!n3_r5_c2 !n3_r6_c3
!n4_r5_c2 !n5_r5_c2
!n4_r5_c2 !n6_r5_c2
!n4_r5_c2 !n7_r5_c2
!n4_r5_c2 !n8_r5_c2
!n4_r5_c2 !n9_r5_c2
!n4_r5_c2 !n4_r5_c3
!n4_r5_c2 !n4_r5_c4
!n4_r5_c2 !n4_r5_c5
!n4_r5_c2 !n4_r5_c6
!n4_r5_c2 !n4_r5_c7
!n4_r5_c2 !n4_r5_c8
!n4_r5_c2 !n4_r5_c9
!n4_r5_c2 !n4_r6_c2
!n4_r5_c2 !n4_r7_c2
!n4_r5_c2 !n4_r8_c2
!n4_r5_c2 !n4_r9_c2
!n4_r5_c2 !n4_r4_c1
!n4_r5_c2 !n4_r4_c3
!n4_r5_c2 !n4_r6_c1
!n4_r5_c2 !n4_r6_c3
!n5_r5_c2 !n6_r5_c2
!n5_r5_c2 !n7_r5_c2
!n5_r5_c2 !n8_r5_c2
!n5_r5_c2 !n9_r5_c2
!n5_r5_c2 !n5_r5_c3
!n5_r5_c2 !n5_r5_c4
!n5_r5_c2 !n5_r5_c5
!n5_r5_c2 !n5_r5_c6
!n5_r5_c2 !n5_r5_c7
!n5_r5_c2 !n5_r5_c8
!n5_r5_c2 !n5_r5_c9
!n5_r5_c2 !n5_r6_c2
!n5_r5_c2 !n5_r7_c2
!n5_r5_c2 !n5_r8_c2
!n5_r5_c2 !n5_r9_c2
!n5_r5_c2 !n5_r4_c1
!n5_r5_c2 !n5_r4_c3
!n5_r5_c2 !n5_r6_c1
!n5_r5_c2 !n5_r6_c3
!n6_r5_c2 !n7_r5_c2
!n6_r5_c2 !n8_r5_c2
!n6_r5_c2 !n9_r5_c2
!n6_r5_c2 !n6_r5_c3
!n6_r5_c2 !n6_r5_c4
!n6_r5_c2 !n6_r5_c5
!n6_r5_c2 !n6_r5_c6
!n6_r5_c2 !n6_r5_c7
!n6_r5_c2 !n6_r5_c8
!n6_r5_c2 !n6_r5_c9
!n6_r5_c2 !n6_r6_c2
!n6_r5_c2 !n6_r7_c2
!n6_r5_c2 !n6_r8_c2
!n6_r5_c2 !n6_r9_c2
!n6_r5_c2 !n6_r4_c1
!n6_r5_c2 !n6_r4_c3
!n6_r5_c2 !n6_r6_c1
!n6_r5_c2 !n6_r6_c3
!n7_r5_c2 !n8_r5_c2
!n7_r5_c2 !n9_r5_c2
!n7_r5_c2 !n7_r5_c3
!n7_r5_c2 !n7_r5_c4
!n7_r5_c2 !n7_r5_c5
!n7_r5_c2 !n7_r5_c6
!n7_r5_c2 !n7_r5_c7
!n7_r5_c2 !n7_r5_c8
!n7_r5_c2 !n7_r5_c9
!n7_r5_c2 !n7_r6_c2
!n7_r5_c2 !n7_r7_c2
!n7_r5_c2 !n7_r8_c2
!n7_r5_c2 !n7_r9_c2
!n7_r5_c2 !n7_r4_c1
!n7_r5_c2 !n7_r4_c3
!n7_r5_c2 !n7_r6_c1
!n7_r5_c2 !n7_r6_c3
!n8_r5_c2 !n9_r5_c2
!n8_r5_c2 !n8_r5_c3
!n8_r5_c2 !n8_r5_c4
!n8_r5_c2 !n8_r5_c5
!n8_r5_c2 !n8_r5_c6
!n8_r5_c2 !n8_r5_c7
!n8_r5_c2 !n8_r5_c8
!n8_r5_c2 !n8_r5_c9
!n8_r5_c2 !n8_r6_c2
!n8_r5_c2 !n8_r7_c2
!n8_r5_c2 !n8_r8_c2
!n8_r5_c2 !n8_r9_c2
!n8_r5_c2 !n8_r4_c1
!n8_r5_c2 !n8_r4_c3
!n8_r5_c2 !n8_r6_c1
!n8_r5_c2 !n8_r6_c3
!n9_r5_c2 !n9_r5_c3
!n9_r5_c2 !n9_r5_c4
!n9_r5_c2 !n9_r5_c5
!n9_r5_c2 !n9_r5_c6
!n9_r5_c2 !n9_r5_c7
!n9_r5_c2 !n9_r5_c8
!n9_r5_c2 !n9_r5_c9
!n9_r5_c2 !n9_r6_c2
!n9_r5_c2 !n9_r7_c2
!n9_r5_c2 !n9_r8_c2
!n9_r5_c2 !n9_r9_c2
!n9_r5_c2 !n9_r4_c1
!n9_r5_c2 !n9_r4_c3
!n9_r5_c2 !n9_r6_c1
!n9_r5_c2 !n9_r6_c3
n1_r5_c2 n2_r5_c2 n3_r5_c2 n4_r5_c2 n5_r5_c2 n6_r5_c2 n7_r5_c2 n8_r5_c2 n9_r5_c2
!n1_r5_c3 !n2_r5_c3
!n1_r5_c3 !n3_r5_c3
!n1_r5_c3 !n4_r5_c3
!n1_r5_c3 !n5_r5_c3
!n1_r5_c3 !n6_r5_c3
!n1_r5_c3 !n7_r5_c3
!n1_r5_c3 !n8_r5_c3
!n1_r5_c3 !n9_r5_c3
!n1_r5_c3 !n1_r5_c4
!n1_r5_c3 !n1_r5_c5
!n1_r5_c3 !n1_r5_c6
!n1_r5_c3 !n1_r5_c7
!n1_r5_c3 !n1_r5_c8
!n1_r5_c3 !n1_r5_c9
!n1_r5_c3 !n1_r6_c3
!n1_r5_c3 !n1_r7_c3
!n1_r5_c3 !n1_r8_c3
!n1_r5_c3 !n1_r9_c3
!n1_r5_c3 !n1_r4_c1
!n1_r5_c3 !n1_r4_c2
!n1_r5_c3 !n1_r6_c1
!n1_r5_c3 !n1_r6_c2
!n2_r5_c3 !n3_r5_c3
!n2_r5_c3 !n4_r5_c3
!n2_r5_c3 !n5_r5_c3
!n2_r5_c3 !n6_r5_c3
!n2_r5_c3 !n7_r5_c3
!n2_r5_c3 !n8_r5_c3
!n2_r5_c3 !n9_r5_c3
!n2_r5_c3 !n2_r5_c4
!n2_r5_c3 !n2_r5_c5
!n2_r5_c3 !n2_r5_c6
!n2_r5_c3 !n2_r5_c7
!n2_r5_c3 !n2_r5_c8
!n2_r5_c3 !n2_r5_c9
!n2_r5_c3 !n2_r6_c3
!n2_r5_c3 !n2_r7_c3
!n2_r5_c3 !n2_r8_c3
!n2_r5_c3 !n2_r9_c3
!n2_r5_c3 !n2_r4_c1
!n2_r5_c3 !n2_r4_c2
!n2_r5_c3 !n2_r6_c1
!n2_r5_c3 !n2_r6_c2
!n3_r5_c3 !n4_r5_c3
!n3_r5_c3 !n5_r5_c3
!n3_r5_c3 !n6_r5_c3
!n3_r5_c3 !n7_r5_c3
!n3_r5_c3 !n8_r5_c3
!n3_r5_c3 !n9_r5_c3
!n3_r5_c3 !n3_r5_c4
!n3_r5_c3 !n3_r5_c5
!n3_r5_c3 !n3_r5_c6
!n3_r5_c3 !n3_r5_c7
!n3_r5_c3 !n3_r5_c8
!n3_r5_c3 !n3_r5_c9
!n3_r5_c3 !n3_r6_c3
!n3_r5_c3 !n3_r7_c3
!n3_r5_c3 !n3_r8_c3
!n3_r5_c3 !n3_r9_c3
!n3_r5_c3 !n3_r4_c1
!n3_r5_c3 !n3_r4_c2
!n3_r5_c3 !n3_r6_c1
!n3_r5_c3 !n3_r6_c2
!n4_r5_c3 !n5_r5_c3
!n4_r5_c3 !n6_r5_c3
!n4_r5_c3 !n7_r5_c3
!n4_r5_c3 !n8_r5_c3
!n4_r5_c3 !n9_r5_c3
!n4_r5_c3 !n4_r5_c4
!n4_r5_c3 !n4_r5_c5
!n4_r5_c3 !n4_r5_c6
!n4_r5_c3 !n4_r5_c7
!n4_r5_c3 !n4_r5_c8
!n4_r5_c3 !n4_r5_c9
!n4_r5_c3 !n4_r6_c3
!n4_r5_c3 !n4_r7_c3
!n4_r5_c3 !n4_r8_c3
!n4_r5_c3 !n4_r9_c3
!n4_r5_c3 !n4_r4_c1
!n4_r5_c3 !n4_r4_c2
!n4_r5_c3 !n4_r6_c1
!n4_r5_c3 !n4_r6_c2
!n5_r5_c3 !n6_r5_c3
!n5_r5_c3 !n7_r5_c3
!n5_r5_c3 !n8_r5_c3
!n5_r5_c3 !n9_r5_c3
!n5_r5_c3 !n5_r5_c4
!n5_r5_c3 !n5_r5_c5
!n5_r5_c3 !n5_r5_c6
!n5_r5_c3 !n5_r5_c7
!n5_r5_c3 !n5_r5_c8
!n5_r5_c3 !n5_r5_c9
!n5_r5_c3 !n5_r6_c3
!n5_r5_c3 !n5_r7_c3
!n5_r5_c3 !n5_r8_c3
!n5_r5_c3 !n5_r9_c3
!n5_r5_c3 !n5_r4_c1
!n5_r5_c3 !n5_r4_c2
!n5_r5_c3 !n5_r6_c1
!n5_r5_c3 !n5_r6_c2
!n6_r5_c3 !n7_r5_c3
!n6_r5_c3 !n8_r5_c3
!n6_r5_c3 !n9_r5_c3
!n6_r5_c3 !n6_r5_c4
!n6_r5_c3 !n6_r5_c5
!n6_r5_c3 !n6_r5_c6
!n6_r5_c3 !n6_r5_c7
!n6_r5_c3 !n6_r5_c8
!n6_r5_c3 !n6_r5_c9
!n6_r5_c3 !n6_r6_c3
!n6_r5_c3 !n6_r7_c3
!n6_r5_c3 !n6_r8_c3
!n6_r5_c3 !n6_r9_c3
!n6_r5_c3 !n6_r4_c1
!n6_r5_c3 !n6_r4_c2
!n6_r5_c3 !n6_r6_c1
!n6_r5_c3 !n6_r6_c2
!n7_r5_c3 !n8_r5_c3
!n7_r5_c3 !n9_r5_c3
!n7_r5_c3 !n7_r5_c4
!n7_r5_c3 !n7_r5_c5
!n7_r5_c3 !n7_r5_c6
!n7_r5_c3 !n7_r5_c7
!n7_r5_c3 !n7_r5_c8
!n7_r5_c3 !n7_r5_c9
!n7_r5_c3 !n7_r6_c3
!n7_r5_c3 !n7_r7_c3
!n7_r5_c3 !n7_r8_c3
!n7_r5_c3 !n7_r9_c3
!n7_r5_c3 !n7_r4_c1
!n7_r5_c3 !n7_r4_c2
!n7_r5_c3 !n7_r6_c1
!n7_r5_c3 !n7_r6_c2
!n8_r5_c3 !n9_r5_c3
!n8_r5_c3 !n8_r5_c4
!n8_r5_c3 !n8_r5_c5
!n8_r5_c3 !n8_r5_c6
!n8_r5_c3 !n8_r5_c7
!n8_r5_c3 !n8_r5_c8
!n8_r5_c3 !n8_r5_c9
!n8_r5_c3 !n8_r6_c3
!n8_r5_c3 !n8_r7_c3
!n8_r5_c3 !n8_r8_c3
!n8_r5_c3 !n8_r9_c3
!n8_r5_c3 !n8_r4_c1
!n8_r5_c3 !n8_r4_c2
!n8_r5_c3 !n8_r6_c1
!n8_r5_c3 !n8_r6_c2
!n9_r5_c3 !n9_r5_c4
!n9_r5_c3 !n9_r5_c5
!n9_r5_c3 !n9_r5_c6
!n9_r5_c3 !n9_r5_c7
!n9_r5_c3 !n9_r5_c8
!n9_r5_c3 !n9_r5_c9
!n9_r5_c3 !n9_r6_c3
!n9_r5_c3 !n9_r7_c3
!n9_r5_c3 !n9_r8_c3
!n9_r5_c3 !n9_r9_c3
!n9_r5_c3 !n9_r4_c1
!n9_r5_c3 !n9_r4_c2
!n9_r5_c3 !n9_r6_c1
!n9_r5_c3 !n9_r6_c2
n1_r5_c3 n2_r5_c3 n3_r5_c3 n4_r5_c3 n5_r5_c3 n6_r5_c3 n7_r5_c3 n8_r5_c3 n9_r5_c3
!n1_r5_c4 !n2_r5_c4
!n1_r5_c4 !n3_r5_c4
!n1_r5_c4 !n4_r5_c4
!n1_r5_c4 !n5_r5_c4
!n1_r5_c4 !n6_r5_c4
!n1_r5_c4 !n7_r5_c4
!n1_r5_c4 !n8_r5_c4
!n1_r5_c4 !n9_r5_c4
!n1_r5_c4 !n1_r5_c5
!n1_r5_c4 !n1_r5_c6
!n1_r5_c4 !n1_r5_c7
!n1_r5_c4 !n1_r5_c8
!n1_r5_c4 !n1_r5_c9
!n1_r5_c4 !n1_r6_c4
!n1_r5_c4 !n1_r7_c4
!n1_r5_c4 !n1_r8_c4
!n1_r5_c4 !n1_r9_c4
!n1_r5_c4 !n1_r4_c5
!n1_r5_c4 !n1_r4_c6
!n1_r5_c4 !n1_r6_c5
!n1_r5_c4 !n1_r6_c6
!n2_r5_c4 !n3_r5_c4
!n2_r5_c4 !n4_r5_c4
!n2_r5_c4 !n5_r5_c4
!n2_r5_c4 !n6_r5_c4
!n2_r5_c4 !n7_r5_c4
!n2_r5_c4 !n8_r5_c4
!n2_r5_c4 !n9_r5_c4
!n2_r5_c4 !n2_r5_c5
!n2_r5_c4 !n2_r5_c6
!n2_r5_c4 !n2_r5_c7
!n2_r5_c4 !n2_r5_c8
!n2_r5_c4 !n2_r5_c9
!n2_r5_c4 !n2_r6_c4
!n2_r5_c4 !n2_r7_c4
!n2_r5_c4 !n2_r8_c4
!n2_r5_c4 !n2_r9_c4
!n2_r5_c4 !n2_r4_c5
!n2_r5_c4 !n2_r4_c6
!n2_r5_c4 !n2_r6_c5
!n2_r5_c4 !n2_r6_c6
!n3_r5_c4 !n4_r5_c4
!n3_r5_c4 !n5_r5_c4
!n3_r5_c4 !n6_r5_c4
!n3_r5_c4 !n7_r5_c4
!n3_r5_c4 !n8_r5_c4
!n3_r5_c4 !n9_r5_c4
!n3_r5_c4 !n3_r5_c5
!n3_r5_c4 !n3_r5_c6
!n3_r5_c4 !n3_r5_c7
!n3_r5_c4 !n3_r5_c8
!n3_r5_c4 !n3_r5_c9
!n3_r5_c4 !n3_r6_c4
!n3_r5_c4 !n3_r7_c4
!n3_r5_c4 !n3_r8_c4
!n3_r5_c4 !n3_r9_c4
!n3_r5_c4 !n3_r4_c5
!n3_r5_c4 !n3_r4_c6
!n3_r5_c4 !n3_r6_c5
!n3_r5_c4 !n3_r6_c6
!n4_r5_c4 !n5_r5_c4
!n4_r5_c4 !n6_r5_c4
!n4_r5_c4 !n7_r5_c4
!n4_r5_c4 !n8_r5_c4
!n4_r5_c4 !n9_r5_c4
!n4_r5_c4 !n4_r5_c5
!n4_r5_c4 !n4_r5_c6
!n4_r5_c4 !n4_r5_c7
!n4_r5_c4 !n4_r5_c8
!n4_r5_c4 !n4_r5_c9
!n4_r5_c4 !n4_r6_c4
!n4_r5_c4 !n4_r7_c4
!n4_r5_c4 !n4_r8_c4
!n4_r5_c4 !n4_r9_c4
!n4_r5_c4 !n4_r4_c5
!n4_r5_c4 !n4_r4_c6
!n4_r5_c4 !n4_r6_c5
!n4_r5_c4 !n4_r6_c6
!n5_r5_c4 !n6_r5_c4
!n5_r5_c4 !n7_r5_c4
!n5_r5_c4 !n8_r5_c4
!n5_r5_c4 !n9_r5_c4
!n5_r5_c4 !n5_r5_c5
!n5_r5_c4 !n5_r5_c6
!n5_r5_c4 !n5_r5_c7
!n5_r5_c4 !n5_r5_c8
!n5_r5_c4 !n5_r5_c9
!n5_r5_c4 !n5_r6_c4
!n5_r5_c4 !n5_r7_c4
!n5_r5_c4 !n5_r8_c4
!n5_r5_c4 !n5_r9_c4
!n5_r5_c4 !n5_r4_c5
!n5_r5_c4 !n5_r4_c6
!n5_r5_c4 !n5_r6_c5
!n5_r5_c4 !n5_r6_c6
!n6_r5_c4 !n7_r5_c4
!n6_r5_c4 !n8_r5_c4
!n6_r5_c4 !n9_r5_c4
!n6_r5_c4 !n6_r5_c5
!n6_r5_c4 !n6_r5_c6
!n6_r5_c4 !n6_r5_c7
!n6_r5_c4 !n6_r5_c8
!n6_r5_c4 !n6_r5_c9
!n6_r5_c4 !n6_r6_c4
!n6_r5_c4 !n6_r7_c4
!n6_r5_c4 !n6_r8_c4
!n6_r5_c4 !n6_r9_c4
!n6_r5_c4 !n6_r4_c5
!n6_r5_c4 !n6_r4_c6
!n6_r5_c4 !n6_r6_c5
!n6_r5_c4 !n6_r6_c6
!n7_r5_c4 !n8_r5_c4
!n7_r5_c4 !n9_r5_c4
!n7_r5_c4 !n7_r5_c5
!n7_r5_c4 !n7_r5_c6
!n7_r5_c4 !n7_r5_c7
!n7_r5_c4 !n7_r5_c8
!n7_r5_c4 !n7_r5_c9
!n7_r5_c4 !n7_r6_c4
!n7_r5_c4 !n7_r7_c4
!n7_r5_c4 !n7_r8_c4
!n7_r5_c4 !n7_r9_c4
!n7_r5_c4 !n7_r4_c5
!n7_r5_c4 !n7_r4_c6
!n7_r5_c4 !n7_r6_c5
!n7_r5_c4 !n7_r6_c6
!n8_r5_c4 !n9_r5_c4
!n8_r5_c4 !n8_r5_c5
!n8_r5_c4 !n8_r5_c6
!n8_r5_c4 !n8_r5_c7
!n8_r5_c4 !n8_r5_c8
!n8_r5_c4 !n8_r5_c9
!n8_r5_c4 !n8_r6_c4
!n8_r5_c4 !n8_r7_c4
!n8_r5_c4 !n8_r8_c4
!n8_r5_c4 !n8_r9_c4
!n8_r5_c4 !n8_r4_c5
!n8_r5_c4 !n8_r4_c6
!n8_r5_c4 !n8_r6_c5
!n8_r5_c4 !n8_r6_c6
!n9_r5_c4 !n9_r5_c5
!n9_r5_c4 !n9_r5_c6
!n9_r5_c4 !n9_r5_c7
!n9_r5_c4 !n9_r5_c8
!n9_r5_c4 !n9_r5_c9
!n9_r5_c4 !n9_r6_c4
!n9_r5_c4 !n9_r7_c4
!n9_r5_c4 !n9_r8_c4
!n9_r5_c4 !n9_r9_c4
!n9_r5_c4 !n9_r4_c5
!n9_r5_c4 !n9_r4_c6
!n9_r5_c4 !n9_r6_c5
!n9_r5_c4 !n9_r6_c6
n1_r5_c4 n2_r5_c4 n3_r5_c4 n4_r5_c4 n5_r5_c4 n6_r5_c4 n7_r5_c4 n8_r5_c4 n9_r5_c4
!n1_r5_c5 !n2_r5_c5
!n1_r5_c5 !n3_r5_c5
!n1_r5_c5 !n4_r5_c5
!n1_r5_c5 !n5_r5_c5
!n1_r5_c5 !n6_r5_c5
!n1_r5_c5 !n7_r5_c5
!n1_r5_c5 !n8_r5_c5
!n1_r5_c5 !n9_r5_c5
!n1_r5_c5 !n1_r5_c6
!n1_r5_c5 !n1_r5_c7
!n1_r5_c5 !n1_r5_c8
!n1_r5_c5 !n1_r5_c9
!n1_r5_c5 !n1_r6_c5
!n1_r5_c5 !n1_r7_c5
!n1_r5_c5 !n1_r8_c5
!n1_r5_c5 !n1_r9_c5
!n1_r5_c5 !n1_r4_c4
!n1_r5_c5 !n1_r4_c6
!n1_r5_c5 !n1_r6_c4
!n1_r5_c5 !n1_r6_c6
!n2_r5_c5 !n3_r5_c5
!n2_r5_c5 !n4_r5_c5
!n2_r5_c5 !n5_r5_c5
!n2_r5_c5 !n6_r5_c5
!n2_r5_c5 !n7_r5_c5
!n2_r5_c5 !n8_r5_c5
!n2_r5_c5 !n9_r5_c5
!n2_r5_c5 !n2_r5_c6
!n2_r5_c5 !n2_r5_c7
!n2_r5_c5 !n2_r5_c8
!n2_r5_c5 !n2_r5_c9
!n2_r5_c5 !n2_r6_c5
!n2_r5_c5 !n2_r7_c5
!n2_r5_c5 !n2_r8_c5
!n2_r5_c5 !n2_r9_c5
!n2_r5_c5 !n2_r4_c4
!n2_r5_c5 !n2_r4_c6
!n2_r5_c5 !n2_r6_c4
!n2_r5_c5 !n2_r6_c6
!n3_r5_c5 !n4_r5_c5
!n3_r5_c5 !n5_r5_c5
!n3_r5_c5 !n6_r5_c5
!n3_r5_c5 !n7_r5_c5
!n3_r5_c5 !n8_r5_c5
!n3_r5_c5 !n9_r5_c5
!n3_r5_c5 !n3_r5_c6
!n3_r5_c5 !n3_r5_c7
!n3_r5_c5 !n3_r5_c8
!n3_r5_c5 !n3_r5_c9
!n3_r5_c5 !n3_r6_c5
!n3_r5_c5 !n3_r7_c5
!n3_r5_c5 !n3_r8_c5
!n3_r5_c5 !n3_r9_c5
!n3_r5_c5 !n3_r4_c4
!n3_r5_c5 !n3_r4_c6
!n3_r5_c5 !n3_r6_c4
!n3_r5_c5 !n3_r6_c6
!n4_r5_c5 !n5_r5_c5
!n4_r5_c5 !n6_r5_c5
!n4_r5_c5 !n7_r5_c5
!n4_r5_c5 !n8_r5_c5
!n4_r5_c5 !n9_r5_c5
!n4_r5_c5 !n4_r5_c6
!n4_r5_c5 !n4_r5_c7
!n4_r5_c5 !n4_r5_c8
!n4_r5_c5 !n4_r5_c9
!n4_r5_c5 !n4_r6_c5
!n4_r5_c5 !n4_r7_c5
!n4_r5_c5 !n4_r8_c5
!n4_r5_c5 !n4_r9_c5
!n4_r5_c5 !n4_r4_c4
!n4_r5_c5 !n4_r4_c6
!n4_r5_c5 !n4_r6_c4
!n4_r5_c5 !n4_r6_c6
!n5_r5_c5 !n6_r5_c5
!n5_r5_c5 !n7_r5_c5
!n5_r5_c5 !n8_r5_c5
!n5_r5_c5 !n9_r5_c5
!n5_r5_c5 !n5_r5_c6
!n5_r5_c5 !n5_r5_c7
!n5_r5_c5 !n5_r5_c8
!n5_r5_c5 !n5_r5_c9
!n5_r5_c5 !n5_r6_c5
!n5_r5_c5 !n5_r7_c5
!n5_r5_c5 !n5_r8_c5
!n5_r5_c5 !n5_r9_c5
!n5_r5_c5 !n5_r4_c4
!n5_r5_c5 !n5_r4_c6
!n5_r5_c5 !n5_r6_c4
!n5_r5_c5 !n5_r6_c6
!n6_r5_c5 !n7_r5_c5
!n6_r5_c5 !n8_r5_c5
!n6_r5_c5 !n9_r5_c5
!n6_r5_c5 !n6_r5_c6
!n6_r5_c5 !n6_r5_c7
!n6_r5_c5 !n6_r5_c8
!n6_r5_c5 !n6_r5_c9
!n6_r5_c5 !n6_r6_c5
!n6_r5_c5 !n6_r7_c5
!n6_r5_c5 !n6_r8_c5
!n6_r5_c5 !n6_r9_c5
!n6_r5_c5 !n6_r4_c4
!n6_r5_c5 !n6_r4_c6
!n6_r5_c5 !n6_r6_c4
!n6_r5_c5 !n6_r6_c6
!n7_r5_c5 !n8_r5_c5
!n7_r5_c5 !n9_r5_c5
!n7_r5_c5 !n7_r5_c6
!n7_r5_c5 !n7_r5_c7
!n7_r5_c5 !n7_r5_c8
!n7_r5_c5 !n7_r5_c9
!n7_r5_c5 !n7_r6_c5
!n7_r5_c5 !n7_r7_c5
!n7_r5_c5 !n7_r8_c5
!n7_r5_c5 !n7_r9_c5
!n7_r5_c5 !n7_r4_c4
!n7_r5_c5 !n7_r4_c6
!n7_r5_c5 !n7_r6_c4
!n7_r5_c5 !n7_r6_c6
!n8_r5_c5 !n9_r5_c5
!n8_r5_c5 !n8_r5_c6
!n8_r5_c5 !n8_r5_c7
!n8_r5_c5 !n8_r5_c8
!n8_r5_c5 !n8_r5_c9
!n8_r5_c5 !n8_r6_c5
!n8_r5_c5 !n8_r7_c5
!n8_r5_c5 !n8_r8_c5
!n8_r5_c5 !n8_r9_c5
!n8_r5_c5 !n8_r4_c4
!n8_r5_c5 !n8_r4_c6
!n8_r5_c5 !n8_r6_c4
!n8_r5_c5 !n8_r6_c6
!n9_r5_c5 !n9_r5_c6
!n9_r5_c5 !n9_r5_c7
!n9_r5_c5 !n9_r5_c8
!n9_r5_c5 !n9_r5_c9
!n9_r5_c5 !n9_r6_c5
!n9_r5_c5 !n9_r7_c5
!n9_r5_c5 !n9_r8_c5
!n9_r5_c5 !n9_r9_c5
!n9_r5_c5 !n9_r4_c4
!n9_r5_c5 !n9_r4_c6
!n9_r5_c5 !n9_r6_c4
!n9_r5_c5 !n9_r6_c6
n1_r5_c5 n2_r5_c5 n3_r5_c5 n4_r5_c5 n5_r5_c5 n6_r5_c5 n7_r5_c5 n8_r5_c5 n9_r5_c5
!n1_r5_c6 !n2_r5_c6
!n1_r5_c6 !n3_r5_c6
!n1_r5_c6 !n4_r5_c6
!n1_r5_c6 !n5_r5_c6
!n1_r5_c6 !n6_r5_c6
!n1_r5_c6 !n7_r5_c6
!n1_r5_c6 !n8_r5_c6
!n1_r5_c6 !n9_r5_c6
!n1_r5_c6 !n1_r5_c7
!n1_r5_c6 !n1_r5_c8
!n1_r5_c6 !n1_r5_c9
!n1_r5_c6 !n1_r6_c6
!n1_r5_c6 !n1_r7_c6
!n1_r5_c6 !n1_r8_c6
!n1_r5_c6 !n1_r9_c6
!n1_r5_c6 !n1_r4_c4
!n1_r5_c6 !n1_r4_c5
!n1_r5_c6 !n1_r6_c4
!n1_r5_c6 !n1_r6_c5
!n2_r5_c6 !n3_r5_c6
!n2_r5_c6 !n4_r5_c6
!n2_r5_c6 !n5_r5_c6
!n2_r5_c6 !n6_r5_c6
!n2_r5_c6 !n7_r5_c6
!n2_r5_c6 !n8_r5_c6
!n2_r5_c6 !n9_r5_c6
!n2_r5_c6 !n2_r5_c7
!n2_r5_c6 !n2_r5_c8
!n2_r5_c6 !n2_r5_c9
!n2_r5_c6 !n2_r6_c6
!n2_r5_c6 !n2_r7_c6
!n2_r5_c6 !n2_r8_c6
!n2_r5_c6 !n2_r9_c6
!n2_r5_c6 !n2_r4_c4
!n2_r5_c6 !n2_r4_c5
!n2_r5_c6 !n2_r6_c4
!n2_r5_c6 !n2_r6_c5
!n3_r5_c6 !n4_r5_c6
!n3_r5_c6 !n5_r5_c6
!n3_r5_c6 !n6_r5_c6
!n3_r5_c6 !n7_r5_c6
!n3_r5_c6 !n8_r5_c6
!n3_r5_c6 !n9_r5_c6
!n3_r5_c6 !n3_r5_c7
!n3_r5_c6 !n3_r5_c8
!n3_r5_c6 !n3_r5_c9
!n3_r5_c6 !n3_r6_c6
!n3_r5_c6 !n3_r7_c6
!n3_r5_c6 !n3_r8_c6
!n3_r5_c6 !n3_r9_c6
!n3_r5_c6 !n3_r4_c4
!n3_r5_c6 !n3_r4_c5
!n3_r5_c6 !n3_r6_c4
!n3_r5_c6 !n3_r6_c5
!n4_r5_c6 !n5_r5_c6
!n4_r5_c6 !n6_r5_c6
!n4_r5_c6 !n7_r5_c6
!n4_r5_c6 !n8_r5_c6
!n4_r5_c6 !n9_r5_c6
!n4_r5_c6 !n4_r5_c7
!n4_r5_c6 !n4_r5_c8
!n4_r5_c6 !n4_r5_c9
!n4_r5_c6 !n4_r6_c6
!n4_r5_c6 !n4_r7_c6
!n4_r5_c6 !n4_r8_c6
!n4_r5_c6 !n4_r9_c6
!n4_r5_c6 !n4_r4_c4
!n4_r5_c6 !n4_r4_c5
!n4_r5_c6 !n4_r6_c4
!n4_r5_c6 !n4_r6_c5
!n5_r5_c6 !n6_r5_c6
!n5_r5_c6 !n7_r5_c6
!n5_r5_c6 !n8_r5_c6
!n5_r5_c6 !n9_r5_c6
!n5_r5_c6 !n5_r5_c7
!n5_r5_c6 !n5_r5_c8
!n5_r5_c6 !n5_r5_c9
!n5_r5_c6 !n5_r6_c6
!n5_r5_c6 !n5_r7_c6
!n5_r5_c6 !n5_r8_c6
!n5_r5_c6 !n5_r9_c6
!n5_r5_c6 !n5_r4_c4
!n5_r5_c6 !n5_r4_c5
!n5_r5_c6 !n5_r6_c4
!n5_r5_c6 !n5_r6_c5
!n6_r5_c6 !n7_r5_c6
!n6_r5_c6 !n8_r5_c6
!n6_r5_c6 !n9_r5_c6
!n6_r5_c6 !n6_r5_c7
!n6_r5_c6 !n6_r5_c8
!n6_r5_c6 !n6_r5_c9
!n6_r5_c6 !n6_r6_c6
!n6_r5_c6 !n6_r7_c6
!n6_r5_c6 !n6_r8_c6
!n6_r5_c6 !n6_r9_c6
!n6_r5_c6 !n6_r4_c4
!n6_r5_c6 !n6_r4_c5
!n6_r5_c6 !n6_r6_c4
!n6_r5_c6 !n6_r6_c5
!n7_r5_c6 !n8_r5_c6
!n7_r5_c6 !n9_r5_c6
!n7_r5_c6 !n7_r5_c7
!n7_r5_c6 !n7_r5_c8
!n7_r5_c6 !n7_r5_c9
!n7_r5_c6 !n7_r6_c6
!n7_r5_c6 !n7_r7_c6
!n7_r5_c6 !n7_r8_c6
!n7_r5_c6 !n7_r9_c6
!n7_r5_c6 !n7_r4_c4
!n7_r5_c6 !n7_r4_c5
!n7_r5_c6 !n7_r6_c4
!n7_r5_c6 !n7_r6_c5
!n8_r5_c6 !n9_r5_c6
!n8_r5_c6 !n8_r5_c7
!n8_r5_c6 !n8_r5_c8
!n8_r5_c6 !n8_r5_c9
!n8_r5_c6 !n8_r6_c6
!n8_r5_c6 !n8_r7_c6
!n8_r5_c6 !n8_r8_c6
!n8_r5_c6 !n8_r9_c6
!n8_r5_c6 !n8_r4_c4
!n8_r5_c6 !n8_r4_c5
!n8_r5_c6 !n8_r6_c4
!n8_r5_c6 !n8_r6_c5
!n9_r5_c6 !n9_r5_c7
!n9_r5_c6 !n9_r5_c8
!n9_r5_c6 !n9_r5_c9
!n9_r5_c6 !n9_r6_c6
!n9_r5_c6 !n9_r7_c6
!n9_r5_c6 !n9_r8_c6
!n9_r5_c6 !n9_r9_c6
!n9_r5_c6 !n9_r4_c4
!n9_r5_c6 !n9_r4_c5
!n9_r5_c6 !n9_r6_c4
!n9_r5_c6 !n9_r6_c5
n1_r5_c6 n2_r5_c6 n3_r5_c6 n4_r5_c6 n5_r5_c6 n6_r5_c6 n7_r5_c6 n8_r5_c6 n9_r5_c6
!n1_r5_c7 !n2_r5_c7
!n1_r5_c7 !n3_r5_c7
!n1_r5_c7 !n4_r5_c7
!n1_r5_c7 !n5_r5_c7
!n1_r5_c7 !n6_r5_c7
!n1_r5_c7 !n7_r5_c7
!n1_r5_c7 !n8_r5_c7
!n1_r5_c7 !n9_r5_c7
!n1_r5_c7 !n1_r5_c8
!n1_r5_c7 !n1_r5_c9
!n1_r5_c7 !n1_r6_c7
!n1_r5_c7 !n1_r7_c7
!n1_r5_c7 !n1_r8_c7
!n1_r5_c7 !n1_r9_c7
!n1_r5_c7 !n1_r4_c8
!n1_r5_c7 !n1_r4_c9
!n1_r5_c7 !n1_r6_c8
!n1_r5_c7 !n1_r6_c9
!n2_r5_c7 !n3_r5_c7
!n2_r5_c7 !n4_r5_c7
!n2_r5_c7 !n5_r5_c7
!n2_r5_c7 !n6_r5_c7
!n2_r5_c7 !n7_r5_c7
!n2_r5_c7 !n8_r5_c7
!n2_r5_c7 !n9_r5_c7
!n2_r5_c7 !n2_r5_c8
!n2_r5_c7 !n2_r5_c9
!n2_r5_c7 !n2_r6_c7
!n2_r5_c7 !n2_r7_c7
!n2_r5_c7 !n2_r8_c7
!n2_r5_c7 !n2_r9_c7
!n2_r5_c7 !n2_r4_c8
!n2_r5_c7 !n2_r4_c9
!n2_r5_c7 !n2_r6_c8
!n2_r5_c7 !n2_r6_c9
!n3_r5_c7 !n4_r5_c7
!n3_r5_c7 !n5_r5_c7
!n3_r5_c7 !n6_r5_c7
!n3_r5_c7 !n7_r5_c7
!n3_r5_c7 !n8_r5_c7
!n3_r5_c7 !n9_r5_c7
!n3_r5_c7 !n3_r5_c8
!n3_r5_c7 !n3_r5_c9
!n3_r5_c7 !n3_r6_c7
!n3_r5_c7 !n3_r7_c7
!n3_r5_c7 !n3_r8_c7
!n3_r5_c7 !n3_r9_c7
!n3_r5_c7 !n3_r4_c8
!n3_r5_c7 !n3_r4_c9
!n3_r5_c7 !n3_r6_c8
!n3_r5_c7 !n3_r6_c9
!n4_r5_c7 !n5_r5_c7
!n4_r5_c7 !n6_r5_c7
!n4_r5_c7 !n7_r5_c7
!n4_r5_c7 !n8_r5_c7
!n4_r5_c7 !n9_r5_c7
!n4_r5_c7 !n4_r5_c8
!n4_r5_c7 !n4_r5_c9
!n4_r5_c7 !n4_r6_c7
!n4_r5_c7 !n4_r7_c7
!n4_r5_c7 !n4_r8_c7
!n4_r5_c7 !n4_r9_c7
!n4_r5_c7 !n4_r4_c8
!n4_r5_c7 !n4_r4_c9
!n4_r5_c7 !n4_r6_c8
!n4_r5_c7 !n4_r6_c9
!n5_r5_c7 !n6_r5_c7
!n5_r5_c7 !n7_r5_c7
!n5_r5_c7 !n8_r5_c7
!n5_r5_c7 !n9_r5_c7
!n5_r5_c7 !n5_r5_c8
!n5_r5_c7 !n5_r5_c9
!n5_r5_c7 !n5_r6_c7
!n5_r5_c7 !n5_r7_c7
!n5_r5_c7 !n5_r8_c7
!n5_r5_c7 !n5_r9_c7
!n5_r5_c7 !n5_r4_c8
!n5_r5_c7 !n5_r4_c9
!n5_r5_c7 !n5_r6_c8
!n5_r5_c7 !n5_r6_c9
!n6_r5_c7 !n7_r5_c7
!n6_r5_c7 !n8_r5_c7
!n6_r5_c7 !n9_r5_c7
!n6_r5_c7 !n6_r5_c8
!n6_r5_c7 !n6_r5_c9
!n6_r5_c7 !n6_r6_c7
!n6_r5_c7 !n6_r7_c7
!n6_r5_c7 !n6_r8_c7
!n6_r5_c7 !n6_r9_c7
!n6_r5_c7 !n6_r4_c8
!n6_r5_c7 !n6_r4_c9
!n6_r5_c7 !n6_r6_c8
!n6_r5_c7 !n6_r6_c9
!n7_r5_c7 !n8_r5_c7
!n7_r5_c7 !n9_r5_c7
!n7_r5_c7 !n7_r5_c8
!n7_r5_c7 !n7_r5_c9
!n7_r5_c7 !n7_r6_c7
!n7_r5_c7 !n7_r7_c7
!n7_r5_c7 !n7_r8_c7
!n7_r5_c7 !n7_r9_c7
!n7_r5_c7 !n7_r4_c8
!n7_r5_c7 !n7_r4_c9
!n7_r5_c7 !n7_r6_c8
!n7_r5_c7 !n7_r6_c9
!n8_r5_c7 !n9_r5_c7
!n8_r5_c7 !n8_r5_c8
!n8_r5_c7 !n8_r5_c9
!n8_r5_c7 !n8_r6_c7
!n8_r5_c7 !n8_r7_c7
!n8_r5_c7 !n8_r8_c7
!n8_r5_c7 !n8_r9_c7
!n8_r5_c7 !n8_r4_c8
!n8_r5_c7 !n8_r4_c9
!n8_r5_c7 !n8_r6_c8
!n8_r5_c7 !n8_r6_c9
!n9_r5_c7 !n9_r5_c8
!n9_r5_c7 !n9_r5_c9
!n9_r5_c7 !n9_r6_c7
!n9_r5_c7 !n9_r7_c7
!n9_r5_c7 !n9_r8_c7
!n9_r5_c7 !n9_r9_c7
!n9_r5_c7 !n9_r4_c8
!n9_r5_c7 !n9_r4_c9
!n9_r5_c7 !n9_r6_c8
!n9_r5_c7 !n9_r6_c9
n1_r5_c7 n2_r5_c7 n3_r5_c7 n4_r5_c7 n5_r5_c7 n6_r5_c7 n7_r5_c7 n8_r5_c7 n9_r5_c7
!n1_r5_c8 !n2_r5_c8
!n1_r5_c8 !n3_r5_c8
!n1_r5_c8 !n4_r5_c8
!n1_r5_c8 !n5_r5_c8
!n1_r5_c8 !n6_r5_c8
!n1_r5_c8 !n7_r5_c8
!n1_r5_c8 !n8_r5_c8
!n1_r5_c8 !n9_r5_c8
!n1_r5_c8 !n1_r5_c9
!n1_r5_c8 !n1_r6_c8
!n1_r5_c8 !n1_r7_c8
!n1_r5_c8 !n1_r8_c8
!n1_r5_c8 !n1_r9_c8
!n1_r5_c8 !n1_r4_c7
!n1_r5_c8 !n1_r4_c9
!n1_r5_c8 !n1_r6_c7
!n1_r5_c8 !n1_r6_c9
!n2_r5_c8 !n3_r5_c8
!n2_r5_c8 !n4_r5_c8
!n2_r5_c8 !n5_r5_c8
!n2_r5_c8 !n6_r5_c8
!n2_r5_c8 !n7_r5_c8
!n2_r5_c8 !n8_r5_c8
!n2_r5_c8 !n9_r5_c8
!n2_r5_c8 !n2_r5_c9
!n2_r5_c8 !n2_r6_c8
!n2_r5_c8 !n2_r7_c8
!n2_r5_c8 !n2_r8_c8
!n2_r5_c8 !n2_r9_c8
!n2_r5_c8 !n2_r4_c7
!n2_r5_c8 !n2_r4_c9
!n2_r5_c8 !n2_r6_c7
!n2_r5_c8 !n2_r6_c9
!n3_r5_c8 !n4_r5_c8
!n3_r5_c8 !n5_r5_c8
!n3_r5_c8 !n6_r5_c8
!n3_r5_c8 !n7_r5_c8
!n3_r5_c8 !n8_r5_c8
!n3_r5_c8 !n9_r5_c8
!n3_r5_c8 !n3_r5_c9
!n3_r5_c8 !n3_r6_c8
!n3_r5_c8 !n3_r7_c8
!n3_r5_c8 !n3_r8_c8
!n3_r5_c8 !n3_r9_c8
!n3_r5_c8 !n3_r4_c7
!n3_r5_c8 !n3_r4_c9
!n3_r5_c8 !n3_r6_c7
!n3_r5_c8 !n3_r6_c9
!n4_r5_c8 !n5_r5_c8
!n4_r5_c8 !n6_r5_c8
!n4_r5_c8 !n7_r5_c8
!n4_r5_c8 !n8_r5_c8
!n4_r5_c8 !n9_r5_c8
!n4_r5_c8 !n4_r5_c9
!n4_r5_c8 !n4_r6_c8
!n4_r5_c8 !n4_r7_c8
!n4_r5_c8 !n4_r8_c8
!n4_r5_c8 !n4_r9_c8
!n4_r5_c8 !n4_r4_c7
!n4_r5_c8 !n4_r4_c9
!n4_r5_c8 !n4_r6_c7
!n4_r5_c8 !n4_r6_c9
!n5_r5_c8 !n6_r5_c8
!n5_r5_c8 !n7_r5_c8
!n5_r5_c8 !n8_r5_c8
!n5_r5_c8 !n9_r5_c8
!n5_r5_c8 !n5_r5_c9
!n5_r5_c8 !n5_r6_c8
!n5_r5_c8 !n5_r7_c8
!n5_r5_c8 !n5_r8_c8
!n5_r5_c8 !n5_r9_c8
!n5_r5_c8 !n5_r4_c7
!n5_r5_c8 !n5_r4_c9
!n5_r5_c8 !n5_r6_c7
!n5_r5_c8 !n5_r6_c9
!n6_r5_c8 !n7_r5_c8
!n6_r5_c8 !n8_r5_c8
!n6_r5_c8 !n9_r5_c8
!n6_r5_c8 !n6_r5_c9
!n6_r5_c8 !n6_r6_c8
!n6_r5_c8 !n6_r7_c8
!n6_r5_c8 !n6_r8_c8
!n6_r5_c8 !n6_r9_c8
!n6_r5_c8 !n6_r4_c7
!n6_r5_c8 !n6_r4_c9
!n6_r5_c8 !n6_r6_c7
!n6_r5_c8 !n6_r6_c9
!n7_r5_c8 !n8_r5_c8
!n7_r5_c8 !n9_r5_c8
!n7_r5_c8 !n7_r5_c9
!n7_r5_c8 !n7_r6_c8
!n7_r5_c8 !n7_r7_c8
!n7_r5_c8 !n7_r8_c8
!n7_r5_c8 !n7_r9_c8
!n7_r5_c8 !n7_r4_c7
!n7_r5_c8 !n7_r4_c9
!n7_r5_c8 !n7_r6_c7
!n7_r5_c8 !n7_r6_c9
!n8_r5_c8 !n9_r5_c8
!n8_r5_c8 !n8_r5_c9
!n8_r5_c8 !n8_r6_c8
!n8_r5_c8 !n8_r7_c8
!n8_r5_c8 !n8_r8_c8
!n8_r5_c8 !n8_r9_c8
!n8_r5_c8 !n8_r4_c7
!n8_r5_c8 !n8_r4_c9
!n8_r5_c8 !n8_r6_c7
!n8_r5_c8 !n8_r6_c9
!n9_r5_c8 !n9_r5_c9
!n9_r5_c8 !n9_r6_c8
!n9_r5_c8 !n9_r7_c8
!n9_r5_c8 !n9_r8_c8
!n9_r5_c8 !n9_r9_c8
!n9_r5_c8 !n9_r4_c7
!n9_r5_c8 !n9_r4_c9
!n9_r5_c8 !n9_r6_c7
!n9_r5_c8 !n9_r6_c9
n1_r5_c8 n2_r5_c8 n3_r5_c8 n4_r5_c8 n5_r5_c8 n6_r5_c8 n7_r5_c8 n8_r5_c8 n9_r5_c8
!n1_r5_c9 !n2_r5_c9
!n1_r5_c9 !n3_r5_c9
!n1_r5_c9 !n4_r5_c9
!n1_r5_c9 !n5_r5_c9
!n1_r5_c9 !n6_r5_c9
!n1_r5_c9 !n7_r5_c9
!n1_r5_c9 !n8_r5_c9
!n1_r5_c9 !n9_r5_c9
!n1_r5_c9 !n1_r6_c9
!n1_r5_c9 !n1_r7_c9
!n1_r5_c9 !n1_r8_c9
!n1_r5_c9 !n1_r9_c9
!n1_r5_c9 !n1_r4_c7
!n1_r5_c9 !n1_r4_c8
!n1_r5_c9 !n1_r6_c7
!n1_r5_c9 !n1_r6_c8
!n2_r5_c9 !n3_r5_c9
!n2_r5_c9 !n4_r5_c9
!n2_r5_c9 !n5_r5_c9
!n2_r5_c9 !n6_r5_c9
!n2_r5_c9 !n7_r5_c9
!n2_r5_c9 !n8_r5_c9
!n2_r5_c9 !n9_r5_c9
!n2_r5_c9 !n2_r6_c9
!n2_r5_c9 !n2_r7_c9
!n2_r5_c9 !n2_r8_c9
!n2_r5_c9 !n2_r9_c9
!n2_r5_c9 !n2_r4_c7
!n2_r5_c9 !n2_r4_c8
!n2_r5_c9 !n2_r6_c7
!n2_r5_c9 !n2_r6_c8
!n3_r5_c9 !n4_r5_c9
!n3_r5_c9 !n5_r5_c9
!n3_r5_c9 !n6_r5_c9
!n3_r5_c9 !n7_r5_c9
!n3_r5_c9 !n8_r5_c9
!n3_r5_c9 !n9_r5_c9
!n3_r5_c9 !n3_r6_c9
!n3_r5_c9 !n3_r7_c9
!n3_r5_c9 !n3_r8_c9
!n3_r5_c9 !n3_r9_c9
!n3_r5_c9 !n3_r4_c7
!n3_r5_c9 !n3_r4_c8
!n3_r5_c9 !n3_r6_c7
!n3_r5_c9 !n3_r6_c8
!n4_r5_c9 !n5_r5_c9
!n4_r5_c9 !n6_r5_c9
!n4_r5_c9 !n7_r5_c9
!n4_r5_c9 !n8_r5_c9
!n4_r5_c9 !n9_r5_c9
!n4_r5_c9 !n4_r6_c9
!n4_r5_c9 !n4_r7_c9
!n4_r5_c9 !n4_r8_c9
!n4_r5_c9 !n4_r9_c9
!n4_r5_c9 !n4_r4_c7
!n4_r5_c9 !n4_r4_c8
!n4_r5_c9 !n4_r6_c7
!n4_r5_c9 !n4_r6_c8
!n5_r5_c9 !n6_r5_c9
!n5_r5_c9 !n7_r5_c9
!n5_r5_c9 !n8_r5_c9
!n5_r5_c9 !n9_r5_c9
!n5_r5_c9 !n5_r6_c9
!n5_r5_c9 !n5_r7_c9
!n5_r5_c9 !n5_r8_c9
!n5_r5_c9 !n5_r9_c9
!n5_r5_c9 !n5_r4_c7
!n5_r5_c9 !n5_r4_c8
!n5_r5_c9 !n5_r6_c7
!n5_r5_c9 !n5_r6_c8
!n6_r5_c9 !n7_r5_c9
!n6_r5_c9 !n8_r5_c9
!n6_r5_c9 !n9_r5_c9
!n6_r5_c9 !n6_r6_c9
!n6_r5_c9 !n6_r7_c9
!n6_r5_c9 !n6_r8_c9
!n6_r5_c9 !n6_r9_c9
!n6_r5_c9 !n6_r4_c7
!n6_r5_c9 !n6_r4_c8
!n6_r5_c9 !n6_r6_c7
!n6_r5_c9 !n6_r6_c8
!n7_r5_c9 !n8_r5_c9
!n7_r5_c9 !n9_r5_c9
!n7_r5_c9 !n7_r6_c9
!n7_r5_c9 !n7_r7_c9
!n7_r5_c9 !n7_r8_c9
!n7_r5_c9 !n7_r9_c9
!n7_r5_c9 !n7_r4_c7
!n7_r5_c9 !n7_r4_c8
!n7_r5_c9 !n7_r6_c7
!n7_r5_c9 !n7_r6_c8
!n8_r5_c9 !n9_r5_c9
!n8_r5_c9 !n8_r6_c9
!n8_r5_c9 !n8_r7_c9
!n8_r5_c9 !n8_r8_c9
!n8_r5_c9 !n8_r9_c9
!n8_r5_c9 !n8_r4_c7
!n8_r5_c9 !n8_r4_c8
!n8_r5_c9 !n8_r6_c7
!n8_r5_c9 !n8_r6_c8
!n9_r5_c9 !n9_r6_c9
!n9_r5_c9 !n9_r7_c9
!n9_r5_c9 !n9_r8_c9
!n9_r5_c9 !n9_r9_c9
!n9_r5_c9 !n9_r4_c7
!n9_r5_c9 !n9_r4_c8
!n9_r5_c9 !n9_r6_c7
!n9_r5_c9 !n9_r6_c8
n1_r5_c9 n2_r5_c9 n3_r5_c9 n4_r5_c9 n5_r5_c9 n6_r5_c9 n7_r5_c9 n8_r5_c9 n9_r5_c9
!n1_r6_c1 !n2_r6_c1
!n1_r6_c1 !n3_r6_c1
!n1_r6_c1 !n4_r6_c1
!n1_r6_c1 !n5_r6_c1
!n1_r6_c1 !n6_r6_c1
!n1_r6_c1 !n7_r6_c1
!n1_r6_c1 !n8_r6_c1
!n1_r6_c1 !n9_r6_c1
!n1_r6_c1 !n1_r6_c2
!n1_r6_c1 !n1_r6_c3
!n1_r6_c1 !n1_r6_c4
!n1_r6_c1 !n1_r6_c5
!n1_r6_c1 !n1_r6_c6
!n1_r6_c1 !n1_r6_c7
!n1_r6_c1 !n1_r6_c8
!n1_r6_c1 !n1_r6_c9
!n1_r6_c1 !n1_r7_c1
!n1_r6_c1 !n1_r8_c1
!n1_r6_c1 !n1_r9_c1
!n1_r6_c1 !n1_r4_c2
!n1_r6_c1 !n1_r4_c3
!n1_r6_c1 !n1_r5_c2
!n1_r6_c1 !n1_r5_c3
!n2_r6_c1 !n3_r6_c1
!n2_r6_c1 !n4_r6_c1
!n2_r6_c1 !n5_r6_c1
!n2_r6_c1 !n6_r6_c1
!n2_r6_c1 !n7_r6_c1
!n2_r6_c1 !n8_r6_c1
!n2_r6_c1 !n9_r6_c1
!n2_r6_c1 !n2_r6_c2
!n2_r6_c1 !n2_r6_c3
!n2_r6_c1 !n2_r6_c4
!n2_r6_c1 !n2_r6_c5
!n2_r6_c1 !n2_r6_c6
!n2_r6_c1 !n2_r6_c7
!n2_r6_c1 !n2_r6_c8
!n2_r6_c1 !n2_r6_c9
!n2_r6_c1 !n2_r7_c1
!n2_r6_c1 !n2_r8_c1
!n2_r6_c1 !n2_r9_c1
!n2_r6_c1 !n2_r4_c2
!n2_r6_c1 !n2_r4_c3
!n2_r6_c1 !n2_r5_c2
!n2_r6_c1 !n2_r5_c3
!n3_r6_c1 !n4_r6_c1
!n3_r6_c1 !n5_r6_c1
!n3_r6_c1 !n6_r6_c1
!n3_r6_c1 !n7_r6_c1
!n3_r6_c1 !n8_r6_c1
!n3_r6_c1 !n9_r6_c1
!n3_r6_c1 !n3_r6_c2
!n3_r6_c1 !n3_r6_c3
!n3_r6_c1 !n3_r6_c4
!n3_r6_c1 !n3_r6_c5
!n3_r6_c1 !n3_r6_c6
!n3_r6_c1 !n3_r6_c7
!n3_r6_c1 !n3_r6_c8
!n3_r6_c1 !n3_r6_c9
!n3_r6_c1 !n3_r7_c1
!n3_r6_c1 !n3_r8_c1
!n3_r6_c1 !n3_r9_c1
!n3_r6_c1 !n3_r4_c2
!n3_r6_c1 !n3_r4_c3
!n3_r6_c1 !n3_r5_c2
!n3_r6_c1 !n3_r5_c3
!n4_r6_c1 !n5_r6_c1
!n4_r6_c1 !n6_r6_c1
!n4_r6_c1 !n7_r6_c1
!n4_r6_c1 !n8_r6_c1
!n4_r6_c1 !n9_r6_c1
!n4_r6_c1 !n4_r6_c2
!n4_r6_c1 !n4_r6_c3
!n4_r6_c1 !n4_r6_c4
!n4_r6_c1 !n4_r6_c5
!n4_r6_c1 !n4_r6_c6
!n4_r6_c1 !n4_r6_c7
!n4_r6_c1 !n4_r6_c8
!n4_r6_c1 !n4_r6_c9
!n4_r6_c1 !n4_r7_c1
!n4_r6_c1 !n4_r8_c1
!n4_r6_c1 !n4_r9_c1
!n4_r6_c1 !n4_r4_c2
!n4_r6_c1 !n4_r4_c3
!n4_r6_c1 !n4_r5_c2
!n4_r6_c1 !n4_r5_c3
!n5_r6_c1 !n6_r6_c1
!n5_r6_c1 !n7_r6_c1
!n5_r6_c1 !n8_r6_c1
!n5_r6_c1 !n9_r6_c1
!n5_r6_c1 !n5_r6_c2
!n5_r6_c1 !n5_r6_c3
!n5_r6_c1 !n5_r6_c4
!n5_r6_c1 !n5_r6_c5
!n5_r6_c1 !n5_r6_c6
!n5_r6_c1 !n5_r6_c7
!n5_r6_c1 !n5_r6_c8
!n5_r6_c1 !n5_r6_c9
!n5_r6_c1 !n5_r7_c1
!n5_r6_c1 !n5_r8_c1
!n5_r6_c1 !n5_r9_c1
!n5_r6_c1 !n5_r4_c2
!n5_r6_c1 !n5_r4_c3
!n5_r6_c1 !n5_r5_c2
!n5_r6_c1 !n5_r5_c3
!n6_r6_c1 !n7_r6_c1
!n6_r6_c1 !n8_r6_c1
!n6_r6_c1 !n9_r6_c1
!n6_r6_c1 !n6_r6_c2
!n6_r6_c1 !n6_r6_c3
!n6_r6_c1 !n6_r6_c4
!n6_r6_c1 !n6_r6_c5
!n6_r6_c1 !n6_r6_c6
!n6_r6_c1 !n6_r6_c7
!n6_r6_c1 !n6_r6_c8
!n6_r6_c1 !n6_r6_c9
!n6_r6_c1 !n6_r7_c1
!n6_r6_c1 !n6_r8_c1
!n6_r6_c1 !n6_r9_c1
!n6_r6_c1 !n6_r4_c2
!n6_r6_c1 !n6_r4_c3
!n6_r6_c1 !n6_r5_c2
!n6_r6_c1 !n6_r5_c3
!n7_r6_c1 !n8_r6_c1
!n7_r6_c1 !n9_r6_c1
!n7_r6_c1 !n7_r6_c2
!n7_r6_c1 !n7_r6_c3
!n7_r6_c1 !n7_r6_c4
!n7_r6_c1 !n7_r6_c5
!n7_r6_c1 !n7_r6_c6
!n7_r6_c1 !n7_r6_c7
!n7_r6_c1 !n7_r6_c8
!n7_r6_c1 !n7_r6_c9
!n7_r6_c1 !n7_r7_c1
!n7_r6_c1 !n7_r8_c1
!n7_r6_c1 !n7_r9_c1
!n7_r6_c1 !n7_r4_c2
!n7_r6_c1 !n7_r4_c3
!n7_r6_c1 !n7_r5_c2
!n7_r6_c1 !n7_r5_c3
!n8_r6_c1 !n9_r6_c1
!n8_r6_c1 !n8_r6_c2
!n8_r6_c1 !n8_r6_c3
!n8_r6_c1 !n8_r6_c4
!n8_r6_c1 !n8_r6_c5
!n8_r6_c1 !n8_r6_c6
!n8_r6_c1 !n8_r6_c7
!n8_r6_c1 !n8_r6_c8
!n8_r6_c1 !n8_r6_c9
!n8_r6_c1 !n8_r7_c1
!n8_r6_c1 !n8_r8_c1
!n8_r6_c1 !n8_r9_c1
!n8_r6_c1 !n8_r4_c2
!n8_r6_c1 !n8_r4_c3
!n8_r6_c1 !n8_r5_c2
!n8_r6_c1 !n8_r5_c3
!n9_r6_c1 !n9_r6_c2
!n9_r6_c1 !n9_r6_c3
!n9_r6_c1 !n9_r6_c4
!n9_r6_c1 !n9_r6_c5
!n9_r6_c1 !n9_r6_c6
!n9_r6_c1 !n9_r6_c7
!n9_r6_c1 !n9_r6_c8
!n9_r6_c1 !n9_r6_c9
!n9_r6_c1 !n9_r7_c1
!n9_r6_c1 !n9_r8_c1
!n9_r6_c1 !n9_r9_c1
!n9_r6_c1 !n9_r4_c2
!n9_r6_c1 !n9_r4_c3
!n9_r6_c1 !n9_r5_c2
!n9_r6_c1 !n9_r5_c3
n1_r6_c1 n2_r6_c1 n3_r6_c1 n4_r6_c1 n5_r6_c1 n6_r6_c1 n7_r6_c1 n8_r6_c1 n9_r6_c1
!n1_r6_c2 !n2_r6_c2
!n1_r6_c2 !n3_r6_c2
!n1_r6_c2 !n4_r6_c2
!n1_r6_c2 !n5_r6_c2
!n1_r6_c2 !n6_r6_c2
!n1_r6_c2 !n7_r6_c2
!n1_r6_c2 !n8_r6_c2
!n1_r6_c2 !n9_r6_c2
!n1_r6_c2 !n1_r6_c3
!n1_r6_c2 !n1_r6_c4
!n1_r6_c2 !n1_r6_c5
!n1_r6_c2 !n1_r6_c6
!n1_r6_c2 !n1_r6_c7
!n1_r6_c2 !n1_r6_c8
!n1_r6_c2 !n1_r6_c9
!n1_r6_c2 !n1_r7_c2
!n1_r6_c2 !n1_r8_c2
!n1_r6_c2 !n1_r9_c2
!n1_r6_c2 !n1_r4_c1
!n1_r6_c2 !n1_r4_c3
!n1_r6_c2 !n1_r5_c1
!n1_r6_c2 !n1_r5_c3
!n2_r6_c2 !n3_r6_c2
!n2_r6_c2 !n4_r6_c2
!n2_r6_c2 !n5_r6_c2
!n2_r6_c2 !n6_r6_c2
!n2_r6_c2 !n7_r6_c2
!n2_r6_c2 !n8_r6_c2
!n2_r6_c2 !n9_r6_c2
!n2_r6_c2 !n2_r6_c3
!n2_r6_c2 !n2_r6_c4
!n2_r6_c2 !n2_r6_c5
!n2_r6_c2 !n2_r6_c6
!n2_r6_c2 !n2_r6_c7
!n2_r6_c2 !n2_r6_c8
!n2_r6_c2 !n2_r6_c9
!n2_r6_c2 !n2_r7_c2
!n2_r6_c2 !n2_r8_c2
!n2_r6_c2 !n2_r9_c2
!n2_r6_c2 !n2_r4_c1
!n2_r6_c2 !n2_r4_c3
!n2_r6_c2 !n2_r5_c1
!n2_r6_c2 !n2_r5_c3
!n3_r6_c2 !n4_r6_c2
!n3_r6_c2 !n5_r6_c2
!n3_r6_c2 !n6_r6_c2
!n3_r6_c2 !n7_r6_c2
!n3_r6_c2 !n8_r6_c2
!n3_r6_c2 !n9_r6_c2
!n3_r6_c2 !n3_r6_c3
!n3_r6_c2 !n3_r6_c4
!n3_r6_c2 !n3_r6_c5
!n3_r6_c2 !n3_r6_c6
!n3_r6_c2 !n3_r6_c7
!n3_r6_c2 !n3_r6_c8
!n3_r6_c2 !n3_r6_c9
!n3_r6_c2 !n3_r7_c2
!n3_r6_c2 !n3_r8_c2
!n3_r6_c2 !n3_r9_c2
!n3_r6_c2 !n3_r4_c1
!n3_r6_c2 !n3_r4_c3
!n3_r6_c2 !n3_r5_c1
!n3_r6_c2 !n3_r5_c3
!n4_r6_c2 !n5_r6_c2
!n4_r6_c2 !n6_r6_c2
!n4_r6_c2 !n7_r6_c2
!n4_r6_c2 !n8_r6_c2
!n4_r6_c2 !n9_r6_c2
!n4_r6_c2 !n4_r6_c3
!n4_r6_c2 !n4_r6_c4
!n4_r6_c2 !n4_r6_c5
!n4_r6_c2 !n4_r6_c6
!n4_r6_c2 !n4_r6_c7
!n4_r6_c2 !n4_r6_c8
!n4_r6_c2 !n4_r6_c9
!n4_r6_c2 !n4_r7_c2
!n4_r6_c2 !n4_r8_c2
!n4_r6_c2 !n4_r9_c2
!n4_r6_c2 !n4_r4_c1
!n4_r6_c2 !n4_r4_c3
!n4_r6_c2 !n4_r5_c1
!n4_r6_c2 !n4_r5_c3
!n5_r6_c2 !n6_r6_c2
!n5_r6_c2 !n7_r6_c2
!n5_r6_c2 !n8_r6_c2
!n5_r6_c2 !n9_r6_c2
!n5_r6_c2 !n5_r6_c3
!n5_r6_c2 !n5_r6_c4
!n5_r6_c2 !n5_r6_c5
!n5_r6_c2 !n5_r6_c6
!n5_r6_c2 !n5_r6_c7
!n5_r6_c2 !n5_r6_c8
!n5_r6_c2 !n5_r6_c9
!n5_r6_c2 !n5_r7_c2
!n5_r6_c2 !n5_r8_c2
!n5_r6_c2 !n5_r9_c2
!n5_r6_c2 !n5_r4_c1
!n5_r6_c2 !n5_r4_c3
!n5_r6_c2 !n5_r5_c1
!n5_r6_c2 !n5_r5_c3
!n6_r6_c2 !n7_r6_c2
!n6_r6_c2 !n8_r6_c2
!n6_r6_c2 !n9_r6_c2
!n6_r6_c2 !n6_r6_c3
!n6_r6_c2 !n6_r6_c4
!n6_r6_c2 !n6_r6_c5
!n6_r6_c2 !n6_r6_c6
!n6_r6_c2 !n6_r6_c7
!n6_r6_c2 !n6_r6_c8
!n6_r6_c2 !n6_r6_c9
!n6_r6_c2 !n6_r7_c2
!n6_r6_c2 !n6_r8_c2
!n6_r6_c2 !n6_r9_c2
!n6_r6_c2 !n6_r4_c1
!n6_r6_c2 !n6_r4_c3
!n6_r6_c2 !n6_r5_c1
!n6_r6_c2 !n6_r5_c3
!n7_r6_c2 !n8_r6_c2
!n7_r6_c2 !n9_r6_c2
!n7_r6_c2 !n7_r6_c3
!n7_r6_c2 !n7_r6_c4
!n7_r6_c2 !n7_r6_c5
!n7_r6_c2 !n7_r6_c6
!n7_r6_c2 !n7_r6_c7
!n7_r6_c2 !n7_r6_c8
!n7_r6_c2 !n7_r6_c9
!n7_r6_c2 !n7_r7_c2
!n7_r6_c2 !n7_r8_c2
!n7_r6_c2 !n7_r9_c2
!n7_r6_c2 !n7_r4_c1
!n7_r6_c2 !n7_r4_c3
!n7_r6_c2 !n7_r5_c1
!n7_r6_c2 !n7_r5_c3
!n8_r6_c2 !n9_r6_c2
!n8_r6_c2 !n8_r6_c3
!n8_r6_c2 !n8_r6_c4
!n8_r6_c2 !n8_r6_c5
!n8_r6_c2 !n8_r6_c6
!n8_r6_c2 !n8_r6_c7
!n8_r6_c2 !n8_r6_c8
!n8_r6_c2 !n8_r6_c9
!n8_r6_c2 !n8_r7_c2
!n8_r6_c2 !n8_r8_c2
!n8_r6_c2 !n8_r9_c2
!n8_r6_c2 !n8_r4_c1
!n8_r6_c2 !n8_r4_c3
!n8_r6_c2 !n8_r5_c1
!n8_r6_c2 !n8_r5_c3
!n9_r6_c2 !n9_r6_c3
!n9_r6_c2 !n9_r6_c4
!n9_r6_c2 !n9_r6_c5
!n9_r6_c2 !n9_r6_c6
!n9_r6_c2 !n9_r6_c7
!n9_r6_c2 !n9_r6_c8
!n9_r6_c2 !n9_r6_c9
!n9_r6_c2 !n9_r7_c2
!n9_r6_c2 !n9_r8_c2
!n9_r6_c2 !n9_r9_c2
!n9_r6_c2 !n9_r4_c1
!n9_r6_c2 !n9_r4_c3
!n9_r6_c2 !n9_r5_c1
!n9_r6_c2 !n9_r5_c3
n1_r6_c2 n2_r6_c2 n3_r6_c2 n4_r6_c2 n5_r6_c2 n6_r6_c2 n7_r6_c2 n8_r6_c2 n9_r6_c2
!n1_r6_c3 !n2_r6_c3
!n1_r6_c3 !n3_r6_c3
!n1_r6_c3 !n4_r6_c3
!n1_r6_c3 !n5_r6_c3
!n1_r6_c3 !n6_r6_c3
!n1_r6_c3 !n7_r6_c3
!n1_r6_c3 !n8_r6_c3
!n1_r6_c3 !n9_r6_c3
!n1_r6_c3 !n1_r6_c4
!n1_r6_c3 !n1_r6_c5
!n1_r6_c3 !n1_r6_c6
!n1_r6_c3 !n1_r6_c7
!n1_r6_c3 !n1_r6_c8
!n1_r6_c3 !n1_r6_c9
!n1_r6_c3 !n1_r7_c3
!n1_r6_c3 !n1_r8_c3
!n1_r6_c3 !n1_r9_c3
!n1_r6_c3 !n1_r4_c1
!n1_r6_c3 !n1_r4_c2
!n1_r6_c3 !n1_r5_c1
!n1_r6_c3 !n1_r5_c2
!n2_r6_c3 !n3_r6_c3
!n2_r6_c3 !n4_r6_c3
!n2_r6_c3 !n5_r6_c3
!n2_r6_c3 !n6_r6_c3
!n2_r6_c3 !n7_r6_c3
!n2_r6_c3 !n8_r6_c3
!n2_r6_c3 !n9_r6_c3
!n2_r6_c3 !n2_r6_c4
!n2_r6_c3 !n2_r6_c5
!n2_r6_c3 !n2_r6_c6
!n2_r6_c3 !n2_r6_c7
!n2_r6_c3 !n2_r6_c8
!n2_r6_c3 !n2_r6_c9
!n2_r6_c3 !n2_r7_c3
!n2_r6_c3 !n2_r8_c3
!n2_r6_c3 !n2_r9_c3
!n2_r6_c3 !n2_r4_c1
!n2_r6_c3 !n2_r4_c2
!n2_r6_c3 !n2_r5_c1
!n2_r6_c3 !n2_r5_c2
!n3_r6_c3 !n4_r6_c3
!n3_r6_c3 !n5_r6_c3
!n3_r6_c3 !n6_r6_c3
!n3_r6_c3 !n7_r6_c3
!n3_r6_c3 !n8_r6_c3
!n3_r6_c3 !n9_r6_c3
!n3_r6_c3 !n3_r6_c4
!n3_r6_c3 !n3_r6_c5
!n3_r6_c3 !n3_r6_c6
!n3_r6_c3 !n3_r6_c7
!n3_r6_c3 !n3_r6_c8
!n3_r6_c3 !n3_r6_c9
!n3_r6_c3 !n3_r7_c3
!n3_r6_c3 !n3_r8_c3
!n3_r6_c3 !n3_r9_c3
!n3_r6_c3 !n3_r4_c1
!n3_r6_c3 !n3_r4_c2
!n3_r6_c3 !n3_r5_c1
!n3_r6_c3 !n3_r5_c2
!n4_r6_c3 !n5_r6_c3
!n4_r6_c3 !n6_r6_c3
!n4_r6_c3 !n7_r6_c3
!n4_r6_c3 !n8_r6_c3
!n4_r6_c3 !n9_r6_c3
!n4_r6_c3 !n4_r6_c4
!n4_r6_c3 !n4_r6_c5
!n4_r6_c3 !n4_r6_c6
!n4_r6_c3 !n4_r6_c7
!n4_r6_c3 !n4_r6_c8
!n4_r6_c3 !n4_r6_c9
!n4_r6_c3 !n4_r7_c3
!n4_r6_c3 !n4_r8_c3
!n4_r6_c3 !n4_r9_c3
!n4_r6_c3 !n4_r4_c1
!n4_r6_c3 !n4_r4_c2
!n4_r6_c3 !n4_r5_c1
!n4_r6_c3 !n4_r5_c2
!n5_r6_c3 !n6_r6_c3
!n5_r6_c3 !n7_r6_c3
!n5_r6_c3 !n8_r6_c3
!n5_r6_c3 !n9_r6_c3
!n5_r6_c3 !n5_r6_c4
!n5_r6_c3 !n5_r6_c5
!n5_r6_c3 !n5_r6_c6
!n5_r6_c3 !n5_r6_c7
!n5_r6_c3 !n5_r6_c8
!n5_r6_c3 !n5_r6_c9
!n5_r6_c3 !n5_r7_c3
!n5_r6_c3 !n5_r8_c3
!n5_r6_c3 !n5_r9_c3
!n5_r6_c3 !n5_r4_c1
!n5_r6_c3 !n5_r4_c2
!n5_r6_c3 !n5_r5_c1
!n5_r6_c3 !n5_r5_c2
!n6_r6_c3 !n7_r6_c3
!n6_r6_c3 !n8_r6_c3
!n6_r6_c3 !n9_r6_c3
!n6_r6_c3 !n6_r6_c4
!n6_r6_c3 !n6_r6_c5
!n6_r6_c3 !n6_r6_c6
!n6_r6_c3 !n6_r6_c7
!n6_r6_c3 !n6_r6_c8
!n6_r6_c3 !n6_r6_c9
!n6_r6_c3 !n6_r7_c3
!n6_r6_c3 !n6_r8_c3
!n6_r6_c3 !n6_r9_c3
!n6_r6_c3 !n6_r4_c1
!n6_r6_c3 !n6_r4_c2
!n6_r6_c3 !n6_r5_c1
!n6_r6_c3 !n6_r5_c2
!n7_r6_c3 !n8_r6_c3
!n7_r6_c3 !n9_r6_c3
!n7_r6_c3 !n7_r6_c4
!n7_r6_c3 !n7_r6_c5
!n7_r6_c3 !n7_r6_c6
!n7_r6_c3 !n7_r6_c7
!n7_r6_c3 !n7_r6_c8
!n7_r6_c3 !n7_r6_c9
!n7_r6_c3 !n7_r7_c3
!n7_r6_c3 !n7_r8_c3
!n7_r6_c3 !n7_r9_c3
!n7_r6_c3 !n7_r4_c1
!n7_r6_c3 !n7_r4_c2
!n7_r6_c3 !n7_r5_c1
!n7_r6_c3 !n7_r5_c2
!n8_r6_c3 !n9_r6_c3
!n8_r6_c3 !n8_r6_c4
!n8_r6_c3 !n8_r6_c5
!n8_r6_c3 !n8_r6_c6
!n8_r6_c3 !n8_r6_c7
!n8_r6_c3 !n8_r6_c8
!n8_r6_c3 !n8_r6_c9
!n8_r6_c3 !n8_r7_c3
!n8_r6_c3 !n8_r8_c3
!n8_r6_c3 !n8_r9_c3
!n8_r6_c3 !n8_r4_c1
!n8_r6_c3 !n8_r4_c2
!n8_r6_c3 !n8_r5_c1
!n8_r6_c3 !n8_r5_c2
!n9_r6_c3 !n9_r6_c4
!n9_r6_c3 !n9_r6_c5
!n9_r6_c3 !n9_r6_c6
!n9_r6_c3 !n9_r6_c7
!n9_r6_c3 !n9_r6_c8
!n9_r6_c3 !n9_r6_c9
!n9_r6_c3 !n9_r7_c3
!n9_r6_c3 !n9_r8_c3
!n9_r6_c3 !n9_r9_c3
!n9_r6_c3 !n9_r4_c1
!n9_r6_c3 !n9_r4_c2
!n9_r6_c3 !n9_r5_c1
!n9_r6_c3 !n9_r5_c2
n1_r6_c3 n2_r6_c3 n3_r6_c3 n4_r6_c3 n5_r6_c3 n6_r6_c3 n7_r6_c3 n8_r6_c3 n9_r6_c3
!n1_r6_c4 !n2_r6_c4
!n1_r6_c4 !n3_r6_c4
!n1_r6_c4 !n4_r6_c4
!n1_r6_c4 !n5_r6_c4
!n1_r6_c4 !n6_r6_c4
!n1_r6_c4 !n7_r6_c4
!n1_r6_c4 !n8_r6_c4
!n1_r6_c4 !n9_r6_c4
!n1_r6_c4 !n1_r6_c5
!n1_r6_c4 !n1_r6_c6
!n1_r6_c4 !n1_r6_c7
!n1_r6_c4 !n1_r6_c8
!n1_r6_c4 !n1_r6_c9
!n1_r6_c4 !n1_r7_c4
!n1_r6_c4 !n1_r8_c4
!n1_r6_c4 !n1_r9_c4
!n1_r6_c4 !n1_r4_c5
!n1_r6_c4 !n1_r4_c6
!n1_r6_c4 !n1_r5_c5
!n1_r6_c4 !n1_r5_c6
!n2_r6_c4 !n3_r6_c4
!n2_r6_c4 !n4_r6_c4
!n2_r6_c4 !n5_r6_c4
!n2_r6_c4 !n6_r6_c4
!n2_r6_c4 !n7_r6_c4
!n2_r6_c4 !n8_r6_c4
!n2_r6_c4 !n9_r6_c4
!n2_r6_c4 !n2_r6_c5
!n2_r6_c4 !n2_r6_c6
!n2_r6_c4 !n2_r6_c7
!n2_r6_c4 !n2_r6_c8
!n2_r6_c4 !n2_r6_c9
!n2_r6_c4 !n2_r7_c4
!n2_r6_c4 !n2_r8_c4
!n2_r6_c4 !n2_r9_c4
!n2_r6_c4 !n2_r4_c5
!n2_r6_c4 !n2_r4_c6
!n2_r6_c4 !n2_r5_c5
!n2_r6_c4 !n2_r5_c6
!n3_r6_c4 !n4_r6_c4
!n3_r6_c4 !n5_r6_c4
!n3_r6_c4 !n6_r6_c4
!n3_r6_c4 !n7_r6_c4
!n3_r6_c4 !n8_r6_c4
!n3_r6_c4 !n9_r6_c4
!n3_r6_c4 !n3_r6_c5
!n3_r6_c4 !n3_r6_c6
!n3_r6_c4 !n3_r6_c7
!n3_r6_c4 !n3_r6_c8
!n3_r6_c4 !n3_r6_c9
!n3_r6_c4 !n3_r7_c4
!n3_r6_c4 !n3_r8_c4
!n3_r6_c4 !n3_r9_c4
!n3_r6_c4 !n3_r4_c5
!n3_r6_c4 !n3_r4_c6
!n3_r6_c4 !n3_r5_c5
!n3_r6_c4 !n3_r5_c6
!n4_r6_c4 !n5_r6_c4
!n4_r6_c4 !n6_r6_c4
!n4_r6_c4 !n7_r6_c4
!n4_r6_c4 !n8_r6_c4
!n4_r6_c4 !n9_r6_c4
!n4_r6_c4 !n4_r6_c5
!n4_r6_c4 !n4_r6_c6
!n4_r6_c4 !n4_r6_c7
!n4_r6_c4 !n4_r6_c8
!n4_r6_c4 !n4_r6_c9
!n4_r6_c4 !n4_r7_c4
!n4_r6_c4 !n4_r8_c4
!n4_r6_c4 !n4_r9_c4
!n4_r6_c4 !n4_r4_c5
!n4_r6_c4 !n4_r4_c6
!n4_r6_c4 !n4_r5_c5
!n4_r6_c4 !n4_r5_c6
!n5_r6_c4 !n6_r6_c4
!n5_r6_c4 !n7_r6_c4
!n5_r6_c4 !n8_r6_c4
!n5_r6_c4 !n9_r6_c4
!n5_r6_c4 !n5_r6_c5
!n5_r6_c4 !n5_r6_c6
!n5_r6_c4 !n5_r6_c7
!n5_r6_c4 !n5_r6_c8
!n5_r6_c4 !n5_r6_c9
!n5_r6_c4 !n5_r7_c4
!n5_r6_c4 !n5_r8_c4
!n5_r6_c4 !n5_r9_c4
!n5_r6_c4 !n5_r4_c5
!n5_r6_c4 !n5_r4_c6
!n5_r6_c4 !n5_r5_c5
!n5_r6_c4 !n5_r5_c6
!n6_r6_c4 !n7_r6_c4
!n6_r6_c4 !n8_r6_c4
!n6_r6_c4 !n9_r6_c4
!n6_r6_c4 !n6_r6_c5
!n6_r6_c4 !n6_r6_c6
!n6_r6_c4 !n6_r6_c7
!n6_r6_c4 !n6_r6_c8
!n6_r6_c4 !n6_r6_c9
!n6_r6_c4 !n6_r7_c4
!n6_r6_c4 !n6_r8_c4
!n6_r6_c4 !n6_r9_c4
!n6_r6_c4 !n6_r4_c5
!n6_r6_c4 !n6_r4_c6
!n6_r6_c4 !n6_r5_c5
!n6_r6_c4 !n6_r5_c6
!n7_r6_c4 !n8_r6_c4
!n7_r6_c4 !n9_r6_c4
!n7_r6_c4 !n7_r6_c5
!n7_r6_c4 !n7_r6_c6
!n7_r6_c4 !n7_r6_c7
!n7_r6_c4 !n7_r6_c8
!n7_r6_c4 !n7_r6_c9
!n7_r6_c4 !n7_r7_c4
!n7_r6_c4 !n7_r8_c4
!n7_r6_c4 !n7_r9_c4
!n7_r6_c4 !n7_r4_c5
!n7_r6_c4 !n7_r4_c6
!n7_r6_c4 !n7_r5_c5
!n7_r6_c4 !n7_r5_c6
!n8_r6_c4 !n9_r6_c4
!n8_r6_c4 !n8_r6_c5
!n8_r6_c4 !n8_r6_c6
!n8_r6_c4 !n8_r6_c7
!n8_r6_c4 !n8_r6_c8
!n8_r6_c4 !n8_r6_c9
!n8_r6_c4 !n8_r7_c4
!n8_r6_c4 !n8_r8_c4
!n8_r6_c4 !n8_r9_c4
!n8_r6_c4 !n8_r4_c5
!n8_r6_c4 !n8_r4_c6
!n8_r6_c4 !n8_r5_c5
!n8_r6_c4 !n8_r5_c6
!n9_r6_c4 !n9_r6_c5
!n9_r6_c4 !n9_r6_c6
!n9_r6_c4 !n9_r6_c7
!n9_r6_c4 !n9_r6_c8
!n9_r6_c4 !n9_r6_c9
!n9_r6_c4 !n9_r7_c4
!n9_r6_c4 !n9_r8_c4
!n9_r6_c4 !n9_r9_c4
!n9_r6_c4 !n9_r4_c5
!n9_r6_c4 !n9_r4_c6
!n9_r6_c4 !n9_r5_c5
!n9_r6_c4 !n9_r5_c6
n1_r6_c4 n2_r6_c4 n3_r6_c4 n4_r6_c4 n5_r6_c4 n6_r6_c4 n7_r6_c4 n8_r6_c4 n9_r6_c4
!n1_r6_c5 !n2_r6_c5
!n1_r6_c5 !n3_r6_c5
!n1_r6_c5 !n4_r6_c5
!n1_r6_c5 !n5_r6_c5
!n1_r6_c5 !n6_r6_c5
!n1_r6_c5 !n7_r6_c5
!n1_r6_c5 !n8_r6_c5
!n1_r6_c5 !n9_r6_c5
!n1_r6_c5 !n1_r6_c6
!n1_r6_c5 !n1_r6_c7
!n1_r6_c5 !n1_r6_c8
!n1_r6_c5 !n1_r6_c9
!n1_r6_c5 !n1_r7_c5
!n1_r6_c5 !n1_r8_c5
!n1_r6_c5 !n1_r9_c5
!n1_r6_c5 !n1_r4_c4
!n1_r6_c5 !n1_r4_c6
!n1_r6_c5 !n1_r5_c4
!n1_r6_c5 !n1_r5_c6
!n2_r6_c5 !n3_r6_c5
!n2_r6_c5 !n4_r6_c5
!n2_r6_c5 !n5_r6_c5
!n2_r6_c5 !n6_r6_c5
!n2_r6_c5 !n7_r6_c5
!n2_r6_c5 !n8_r6_c5
!n2_r6_c5 !n9_r6_c5
!n2_r6_c5 !n2_r6_c6
!n2_r6_c5 !n2_r6_c7
!n2_r6_c5 !n2_r6_c8
!n2_r6_c5 !n2_r6_c9
!n2_r6_c5 !n2_r7_c5
!n2_r6_c5 !n2_r8_c5
!n2_r6_c5 !n2_r9_c5
!n2_r6_c5 !n2_r4_c4
!n2_r6_c5 !n2_r4_c6
!n2_r6_c5 !n2_r5_c4
!n2_r6_c5 !n2_r5_c6
!n3_r6_c5 !n4_r6_c5
!n3_r6_c5 !n5_r6_c5
!n3_r6_c5 !n6_r6_c5
!n3_r6_c5 !n7_r6_c5
!n3_r6_c5 !n8_r6_c5
!n3_r6_c5 !n9_r6_c5
!n3_r6_c5 !n3_r6_c6
!n3_r6_c5 !n3_r6_c7
!n3_r6_c5 !n3_r6_c8
!n3_r6_c5 !n3_r6_c9
!n3_r6_c5 !n3_r7_c5
!n3_r6_c5 !n3_r8_c5
!n3_r6_c5 !n3_r9_c5
!n3_r6_c5 !n3_r4_c4
!n3_r6_c5 !n3_r4_c6
!n3_r6_c5 !n3_r5_c4
!n3_r6_c5 !n3_r5_c6
!n4_r6_c5 !n5_r6_c5
!n4_r6_c5 !n6_r6_c5
!n4_r6_c5 !n7_r6_c5
!n4_r6_c5 !n8_r6_c5
!n4_r6_c5 !n9_r6_c5
!n4_r6_c5 !n4_r6_c6
!n4_r6_c5 !n4_r6_c7
!n4_r6_c5 !n4_r6_c8
!n4_r6_c5 !n4_r6_c9
!n4_r6_c5 !n4_r7_c5
!n4_r6_c5 !n4_r8_c5
!n4_r6_c5 !n4_r9_c5
!n4_r6_c5 !n4_r4_c4
!n4_r6_c5 !n4_r4_c6
!n4_r6_c5 !n4_r5_c4
!n4_r6_c5 !n4_r5_c6
!n5_r6_c5 !n6_r6_c5
!n5_r6_c5 !n7_r6_c5
!n5_r6_c5 !n8_r6_c5
!n5_r6_c5 !n9_r6_c5
!n5_r6_c5 !n5_r6_c6
!n5_r6_c5 !n5_r6_c7
!n5_r6_c5 !n5_r6_c8
!n5_r6_c5 !n5_r6_c9
!n5_r6_c5 !n5_r7_c5
!n5_r6_c5 !n5_r8_c5
!n5_r6_c5 !n5_r9_c5
!n5_r6_c5 !n5_r4_c4
!n5_r6_c5 !n5_r4_c6
!n5_r6_c5 !n5_r5_c4
!n5_r6_c5 !n5_r5_c6
!n6_r6_c5 !n7_r6_c5
!n6_r6_c5 !n8_r6_c5
!n6_r6_c5 !n9_r6_c5
!n6_r6_c5 !n6_r6_c6
!n6_r6_c5 !n6_r6_c7
!n6_r6_c5 !n6_r6_c8
!n6_r6_c5 !n6_r6_c9
!n6_r6_c5 !n6_r7_c5
!n6_r6_c5 !n6_r8_c5
!n6_r6_c5 !n6_r9_c5
!n6_r6_c5 !n6_r4_c4
!n6_r6_c5 !n6_r4_c6
!n6_r6_c5 !n6_r5_c4
!n6_r6_c5 !n6_r5_c6
!n7_r6_c5 !n8_r6_c5
!n7_r6_c5 !n9_r6_c5
!n7_r6_c5 !n7_r6_c6
!n7_r6_c5 !n7_r6_c7
!n7_r6_c5 !n7_r6_c8
!n7_r6_c5 !n7_r6_c9
!n7_r6_c5 !n7_r7_c5
!n7_r6_c5 !n7_r8_c5
!n7_r6_c5 !n7_r9_c5
!n7_r6_c5 !n7_r4_c4
!n7_r6_c5 !n7_r4_c6
!n7_r6_c5 !n7_r5_c4
!n7_r6_c5 !n7_r5_c6
!n8_r6_c5 !n9_r6_c5
!n8_r6_c5 !n8_r6_c6
!n8_r6_c5 !n8_r6_c7
!n8_r6_c5 !n8_r6_c8
!n8_r6_c5 !n8_r6_c9
!n8_r6_c5 !n8_r7_c5
!n8_r6_c5 !n8_r8_c5
!n8_r6_c5 !n8_r9_c5
!n8_r6_c5 !n8_r4_c4
!n8_r6_c5 !n8_r4_c6
!n8_r6_c5 !n8_r5_c4
!n8_r6_c5 !n8_r5_c6
!n9_r6_c5 !n9_r6_c6
!n9_r6_c5 !n9_r6_c7
!n9_r6_c5 !n9_r6_c8
!n9_r6_c5 !n9_r6_c9
!n9_r6_c5 !n9_r7_c5
!n9_r6_c5 !n9_r8_c5
!n9_r6_c5 !n9_r9_c5
!n9_r6_c5 !n9_r4_c4
!n9_r6_c5 !n9_r4_c6
!n9_r6_c5 !n9_r5_c4
!n9_r6_c5 !n9_r5_c6
n1_r6_c5 n2_r6_c5 n3_r6_c5 n4_r6_c5 n5_r6_c5 n6_r6_c5 n7_r6_c5 n8_r6_c5 n9_r6_c5
!n1_r6_c6 !n2_r6_c6
!n1_r6_c6 !n3_r6_c6
!n1_r6_c6 !n4_r6_c6
!n1_r6_c6 !n5_r6_c6
!n1_r6_c6 !n6_r6_c6
!n1_r6_c6 !n7_r6_c6
!n1_r6_c6 !n8_r6_c6
!n1_r6_c6 !n9_r6_c6
!n1_r6_c6 !n1_r6_c7
!n1_r6_c6 !n1_r6_c8
!n1_r6_c6 !n1_r6_c9
!n1_r6_c6 !n1_r7_c6
!n1_r6_c6 !n1_r8_c6
!n1_r6_c6 !n1_r9_c6
!n1_r6_c6 !n1_r4_c4
!n1_r6_c6 !n1_r4_c5
!n1_r6_c6 !n1_r5_c4
!n1_r6_c6 !n1_r5_c5
!n2_r6_c6 !n3_r6_c6
!n2_r6_c6 !n4_r6_c6
!n2_r6_c6 !n5_r6_c6
!n2_r6_c6 !n6_r6_c6
!n2_r6_c6 !n7_r6_c6
!n2_r6_c6 !n8_r6_c6
!n2_r6_c6 !n9_r6_c6
!n2_r6_c6 !n2_r6_c7
!n2_r6_c6 !n2_r6_c8
!n2_r6_c6 !n2_r6_c9
!n2_r6_c6 !n2_r7_c6
!n2_r6_c6 !n2_r8_c6
!n2_r6_c6 !n2_r9_c6
!n2_r6_c6 !n2_r4_c4
!n2_r6_c6 !n2_r4_c5
!n2_r6_c6 !n2_r5_c4
!n2_r6_c6 !n2_r5_c5
!n3_r6_c6 !n4_r6_c6
!n3_r6_c6 !n5_r6_c6
!n3_r6_c6 !n6_r6_c6
!n3_r6_c6 !n7_r6_c6
!n3_r6_c6 !n8_r6_c6
!n3_r6_c6 !n9_r6_c6
!n3_r6_c6 !n3_r6_c7
!n3_r6_c6 !n3_r6_c8
!n3_r6_c6 !n3_r6_c9
!n3_r6_c6 !n3_r7_c6
!n3_r6_c6 !n3_r8_c6
!n3_r6_c6 !n3_r9_c6
!n3_r6_c6 !n3_r4_c4
!n3_r6_c6 !n3_r4_c5
!n3_r6_c6 !n3_r5_c4
!n3_r6_c6 !n3_r5_c5
!n4_r6_c6 !n5_r6_c6
!n4_r6_c6 !n6_r6_c6
!n4_r6_c6 !n7_r6_c6
!n4_r6_c6 !n8_r6_c6
!n4_r6_c6 !n9_r6_c6
!n4_r6_c6 !n4_r6_c7
!n4_r6_c6 !n4_r6_c8
!n4_r6_c6 !n4_r6_c9
!n4_r6_c6 !n4_r7_c6
!n4_r6_c6 !n4_r8_c6
!n4_r6_c6 !n4_r9_c6
!n4_r6_c6 !n4_r4_c4
!n4_r6_c6 !n4_r4_c5
!n4_r6_c6 !n4_r5_c4
!n4_r6_c6 !n4_r5_c5
!n5_r6_c6 !n6_r6_c6
!n5_r6_c6 !n7_r6_c6
!n5_r6_c6 !n8_r6_c6
!n5_r6_c6 !n9_r6_c6
!n5_r6_c6 !n5_r6_c7
!n5_r6_c6 !n5_r6_c8
!n5_r6_c6 !n5_r6_c9
!n5_r6_c6 !n5_r7_c6
!n5_r6_c6 !n5_r8_c6
!n5_r6_c6 !n5_r9_c6
!n5_r6_c6 !n5_r4_c4
!n5_r6_c6 !n5_r4_c5
!n5_r6_c6 !n5_r5_c4
!n5_r6_c6 !n5_r5_c5
!n6_r6_c6 !n7_r6_c6
!n6_r6_c6 !n8_r6_c6
!n6_r6_c6 !n9_r6_c6
!n6_r6_c6 !n6_r6_c7
!n6_r6_c6 !n6_r6_c8
!n6_r6_c6 !n6_r6_c9
!n6_r6_c6 !n6_r7_c6
!n6_r6_c6 !n6_r8_c6
!n6_r6_c6 !n6_r9_c6
!n6_r6_c6 !n6_r4_c4
!n6_r6_c6 !n6_r4_c5
!n6_r6_c6 !n6_r5_c4
!n6_r6_c6 !n6_r5_c5
!n7_r6_c6 !n8_r6_c6
!n7_r6_c6 !n9_r6_c6
!n7_r6_c6 !n7_r6_c7
!n7_r6_c6 !n7_r6_c8
!n7_r6_c6 !n7_r6_c9
!n7_r6_c6 !n7_r7_c6
!n7_r6_c6 !n7_r8_c6
!n7_r6_c6 !n7_r9_c6
!n7_r6_c6 !n7_r4_c4
!n7_r6_c6 !n7_r4_c5
!n7_r6_c6 !n7_r5_c4
!n7_r6_c6 !n7_r5_c5
!n8_r6_c6 !n9_r6_c6
!n8_r6_c6 !n8_r6_c7
!n8_r6_c6 !n8_r6_c8
!n8_r6_c6 !n8_r6_c9
!n8_r6_c6 !n8_r7_c6
!n8_r6_c6 !n8_r8_c6
!n8_r6_c6 !n8_r9_c6
!n8_r6_c6 !n8_r4_c4
!n8_r6_c6 !n8_r4_c5
!n8_r6_c6 !n8_r5_c4
!n8_r6_c6 !n8_r5_c5
!n9_r6_c6 !n9_r6_c7
!n9_r6_c6 !n9_r6_c8
!n9_r6_c6 !n9_r6_c9
!n9_r6_c6 !n9_r7_c6
!n9_r6_c6 !n9_r8_c6
!n9_r6_c6 !n9_r9_c6
!n9_r6_c6 !n9_r4_c4
!n9_r6_c6 !n9_r4_c5
!n9_r6_c6 !n9_r5_c4
!n9_r6_c6 !n9_r5_c5
n1_r6_c6 n2_r6_c6 n3_r6_c6 n4_r6_c6 n5_r6_c6 n6_r6_c6 n7_r6_c6 n8_r6_c6 n9_r6_c6
!n1_r6_c7 !n2_r6_c7
!n1_r6_c7 !n3_r6_c7
!n1_r6_c7 !n4_r6_c7
!n1_r6_c7 !n5_r6_c7
!n1_r6_c7 !n6_r6_c7
!n1_r6_c7 !n7_r6_c7
!n1_r6_c7 !n8_r6_c7
!n1_r6_c7 !n9_r6_c7
!n1_r6_c7 !n1_r6_c8
!n1_r6_c7 !n1_r6_c9
!n1_r6_c7 !n1_r7_c7
!n1_r6_c7 !n1_r8_c7
!n1_r6_c7 !n1_r9_c7
!n1_r6_c7 !n1_r4_c8
!n1_r6_c7 !n1_r4_c9
!n1_r6_c7 !n1_r5_c8
!n1_r6_c7 !n1_r5_c9
!n2_r6_c7 !n3_r6_c7
!n2_r6_c7 !n4_r6_c7
!n2_r6_c7 !n5_r6_c7
!n2_r6_c7 !n6_r6_c7
!n2_r6_c7 !n7_r6_c7
!n2_r6_c7 !n8_r6_c7
!n2_r6_c7 !n9_r6_c7
!n2_r6_c7 !n2_r6_c8
!n2_r6_c7 !n2_r6_c9
!n2_r6_c7 !n2_r7_c7
!n2_r6_c7 !n2_r8_c7
!n2_r6_c7 !n2_r9_c7
!n2_r6_c7 !n2_r4_c8
!n2_r6_c7 !n2_r4_c9
!n2_r6_c7 !n2_r5_c8
!n2_r6_c7 !n2_r5_c9
!n3_r6_c7 !n4_r6_c7
!n3_r6_c7 !n5_r6_c7
!n3_r6_c7 !n6_r6_c7
!n3_r6_c7 !n7_r6_c7
!n3_r6_c7 !n8_r6_c7
!n3_r6_c7 !n9_r6_c7
!n3_r6_c7 !n3_r6_c8
!n3_r6_c7 !n3_r6_c9
!n3_r6_c7 !n3_r7_c7
!n3_r6_c7 !n3_r8_c7
!n3_r6_c7 !n3_r9_c7
!n3_r6_c7 !n3_r4_c8
!n3_r6_c7 !n3_r4_c9
!n3_r6_c7 !n3_r5_c8
!n3_r6_c7 !n3_r5_c9
!n4_r6_c7 !n5_r6_c7
!n4_r6_c7 !n6_r6_c7
!n4_r6_c7 !n7_r6_c7
!n4_r6_c7 !n8_r6_c7
!n4_r6_c7 !n9_r6_c7
!n4_r6_c7 !n4_r6_c8
!n4_r6_c7 !n4_r6_c9
!n4_r6_c7 !n4_r7_c7
!n4_r6_c7 !n4_r8_c7
!n4_r6_c7 !n4_r9_c7
!n4_r6_c7 !n4_r4_c8
!n4_r6_c7 !n4_r4_c9
!n4_r6_c7 !n4_r5_c8
!n4_r6_c7 !n4_r5_c9
!n5_r6_c7 !n6_r6_c7
!n5_r6_c7 !n7_r6_c7
!n5_r6_c7 !n8_r6_c7
!n5_r6_c7 !n9_r6_c7
!n5_r6_c7 !n5_r6_c8
!n5_r6_c7 !n5_r6_c9
!n5_r6_c7 !n5_r7_c7
!n5_r6_c7 !n5_r8_c7
!n5_r6_c7 !n5_r9_c7
!n5_r6_c7 !n5_r4_c8
!n5_r6_c7 !n5_r4_c9
!n5_r6_c7 !n5_r5_c8
!n5_r6_c7 !n5_r5_c9
!n6_r6_c7 !n7_r6_c7
!n6_r6_c7 !n8_r6_c7
!n6_r6_c7 !n9_r6_c7
!n6_r6_c7 !n6_r6_c8
!n6_r6_c7 !n6_r6_c9
!n6_r6_c7 !n6_r7_c7
!n6_r6_c7 !n6_r8_c7
!n6_r6_c7 !n6_r9_c7
!n6_r6_c7 !n6_r4_c8
!n6_r6_c7 !n6_r4_c9
!n6_r6_c7 !n6_r5_c8
!n6_r6_c7 !n6_r5_c9
!n7_r6_c7 !n8_r6_c7
!n7_r6_c7 !n9_r6_c7
!n7_r6_c7 !n7_r6_c8
!n7_r6_c7 !n7_r6_c9
!n7_r6_c7 !n7_r7_c7
!n7_r6_c7 !n7_r8_c7
!n7_r6_c7 !n7_r9_c7
!n7_r6_c7 !n7_r4_c8
!n7_r6_c7 !n7_r4_c9
!n7_r6_c7 !n7_r5_c8
!n7_r6_c7 !n7_r5_c9
!n8_r6_c7 !n9_r6_c7
!n8_r6_c7 !n8_r6_c8
!n8_r6_c7 !n8_r6_c9
!n8_r6_c7 !n8_r7_c7
!n8_r6_c7 !n8_r8_c7
!n8_r6_c7 !n8_r9_c7
!n8_r6_c7 !n8_r4_c8
!n8_r6_c7 !n8_r4_c9
!n8_r6_c7 !n8_r5_c8
!n8_r6_c7 !n8_r5_c9
!n9_r6_c7 !n9_r6_c8
!n9_r6_c7 !n9_r6_c9
!n9_r6_c7 !n9_r7_c7
!n9_r6_c7 !n9_r8_c7
!n9_r6_c7 !n9_r9_c7
!n9_r6_c7 !n9_r4_c8
!n9_r6_c7 !n9_r4_c9
!n9_r6_c7 !n9_r5_c8
!n9_r6_c7 !n9_r5_c9
n1_r6_c7 n2_r6_c7 n3_r6_c7 n4_r6_c7 n5_r6_c7 n6_r6_c7 n7_r6_c7 n8_r6_c7 n9_r6_c7
!n1_r6_c8 !n2_r6_c8
!n1_r6_c8 !n3_r6_c8
!n1_r6_c8 !n4_r6_c8
!n1_r6_c8 !n5_r6_c8
!n1_r6_c8 !n6_r6_c8
!n1_r6_c8 !n7_r6_c8
!n1_r6_c8 !n8_r6_c8
!n1_r6_c8 !n9_r6_c8
!n1_r6_c8 !n1_r6_c9
!n1_r6_c8 !n1_r7_c8
!n1_r6_c8 !n1_r8_c8
!n1_r6_c8 !n1_r9_c8
!n1_r6_c8 !n1_r4_c7
!n1_r6_c8 !n1_r4_c9
!n1_r6_c8 !n1_r5_c7
!n1_r6_c8 !n1_r5_c9
!n2_r6_c8 !n3_r6_c8
!n2_r6_c8 !n4_r6_c8
!n2_r6_c8 !n5_r6_c8
!n2_r6_c8 !n6_r6_c8
!n2_r6_c8 !n7_r6_c8
!n2_r6_c8 !n8_r6_c8
!n2_r6_c8 !n9_r6_c8
!n2_r6_c8 !n2_r6_c9
!n2_r6_c8 !n2_r7_c8
!n2_r6_c8 !n2_r8_c8
!n2_r6_c8 !n2_r9_c8
!n2_r6_c8 !n2_r4_c7
!n2_r6_c8 !n2_r4_c9
!n2_r6_c8 !n2_r5_c7
!n2_r6_c8 !n2_r5_c9
!n3_r6_c8 !n4_r6_c8
!n3_r6_c8 !n5_r6_c8
!n3_r6_c8 !n6_r6_c8
!n3_r6_c8 !n7_r6_c8
!n3_r6_c8 !n8_r6_c8
!n3_r6_c8 !n9_r6_c8
!n3_r6_c8 !n3_r6_c9
!n3_r6_c8 !n3_r7_c8
!n3_r6_c8 !n3_r8_c8
!n3_r6_c8 !n3_r9_c8
!n3_r6_c8 !n3_r4_c7
!n3_r6_c8 !n3_r4_c9
!n3_r6_c8 !n3_r5_c7
!n3_r6_c8 !n3_r5_c9
!n4_r6_c8 !n5_r6_c8
!n4_r6_c8 !n6_r6_c8
!n4_r6_c8 !n7_r6_c8
!n4_r6_c8 !n8_r6_c8
!n4_r6_c8 !n9_r6_c8
!n4_r6_c8 !n4_r6_c9
!n4_r6_c8 !n4_r7_c8
!n4_r6_c8 !n4_r8_c8
!n4_r6_c8 !n4_r9_c8
!n4_r6_c8 !n4_r4_c7
!n4_r6_c8 !n4_r4_c9
!n4_r6_c8 !n4_r5_c7
!n4_r6_c8 !n4_r5_c9
!n5_r6_c8 !n6_r6_c8
!n5_r6_c8 !n7_r6_c8
!n5_r6_c8 !n8_r6_c8
!n5_r6_c8 !n9_r6_c8
!n5_r6_c8 !n5_r6_c9
!n5_r6_c8 !n5_r7_c8
!n5_r6_c8 !n5_r8_c8
!n5_r6_c8 !n5_r9_c8
!n5_r6_c8 !n5_r4_c7
!n5_r6_c8 !n5_r4_c9
!n5_r6_c8 !n5_r5_c7
!n5_r6_c8 !n5_r5_c9
!n6_r6_c8 !n7_r6_c8
!n6_r6_c8 !n8_r6_c8
!n6_r6_c8 !n9_r6_c8
!n6_r6_c8 !n6_r6_c9
!n6_r6_c8 !n6_r7_c8
!n6_r6_c8 !n6_r8_c8
!n6_r6_c8 !n6_r9_c8
!n6_r6_c8 !n6_r4_c7
!n6_r6_c8 !n6_r4_c9
!n6_r6_c8 !n6_r5_c7
!n6_r6_c8 !n6_r5_c9
!n7_r6_c8 !n8_r6_c8
!n7_r6_c8 !n9_r6_c8
!n7_r6_c8 !n7_r6_c9
!n7_r6_c8 !n7_r7_c8
!n7_r6_c8 !n7_r8_c8
!n7_r6_c8 !n7_r9_c8
!n7_r6_c8 !n7_r4_c7
!n7_r6_c8 !n7_r4_c9
!n7_r6_c8 !n7_r5_c7
!n7_r6_c8 !n7_r5_c9
!n8_r6_c8 !n9_r6_c8
!n8_r6_c8 !n8_r6_c9
!n8_r6_c8 !n8_r7_c8
!n8_r6_c8 !n8_r8_c8
!n8_r6_c8 !n8_r9_c8
!n8_r6_c8 !n8_r4_c7
!n8_r6_c8 !n8_r4_c9
!n8_r6_c8 !n8_r5_c7
!n8_r6_c8 !n8_r5_c9
!n9_r6_c8 !n9_r6_c9
!n9_r6_c8 !n9_r7_c8
!n9_r6_c8 !n9_r8_c8
!n9_r6_c8 !n9_r9_c8
!n9_r6_c8 !n9_r4_c7
!n9_r6_c8 !n9_r4_c9
!n9_r6_c8 !n9_r5_c7
!n9_r6_c8 !n9_r5_c9
n1_r6_c8 n2_r6_c8 n3_r6_c8 n4_r6_c8 n5_r6_c8 n6_r6_c8 n7_r6_c8 n8_r6_c8 n9_r6_c8
!n1_r6_c9 !n2_r6_c9
!n1_r6_c9 !n3_r6_c9
!n1_r6_c9 !n4_r6_c9
!n1_r6_c9 !n5_r6_c9
!n1_r6_c9 !n6_r6_c9
!n1_r6_c9 !n7_r6_c9
!n1_r6_c9 !n8_r6_c9
!n1_r6_c9 !n9_r6_c9
!n1_r6_c9 !n1_r7_c9
!n1_r6_c9 !n1_r8_c9
!n1_r6_c9 !n1_r9_c9
!n1_r6_c9 !n1_r4_c7
!n1_r6_c9 !n1_r4_c8
!n1_r6_c9 !n1_r5_c7
!n1_r6_c9 !n1_r5_c8
!n2_r6_c9 !n3_r6_c9
!n2_r6_c9 !n4_r6_c9
!n2_r6_c9 !n5_r6_c9
!n2_r6_c9 !n6_r6_c9
!n2_r6_c9 !n7_r6_c9
!n2_r6_c9 !n8_r6_c9
!n2_r6_c9 !n9_r6_c9
!n2_r6_c9 !n2_r7_c9
!n2_r6_c9 !n2_r8_c9
!n2_r6_c9 !n2_r9_c9
!n2_r6_c9 !n2_r4_c7
!n2_r6_c9 !n2_r4_c8
!n2_r6_c9 !n2_r5_c7
!n2_r6_c9 !n2_r5_c8
!n3_r6_c9 !n4_r6_c9
!n3_r6_c9 !n5_r6_c9
!n3_r6_c9 !n6_r6_c9
!n3_r6_c9 !n7_r6_c9
!n3_r6_c9 !n8_r6_c9
!n3_r6_c9 !n9_r6_c9
!n3_r6_c9 !n3_r7_c9
!n3_r6_c9 !n3_r8_c9
!n3_r6_c9 !n3_r9_c9
!n3_r6_c9 !n3_r4_c7
!n3_r6_c9 !n3_r4_c8
!n3_r6_c9 !n3_r5_c7
!n3_r6_c9 !n3_r5_c8
!n4_r6_c9 !n5_r6_c9
!n4_r6_c9 !n6_r6_c9
!n4_r6_c9 !n7_r6_c9
!n4_r6_c9 !n8_r6_c9
!n4_r6_c9 !n9_r6_c9
!n4_r6_c9 !n4_r7_c9
!n4_r6_c9 !n4_r8_c9
!n4_r6_c9 !n4_r9_c9
!n4_r6_c9 !n4_r4_c7
!n4_r6_c9 !n4_r4_c8
!n4_r6_c9 !n4_r5_c7
!n4_r6_c9 !n4_r5_c8
!n5_r6_c9 !n6_r6_c9
!n5_r6_c9 !n7_r6_c9
!n5_r6_c9 !n8_r6_c9
!n5_r6_c9 !n9_r6_c9
!n5_r6_c9 !n5_r7_c9
!n5_r6_c9 !n5_r8_c9
!n5_r6_c9 !n5_r9_c9
!n5_r6_c9 !n5_r4_c7
!n5_r6_c9 !n5_r4_c8
!n5_r6_c9 !n5_r5_c7
!n5_r6_c9 !n5_r5_c8
!n6_r6_c9 !n7_r6_c9
!n6_r6_c9 !n8_r6_c9
!n6_r6_c9 !n9_r6_c9
!n6_r6_c9 !n6_r7_c9
!n6_r6_c9 !n6_r8_c9
!n6_r6_c9 !n6_r9_c9
!n6_r6_c9 !n6_r4_c7
!n6_r6_c9 !n6_r4_c8
!n6_r6_c9 !n6_r5_c7
!n6_r6_c9 !n6_r5_c8
!n7_r6_c9 !n8_r6_c9
!n7_r6_c9 !n9_r6_c9
!n7_r6_c9 !n7_r7_c9
!n7_r6_c9 !n7_r8_c9
!n7_r6_c9 !n7_r9_c9
!n7_r6_c9 !n7_r4_c7
!n7_r6_c9 !n7_r4_c8
!n7_r6_c9 !n7_r5_c7
!n7_r6_c9 !n7_r5_c8
!n8_r6_c9 !n9_r6_c9
!n8_r6_c9 !n8_r7_c9
!n8_r6_c9 !n8_r8_c9
!n8_r6_c9 !n8_r9_c9
!n8_r6_c9 !n8_r4_c7
!n8_r6_c9 !n8_r4_c8
!n8_r6_c9 !n8_r5_c7
!n8_r6_c9 !n8_r5_c8
!n9_r6_c9 !n9_r7_c9
!n9_r6_c9 !n9_r8_c9
!n9_r6_c9 !n9_r9_c9
!n9_r6_c9 !n9_r4_c7
!n9_r6_c9 !n9_r4_c8
!n9_r6_c9 !n9_r5_c7
!n9_r6_c9 !n9_r5_c8
n1_r6_c9 n2_r6_c9 n3_r6_c9 n4_r6_c9 n5_r6_c9 n6_r6_c9 n7_r6_c9 n8_r6_c9 n9_r6_c9
!n1_r7_c1 !n2_r7_c1
!n1_r7_c1 !n3_r7_c1
!n1_r7_c1 !n4_r7_c1
!n1_r7_c1 !n5_r7_c1
!n1_r7_c1 !n6_r7_c1
!n1_r7_c1 !n7_r7_c1
!n1_r7_c1 !n8_r7_c1
!n1_r7_c1 !n9_r7_c1
!n1_r7_c1 !n1_r7_c2
!n1_r7_c1 !n1_r7_c3
!n1_r7_c1 !n1_r7_c4
!n1_r7_c1 !n1_r7_c5
!n1_r7_c1 !n1_r7_c6
!n1_r7_c1 !n1_r7_c7
!n1_r7_c1 !n1_r7_c8
!n1_r7_c1 !n1_r7_c9
!n1_r7_c1 !n1_r8_c1
!n1_r7_c1 !n1_r9_c1
!n1_r7_c1 !n1_r8_c2
!n1_r7_c1 !n1_r8_c3
!n1_r7_c1 !n1_r9_c2
!n1_r7_c1 !n1_r9_c3
!n2_r7_c1 !n3_r7_c1
!n2_r7_c1 !n4_r7_c1
!n2_r7_c1 !n5_r7_c1
!n2_r7_c1 !n6_r7_c1
!n2_r7_c1 !n7_r7_c1
!n2_r7_c1 !n8_r7_c1
!n2_r7_c1 !n9_r7_c1
!n2_r7_c1 !n2_r7_c2
!n2_r7_c1 !n2_r7_c3
!n2_r7_c1 !n2_r7_c4
!n2_r7_c1 !n2_r7_c5
!n2_r7_c1 !n2_r7_c6
!n2_r7_c1 !n2_r7_c7
!n2_r7_c1 !n2_r7_c8
!n2_r7_c1 !n2_r7_c9
!n2_r7_c1 !n2_r8_c1
!n2_r7_c1 !n2_r9_c1
!n2_r7_c1 !n2_r8_c2
!n2_r7_c1 !n2_r8_c3
!n2_r7_c1 !n2_r9_c2
!n2_r7_c1 !n2_r9_c3
!n3_r7_c1 !n4_r7_c1
!n3_r7_c1 !n5_r7_c1
!n3_r7_c1 !n6_r7_c1
!n3_r7_c1 !n7_r7_c1
!n3_r7_c1 !n8_r7_c1
!n3_r7_c1 !n9_r7_c1
!n3_r7_c1 !n3_r7_c2
!n3_r7_c1 !n3_r7_c3
!n3_r7_c1 !n3_r7_c4
!n3_r7_c1 !n3_r7_c5
!n3_r7_c1 !n3_r7_c6
!n3_r7_c1 !n3_r7_c7
!n3_r7_c1 !n3_r7_c8
!n3_r7_c1 !n3_r7_c9
!n3_r7_c1 !n3_r8_c1
!n3_r7_c1 !n3_r9_c1
!n3_r7_c1 !n3_r8_c2
!n3_r7_c1 !n3_r8_c3
!n3_r7_c1 !n3_r9_c2
!n3_r7_c1 !n3_r9_c3
!n4_r7_c1 !n5_r7_c1
!n4_r7_c1 !n6_r7_c1
!n4_r7_c1 !n7_r7_c1
!n4_r7_c1 !n8_r7_c1
!n4_r7_c1 !n9_r7_c1
!n4_r7_c1 !n4_r7_c2
!n4_r7_c1 !n4_r7_c3
!n4_r7_c1 !n4_r7_c4
!n4_r7_c1 !n4_r7_c5
!n4_r7_c1 !n4_r7_c6
!n4_r7_c1 !n4_r7_c7
!n4_r7_c1 !n4_r7_c8
!n4_r7_c1 !n4_r7_c9
!n4_r7_c1 !n4_r8_c1
!n4_r7_c1 !n4_r9_c1
!n4_r7_c1 !n4_r8_c2
!n4_r7_c1 !n4_r8_c3
!n4_r7_c1 !n4_r9_c2
!n4_r7_c1 !n4_r9_c3
!n5_r7_c1 !n6_r7_c1
!n5_r7_c1 !n7_r7_c1
!n5_r7_c1 !n8_r7_c1
!n5_r7_c1 !n9_r7_c1
!n5_r7_c1 !n5_r7_c2
!n5_r7_c1 !n5_r7_c3
!n5_r7_c1 !n5_r7_c4
!n5_r7_c1 !n5_r7_c5
!n5_r7_c1 !n5_r7_c6
!n5_r7_c1 !n5_r7_c7
!n5_r7_c1 !n5_r7_c8
!n5_r7_c1 !n5_r7_c9
!n5_r7_c1 !n5_r8_c1
!n5_r7_c1 !n5_r9_c1
!n5_r7_c1 !n5_r8_c2
!n5_r7_c1 !n5_r8_c3
!n5_r7_c1 !n5_r9_c2
!n5_r7_c1 !n5_r9_c3
!n6_r7_c1 !n7_r7_c1
!n6_r7_c1 !n8_r7_c1
!n6_r7_c1 !n9_r7_c1
!n6_r7_c1 !n6_r7_c2
!n6_r7_c1 !n6_r7_c3
!n6_r7_c1 !n6_r7_c4
!n6_r7_c1 !n6_r7_c5
!n6_r7_c1 !n6_r7_c6
!n6_r7_c1 !n6_r7_c7
!n6_r7_c1 !n6_r7_c8
!n6_r7_c1 !n6_r7_c9
!n6_r7_c1 !n6_r8_c1
!n6_r7_c1 !n6_r9_c1
!n6_r7_c1 !n6_r8_c2
!n6_r7_c1 !n6_r8_c3
!n6_r7_c1 !n6_r9_c2
!n6_r7_c1 !n6_r9_c3
!n7_r7_c1 !n8_r7_c1
!n7_r7_c1 !n9_r7_c1
!n7_r7_c1 !n7_r7_c2
!n7_r7_c1 !n7_r7_c3
!n7_r7_c1 !n7_r7_c4
!n7_r7_c1 !n7_r7_c5
!n7_r7_c1 !n7_r7_c6
!n7_r7_c1 !n7_r7_c7
!n7_r7_c1 !n7_r7_c8
!n7_r7_c1 !n7_r7_c9
!n7_r7_c1 !n7_r8_c1
!n7_r7_c1 !n7_r9_c1
!n7_r7_c1 !n7_r8_c2
!n7_r7_c1 !n7_r8_c3
!n7_r7_c1 !n7_r9_c2
!n7_r7_c1 !n7_r9_c3
!n8_r7_c1 !n9_r7_c1
!n8_r7_c1 !n8_r7_c2
!n8_r7_c1 !n8_r7_c3
!n8_r7_c1 !n8_r7_c4
!n8_r7_c1 !n8_r7_c5
!n8_r7_c1 !n8_r7_c6
!n8_r7_c1 !n8_r7_c7
!n8_r7_c1 !n8_r7_c8
!n8_r7_c1 !n8_r7_c9
!n8_r7_c1 !n8_r8_c1
!n8_r7_c1 !n8_r9_c1
!n8_r7_c1 !n8_r8_c2
!n8_r7_c1 !n8_r8_c3
!n8_r7_c1 !n8_r9_c2
!n8_r7_c1 !n8_r9_c3
!n9_r7_c1 !n9_r7_c2
!n9_r7_c1 !n9_r7_c3
!n9_r7_c1 !n9_r7_c4
!n9_r7_c1 !n9_r7_c5
!n9_r7_c1 !n9_r7_c6
!n9_r7_c1 !n9_r7_c7
!n9_r7_c1 !n9_r7_c8
!n9_r7_c1 !n9_r7_c9
!n9_r7_c1 !n9_r8_c1
!n9_r7_c1 !n9_r9_c1
!n9_r7_c1 !n9_r8_c2
!n9_r7_c1 !n9_r8_c3
!n9_r7_c1 !n9_r9_c2
!n9_r7_c1 !n9_r9_c3
n1_r7_c1 n2_r7_c1 n3_r7_c1 n4_r7_c1 n5_r7_c1 n6_r7_c1 n7_r7_c1 n8_r7_c1 n9_r7_c1
!n1_r7_c2 !n2_r7_c2
!n1_r7_c2 !n3_r7_c2
!n1_r7_c2 !n4_r7_c2
!n1_r7_c2 !n5_r7_c2
!n1_r7_c2 !n6_r7_c2
!n1_r7_c2 !n7_r7_c2
!n1_r7_c2 !n8_r7_c2
!n1_r7_c2 !n9_r7_c2
!n1_r7_c2 !n1_r7_c3
!n1_r7_c2 !n1_r7_c4
!n1_r7_c2 !n1_r7_c5
!n1_r7_c2 !n1_r7_c6
!n1_r7_c2 !n1_r7_c7
!n1_r7_c2 !n1_r7_c8
!n1_r7_c2 !n1_r7_c9
!n1_r7_c2 !n1_r8_c2
!n1_r7_c2 !n1_r9_c2
!n1_r7_c2 !n1_r8_c1
!n1_r7_c2 !n1_r8_c3
!n1_r7_c2 !n1_r9_c1
!n1_r7_c2 !n1_r9_c3
!n2_r7_c2 !n3_r7_c2
!n2_r7_c2 !n4_r7_c2
!n2_r7_c2 !n5_r7_c2
!n2_r7_c2 !n6_r7_c2
!n2_r7_c2 !n7_r7_c2
!n2_r7_c2 !n8_r7_c2
!n2_r7_c2 !n9_r7_c2
!n2_r7_c2 !n2_r7_c3
!n2_r7_c2 !n2_r7_c4
!n2_r7_c2 !n2_r7_c5
!n2_r7_c2 !n2_r7_c6
!n2_r7_c2 !n2_r7_c7
!n2_r7_c2 !n2_r7_c8
!n2_r7_c2 !n2_r7_c9
!n2_r7_c2 !n2_r8_c2
!n2_r7_c2 !n2_r9_c2
!n2_r7_c2 !n2_r8_c1
!n2_r7_c2 !n2_r8_c3
!n2_r7_c2 !n2_r9_c1
!n2_r7_c2 !n2_r9_c3
!n3_r7_c2 !n4_r7_c2
!n3_r7_c2 !n5_r7_c2
!n3_r7_c2 !n6_r7_c2
!n3_r7_c2 !n7_r7_c2
!n3_r7_c2 !n8_r7_c2
!n3_r7_c2 !n9_r7_c2
!n3_r7_c2 !n3_r7_c3
!n3_r7_c2 !n3_r7_c4
!n3_r7_c2 !n3_r7_c5
!n3_r7_c2 !n3_r7_c6
!n3_r7_c2 !n3_r7_c7
!n3_r7_c2 !n3_r7_c8
!n3_r7_c2 !n3_r7_c9
!n3_r7_c2 !n3_r8_c2
!n3_r7_c2 !n3_r9_c2
!n3_r7_c2 !n3_r8_c1
!n3_r7_c2 !n3_r8_c3
!n3_r7_c2 !n3_r9_c1
!n3_r7_c2 !n3_r9_c3
!n4_r7_c2 !n5_r7_c2
!n4_r7_c2 !n6_r7_c2
!n4_r7_c2 !n7_r7_c2
!n4_r7_c2 !n8_r7_c2
!n4_r7_c2 !n9_r7_c2
!n4_r7_c2 !n4_r7_c3
!n4_r7_c2 !n4_r7_c4
!n4_r7_c2 !n4_r7_c5
!n4_r7_c2 !n4_r7_c6
!n4_r7_c2 !n4_r7_c7
!n4_r7_c2 !n4_r7_c8
!n4_r7_c2 !n4_r7_c9
!n4_r7_c2 !n4_r8_c2
!n4_r7_c2 !n4_r9_c2
!n4_r7_c2 !n4_r8_c1
!n4_r7_c2 !n4_r8_c3
!n4_r7_c2 !n4_r9_c1
!n4_r7_c2 !n4_r9_c3
!n5_r7_c2 !n6_r7_c2
!n5_r7_c2 !n7_r7_c2
!n5_r7_c2 !n8_r7_c2
!n5_r7_c2 !n9_r7_c2
!n5_r7_c2 !n5_r7_c3
!n5_r7_c2 !n5_r7_c4
!n5_r7_c2 !n5_r7_c5
!n5_r7_c2 !n5_r7_c6
!n5_r7_c2 !n5_r7_c7
!n5_r7_c2 !n5_r7_c8
!n5_r7_c2 !n5_r7_c9
!n5_r7_c2 !n5_r8_c2
!n5_r7_c2 !n5_r9_c2
!n5_r7_c2 !n5_r8_c1
!n5_r7_c2 !n5_r8_c3
!n5_r7_c2 !n5_r9_c1
!n5_r7_c2 !n5_r9_c3
!n6_r7_c2 !n7_r7_c2
!n6_r7_c2 !n8_r7_c2
!n6_r7_c2 !n9_r7_c2
!n6_r7_c2 !n6_r7_c3
!n6_r7_c2 !n6_r7_c4
!n6_r7_c2 !n6_r7_c5
!n6_r7_c2 !n6_r7_c6
!n6_r7_c2 !n6_r7_c7
!n6_r7_c2 !n6_r7_c8
!n6_r7_c2 !n6_r7_c9
!n6_r7_c2 !n6_r8_c2
!n6_r7_c2 !n6_r9_c2
!n6_r7_c2 !n6_r8_c1
!n6_r7_c2 !n6_r8_c3
!n6_r7_c2 !n6_r9_c1
!n6_r7_c2 !n6_r9_c3
!n7_r7_c2 !n8_r7_c2
!n7_r7_c2 !n9_r7_c2
!n7_r7_c2 !n7_r7_c3
!n7_r7_c2 !n7_r7_c4
!n7_r7_c2 !n7_r7_c5
!n7_r7_c2 !n7_r7_c6
!n7_r7_c2 !n7_r7_c7
!n7_r7_c2 !n7_r7_c8
!n7_r7_c2 !n7_r7_c9
!n7_r7_c2 !n7_r8_c2
!n7_r7_c2 !n7_r9_c2
!n7_r7_c2 !n7_r8_c1
!n7_r7_c2 !n7_r8_c3
!n7_r7_c2 !n7_r9_c1
!n7_r7_c2 !n7_r9_c3
!n8_r7_c2 !n9_r7_c2
!n8_r7_c2 !n8_r7_c3
!n8_r7_c2 !n8_r7_c4
!n8_r7_c2 !n8_r7_c5
!n8_r7_c2 !n8_r7_c6
!n8_r7_c2 !n8_r7_c7
!n8_r7_c2 !n8_r7_c8
!n8_r7_c2 !n8_r7_c9
!n8_r7_c2 !n8_r8_c2
!n8_r7_c2 !n8_r9_c2
!n8_r7_c2 !n8_r8_c1
!n8_r7_c2 !n8_r8_c3
!n8_r7_c2 !n8_r9_c1
!n8_r7_c2 !n8_r9_c3
!n9_r7_c2 !n9_r7_c3
!n9_r7_c2 !n9_r7_c4
!n9_r7_c2 !n9_r7_c5
!n9_r7_c2 !n9_r7_c6
!n9_r7_c2 !n9_r7_c7
!n9_r7_c2 !n9_r7_c8
!n9_r7_c2 !n9_r7_c9
!n9_r7_c2 !n9_r8_c2
!n9_r7_c2 !n9_r9_c2
!n9_r7_c2 !n9_r8_c1
!n9_r7_c2 !n9_r8_c3
!n9_r7_c2 !n9_r9_c1
!n9_r7_c2 !n9_r9_c3
n1_r7_c2 n2_r7_c2 n3_r7_c2 n4_r7_c2 n5_r7_c2 n6_r7_c2 n7_r7_c2 n8_r7_c2 n9_r7_c2
!n1_r7_c3 !n2_r7_c3
!n1_r7_c3 !n3_r7_c3
!n1_r7_c3 !n4_r7_c3
!n1_r7_c3 !n5_r7_c3
!n1_r7_c3 !n6_r7_c3
!n1_r7_c3 !n7_r7_c3
!n1_r7_c3 !n8_r7_c3
!n1_r7_c3 !n9_r7_c3
!n1_r7_c3 !n1_r7_c4
!n1_r7_c3 !n1_r7_c5
!n1_r7_c3 !n1_r7_c6
!n1_r7_c3 !n1_r7_c7
!n1_r7_c3 !n1_r7_c8
!n1_r7_c3 !n1_r7_c9
!n1_r7_c3 !n1_r8_c3
!n1_r7_c3 !n1_r9_c3
!n1_r7_c3 !n1_r8_c1
!n1_r7_c3 !n1_r8_c2
!n1_r7_c3 !n1_r9_c1
!n1_r7_c3 !n1_r9_c2
!n2_r7_c3 !n3_r7_c3
!n2_r7_c3 !n4_r7_c3
!n2_r7_c3 !n5_r7_c3
!n2_r7_c3 !n6_r7_c3
!n2_r7_c3 !n7_r7_c3
!n2_r7_c3 !n8_r7_c3
!n2_r7_c3 !n9_r7_c3
!n2_r7_c3 !n2_r7_c4
!n2_r7_c3 !n2_r7_c5
!n2_r7_c3 !n2_r7_c6
!n2_r7_c3 !n2_r7_c7
!n2_r7_c3 !n2_r7_c8
!n2_r7_c3 !n2_r7_c9
!n2_r7_c3 !n2_r8_c3
!n2_r7_c3 !n2_r9_c3
!n2_r7_c3 !n2_r8_c1
!n2_r7_c3 !n2_r8_c2
!n2_r7_c3 !n2_r9_c1
!n2_r7_c3 !n2_r9_c2
!n3_r7_c3 !n4_r7_c3
!n3_r7_c3 !n5_r7_c3
!n3_r7_c3 !n6_r7_c3
!n3_r7_c3 !n7_r7_c3
!n3_r7_c3 !n8_r7_c3
!n3_r7_c3 !n9_r7_c3
!n3_r7_c3 !n3_r7_c4
!n3_r7_c3 !n3_r7_c5
!n3_r7_c3 !n3_r7_c6
!n3_r7_c3 !n3_r7_c7
!n3_r7_c3 !n3_r7_c8
!n3_r7_c3 !n3_r7_c9
!n3_r7_c3 !n3_r8_c3
!n3_r7_c3 !n3_r9_c3
!n3_r7_c3 !n3_r8_c1
!n3_r7_c3 !n3_r8_c2
!n3_r7_c3 !n3_r9_c1
!n3_r7_c3 !n3_r9_c2
!n4_r7_c3 !n5_r7_c3
!n4_r7_c3 !n6_r7_c3
!n4_r7_c3 !n7_r7_c3
!n4_r7_c3 !n8_r7_c3
!n4_r7_c3 !n9_r7_c3
!n4_r7_c3 !n4_r7_c4
!n4_r7_c3 !n4_r7_c5
!n4_r7_c3 !n4_r7_c6
!n4_r7_c3 !n4_r7_c7
!n4_r7_c3 !n4_r7_c8
!n4_r7_c3 !n4_r7_c9
!n4_r7_c3 !n4_r8_c3
!n4_r7_c3 !n4_r9_c3
!n4_r7_c3 !n4_r8_c1
!n4_r7_c3 !n4_r8_c2
!n4_r7_c3 !n4_r9_c1
!n4_r7_c3 !n4_r9_c2
!n5_r7_c3 !n6_r7_c3
!n5_r7_c3 !n7_r7_c3
!n5_r7_c3 !n8_r7_c3
!n5_r7_c3 !n9_r7_c3
!n5_r7_c3 !n5_r7_c4
!n5_r7_c3 !n5_r7_c5
!n5_r7_c3 !n5_r7_c6
!n5_r7_c3 !n5_r7_c7
!n5_r7_c3 !n5_r7_c8
!n5_r7_c3 !n5_r7_c9
!n5_r7_c3 !n5_r8_c3
!n5_r7_c3 !n5_r9_c3
!n5_r7_c3 !n5_r8_c1
!n5_r7_c3 !n5_r8_c2
!n5_r7_c3 !n5_r9_c1
!n5_r7_c3 !n5_r9_c2
!n6_r7_c3 !n7_r7_c3
!n6_r7_c3 !n8_r7_c3
!n6_r7_c3 !n9_r7_c3
!n6_r7_c3 !n6_r7_c4
!n6_r7_c3 !n6_r7_c5
!n6_r7_c3 !n6_r7_c6
!n6_r7_c3 !n6_r7_c7
!n6_r7_c3 !n6_r7_c8
!n6_r7_c3 !n6_r7_c9
!n6_r7_c3 !n6_r8_c3
!n6_r7_c3 !n6_r9_c3
!n6_r7_c3 !n6_r8_c1
!n6_r7_c3 !n6_r8_c2
!n6_r7_c3 !n6_r9_c1
!n6_r7_c3 !n6_r9_c2
!n7_r7_c3 !n8_r7_c3
!n7_r7_c3 !n9_r7_c3
!n7_r7_c3 !n7_r7_c4
!n7_r7_c3 !n7_r7_c5
!n7_r7_c3 !n7_r7_c6
!n7_r7_c3 !n7_r7_c7
!n7_r7_c3 !n7_r7_c8
!n7_r7_c3 !n7_r7_c9
!n7_r7_c3 !n7_r8_c3
!n7_r7_c3 !n7_r9_c3
!n7_r7_c3 !n7_r8_c1
!n7_r7_c3 !n7_r8_c2
!n7_r7_c3 !n7_r9_c1
!n7_r7_c3 !n7_r9_c2
!n8_r7_c3 !n9_r7_c3
!n8_r7_c3 !n8_r7_c4
!n8_r7_c3 !n8_r7_c5
!n8_r7_c3 !n8_r7_c6
!n8_r7_c3 !n8_r7_c7
!n8_r7_c3 !n8_r7_c8
!n8_r7_c3 !n8_r7_c9
!n8_r7_c3 !n8_r8_c3
!n8_r7_c3 !n8_r9_c3
!n8_r7_c3 !n8_r8_c1
!n8_r7_c3 !n8_r8_c2
!n8_r7_c3 !n8_r9_c1
!n8_r7_c3 !n8_r9_c2
!n9_r7_c3 !n9_r7_c4
!n9_r7_c3 !n9_r7_c5
!n9_r7_c3 !n9_r7_c6
!n9_r7_c3 !n9_r7_c7
!n9_r7_c3 !n9_r7_c8
!n9_r7_c3 !n9_r7_c9
!n9_r7_c3 !n9_r8_c3
!n9_r7_c3 !n9_r9_c3
!n9_r7_c3 !n9_r8_c1
!n9_r7_c3 !n9_r8_c2
!n9_r7_c3 !n9_r9_c1
!n9_r7_c3 !n9_r9_c2
n1_r7_c3 n2_r7_c3 n3_r7_c3 n4_r7_c3 n5_r7_c3 n6_r7_c3 n7_r7_c3 n8_r7_c3 n9_r7_c3
!n1_r7_c4 !n2_r7_c4
!n1_r7_c4 !n3_r7_c4
!n1_r7_c4 !n4_r7_c4
!n1_r7_c4 !n5_r7_c4
!n1_r7_c4 !n6_r7_c4
!n1_r7_c4 !n7_r7_c4
!n1_r7_c4 !n8_r7_c4
!n1_r7_c4 !n9_r7_c4
!n1_r7_c4 !n1_r7_c5
!n1_r7_c4 !n1_r7_c6
!n1_r7_c4 !n1_r7_c7
!n1_r7_c4 !n1_r7_c8
!n1_r7_c4 !n1_r7_c9
!n1_r7_c4 !n1_r8_c4
!n1_r7_c4 !n1_r9_c4
!n1_r7_c4 !n1_r8_c5
!n1_r7_c4 !n1_r8_c6
!n1_r7_c4 !n1_r9_c5
!n1_r7_c4 !n1_r9_c6
!n2_r7_c4 !n3_r7_c4
!n2_r7_c4 !n4_r7_c4
!n2_r7_c4 !n5_r7_c4
!n2_r7_c4 !n6_r7_c4
!n2_r7_c4 !n7_r7_c4
!n2_r7_c4 !n8_r7_c4
!n2_r7_c4 !n9_r7_c4
!n2_r7_c4 !n2_r7_c5
!n2_r7_c4 !n2_r7_c6
!n2_r7_c4 !n2_r7_c7
!n2_r7_c4 !n2_r7_c8
!n2_r7_c4 !n2_r7_c9
!n2_r7_c4 !n2_r8_c4
!n2_r7_c4 !n2_r9_c4
!n2_r7_c4 !n2_r8_c5
!n2_r7_c4 !n2_r8_c6
!n2_r7_c4 !n2_r9_c5
!n2_r7_c4 !n2_r9_c6
!n3_r7_c4 !n4_r7_c4
!n3_r7_c4 !n5_r7_c4
!n3_r7_c4 !n6_r7_c4
!n3_r7_c4 !n7_r7_c4
!n3_r7_c4 !n8_r7_c4
!n3_r7_c4 !n9_r7_c4
!n3_r7_c4 !n3_r7_c5
!n3_r7_c4 !n3_r7_c6
!n3_r7_c4 !n3_r7_c7
!n3_r7_c4 !n3_r7_c8
!n3_r7_c4 !n3_r7_c9
!n3_r7_c4 !n3_r8_c4
!n3_r7_c4 !n3_r9_c4
!n3_r7_c4 !n3_r8_c5
!n3_r7_c4 !n3_r8_c6
!n3_r7_c4 !n3_r9_c5
!n3_r7_c4 !n3_r9_c6
!n4_r7_c4 !n5_r7_c4
!n4_r7_c4 !n6_r7_c4
!n4_r7_c4 !n7_r7_c4
!n4_r7_c4 !n8_r7_c4
!n4_r7_c4 !n9_r7_c4
!n4_r7_c4 !n4_r7_c5
!n4_r7_c4 !n4_r7_c6
!n4_r7_c4 !n4_r7_c7
!n4_r7_c4 !n4_r7_c8
!n4_r7_c4 !n4_r7_c9
!n4_r7_c4 !n4_r8_c4
!n4_r7_c4 !n4_r9_c4
!n4_r7_c4 !n4_r8_c5
!n4_r7_c4 !n4_r8_c6
!n4_r7_c4 !n4_r9_c5
!n4_r7_c4 !n4_r9_c6
!n5_r7_c4 !n6_r7_c4
!n5_r7_c4 !n7_r7_c4
!n5_r7_c4 !n8_r7_c4
!n5_r7_c4 !n9_r7_c4
!n5_r7_c4 !n5_r7_c5
!n5_r7_c4 !n5_r7_c6
!n5_r7_c4 !n5_r7_c7
!n5_r7_c4 !n5_r7_c8
!n5_r7_c4 !n5_r7_c9
!n5_r7_c4 !n5_r8_c4
!n5_r7_c4 !n5_r9_c4
!n5_r7_c4 !n5_r8_c5
!n5_r7_c4 !n5_r8_c6
!n5_r7_c4 !n5_r9_c5
!n5_r7_c4 !n5_r9_c6
!n6_r7_c4 !n7_r7_c4
!n6_r7_c4 !n8_r7_c4
!n6_r7_c4 !n9_r7_c4
!n6_r7_c4 !n6_r7_c5
!n6_r7_c4 !n6_r7_c6
!n6_r7_c4 !n6_r7_c7
!n6_r7_c4 !n6_r7_c8
!n6_r7_c4 !n6_r7_c9
!n6_r7_c4 !n6_r8_c4
!n6_r7_c4 !n6_r9_c4
!n6_r7_c4 !n6_r8_c5
!n6_r7_c4 !n6_r8_c6
!n6_r7_c4 !n6_r9_c5
!n6_r7_c4 !n6_r9_c6
!n7_r7_c4 !n8_r7_c4
!n7_r7_c4 !n9_r7_c4
!n7_r7_c4 !n7_r7_c5
!n7_r7_c4 !n7_r7_c6
!n7_r7_c4 !n7_r7_c7
!n7_r7_c4 !n7_r7_c8
!n7_r7_c4 !n7_r7_c9
!n7_r7_c4 !n7_r8_c4
!n7_r7_c4 !n7_r9_c4
!n7_r7_c4 !n7_r8_c5
!n7_r7_c4 !n7_r8_c6
!n7_r7_c4 !n7_r9_c5
!n7_r7_c4 !n7_r9_c6
!n8_r7_c4 !n9_r7_c4
!n8_r7_c4 !n8_r7_c5
!n8_r7_c4 !n8_r7_c6
!n8_r7_c4 !n8_r7_c7
!n8_r7_c4 !n8_r7_c8
!n8_r7_c4 !n8_r7_c9
!n8_r7_c4 !n8_r8_c4
!n8_r7_c4 !n8_r9_c4
!n8_r7_c4 !n8_r8_c5
!n8_r7_c4 !n8_r8_c6
!n8_r7_c4 !n8_r9_c5
!n8_r7_c4 !n8_r9_c6
!n9_r7_c4 !n9_r7_c5
!n9_r7_c4 !n9_r7_c6
!n9_r7_c4 !n9_r7_c7
!n9_r7_c4 !n9_r7_c8
!n9_r7_c4 !n9_r7_c9
!n9_r7_c4 !n9_r8_c4
!n9_r7_c4 !n9_r9_c4
!n9_r7_c4 !n9_r8_c5
!n9_r7_c4 !n9_r8_c6
!n9_r7_c4 !n9_r9_c5
!n9_r7_c4 !n9_r9_c6
n1_r7_c4 n2_r7_c4 n3_r7_c4 n4_r7_c4 n5_r7_c4 n6_r7_c4 n7_r7_c4 n8_r7_c4 n9_r7_c4
!n1_r7_c5 !n2_r7_c5
!n1_r7_c5 !n3_r7_c5
!n1_r7_c5 !n4_r7_c5
!n1_r7_c5 !n5_r7_c5
!n1_r7_c5 !n6_r7_c5
!n1_r7_c5 !n7_r7_c5
!n1_r7_c5 !n8_r7_c5
!n1_r7_c5 !n9_r7_c5
!n1_r7_c5 !n1_r7_c6
!n1_r7_c5 !n1_r7_c7
!n1_r7_c5 !n1_r7_c8
!n1_r7_c5 !n1_r7_c9
!n1_r7_c5 !n1_r8_c5
!n1_r7_c5 !n1_r9_c5
!n1_r7_c5 !n1_r8_c4
!n1_r7_c5 !n1_r8_c6
!n1_r7_c5 !n1_r9_c4
!n1_r7_c5 !n1_r9_c6
!n2_r7_c5 !n3_r7_c5
!n2_r7_c5 !n4_r7_c5
!n2_r7_c5 !n5_r7_c5
!n2_r7_c5 !n6_r7_c5
!n2_r7_c5 !n7_r7_c5
!n2_r7_c5 !n8_r7_c5
!n2_r7_c5 !n9_r7_c5
!n2_r7_c5 !n2_r7_c6
!n2_r7_c5 !n2_r7_c7
!n2_r7_c5 !n2_r7_c8
!n2_r7_c5 !n2_r7_c9
!n2_r7_c5 !n2_r8_c5
!n2_r7_c5 !n2_r9_c5
!n2_r7_c5 !n2_r8_c4
!n2_r7_c5 !n2_r8_c6
!n2_r7_c5 !n2_r9_c4
!n2_r7_c5 !n2_r9_c6
!n3_r7_c5 !n4_r7_c5
!n3_r7_c5 !n5_r7_c5
!n3_r7_c5 !n6_r7_c5
!n3_r7_c5 !n7_r7_c5
!n3_r7_c5 !n8_r7_c5
!n3_r7_c5 !n9_r7_c5
!n3_r7_c5 !n3_r7_c6
!n3_r7_c5 !n3_r7_c7
!n3_r7_c5 !n3_r7_c8
!n3_r7_c5 !n3_r7_c9
!n3_r7_c5 !n3_r8_c5
!n3_r7_c5 !n3_r9_c5
!n3_r7_c5 !n3_r8_c4
!n3_r7_c5 !n3_r8_c6
!n3_r7_c5 !n3_r9_c4
!n3_r7_c5 !n3_r9_c6
!n4_r7_c5 !n5_r7_c5
!n4_r7_c5 !n6_r7_c5
!n4_r7_c5 !n7_r7_c5
!n4_r7_c5 !n8_r7_c5
!n4_r7_c5 !n9_r7_c5
!n4_r7_c5 !n4_r7_c6
!n4_r7_c5 !n4_r7_c7
!n4_r7_c5 !n4_r7_c8
!n4_r7_c5 !n4_r7_c9
!n4_r7_c5 !n4_r8_c5
!n4_r7_c5 !n4_r9_c5
!n4_r7_c5 !n4_r8_c4
!n4_r7_c5 !n4_r8_c6
!n4_r7_c5 !n4_r9_c4
!n4_r7_c5 !n4_r9_c6
!n5_r7_c5 !n6_r7_c5
!n5_r7_c5 !n7_r7_c5
!n5_r7_c5 !n8_r7_c5
!n5_r7_c5 !n9_r7_c5
!n5_r7_c5 !n5_r7_c6
!n5_r7_c5 !n5_r7_c7
!n5_r7_c5 !n5_r7_c8
!n5_r7_c5 !n5_r7_c9
!n5_r7_c5 !n5_r8_c5
!n5_r7_c5 !n5_r9_c5
!n5_r7_c5 !n5_r8_c4
!n5_r7_c5 !n5_r8_c6
!n5_r7_c5 !n5_r9_c4
!n5_r7_c5 !n5_r9_c6
!n6_r7_c5 !n7_r7_c5
!n6_r7_c5 !n8_r7_c5
!n6_r7_c5 !n9_r7_c5
!n6_r7_c5 !n6_r7_c6
!n6_r7_c5 !n6_r7_c7
!n6_r7_c5 !n6_r7_c8
!n6_r7_c5 !n6_r7_c9
!n6_r7_c5 !n6_r8_c5
!n6_r7_c5 !n6_r9_c5
!n6_r7_c5 !n6_r8_c4
!n6_r7_c5 !n6_r8_c6
!n6_r7_c5 !n6_r9_c4
!n6_r7_c5 !n6_r9_c6
!n7_r7_c5 !n8_r7_c5
!n7_r7_c5 !n9_r7_c5
!n7_r7_c5 !n7_r7_c6
!n7_r7_c5 !n7_r7_c7
!n7_r7_c5 !n7_r7_c8
!n7_r7_c5 !n7_r7_c9
!n7_r7_c5 !n7_r8_c5
!n7_r7_c5 !n7_r9_c5
!n7_r7_c5 !n7_r8_c4
!n7_r7_c5 !n7_r8_c6
!n7_r7_c5 !n7_r9_c4
!n7_r7_c5 !n7_r9_c6
!n8_r7_c5 !n9_r7_c5
!n8_r7_c5 !n8_r7_c6
!n8_r7_c5 !n8_r7_c7
!n8_r7_c5 !n8_r7_c8
!n8_r7_c5 !n8_r7_c9
!n8_r7_c5 !n8_r8_c5
!n8_r7_c5 !n8_r9_c5
!n8_r7_c5 !n8_r8_c4
!n8_r7_c5 !n8_r8_c6
!n8_r7_c5 !n8_r9_c4
!n8_r7_c5 !n8_r9_c6
!n9_r7_c5 !n9_r7_c6
!n9_r7_c5 !n9_r7_c7
!n9_r7_c5 !n9_r7_c8
!n9_r7_c5 !n9_r7_c9
!n9_r7_c5 !n9_r8_c5
!n9_r7_c5 !n9_r9_c5
!n9_r7_c5 !n9_r8_c4
!n9_r7_c5 !n9_r8_c6
!n9_r7_c5 !n9_r9_c4
!n9_r7_c5 !n9_r9_c6
n1_r7_c5 n2_r7_c5 n3_r7_c5 n4_r7_c5 n5_r7_c5 n6_r7_c5 n7_r7_c5 n8_r7_c5 n9_r7_c5
!n1_r7_c6 !n2_r7_c6
!n1_r7_c6 !n3_r7_c6
!n1_r7_c6 !n4_r7_c6
!n1_r7_c6 !n5_r7_c6
!n1_r7_c6 !n6_r7_c6
!n1_r7_c6 !n7_r7_c6
!n1_r7_c6 !n8_r7_c6
!n1_r7_c6 !n9_r7_c6
!n1_r7_c6 !n1_r7_c7
!n1_r7_c6 !n1_r7_c8
!n1_r7_c6 !n1_r7_c9
!n1_r7_c6 !n1_r8_c6
!n1_r7_c6 !n1_r9_c6
!n1_r7_c6 !n1_r8_c4
!n1_r7_c6 !n1_r8_c5
!n1_r7_c6 !n1_r9_c4
!n1_r7_c6 !n1_r9_c5
!n2_r7_c6 !n3_r7_c6
!n2_r7_c6 !n4_r7_c6
!n2_r7_c6 !n5_r7_c6
!n2_r7_c6 !n6_r7_c6
!n2_r7_c6 !n7_r7_c6
!n2_r7_c6 !n8_r7_c6
!n2_r7_c6 !n9_r7_c6
!n2_r7_c6 !n2_r7_c7
!n2_r7_c6 !n2_r7_c8
!n2_r7_c6 !n2_r7_c9
!n2_r7_c6 !n2_r8_c6
!n2_r7_c6 !n2_r9_c6
!n2_r7_c6 !n2_r8_c4
!n2_r7_c6 !n2_r8_c5
!n2_r7_c6 !n2_r9_c4
!n2_r7_c6 !n2_r9_c5
!n3_r7_c6 !n4_r7_c6
!n3_r7_c6 !n5_r7_c6
!n3_r7_c6 !n6_r7_c6
!n3_r7_c6 !n7_r7_c6
!n3_r7_c6 !n8_r7_c6
!n3_r7_c6 !n9_r7_c6
!n3_r7_c6 !n3_r7_c7
!n3_r7_c6 !n3_r7_c8
!n3_r7_c6 !n3_r7_c9
!n3_r7_c6 !n3_r8_c6
!n3_r7_c6 !n3_r9_c6
!n3_r7_c6 !n3_r8_c4
!n3_r7_c6 !n3_r8_c5
!n3_r7_c6 !n3_r9_c4
!n3_r7_c6 !n3_r9_c5
!n4_r7_c6 !n5_r7_c6
!n4_r7_c6 !n6_r7_c6
!n4_r7_c6 !n7_r7_c6
!n4_r7_c6 !n8_r7_c6
!n4_r7_c6 !n9_r7_c6
!n4_r7_c6 !n4_r7_c7
!n4_r7_c6 !n4_r7_c8
!n4_r7_c6 !n4_r7_c9
!n4_r7_c6 !n4_r8_c6
!n4_r7_c6 !n4_r9_c6
!n4_r7_c6 !n4_r8_c4
!n4_r7_c6 !n4_r8_c5
!n4_r7_c6 !n4_r9_c4
!n4_r7_c6 !n4_r9_c5
!n5_r7_c6 !n6_r7_c6
!n5_r7_c6 !n7_r7_c6
!n5_r7_c6 !n8_r7_c6
!n5_r7_c6 !n9_r7_c6
!n5_r7_c6 !n5_r7_c7
!n5_r7_c6 !n5_r7_c8
!n5_r7_c6 !n5_r7_c9
!n5_r7_c6 !n5_r8_c6
!n5_r7_c6 !n5_r9_c6
!n5_r7_c6 !n5_r8_c4
!n5_r7_c6 !n5_r8_c5
!n5_r7_c6 !n5_r9_c4
!n5_r7_c6 !n5_r9_c5
!n6_r7_c6 !n7_r7_c6
!n6_r7_c6 !n8_r7_c6
!n6_r7_c6 !n9_r7_c6
!n6_r7_c6 !n6_r7_c7
!n6_r7_c6 !n6_r7_c8
!n6_r7_c6 !n6_r7_c9
!n6_r7_c6 !n6_r8_c6
!n6_r7_c6 !n6_r9_c6
!n6_r7_c6 !n6_r8_c4
!n6_r7_c6 !n6_r8_c5
!n6_r7_c6 !n6_r9_c4
!n6_r7_c6 !n6_r9_c5
!n7_r7_c6 !n8_r7_c6
!n7_r7_c6 !n9_r7_c6
!n7_r7_c6 !n7_r7_c7
!n7_r7_c6 !n7_r7_c8
!n7_r7_c6 !n7_r7_c9
!n7_r7_c6 !n7_r8_c6
!n7_r7_c6 !n7_r9_c6
!n7_r7_c6 !n7_r8_c4
!n7_r7_c6 !n7_r8_c5
!n7_r7_c6 !n7_r9_c4
!n7_r7_c6 !n7_r9_c5
!n8_r7_c6 !n9_r7_c6
!n8_r7_c6 !n8_r7_c7
!n8_r7_c6 !n8_r7_c8
!n8_r7_c6 !n8_r7_c9
!n8_r7_c6 !n8_r8_c6
!n8_r7_c6 !n8_r9_c6
!n8_r7_c6 !n8_r8_c4
!n8_r7_c6 !n8_r8_c5
!n8_r7_c6 !n8_r9_c4
!n8_r7_c6 !n8_r9_c5
!n9_r7_c6 !n9_r7_c7
!n9_r7_c6 !n9_r7_c8
!n9_r7_c6 !n9_r7_c9
!n9_r7_c6 !n9_r8_c6
!n9_r7_c6 !n9_r9_c6
!n9_r7_c6 !n9_r8_c4
!n9_r7_c6 !n9_r8_c5
!n9_r7_c6 !n9_r9_c4
!n9_r7_c6 !n9_r9_c5
n1_r7_c6 n2_r7_c6 n3_r7_c6 n4_r7_c6 n5_r7_c6 n6_r7_c6 n7_r7_c6 n8_r7_c6 n9_r7_c6
!n1_r7_c7 !n2_r7_c7
!n1_r7_c7 !n3_r7_c7
!n1_r7_c7 !n4_r7_c7
!n1_r7_c7 !n5_r7_c7
!n1_r7_c7 !n6_r7_c7
!n1_r7_c7 !n7_r7_c7
!n1_r7_c7 !n8_r7_c7
!n1_r7_c7 !n9_r7_c7
!n1_r7_c7 !n1_r7_c8
!n1_r7_c7 !n1_r7_c9
!n1_r7_c7 !n1_r8_c7
!n1_r7_c7 !n1_r9_c7
!n1_r7_c7 !n1_r8_c8
!n1_r7_c7 !n1_r8_c9
!n1_r7_c7 !n1_r9_c8
!n1_r7_c7 !n1_r9_c9
!n2_r7_c7 !n3_r7_c7
!n2_r7_c7 !n4_r7_c7
!n2_r7_c7 !n5_r7_c7
!n2_r7_c7 !n6_r7_c7
!n2_r7_c7 !n7_r7_c7
!n2_r7_c7 !n8_r7_c7
!n2_r7_c7 !n9_r7_c7
!n2_r7_c7 !n2_r7_c8
!n2_r7_c7 !n2_r7_c9
!n2_r7_c7 !n2_r8_c7
!n2_r7_c7 !n2_r9_c7
!n2_r7_c7 !n2_r8_c8
!n2_r7_c7 !n2_r8_c9
!n2_r7_c7 !n2_r9_c8
!n2_r7_c7 !n2_r9_c9
!n3_r7_c7 !n4_r7_c7
!n3_r7_c7 !n5_r7_c7
!n3_r7_c7 !n6_r7_c7
!n3_r7_c7 !n7_r7_c7
!n3_r7_c7 !n8_r7_c7
!n3_r7_c7 !n9_r7_c7
!n3_r7_c7 !n3_r7_c8
!n3_r7_c7 !n3_r7_c9
!n3_r7_c7 !n3_r8_c7
!n3_r7_c7 !n3_r9_c7
!n3_r7_c7 !n3_r8_c8
!n3_r7_c7 !n3_r8_c9
!n3_r7_c7 !n3_r9_c8
!n3_r7_c7 !n3_r9_c9
!n4_r7_c7 !n5_r7_c7
!n4_r7_c7 !n6_r7_c7
!n4_r7_c7 !n7_r7_c7
!n4_r7_c7 !n8_r7_c7
!n4_r7_c7 !n9_r7_c7
!n4_r7_c7 !n4_r7_c8
!n4_r7_c7 !n4_r7_c9
!n4_r7_c7 !n4_r8_c7
!n4_r7_c7 !n4_r9_c7
!n4_r7_c7 !n4_r8_c8
!n4_r7_c7 !n4_r8_c9
!n4_r7_c7 !n4_r9_c8
!n4_r7_c7 !n4_r9_c9
!n5_r7_c7 !n6_r7_c7
!n5_r7_c7 !n7_r7_c7
!n5_r7_c7 !n8_r7_c7
!n5_r7_c7 !n9_r7_c7
!n5_r7_c7 !n5_r7_c8
!n5_r7_c7 !n5_r7_c9
!n5_r7_c7 !n5_r8_c7
!n5_r7_c7 !n5_r9_c7
!n5_r7_c7 !n5_r8_c8
!n5_r7_c7 !n5_r8_c9
!n5_r7_c7 !n5_r9_c8
!n5_r7_c7 !n5_r9_c9
!n6_r7_c7 !n7_r7_c7
!n6_r7_c7 !n8_r7_c7
!n6_r7_c7 !n9_r7_c7
!n6_r7_c7 !n6_r7_c8
!n6_r7_c7 !n6_r7_c9
!n6_r7_c7 !n6_r8_c7
!n6_r7_c7 !n6_r9_c7
!n6_r7_c7 !n6_r8_c8
!n6_r7_c7 !n6_r8_c9
!n6_r7_c7 !n6_r9_c8
!n6_r7_c7 !n6_r9_c9
!n7_r7_c7 !n8_r7_c7
!n7_r7_c7 !n9_r7_c7
!n7_r7_c7 !n7_r7_c8
!n7_r7_c7 !n7_r7_c9
!n7_r7_c7 !n7_r8_c7
!n7_r7_c7 !n7_r9_c7
!n7_r7_c7 !n7_r8_c8
!n7_r7_c7 !n7_r8_c9
!n7_r7_c7 !n7_r9_c8
!n7_r7_c7 !n7_r9_c9
!n8_r7_c7 !n9_r7_c7
!n8_r7_c7 !n8_r7_c8
!n8_r7_c7 !n8_r7_c9
!n8_r7_c7 !n8_r8_c7
!n8_r7_c7 !n8_r9_c7
!n8_r7_c7 !n8_r8_c8
!n8_r7_c7 !n8_r8_c9
!n8_r7_c7 !n8_r9_c8
!n8_r7_c7 !n8_r9_c9
!n9_r7_c7 !n9_r7_c8
!n9_r7_c7 !n9_r7_c9
!n9_r7_c7 !n9_r8_c7
!n9_r7_c7 !n9_r9_c7
!n9_r7_c7 !n9_r8_c8
!n9_r7_c7 !n9_r8_c9
!n9_r7_c7 !n9_r9_c8
!n9_r7_c7 !n9_r9_c9
n1_r7_c7 n2_r7_c7 n3_r7_c7 n4_r7_c7 n5_r7_c7 n6_r7_c7 n7_r7_c7 n8_r7_c7 n9_r7_c7
!n1_r7_c8 !n2_r7_c8
!n1_r7_c8 !n3_r7_c8
!n1_r7_c8 !n4_r7_c8
!n1_r7_c8 !n5_r7_c8
!n1_r7_c8 !n6_r7_c8
!n1_r7_c8 !n7_r7_c8
!n1_r7_c8 !n8_r7_c8
!n1_r7_c8 !n9_r7_c8
!n1_r7_c8 !n1_r7_c9
!n1_r7_c8 !n1_r8_c8
!n1_r7_c8 !n1_r9_c8
!n1_r7_c8 !n1_r8_c7
!n1_r7_c8 !n1_r8_c9
!n1_r7_c8 !n1_r9_c7
!n1_r7_c8 !n1_r9_c9
!n2_r7_c8 !n3_r7_c8
!n2_r7_c8 !n4_r7_c8
!n2_r7_c8 !n5_r7_c8
!n2_r7_c8 !n6_r7_c8
!n2_r7_c8 !n7_r7_c8
!n2_r7_c8 !n8_r7_c8
!n2_r7_c8 !n9_r7_c8
!n2_r7_c8 !n2_r7_c9
!n2_r7_c8 !n2_r8_c8
!n2_r7_c8 !n2_r9_c8
!n2_r7_c8 !n2_r8_c7
!n2_r7_c8 !n2_r8_c9
!n2_r7_c8 !n2_r9_c7
!n2_r7_c8 !n2_r9_c9
!n3_r7_c8 !n4_r7_c8
!n3_r7_c8 !n5_r7_c8
!n3_r7_c8 !n6_r7_c8
!n3_r7_c8 !n7_r7_c8
!n3_r7_c8 !n8_r7_c8
!n3_r7_c8 !n9_r7_c8
!n3_r7_c8 !n3_r7_c9
!n3_r7_c8 !n3_r8_c8
!n3_r7_c8 !n3_r9_c8
!n3_r7_c8 !n3_r8_c7
!n3_r7_c8 !n3_r8_c9
!n3_r7_c8 !n3_r9_c7
!n3_r7_c8 !n3_r9_c9
!n4_r7_c8 !n5_r7_c8
!n4_r7_c8 !n6_r7_c8
!n4_r7_c8 !n7_r7_c8
!n4_r7_c8 !n8_r7_c8
!n4_r7_c8 !n9_r7_c8
!n4_r7_c8 !n4_r7_c9
!n4_r7_c8 !n4_r8_c8
!n4_r7_c8 !n4_r9_c8
!n4_r7_c8 !n4_r8_c7
!n4_r7_c8 !n4_r8_c9
!n4_r7_c8 !n4_r9_c7
!n4_r7_c8 !n4_r9_c9
!n5_r7_c8 !n6_r7_c8
!n5_r7_c8 !n7_r7_c8
!n5_r7_c8 !n8_r7_c8
!n5_r7_c8 !n9_r7_c8
!n5_r7_c8 !n5_r7_c9
!n5_r7_c8 !n5_r8_c8
!n5_r7_c8 !n5_r9_c8
!n5_r7_c8 !n5_r8_c7
!n5_r7_c8 !n5_r8_c9
!n5_r7_c8 !n5_r9_c7
!n5_r7_c8 !n5_r9_c9
!n6_r7_c8 !n7_r7_c8
!n6_r7_c8 !n8_r7_c8
!n6_r7_c8 !n9_r7_c8
!n6_r7_c8 !n6_r7_c9
!n6_r7_c8 !n6_r8_c8
!n6_r7_c8 !n6_r9_c8
!n6_r7_c8 !n6_r8_c7
!n6_r7_c8 !n6_r8_c9
!n6_r7_c8 !n6_r9_c7
!n6_r7_c8 !n6_r9_c9
!n7_r7_c8 !n8_r7_c8
!n7_r7_c8 !n9_r7_c8
!n7_r7_c8 !n7_r7_c9
!n7_r7_c8 !n7_r8_c8
!n7_r7_c8 !n7_r9_c8
!n7_r7_c8 !n7_r8_c7
!n7_r7_c8 !n7_r8_c9
!n7_r7_c8 !n7_r9_c7
!n7_r7_c8 !n7_r9_c9
!n8_r7_c8 !n9_r7_c8
!n8_r7_c8 !n8_r7_c9
!n8_r7_c8 !n8_r8_c8
!n8_r7_c8 !n8_r9_c8
!n8_r7_c8 !n8_r8_c7
!n8_r7_c8 !n8_r8_c9
!n8_r7_c8 !n8_r9_c7
!n8_r7_c8 !n8_r9_c9
!n9_r7_c8 !n9_r7_c9
!n9_r7_c8 !n9_r8_c8
!n9_r7_c8 !n9_r9_c8
!n9_r7_c8 !n9_r8_c7
!n9_r7_c8 !n9_r8_c9
!n9_r7_c8 !n9_r9_c7
!n9_r7_c8 !n9_r9_c9
n1_r7_c8 n2_r7_c8 n3_r7_c8 n4_r7_c8 n5_r7_c8 n6_r7_c8 n7_r7_c8 n8_r7_c8 n9_r7_c8
!n1_r7_c9 !n2_r7_c9
!n1_r7_c9 !n3_r7_c9
!n1_r7_c9 !n4_r7_c9
!n1_r7_c9 !n5_r7_c9
!n1_r7_c9 !n6_r7_c9
!n1_r7_c9 !n7_r7_c9
!n1_r7_c9 !n8_r7_c9
!n1_r7_c9 !n9_r7_c9
!n1_r7_c9 !n1_r8_c9
!n1_r7_c9 !n1_r9_c9
!n1_r7_c9 !n1_r8_c7
!n1_r7_c9 !n1_r8_c8
!n1_r7_c9 !n1_r9_c7
!n1_r7_c9 !n1_r9_c8
!n2_r7_c9 !n3_r7_c9
!n2_r7_c9 !n4_r7_c9
!n2_r7_c9 !n5_r7_c9
!n2_r7_c9 !n6_r7_c9
!n2_r7_c9 !n7_r7_c9
!n2_r7_c9 !n8_r7_c9
!n2_r7_c9 !n9_r7_c9
!n2_r7_c9 !n2_r8_c9
!n2_r7_c9 !n2_r9_c9
!n2_r7_c9 !n2_r8_c7
!n2_r7_c9 !n2_r8_c8
!n2_r7_c9 !n2_r9_c7
!n2_r7_c9 !n2_r9_c8
!n3_r7_c9 !n4_r7_c9
!n3_r7_c9 !n5_r7_c9
!n3_r7_c9 !n6_r7_c9
!n3_r7_c9 !n7_r7_c9
!n3_r7_c9 !n8_r7_c9
!n3_r7_c9 !n9_r7_c9
!n3_r7_c9 !n3_r8_c9
!n3_r7_c9 !n3_r9_c9
!n3_r7_c9 !n3_r8_c7
!n3_r7_c9 !n3_r8_c8
!n3_r7_c9 !n3_r9_c7
!n3_r7_c9 !n3_r9_c8
!n4_r7_c9 !n5_r7_c9
!n4_r7_c9 !n6_r7_c9
!n4_r7_c9 !n7_r7_c9
!n4_r7_c9 !n8_r7_c9
!n4_r7_c9 !n9_r7_c9
!n4_r7_c9 !n4_r8_c9
!n4_r7_c9 !n4_r9_c9
!n4_r7_c9 !n4_r8_c7
!n4_r7_c9 !n4_r8_c8
!n4_r7_c9 !n4_r9_c7
!n4_r7_c9 !n4_r9_c8
!n5_r7_c9 !n6_r7_c9
!n5_r7_c9 !n7_r7_c9
!n5_r7_c9 !n8_r7_c9
!n5_r7_c9 !n9_r7_c9
!n5_r7_c9 !n5_r8_c9
!n5_r7_c9 !n5_r9_c9
!n5_r7_c9 !n5_r8_c7
!n5_r7_c9 !n5_r8_c8
!n5_r7_c9 !n5_r9_c7
!n5_r7_c9 !n5_r9_c8
!n6_r7_c9 !n7_r7_c9
!n6_r7_c9 !n8_r7_c9
!n6_r7_c9 !n9_r7_c9
!n6_r7_c9 !n6_r8_c9
!n6_r7_c9 !n6_r9_c9
!n6_r7_c9 !n6_r8_c7
!n6_r7_c9 !n6_r8_c8
!n6_r7_c9 !n6_r9_c7
!n6_r7_c9 !n6_r9_c8
!n7_r7_c9 !n8_r7_c9
!n7_r7_c9 !n9_r7_c9
!n7_r7_c9 !n7_r8_c9
!n7_r7_c9 !n7_r9_c9
!n7_r7_c9 !n7_r8_c7
!n7_r7_c9 !n7_r8_c8
!n7_r7_c9 !n7_r9_c7
!n7_r7_c9 !n7_r9_c8
!n8_r7_c9 !n9_r7_c9
!n8_r7_c9 !n8_r8_c9
!n8_r7_c9 !n8_r9_c9
!n8_r7_c9 !n8_r8_c7
!n8_r7_c9 !n8_r8_c8
!n8_r7_c9 !n8_r9_c7
!n8_r7_c9 !n8_r9_c8
!n9_r7_c9 !n9_r8_c9
!n9_r7_c9 !n9_r9_c9
!n9_r7_c9 !n9_r8_c7
!n9_r7_c9 !n9_r8_c8
!n9_r7_c9 !n9_r9_c7
!n9_r7_c9 !n9_r9_c8
n1_r7_c9 n2_r7_c9 n3_r7_c9 n4_r7_c9 n5_r7_c9 n6_r7_c9 n7_r7_c9 n8_r7_c9 n9_r7_c9
!n1_r8_c1 !n2_r8_c1
!n1_r8_c1 !n3_r8_c1
!n1_r8_c1 !n4_r8_c1
!n1_r8_c1 !n5_r8_c1
!n1_r8_c1 !n6_r8_c1
!n1_r8_c1 !n7_r8_c1
!n1_r8_c1 !n8_r8_c1
!n1_r8_c1 !n9_r8_c1
!n1_r8_c1 !n1_r8_c2
!n1_r8_c1 !n1_r8_c3
!n1_r8_c1 !n1_r8_c4
!n1_r8_c1 !n1_r8_c5
!n1_r8_c1 !n1_r8_c6
!n1_r8_c1 !n1_r8_c7
!n1_r8_c1 !n1_r8_c8
!n1_r8_c1 !n1_r8_c9
!n1_r8_c1 !n1_r9_c1
!n1_r8_c1 !n1_r7_c2
!n1_r8_c1 !n1_r7_c3
!n1_r8_c1 !n1_r9_c2
!n1_r8_c1 !n1_r9_c3
!n2_r8_c1 !n3_r8_c1
!n2_r8_c1 !n4_r8_c1
!n2_r8_c1 !n5_r8_c1
!n2_r8_c1 !n6_r8_c1
!n2_r8_c1 !n7_r8_c1
!n2_r8_c1 !n8_r8_c1
!n2_r8_c1 !n9_r8_c1
!n2_r8_c1 !n2_r8_c2
!n2_r8_c1 !n2_r8_c3
!n2_r8_c1 !n2_r8_c4
!n2_r8_c1 !n2_r8_c5
!n2_r8_c1 !n2_r8_c6
!n2_r8_c1 !n2_r8_c7
!n2_r8_c1 !n2_r8_c8
!n2_r8_c1 !n2_r8_c9
!n2_r8_c1 !n2_r9_c1
!n2_r8_c1 !n2_r7_c2
!n2_r8_c1 !n2_r7_c3
!n2_r8_c1 !n2_r9_c2
!n2_r8_c1 !n2_r9_c3
!n3_r8_c1 !n4_r8_c1
!n3_r8_c1 !n5_r8_c1
!n3_r8_c1 !n6_r8_c1
!n3_r8_c1 !n7_r8_c1
!n3_r8_c1 !n8_r8_c1
!n3_r8_c1 !n9_r8_c1
!n3_r8_c1 !n3_r8_c2
!n3_r8_c1 !n3_r8_c3
!n3_r8_c1 !n3_r8_c4
!n3_r8_c1 !n3_r8_c5
!n3_r8_c1 !n3_r8_c6
!n3_r8_c1 !n3_r8_c7
!n3_r8_c1 !n3_r8_c8
!n3_r8_c1 !n3_r8_c9
!n3_r8_c1 !n3_r9_c1
!n3_r8_c1 !n3_r7_c2
!n3_r8_c1 !n3_r7_c3
!n3_r8_c1 !n3_r9_c2
!n3_r8_c1 !n3_r9_c3
!n4_r8_c1 !n5_r8_c1
!n4_r8_c1 !n6_r8_c1
!n4_r8_c1 !n7_r8_c1
!n4_r8_c1 !n8_r8_c1
!n4_r8_c1 !n9_r8_c1
!n4_r8_c1 !n4_r8_c2
!n4_r8_c1 !n4_r8_c3
!n4_r8_c1 !n4_r8_c4
!n4_r8_c1 !n4_r8_c5
!n4_r8_c1 !n4_r8_c6
!n4_r8_c1 !n4_r8_c7
!n4_r8_c1 !n4_r8_c8
!n4_r8_c1 !n4_r8_c9
!n4_r8_c1 !n4_r9_c1
!n4_r8_c1 !n4_r7_c2
!n4_r8_c1 !n4_r7_c3
!n4_r8_c1 !n4_r9_c2
!n4_r8_c1 !n4_r9_c3
!n5_r8_c1 !n6_r8_c1
!n5_r8_c1 !n7_r8_c1
!n5_r8_c1 !n8_r8_c1
!n5_r8_c1 !n9_r8_c1
!n5_r8_c1 !n5_r8_c2
!n5_r8_c1 !n5_r8_c3
!n5_r8_c1 !n5_r8_c4
!n5_r8_c1 !n5_r8_c5
!n5_r8_c1 !n5_r8_c6
!n5_r8_c1 !n5_r8_c7
!n5_r8_c1 !n5_r8_c8
!n5_r8_c1 !n5_r8_c9
!n5_r8_c1 !n5_r9_c1
!n5_r8_c1 !n5_r7_c2
!n5_r8_c1 !n5_r7_c3
!n5_r8_c1 !n5_r9_c2
!n5_r8_c1 !n5_r9_c3
!n6_r8_c1 !n7_r8_c1
!n6_r8_c1 !n8_r8_c1
!n6_r8_c1 !n9_r8_c1
!n6_r8_c1 !n6_r8_c2
!n6_r8_c1 !n6_r8_c3
!n6_r8_c1 !n6_r8_c4
!n6_r8_c1 !n6_r8_c5
!n6_r8_c1 !n6_r8_c6
!n6_r8_c1 !n6_r8_c7
!n6_r8_c1 !n6_r8_c8
!n6_r8_c1 !n6_r8_c9
!n6_r8_c1 !n6_r9_c1
!n6_r8_c1 !n6_r7_c2
!n6_r8_c1 !n6_r7_c3
!n6_r8_c1 !n6_r9_c2
!n6_r8_c1 !n6_r9_c3
!n7_r8_c1 !n8_r8_c1
!n7_r8_c1 !n9_r8_c1
!n7_r8_c1 !n7_r8_c2
!n7_r8_c1 !n7_r8_c3
!n7_r8_c1 !n7_r8_c4
!n7_r8_c1 !n7_r8_c5
!n7_r8_c1 !n7_r8_c6
!n7_r8_c1 !n7_r8_c7
!n7_r8_c1 !n7_r8_c8
!n7_r8_c1 !n7_r8_c9
!n7_r8_c1 !n7_r9_c1
!n7_r8_c1 !n7_r7_c2
!n7_r8_c1 !n7_r7_c3
!n7_r8_c1 !n7_r9_c2
!n7_r8_c1 !n7_r9_c3
!n8_r8_c1 !n9_r8_c1
!n8_r8_c1 !n8_r8_c2
!n8_r8_c1 !n8_r8_c3
!n8_r8_c1 !n8_r8_c4
!n8_r8_c1 !n8_r8_c5
!n8_r8_c1 !n8_r8_c6
!n8_r8_c1 !n8_r8_c7
!n8_r8_c1 !n8_r8_c8
!n8_r8_c1 !n8_r8_c9
!n8_r8_c1 !n8_r9_c1
!n8_r8_c1 !n8_r7_c2
!n8_r8_c1 !n8_r7_c3
!n8_r8_c1 !n8_r9_c2
!n8_r8_c1 !n8_r9_c3
!n9_r8_c1 !n9_r8_c2
!n9_r8_c1 !n9_r8_c3
!n9_r8_c1 !n9_r8_c4
!n9_r8_c1 !n9_r8_c5
!n9_r8_c1 !n9_r8_c6
!n9_r8_c1 !n9_r8_c7
!n9_r8_c1 !n9_r8_c8
!n9_r8_c1 !n9_r8_c9
!n9_r8_c1 !n9_r9_c1
!n9_r8_c1 !n9_r7_c2
!n9_r8_c1 !n9_r7_c3
!n9_r8_c1 !n9_r9_c2
!n9_r8_c1 !n9_r9_c3
n1_r8_c1 n2_r8_c1 n3_r8_c1 n4_r8_c1 n5_r8_c1 n6_r8_c1 n7_r8_c1 n8_r8_c1 n9_r8_c1
!n1_r8_c2 !n2_r8_c2
!n1_r8_c2 !n3_r8_c2
!n1_r8_c2 !n4_r8_c2
!n1_r8_c2 !n5_r8_c2
!n1_r8_c2 !n6_r8_c2
!n1_r8_c2 !n7_r8_c2
!n1_r8_c2 !n8_r8_c2
!n1_r8_c2 !n9_r8_c2
!n1_r8_c2 !n1_r8_c3
!n1_r8_c2 !n1_r8_c4
!n1_r8_c2 !n1_r8_c5
!n1_r8_c2 !n1_r8_c6
!n1_r8_c2 !n1_r8_c7
!n1_r8_c2 !n1_r8_c8
!n1_r8_c2 !n1_r8_c9
!n1_r8_c2 !n1_r9_c2
!n1_r8_c2 !n1_r7_c1
!n1_r8_c2 !n1_r7_c3
!n1_r8_c2 !n1_r9_c1
!n1_r8_c2 !n1_r9_c3
!n2_r8_c2 !n3_r8_c2
!n2_r8_c2 !n4_r8_c2
!n2_r8_c2 !n5_r8_c2
!n2_r8_c2 !n6_r8_c2
!n2_r8_c2 !n7_r8_c2
!n2_r8_c2 !n8_r8_c2
!n2_r8_c2 !n9_r8_c2
!n2_r8_c2 !n2_r8_c3
!n2_r8_c2 !n2_r8_c4
!n2_r8_c2 !n2_r8_c5
!n2_r8_c2 !n2_r8_c6
!n2_r8_c2 !n2_r8_c7
!n2_r8_c2 !n2_r8_c8
!n2_r8_c2 !n2_r8_c9
!n2_r8_c2 !n2_r9_c2
!n2_r8_c2 !n2_r7_c1
!n2_r8_c2 !n2_r7_c3
!n2_r8_c2 !n2_r9_c1
!n2_r8_c2 !n2_r9_c3
!n3_r8_c2 !n4_r8_c2
!n3_r8_c2 !n5_r8_c2
!n3_r8_c2 !n6_r8_c2
!n3_r8_c2 !n7_r8_c2
!n3_r8_c2 !n8_r8_c2
!n3_r8_c2 !n9_r8_c2
!n3_r8_c2 !n3_r8_c3
!n3_r8_c2 !n3_r8_c4
!n3_r8_c2 !n3_r8_c5
!n3_r8_c2 !n3_r8_c6
!n3_r8_c2 !n3_r8_c7
!n3_r8_c2 !n3_r8_c8
!n3_r8_c2 !n3_r8_c9
!n3_r8_c2 !n3_r9_c2
!n3_r8_c2 !n3_r7_c1
!n3_r8_c2 !n3_r7_c3
!n3_r8_c2 !n3_r9_c1
!n3_r8_c2 !n3_r9_c3
!n4_r8_c2 !n5_r8_c2
!n4_r8_c2 !n6_r8_c2
!n4_r8_c2 !n7_r8_c2
!n4_r8_c2 !n8_r8_c2
!n4_r8_c2 !n9_r8_c2
!n4_r8_c2 !n4_r8_c3
!n4_r8_c2 !n4_r8_c4
!n4_r8_c2 !n4_r8_c5
!n4_r8_c2 !n4_r8_c6
!n4_r8_c2 !n4_r8_c7
!n4_r8_c2 !n4_r8_c8
!n4_r8_c2 !n4_r8_c9
!n4_r8_c2 !n4_r9_c2
!n4_r8_c2 !n4_r7_c1
!n4_r8_c2 !n4_r7_c3
!n4_r8_c2 !n4_r9_c1
!n4_r8_c2 !n4_r9_c3
!n5_r8_c2 !n6_r8_c2
!n5_r8_c2 !n7_r8_c2
!n5_r8_c2 !n8_r8_c2
!n5_r8_c2 !n9_r8_c2
!n5_r8_c2 !n5_r8_c3
!n5_r8_c2 !n5_r8_c4
!n5_r8_c2 !n5_r8_c5
!n5_r8_c2 !n5_r8_c6
!n5_r8_c2 !n5_r8_c7
!n5_r8_c2 !n5_r8_c8
!n5_r8_c2 !n5_r8_c9
!n5_r8_c2 !n5_r9_c2
!n5_r8_c2 !n5_r7_c1
!n5_r8_c2 !n5_r7_c3
!n5_r8_c2 !n5_r9_c1
!n5_r8_c2 !n5_r9_c3
!n6_r8_c2 !n7_r8_c2
!n6_r8_c2 !n8_r8_c2
!n6_r8_c2 !n9_r8_c2
!n6_r8_c2 !n6_r8_c3
!n6_r8_c2 !n6_r8_c4
!n6_r8_c2 !n6_r8_c5
!n6_r8_c2 !n6_r8_c6
!n6_r8_c2 !n6_r8_c7
!n6_r8_c2 !n6_r8_c8
!n6_r8_c2 !n6_r8_c9
!n6_r8_c2 !n6_r9_c2
!n6_r8_c2 !n6_r7_c1
!n6_r8_c2 !n6_r7_c3
!n6_r8_c2 !n6_r9_c1
!n6_r8_c2 !n6_r9_c3
!n7_r8_c2 !n8_r8_c2
!n7_r8_c2 !n9_r8_c2
!n7_r8_c2 !n7_r8_c3
!n7_r8_c2 !n7_r8_c4
!n7_r8_c2 !n7_r8_c5
!n7_r8_c2 !n7_r8_c6
!n7_r8_c2 !n7_r8_c7
!n7_r8_c2 !n7_r8_c8
!n7_r8_c2 !n7_r8_c9
!n7_r8_c2 !n7_r9_c2
!n7_r8_c2 !n7_r7_c1
!n7_r8_c2 !n7_r7_c3
!n7_r8_c2 !n7_r9_c1
!n7_r8_c2 !n7_r9_c3
!n8_r8_c2 !n9_r8_c2
!n8_r8_c2 !n8_r8_c3
!n8_r8_c2 !n8_r8_c4
!n8_r8_c2 !n8_r8_c5
!n8_r8_c2 !n8_r8_c6
!n8_r8_c2 !n8_r8_c7
!n8_r8_c2 !n8_r8_c8
!n8_r8_c2 !n8_r8_c9
!n8_r8_c2 !n8_r9_c2
!n8_r8_c2 !n8_r7_c1
!n8_r8_c2 !n8_r7_c3
!n8_r8_c2 !n8_r9_c1
!n8_r8_c2 !n8_r9_c3
!n9_r8_c2 !n9_r8_c3
!n9_r8_c2 !n9_r8_c4
!n9_r8_c2 !n9_r8_c5
!n9_r8_c2 !n9_r8_c6
!n9_r8_c2 !n9_r8_c7
!n9_r8_c2 !n9_r8_c8
!n9_r8_c2 !n9_r8_c9
!n9_r8_c2 !n9_r9_c2
!n9_r8_c2 !n9_r7_c1
!n9_r8_c2 !n9_r7_c3
!n9_r8_c2 !n9_r9_c1
!n9_r8_c2 !n9_r9_c3
n1_r8_c2 n2_r8_c2 n3_r8_c2 n4_r8_c2 n5_r8_c2 n6_r8_c2 n7_r8_c2 n8_r8_c2 n9_r8_c2
!n1_r8_c3 !n2_r8_c3
!n1_r8_c3 !n3_r8_c3
!n1_r8_c3 !n4_r8_c3
!n1_r8_c3 !n5_r8_c3
!n1_r8_c3 !n6_r8_c3
!n1_r8_c3 !n7_r8_c3
!n1_r8_c3 !n8_r8_c3
!n1_r8_c3 !n9_r8_c3
!n1_r8_c3 !n1_r8_c4
!n1_r8_c3 !n1_r8_c5
!n1_r8_c3 !n1_r8_c6
!n1_r8_c3 !n1_r8_c7
!n1_r8_c3 !n1_r8_c8
!n1_r8_c3 !n1_r8_c9
!n1_r8_c3 !n1_r9_c3
!n1_r8_c3 !n1_r7_c1
!n1_r8_c3 !n1_r7_c2
!n1_r8_c3 !n1_r9_c1
!n1_r8_c3 !n1_r9_c2
!n2_r8_c3 !n3_r8_c3
!n2_r8_c3 !n4_r8_c3
!n2_r8_c3 !n5_r8_c3
!n2_r8_c3 !n6_r8_c3
!n2_r8_c3 !n7_r8_c3
!n2_r8_c3 !n8_r8_c3
!n2_r8_c3 !n9_r8_c3
!n2_r8_c3 !n2_r8_c4
!n2_r8_c3 !n2_r8_c5
!n2_r8_c3 !n2_r8_c6
!n2_r8_c3 !n2_r8_c7
!n2_r8_c3 !n2_r8_c8
!n2_r8_c3 !n2_r8_c9
!n2_r8_c3 !n2_r9_c3
!n2_r8_c3 !n2_r7_c1
!n2_r8_c3 !n2_r7_c2
!n2_r8_c3 !n2_r9_c1
!n2_r8_c3 !n2_r9_c2
!n3_r8_c3 !n4_r8_c3
!n3_r8_c3 !n5_r8_c3
!n3_r8_c3 !n6_r8_c3
!n3_r8_c3 !n7_r8_c3
!n3_r8_c3 !n8_r8_c3
!n3_r8_c3 !n9_r8_c3
!n3_r8_c3 !n3_r8_c4
!n3_r8_c3 !n3_r8_c5
!n3_r8_c3 !n3_r8_c6
!n3_r8_c3 !n3_r8_c7
!n3_r8_c3 !n3_r8_c8
!n3_r8_c3 !n3_r8_c9
!n3_r8_c3 !n3_r9_c3
!n3_r8_c3 !n3_r7_c1
!n3_r8_c3 !n3_r7_c2
!n3_r8_c3 !n3_r9_c1
!n3_r8_c3 !n3_r9_c2
!n4_r8_c3 !n5_r8_c3
!n4_r8_c3 !n6_r8_c3
!n4_r8_c3 !n7_r8_c3
!n4_r8_c3 !n8_r8_c3
!n4_r8_c3 !n9_r8_c3
!n4_r8_c3 !n4_r8_c4
!n4_r8_c3 !n4_r8_c5
!n4_r8_c3 !n4_r8_c6
!n4_r8_c3 !n4_r8_c7
!n4_r8_c3 !n4_r8_c8
!n4_r8_c3 !n4_r8_c9
!n4_r8_c3 !n4_r9_c3
!n4_r8_c3 !n4_r7_c1
!n4_r8_c3 !n4_r7_c2
!n4_r8_c3 !n4_r9_c1
!n4_r8_c3 !n4_r9_c2
!n5_r8_c3 !n6_r8_c3
!n5_r8_c3 !n7_r8_c3
!n5_r8_c3 !n8_r8_c3
!n5_r8_c3 !n9_r8_c3
!n5_r8_c3 !n5_r8_c4
!n5_r8_c3 !n5_r8_c5
!n5_r8_c3 !n5_r8_c6
!n5_r8_c3 !n5_r8_c7
!n5_r8_c3 !n5_r8_c8
!n5_r8_c3 !n5_r8_c9
!n5_r8_c3 !n5_r9_c3
!n5_r8_c3 !n5_r7_c1
!n5_r8_c3 !n5_r7_c2
!n5_r8_c3 !n5_r9_c1
!n5_r8_c3 !n5_r9_c2
!n6_r8_c3 !n7_r8_c3
!n6_r8_c3 !n8_r8_c3
!n6_r8_c3 !n9_r8_c3
!n6_r8_c3 !n6_r8_c4
!n6_r8_c3 !n6_r8_c5
!n6_r8_c3 !n6_r8_c6
!n6_r8_c3 !n6_r8_c7
!n6_r8_c3 !n6_r8_c8
!n6_r8_c3 !n6_r8_c9
!n6_r8_c3 !n6_r9_c3
!n6_r8_c3 !n6_r7_c1
!n6_r8_c3 !n6_r7_c2
!n6_r8_c3 !n6_r9_c1
!n6_r8_c3 !n6_r9_c2
!n7_r8_c3 !n8_r8_c3
!n7_r8_c3 !n9_r8_c3
!n7_r8_c3 !n7_r8_c4
!n7_r8_c3 !n7_r8_c5
!n7_r8_c3 !n7_r8_c6
!n7_r8_c3 !n7_r8_c7
!n7_r8_c3 !n7_r8_c8
!n7_r8_c3 !n7_r8_c9
!n7_r8_c3 !n7_r9_c3
!n7_r8_c3 !n7_r7_c1
!n7_r8_c3 !n7_r7_c2
!n7_r8_c3 !n7_r9_c1
!n7_r8_c3 !n7_r9_c2
!n8_r8_c3 !n9_r8_c3
!n8_r8_c3 !n8_r8_c4
!n8_r8_c3 !n8_r8_c5
!n8_r8_c3 !n8_r8_c6
!n8_r8_c3 !n8_r8_c7
!n8_r8_c3 !n8_r8_c8
!n8_r8_c3 !n8_r8_c9
!n8_r8_c3 !n8_r9_c3
!n8_r8_c3 !n8_r7_c1
!n8_r8_c3 !n8_r7_c2
!n8_r8_c3 !n8_r9_c1
!n8_r8_c3 !n8_r9_c2
!n9_r8_c3 !n9_r8_c4
!n9_r8_c3 !n9_r8_c5
!n9_r8_c3 !n9_r8_c6
!n9_r8_c3 !n9_r8_c7
!n9_r8_c3 !n9_r8_c8
!n9_r8_c3 !n9_r8_c9
!n9_r8_c3 !n9_r9_c3
!n9_r8_c3 !n9_r7_c1
!n9_r8_c3 !n9_r7_c2
!n9_r8_c3 !n9_r9_c1
!n9_r8_c3 !n9_r9_c2
n1_r8_c3 n2_r8_c3 n3_r8_c3 n4_r8_c3 n5_r8_c3 n6_r8_c3 n7_r8_c3 n8_r8_c3 n9_r8_c3
!n1_r8_c4 !n2_r8_c4
!n1_r8_c4 !n3_r8_c4
!n1_r8_c4 !n4_r8_c4
!n1_r8_c4 !n5_r8_c4
!n1_r8_c4 !n6_r8_c4
!n1_r8_c4 !n7_r8_c4
!n1_r8_c4 !n8_r8_c4
!n1_r8_c4 !n9_r8_c4
!n1_r8_c4 !n1_r8_c5
!n1_r8_c4 !n1_r8_c6
!n1_r8_c4 !n1_r8_c7
!n1_r8_c4 !n1_r8_c8
!n1_r8_c4 !n1_r8_c9
!n1_r8_c4 !n1_r9_c4
!n1_r8_c4 !n1_r7_c5
!n1_r8_c4 !n1_r7_c6
!n1_r8_c4 !n1_r9_c5
!n1_r8_c4 !n1_r9_c6
!n2_r8_c4 !n3_r8_c4
!n2_r8_c4 !n4_r8_c4
!n2_r8_c4 !n5_r8_c4
!n2_r8_c4 !n6_r8_c4
!n2_r8_c4 !n7_r8_c4
!n2_r8_c4 !n8_r8_c4
!n2_r8_c4 !n9_r8_c4
!n2_r8_c4 !n2_r8_c5
!n2_r8_c4 !n2_r8_c6
!n2_r8_c4 !n2_r8_c7
!n2_r8_c4 !n2_r8_c8
!n2_r8_c4 !n2_r8_c9
!n2_r8_c4 !n2_r9_c4
!n2_r8_c4 !n2_r7_c5
!n2_r8_c4 !n2_r7_c6
!n2_r8_c4 !n2_r9_c5
!n2_r8_c4 !n2_r9_c6
!n3_r8_c4 !n4_r8_c4
!n3_r8_c4 !n5_r8_c4
!n3_r8_c4 !n6_r8_c4
!n3_r8_c4 !n7_r8_c4
!n3_r8_c4 !n8_r8_c4
!n3_r8_c4 !n9_r8_c4
!n3_r8_c4 !n3_r8_c5
!n3_r8_c4 !n3_r8_c6
!n3_r8_c4 !n3_r8_c7
!n3_r8_c4 !n3_r8_c8
!n3_r8_c4 !n3_r8_c9
!n3_r8_c4 !n3_r9_c4
!n3_r8_c4 !n3_r7_c5
!n3_r8_c4 !n3_r7_c6
!n3_r8_c4 !n3_r9_c5
!n3_r8_c4 !n3_r9_c6
!n4_r8_c4 !n5_r8_c4
!n4_r8_c4 !n6_r8_c4
!n4_r8_c4 !n7_r8_c4
!n4_r8_c4 !n8_r8_c4
!n4_r8_c4 !n9_r8_c4
!n4_r8_c4 !n4_r8_c5
!n4_r8_c4 !n4_r8_c6
!n4_r8_c4 !n4_r8_c7
!n4_r8_c4 !n4_r8_c8
!n4_r8_c4 !n4_r8_c9
!n4_r8_c4 !n4_r9_c4
!n4_r8_c4 !n4_r7_c5
!n4_r8_c4 !n4_r7_c6
!n4_r8_c4 !n4_r9_c5
!n4_r8_c4 !n4_r9_c6
!n5_r8_c4 !n6_r8_c4
!n5_r8_c4 !n7_r8_c4
!n5_r8_c4 !n8_r8_c4
!n5_r8_c4 !n9_r8_c4
!n5_r8_c4 !n5_r8_c5
!n5_r8_c4 !n5_r8_c6
!n5_r8_c4 !n5_r8_c7
!n5_r8_c4 !n5_r8_c8
!n5_r8_c4 !n5_r8_c9
!n5_r8_c4 !n5_r9_c4
!n5_r8_c4 !n5_r7_c5
!n5_r8_c4 !n5_r7_c6
!n5_r8_c4 !n5_r9_c5
!n5_r8_c4 !n5_r9_c6
!n6_r8_c4 !n7_r8_c4
!n6_r8_c4 !n8_r8_c4
!n6_r8_c4 !n9_r8_c4
!n6_r8_c4 !n6_r8_c5
!n6_r8_c4 !n6_r8_c6
!n6_r8_c4 !n6_r8_c7
!n6_r8_c4 !n6_r8_c8
!n6_r8_c4 !n6_r8_c9
!n6_r8_c4 !n6_r9_c4
!n6_r8_c4 !n6_r7_c5
!n6_r8_c4 !n6_r7_c6
!n6_r8_c4 !n6_r9_c5
!n6_r8_c4 !n6_r9_c6
!n7_r8_c4 !n8_r8_c4
!n7_r8_c4 !n9_r8_c4
!n7_r8_c4 !n7_r8_c5
!n7_r8_c4 !n7_r8_c6
!n7_r8_c4 !n7_r8_c7
!n7_r8_c4 !n7_r8_c8
!n7_r8_c4 !n7_r8_c9
!n7_r8_c4 !n7_r9_c4
!n7_r8_c4 !n7_r7_c5
!n7_r8_c4 !n7_r7_c6
!n7_r8_c4 !n7_r9_c5
!n7_r8_c4 !n7_r9_c6
!n8_r8_c4 !n9_r8_c4
!n8_r8_c4 !n8_r8_c5
!n8_r8_c4 !n8_r8_c6
!n8_r8_c4 !n8_r8_c7
!n8_r8_c4 !n8_r8_c8
!n8_r8_c4 !n8_r8_c9
!n8_r8_c4 !n8_r9_c4
!n8_r8_c4 !n8_r7_c5
!n8_r8_c4 !n8_r7_c6
!n8_r8_c4 !n8_r9_c5
!n8_r8_c4 !n8_r9_c6
!n9_r8_c4 !n9_r8_c5
!n9_r8_c4 !n9_r8_c6
!n9_r8_c4 !n9_r8_c7
!n9_r8_c4 !n9_r8_c8
!n9_r8_c4 !n9_r8_c9
!n9_r8_c4 !n9_r9_c4
!n9_r8_c4 !n9_r7_c5
!n9_r8_c4 !n9_r7_c6
!n9_r8_c4 !n9_r9_c5
!n9_r8_c4 !n9_r9_c6
n1_r8_c4 n2_r8_c4 n3_r8_c4 n4_r8_c4 n5_r8_c4 n6_r8_c4 n7_r8_c4 n8_r8_c4 n9_r8_c4
!n1_r8_c5 !n2_r8_c5
!n1_r8_c5 !n3_r8_c5
!n1_r8_c5 !n4_r8_c5
!n1_r8_c5 !n5_r8_c5
!n1_r8_c5 !n6_r8_c5
!n1_r8_c5 !n7_r8_c5
!n1_r8_c5 !n8_r8_c5
!n1_r8_c5 !n9_r8_c5
!n1_r8_c5 !n1_r8_c6
!n1_r8_c5 !n1_r8_c7
!n1_r8_c5 !n1_r8_c8
!n1_r8_c5 !n1_r8_c9
!n1_r8_c5 !n1_r9_c5
!n1_r8_c5 !n1_r7_c4
!n1_r8_c5 !n1_r7_c6
!n1_r8_c5 !n1_r9_c4
!n1_r8_c5 !n1_r9_c6
!n2_r8_c5 !n3_r8_c5
!n2_r8_c5 !n4_r8_c5
!n2_r8_c5 !n5_r8_c5
!n2_r8_c5 !n6_r8_c5
!n2_r8_c5 !n7_r8_c5
!n2_r8_c5 !n8_r8_c5
!n2_r8_c5 !n9_r8_c5
!n2_r8_c5 !n2_r8_c6
!n2_r8_c5 !n2_r8_c7
!n2_r8_c5 !n2_r8_c8
!n2_r8_c5 !n2_r8_c9
!n2_r8_c5 !n2_r9_c5
!n2_r8_c5 !n2_r7_c4
!n2_r8_c5 !n2_r7_c6
!n2_r8_c5 !n2_r9_c4
!n2_r8_c5 !n2_r9_c6
!n3_r8_c5 !n4_r8_c5
!n3_r8_c5 !n5_r8_c5
!n3_r8_c5 !n6_r8_c5
!n3_r8_c5 !n7_r8_c5
!n3_r8_c5 !n8_r8_c5
!n3_r8_c5 !n9_r8_c5
!n3_r8_c5 !n3_r8_c6
!n3_r8_c5 !n3_r8_c7
!n3_r8_c5 !n3_r8_c8
!n3_r8_c5 !n3_r8_c9
!n3_r8_c5 !n3_r9_c5
!n3_r8_c5 !n3_r7_c4
!n3_r8_c5 !n3_r7_c6
!n3_r8_c5 !n3_r9_c4
!n3_r8_c5 !n3_r9_c6
!n4_r8_c5 !n5_r8_c5
!n4_r8_c5 !n6_r8_c5
!n4_r8_c5 !n7_r8_c5
!n4_r8_c5 !n8_r8_c5
!n4_r8_c5 !n9_r8_c5
!n4_r8_c5 !n4_r8_c6
!n4_r8_c5 !n4_r8_c7
!n4_r8_c5 !n4_r8_c8
!n4_r8_c5 !n4_r8_c9
!n4_r8_c5 !n4_r9_c5
!n4_r8_c5 !n4_r7_c4
!n4_r8_c5 !n4_r7_c6
!n4_r8_c5 !n4_r9_c4
!n4_r8_c5 !n4_r9_c6
!n5_r8_c5 !n6_r8_c5
!n5_r8_c5 !n7_r8_c5
!n5_r8_c5 !n8_r8_c5
!n5_r8_c5 !n9_r8_c5
!n5_r8_c5 !n5_r8_c6
!n5_r8_c5 !n5_r8_c7
!n5_r8_c5 !n5_r8_c8
!n5_r8_c5 !n5_r8_c9
!n5_r8_c5 !n5_r9_c5
!n5_r8_c5 !n5_r7_c4
!n5_r8_c5 !n5_r7_c6
!n5_r8_c5 !n5_r9_c4
!n5_r8_c5 !n5_r9_c6
!n6_r8_c5 !n7_r8_c5
!n6_r8_c5 !n8_r8_c5
!n6_r8_c5 !n9_r8_c5
!n6_r8_c5 !n6_r8_c6
!n6_r8_c5 !n6_r8_c7
!n6_r8_c5 !n6_r8_c8
!n6_r8_c5 !n6_r8_c9
!n6_r8_c5 !n6_r9_c5
!n6_r8_c5 !n6_r7_c4
!n6_r8_c5 !n6_r7_c6
!n6_r8_c5 !n6_r9_c4
!n6_r8_c5 !n6_r9_c6
!n7_r8_c5 !n8_r8_c5
!n7_r8_c5 !n9_r8_c5
!n7_r8_c5 !n7_r8_c6
!n7_r8_c5 !n7_r8_c7
!n7_r8_c5 !n7_r8_c8
!n7_r8_c5 !n7_r8_c9
!n7_r8_c5 !n7_r9_c5
!n7_r8_c5 !n7_r7_c4
!n7_r8_c5 !n7_r7_c6
!n7_r8_c5 !n7_r9_c4
!n7_r8_c5 !n7_r9_c6
!n8_r8_c5 !n9_r8_c5
!n8_r8_c5 !n8_r8_c6
!n8_r8_c5 !n8_r8_c7
!n8_r8_c5 !n8_r8_c8
!n8_r8_c5 !n8_r8_c9
!n8_r8_c5 !n8_r9_c5
!n8_r8_c5 !n8_r7_c4
!n8_r8_c5 !n8_r7_c6
!n8_r8_c5 !n8_r9_c4
!n8_r8_c5 !n8_r9_c6
!n9_r8_c5 !n9_r8_c6
!n9_r8_c5 !n9_r8_c7
!n9_r8_c5 !n9_r8_c8
!n9_r8_c5 !n9_r8_c9
!n9_r8_c5 !n9_r9_c5
!n9_r8_c5 !n9_r7_c4
!n9_r8_c5 !n9_r7_c6
!n9_r8_c5 !n9_r9_c4
!n9_r8_c5 !n9_r9_c6
n1_r8_c5 n2_r8_c5 n3_r8_c5 n4_r8_c5 n5_r8_c5 n6_r8_c5 n7_r8_c5 n8_r8_c5 n9_r8_c5
!n1_r8_c6 !n2_r8_c6
!n1_r8_c6 !n3_r8_c6
!n1_r8_c6 !n4_r8_c6
!n1_r8_c6 !n5_r8_c6
!n1_r8_c6 !n6_r8_c6
!n1_r8_c6 !n7_r8_c6
!n1_r8_c6 !n8_r8_c6
!n1_r8_c6 !n9_r8_c6
!n1_r8_c6 !n1_r8_c7
!n1_r8_c6 !n1_r8_c8
!n1_r8_c6 !n1_r8_c9
!n1_r8_c6 !n1_r9_c6
!n1_r8_c6 !n1_r7_c4
!n1_r8_c6 !n1_r7_c5
!n1_r8_c6 !n1_r9_c4
!n1_r8_c6 !n1_r9_c5
!n2_r8_c6 !n3_r8_c6
!n2_r8_c6 !n4_r8_c6
!n2_r8_c6 !n5_r8_c6
!n2_r8_c6 !n6_r8_c6
!n2_r8_c6 !n7_r8_c6
!n2_r8_c6 !n8_r8_c6
!n2_r8_c6 !n9_r8_c6
!n2_r8_c6 !n2_r8_c7
!n2_r8_c6 !n2_r8_c8
!n2_r8_c6 !n2_r8_c9
!n2_r8_c6 !n2_r9_c6
!n2_r8_c6 !n2_r7_c4
!n2_r8_c6 !n2_r7_c5
!n2_r8_c6 !n2_r9_c4
!n2_r8_c6 !n2_r9_c5
!n3_r8_c6 !n4_r8_c6
!n3_r8_c6 !n5_r8_c6
!n3_r8_c6 !n6_r8_c6
!n3_r8_c6 !n7_r8_c6
!n3_r8_c6 !n8_r8_c6
!n3_r8_c6 !n9_r8_c6
!n3_r8_c6 !n3_r8_c7
!n3_r8_c6 !n3_r8_c8
!n3_r8_c6 !n3_r8_c9
!n3_r8_c6 !n3_r9_c6
!n3_r8_c6 !n3_r7_c4
!n3_r8_c6 !n3_r7_c5
!n3_r8_c6 !n3_r9_c4
!n3_r8_c6 !n3_r9_c5
!n4_r8_c6 !n5_r8_c6
!n4_r8_c6 !n6_r8_c6
!n4_r8_c6 !n7_r8_c6
!n4_r8_c6 !n8_r8_c6
!n4_r8_c6 !n9_r8_c6
!n4_r8_c6 !n4_r8_c7
!n4_r8_c6 !n4_r8_c8
!n4_r8_c6 !n4_r8_c9
!n4_r8_c6 !n4_r9_c6
!n4_r8_c6 !n4_r7_c4
!n4_r8_c6 !n4_r7_c5
!n4_r8_c6 !n4_r9_c4
!n4_r8_c6 !n4_r9_c5
!n5_r8_c6 !n6_r8_c6
!n5_r8_c6 !n7_r8_c6
!n5_r8_c6 !n8_r8_c6
!n5_r8_c6 !n9_r8_c6
!n5_r8_c6 !n5_r8_c7
!n5_r8_c6 !n5_r8_c8
!n5_r8_c6 !n5_r8_c9
!n5_r8_c6 !n5_r9_c6
!n5_r8_c6 !n5_r7_c4
!n5_r8_c6 !n5_r7_c5
!n5_r8_c6 !n5_r9_c4
!n5_r8_c6 !n5_r9_c5
!n6_r8_c6 !n7_r8_c6
!n6_r8_c6 !n8_r8_c6
!n6_r8_c6 !n9_r8_c6
!n6_r8_c6 !n6_r8_c7
!n6_r8_c6 !n6_r8_c8
!n6_r8_c6 !n6_r8_c9
!n6_r8_c6 !n6_r9_c6
!n6_r8_c6 !n6_r7_c4
!n6_r8_c6 !n6_r7_c5
!n6_r8_c6 !n6_r9_c4
!n6_r8_c6 !n6_r9_c5
!n7_r8_c6 !n8_r8_c6
!n7_r8_c6 !n9_r8_c6
!n7_r8_c6 !n7_r8_c7
!n7_r8_c6 !n7_r8_c8
!n7_r8_c6 !n7_r8_c9
!n7_r8_c6 !n7_r9_c6
!n7_r8_c6 !n7_r7_c4
!n7_r8_c6 !n7_r7_c5
!n7_r8_c6 !n7_r9_c4
!n7_r8_c6 !n7_r9_c5
!n8_r8_c6 !n9_r8_c6
!n8_r8_c6 !n8_r8_c7
!n8_r8_c6 !n8_r8_c8
!n8_r8_c6 !n8_r8_c9
!n8_r8_c6 !n8_r9_c6
!n8_r8_c6 !n8_r7_c4
!n8_r8_c6 !n8_r7_c5
!n8_r8_c6 !n8_r9_c4
!n8_r8_c6 !n8_r9_c5
!n9_r8_c6 !n9_r8_c7
!n9_r8_c6 !n9_r8_c8
!n9_r8_c6 !n9_r8_c9
!n9_r8_c6 !n9_r9_c6
!n9_r8_c6 !n9_r7_c4
!n9_r8_c6 !n9_r7_c5
!n9_r8_c6 !n9_r9_c4
!n9_r8_c6 !n9_r9_c5
n1_r8_c6 n2_r8_c6 n3_r8_c6 n4_r8_c6 n5_r8_c6 n6_r8_c6 n7_r8_c6 n8_r8_c6 n9_r8_c6
!n1_r8_c7 !n2_r8_c7
!n1_r8_c7 !n3_r8_c7
!n1_r8_c7 !n4_r8_c7
!n1_r8_c7 !n5_r8_c7
!n1_r8_c7 !n6_r8_c7
!n1_r8_c7 !n7_r8_c7
!n1_r8_c7 !n8_r8_c7
!n1_r8_c7 !n9_r8_c7
!n1_r8_c7 !n1_r8_c8
!n1_r8_c7 !n1_r8_c9
!n1_r8_c7 !n1_r9_c7
!n1_r8_c7 !n1_r7_c8
!n1_r8_c7 !n1_r7_c9
!n1_r8_c7 !n1_r9_c8
!n1_r8_c7 !n1_r9_c9
!n2_r8_c7 !n3_r8_c7
!n2_r8_c7 !n4_r8_c7
!n2_r8_c7 !n5_r8_c7
!n2_r8_c7 !n6_r8_c7
!n2_r8_c7 !n7_r8_c7
!n2_r8_c7 !n8_r8_c7
!n2_r8_c7 !n9_r8_c7
!n2_r8_c7 !n2_r8_c8
!n2_r8_c7 !n2_r8_c9
!n2_r8_c7 !n2_r9_c7
!n2_r8_c7 !n2_r7_c8
!n2_r8_c7 !n2_r7_c9
!n2_r8_c7 !n2_r9_c8
!n2_r8_c7 !n2_r9_c9
!n3_r8_c7 !n4_r8_c7
!n3_r8_c7 !n5_r8_c7
!n3_r8_c7 !n6_r8_c7
!n3_r8_c7 !n7_r8_c7
!n3_r8_c7 !n8_r8_c7
!n3_r8_c7 !n9_r8_c7
!n3_r8_c7 !n3_r8_c8
!n3_r8_c7 !n3_r8_c9
!n3_r8_c7 !n3_r9_c7
!n3_r8_c7 !n3_r7_c8
!n3_r8_c7 !n3_r7_c9
!n3_r8_c7 !n3_r9_c8
!n3_r8_c7 !n3_r9_c9
!n4_r8_c7 !n5_r8_c7
!n4_r8_c7 !n6_r8_c7
!n4_r8_c7 !n7_r8_c7
!n4_r8_c7 !n8_r8_c7
!n4_r8_c7 !n9_r8_c7
!n4_r8_c7 !n4_r8_c8
!n4_r8_c7 !n4_r8_c9
!n4_r8_c7 !n4_r9_c7
!n4_r8_c7 !n4_r7_c8
!n4_r8_c7 !n4_r7_c9
!n4_r8_c7 !n4_r9_c8
!n4_r8_c7 !n4_r9_c9
!n5_r8_c7 !n6_r8_c7
!n5_r8_c7 !n7_r8_c7
!n5_r8_c7 !n8_r8_c7
!n5_r8_c7 !n9_r8_c7
!n5_r8_c7 !n5_r8_c8
!n5_r8_c7 !n5_r8_c9
!n5_r8_c7 !n5_r9_c7
!n5_r8_c7 !n5_r7_c8
!n5_r8_c7 !n5_r7_c9
!n5_r8_c7 !n5_r9_c8
!n5_r8_c7 !n5_r9_c9
!n6_r8_c7 !n7_r8_c7
!n6_r8_c7 !n8_r8_c7
!n6_r8_c7 !n9_r8_c7
!n6_r8_c7 !n6_r8_c8
!n6_r8_c7 !n6_r8_c9
!n6_r8_c7 !n6_r9_c7
!n6_r8_c7 !n6_r7_c8
!n6_r8_c7 !n6_r7_c9
!n6_r8_c7 !n6_r9_c8
!n6_r8_c7 !n6_r9_c9
!n7_r8_c7 !n8_r8_c7
!n7_r8_c7 !n9_r8_c7
!n7_r8_c7 !n7_r8_c8
!n7_r8_c7 !n7_r8_c9
!n7_r8_c7 !n7_r9_c7
!n7_r8_c7 !n7_r7_c8
!n7_r8_c7 !n7_r7_c9
!n7_r8_c7 !n7_r9_c8
!n7_r8_c7 !n7_r9_c9
!n8_r8_c7 !n9_r8_c7
!n8_r8_c7 !n8_r8_c8
!n8_r8_c7 !n8_r8_c9
!n8_r8_c7 !n8_r9_c7
!n8_r8_c7 !n8_r7_c8
!n8_r8_c7 !n8_r7_c9
!n8_r8_c7 !n8_r9_c8
!n8_r8_c7 !n8_r9_c9
!n9_r8_c7 !n9_r8_c8
!n9_r8_c7 !n9_r8_c9
!n9_r8_c7 !n9_r9_c7
!n9_r8_c7 !n9_r7_c8
!n9_r8_c7 !n9_r7_c9
!n9_r8_c7 !n9_r9_c8
!n9_r8_c7 !n9_r9_c9
n1_r8_c7 n2_r8_c7 n3_r8_c7 n4_r8_c7 n5_r8_c7 n6_r8_c7 n7_r8_c7 n8_r8_c7 n9_r8_c7
!n1_r8_c8 !n2_r8_c8
!n1_r8_c8 !n3_r8_c8
!n1_r8_c8 !n4_r8_c8
!n1_r8_c8 !n5_r8_c8
!n1_r8_c8 !n6_r8_c8
!n1_r8_c8 !n7_r8_c8
!n1_r8_c8 !n8_r8_c8
!n1_r8_c8 !n9_r8_c8
!n1_r8_c8 !n1_r8_c9
!n1_r8_c8 !n1_r9_c8
!n1_r8_c8 !n1_r7_c7
!n1_r8_c8 !n1_r7_c9
!n1_r8_c8 !n1_r9_c7
!n1_r8_c8 !n1_r9_c9
!n2_r8_c8 !n3_r8_c8
!n2_r8_c8 !n4_r8_c8
!n2_r8_c8 !n5_r8_c8
!n2_r8_c8 !n6_r8_c8
!n2_r8_c8 !n7_r8_c8
!n2_r8_c8 !n8_r8_c8
!n2_r8_c8 !n9_r8_c8
!n2_r8_c8 !n2_r8_c9
!n2_r8_c8 !n2_r9_c8
!n2_r8_c8 !n2_r7_c7
!n2_r8_c8 !n2_r7_c9
!n2_r8_c8 !n2_r9_c7
!n2_r8_c8 !n2_r9_c9
!n3_r8_c8 !n4_r8_c8
!n3_r8_c8 !n5_r8_c8
!n3_r8_c8 !n6_r8_c8
!n3_r8_c8 !n7_r8_c8
!n3_r8_c8 !n8_r8_c8
!n3_r8_c8 !n9_r8_c8
!n3_r8_c8 !n3_r8_c9
!n3_r8_c8 !n3_r9_c8
!n3_r8_c8 !n3_r7_c7
!n3_r8_c8 !n3_r7_c9
!n3_r8_c8 !n3_r9_c7
!n3_r8_c8 !n3_r9_c9
!n4_r8_c8 !n5_r8_c8
!n4_r8_c8 !n6_r8_c8
!n4_r8_c8 !n7_r8_c8
!n4_r8_c8 !n8_r8_c8
!n4_r8_c8 !n9_r8_c8
!n4_r8_c8 !n4_r8_c9
!n4_r8_c8 !n4_r9_c8
!n4_r8_c8 !n4_r7_c7
!n4_r8_c8 !n4_r7_c9
!n4_r8_c8 !n4_r9_c7
!n4_r8_c8 !n4_r9_c9
!n5_r8_c8 !n6_r8_c8
!n5_r8_c8 !n7_r8_c8
!n5_r8_c8 !n8_r8_c8
!n5_r8_c8 !n9_r8_c8
!n5_r8_c8 !n5_r8_c9
!n5_r8_c8 !n5_r9_c8
!n5_r8_c8 !n5_r7_c7
!n5_r8_c8 !n5_r7_c9
!n5_r8_c8 !n5_r9_c7
!n5_r8_c8 !n5_r9_c9
!n6_r8_c8 !n7_r8_c8
!n6_r8_c8 !n8_r8_c8
!n6_r8_c8 !n9_r8_c8
!n6_r8_c8 !n6_r8_c9
!n6_r8_c8 !n6_r9_c8
!n6_r8_c8 !n6_r7_c7
!n6_r8_c8 !n6_r7_c9
!n6_r8_c8 !n6_r9_c7
!n6_r8_c8 !n6_r9_c9
!n7_r8_c8 !n8_r8_c8
!n7_r8_c8 !n9_r8_c8
!n7_r8_c8 !n7_r8_c9
!n7_r8_c8 !n7_r9_c8
!n7_r8_c8 !n7_r7_c7
!n7_r8_c8 !n7_r7_c9
!n7_r8_c8 !n7_r9_c7
!n7_r8_c8 !n7_r9_c9
!n8_r8_c8 !n9_r8_c8
!n8_r8_c8 !n8_r8_c9
!n8_r8_c8 !n8_r9_c8
!n8_r8_c8 !n8_r7_c7
!n8_r8_c8 !n8_r7_c9
!n8_r8_c8 !n8_r9_c7
!n8_r8_c8 !n8_r9_c9
!n9_r8_c8 !n9_r8_c9
!n9_r8_c8 !n9_r9_c8
!n9_r8_c8 !n9_r7_c7
!n9_r8_c8 !n9_r7_c9
!n9_r8_c8 !n9_r9_c7
!n9_r8_c8 !n9_r9_c9
n1_r8_c8 n2_r8_c8 n3_r8_c8 n4_r8_c8 n5_r8_c8 n6_r8_c8 n7_r8_c8 n8_r8_c8 n9_r8_c8
!n1_r8_c9 !n2_r8_c9
!n1_r8_c9 !n3_r8_c9
!n1_r8_c9 !n4_r8_c9
!n1_r8_c9 !n5_r8_c9
!n1_r8_c9 !n6_r8_c9
!n1_r8_c9 !n7_r8_c9
!n1_r8_c9 !n8_r8_c9
!n1_r8_c9 !n9_r8_c9
!n1_r8_c9 !n1_r9_c9
!n1_r8_c9 !n1_r7_c7
!n1_r8_c9 !n1_r7_c8
!n1_r8_c9 !n1_r9_c7
!n1_r8_c9 !n1_r9_c8
!n2_r8_c9 !n3_r8_c9
!n2_r8_c9 !n4_r8_c9
!n2_r8_c9 !n5_r8_c9
!n2_r8_c9 !n6_r8_c9
!n2_r8_c9 !n7_r8_c9
!n2_r8_c9 !n8_r8_c9
!n2_r8_c9 !n9_r8_c9
!n2_r8_c9 !n2_r9_c9
!n2_r8_c9 !n2_r7_c7
!n2_r8_c9 !n2_r7_c8
!n2_r8_c9 !n2_r9_c7
!n2_r8_c9 !n2_r9_c8
!n3_r8_c9 !n4_r8_c9
!n3_r8_c9 !n5_r8_c9
!n3_r8_c9 !n6_r8_c9
!n3_r8_c9 !n7_r8_c9
!n3_r8_c9 !n8_r8_c9
!n3_r8_c9 !n9_r8_c9
!n3_r8_c9 !n3_r9_c9
!n3_r8_c9 !n3_r7_c7
!n3_r8_c9 !n3_r7_c8
!n3_r8_c9 !n3_r9_c7
!n3_r8_c9 !n3_r9_c8
!n4_r8_c9 !n5_r8_c9
!n4_r8_c9 !n6_r8_c9
!n4_r8_c9 !n7_r8_c9
!n4_r8_c9 !n8_r8_c9
!n4_r8_c9 !n9_r8_c9
!n4_r8_c9 !n4_r9_c9
!n4_r8_c9 !n4_r7_c7
!n4_r8_c9 !n4_r7_c8
!n4_r8_c9 !n4_r9_c7
!n4_r8_c9 !n4_r9_c8
!n5_r8_c9 !n6_r8_c9
!n5_r8_c9 !n7_r8_c9
!n5_r8_c9 !n8_r8_c9
!n5_r8_c9 !n9_r8_c9
!n5_r8_c9 !n5_r9_c9
!n5_r8_c9 !n5_r7_c7
!n5_r8_c9 !n5_r7_c8
!n5_r8_c9 !n5_r9_c7
!n5_r8_c9 !n5_r9_c8
!n6_r8_c9 !n7_r8_c9
!n6_r8_c9 !n8_r8_c9
!n6_r8_c9 !n9_r8_c9
!n6_r8_c9 !n6_r9_c9
!n6_r8_c9 !n6_r7_c7
!n6_r8_c9 !n6_r7_c8
!n6_r8_c9 !n6_r9_c7
!n6_r8_c9 !n6_r9_c8
!n7_r8_c9 !n8_r8_c9
!n7_r8_c9 !n9_r8_c9
!n7_r8_c9 !n7_r9_c9
!n7_r8_c9 !n7_r7_c7
!n7_r8_c9 !n7_r7_c8
!n7_r8_c9 !n7_r9_c7
!n7_r8_c9 !n7_r9_c8
!n8_r8_c9 !n9_r8_c9
!n8_r8_c9 !n8_r9_c9
!n8_r8_c9 !n8_r7_c7
!n8_r8_c9 !n8_r7_c8
!n8_r8_c9 !n8_r9_c7
!n8_r8_c9 !n8_r9_c8
!n9_r8_c9 !n9_r9_c9
!n9_r8_c9 !n9_r7_c7
!n9_r8_c9 !n9_r7_c8
!n9_r8_c9 !n9_r9_c7
!n9_r8_c9 !n9_r9_c8
n1_r8_c9 n2_r8_c9 n3_r8_c9 n4_r8_c9 n5_r8_c9 n6_r8_c9 n7_r8_c9 n8_r8_c9 n9_r8_c9
!n1_r9_c1 !n2_r9_c1
!n1_r9_c1 !n3_r9_c1
!n1_r9_c1 !n4_r9_c1
!n1_r9_c1 !n5_r9_c1
!n1_r9_c1 !n6_r9_c1
!n1_r9_c1 !n7_r9_c1
!n1_r9_c1 !n8_r9_c1
!n1_r9_c1 !n9_r9_c1
!n1_r9_c1 !n1_r9_c2
!n1_r9_c1 !n1_r9_c3
!n1_r9_c1 !n1_r9_c4
!n1_r9_c1 !n1_r9_c5
!n1_r9_c1 !n1_r9_c6
!n1_r9_c1 !n1_r9_c7
!n1_r9_c1 !n1_r9_c8
!n1_r9_c1 !n1_r9_c9
!n1_r9_c1 !n1_r7_c2
!n1_r9_c1 !n1_r7_c3
!n1_r9_c1 !n1_r8_c2
!n1_r9_c1 !n1_r8_c3
!n2_r9_c1 !n3_r9_c1
!n2_r9_c1 !n4_r9_c1
!n2_r9_c1 !n5_r9_c1
!n2_r9_c1 !n6_r9_c1
!n2_r9_c1 !n7_r9_c1
!n2_r9_c1 !n8_r9_c1
!n2_r9_c1 !n9_r9_c1
!n2_r9_c1 !n2_r9_c2
!n2_r9_c1 !n2_r9_c3
!n2_r9_c1 !n2_r9_c4
!n2_r9_c1 !n2_r9_c5
!n2_r9_c1 !n2_r9_c6
!n2_r9_c1 !n2_r9_c7
!n2_r9_c1 !n2_r9_c8
!n2_r9_c1 !n2_r9_c9
!n2_r9_c1 !n2_r7_c2
!n2_r9_c1 !n2_r7_c3
!n2_r9_c1 !n2_r8_c2
!n2_r9_c1 !n2_r8_c3
!n3_r9_c1 !n4_r9_c1
!n3_r9_c1 !n5_r9_c1
!n3_r9_c1 !n6_r9_c1
!n3_r9_c1 !n7_r9_c1
!n3_r9_c1 !n8_r9_c1
!n3_r9_c1 !n9_r9_c1
!n3_r9_c1 !n3_r9_c2
!n3_r9_c1 !n3_r9_c3
!n3_r9_c1 !n3_r9_c4
!n3_r9_c1 !n3_r9_c5
!n3_r9_c1 !n3_r9_c6
!n3_r9_c1 !n3_r9_c7
!n3_r9_c1 !n3_r9_c8
!n3_r9_c1 !n3_r9_c9
!n3_r9_c1 !n3_r7_c2
!n3_r9_c1 !n3_r7_c3
!n3_r9_c1 !n3_r8_c2
!n3_r9_c1 !n3_r8_c3
!n4_r9_c1 !n5_r9_c1
!n4_r9_c1 !n6_r9_c1
!n4_r9_c1 !n7_r9_c1
!n4_r9_c1 !n8_r9_c1
!n4_r9_c1 !n9_r9_c1
!n4_r9_c1 !n4_r9_c2
!n4_r9_c1 !n4_r9_c3
!n4_r9_c1 !n4_r9_c4
!n4_r9_c1 !n4_r9_c5
!n4_r9_c1 !n4_r9_c6
!n4_r9_c1 !n4_r9_c7
!n4_r9_c1 !n4_r9_c8
!n4_r9_c1 !n4_r9_c9
!n4_r9_c1 !n4_r7_c2
!n4_r9_c1 !n4_r7_c3
!n4_r9_c1 !n4_r8_c2
!n4_r9_c1 !n4_r8_c3
!n5_r9_c1 !n6_r9_c1
!n5_r9_c1 !n7_r9_c1
!n5_r9_c1 !n8_r9_c1
!n5_r9_c1 !n9_r9_c1
!n5_r9_c1 !n5_r9_c2
!n5_r9_c1 !n5_r9_c3
!n5_r9_c1 !n5_r9_c4
!n5_r9_c1 !n5_r9_c5
!n5_r9_c1 !n5_r9_c6
!n5_r9_c1 !n5_r9_c7
!n5_r9_c1 !n5_r9_c8
!n5_r9_c1 !n5_r9_c9
!n5_r9_c1 !n5_r7_c2
!n5_r9_c1 !n5_r7_c3
!n5_r9_c1 !n5_r8_c2
!n5_r9_c1 !n5_r8_c3
!n6_r9_c1 !n7_r9_c1
!n6_r9_c1 !n8_r9_c1
!n6_r9_c1 !n9_r9_c1
!n6_r9_c1 !n6_r9_c2
!n6_r9_c1 !n6_r9_c3
!n6_r9_c1 !n6_r9_c4
!n6_r9_c1 !n6_r9_c5
!n6_r9_c1 !n6_r9_c6
!n6_r9_c1 !n6_r9_c7
!n6_r9_c1 !n6_r9_c8
!n6_r9_c1 !n6_r9_c9
!n6_r9_c1 !n6_r7_c2
!n6_r9_c1 !n6_r7_c3
!n6_r9_c1 !n6_r8_c2
!n6_r9_c1 !n6_r8_c3
!n7_r9_c1 !n8_r9_c1
!n7_r9_c1 !n9_r9_c1
!n7_r9_c1 !n7_r9_c2
!n7_r9_c1 !n7_r9_c3
!n7_r9_c1 !n7_r9_c4
!n7_r9_c1 !n7_r9_c5
!n7_r9_c1 !n7_r9_c6
!n7_r9_c1 !n7_r9_c7
!n7_r9_c1 !n7_r9_c8
!n7_r9_c1 !n7_r9_c9
!n7_r9_c1 !n7_r7_c2
!n7_r9_c1 !n7_r7_c3
!n7_r9_c1 !n7_r8_c2
!n7_r9_c1 !n7_r8_c3
!n8_r9_c1 !n9_r9_c1
!n8_r9_c1 !n8_r9_c2
!n8_r9_c1 !n8_r9_c3
!n8_r9_c1 !n8_r9_c4
!n8_r9_c1 !n8_r9_c5
!n8_r9_c1 !n8_r9_c6
!n8_r9_c1 !n8_r9_c7
!n8_r9_c1 !n8_r9_c8
!n8_r9_c1 !n8_r9_c9
!n8_r9_c1 !n8_r7_c2
!n8_r9_c1 !n8_r7_c3
!n8_r9_c1 !n8_r8_c2
!n8_r9_c1 !n8_r8_c3
!n9_r9_c1 !n9_r9_c2
!n9_r9_c1 !n9_r9_c3
!n9_r9_c1 !n9_r9_c4
!n9_r9_c1 !n9_r9_c5
!n9_r9_c1 !n9_r9_c6
!n9_r9_c1 !n9_r9_c7
!n9_r9_c1 !n9_r9_c8
!n9_r9_c1 !n9_r9_c9
!n9_r9_c1 !n9_r7_c2
!n9_r9_c1 !n9_r7_c3
!n9_r9_c1 !n9_r8_c2
!n9_r9_c1 !n9_r8_c3
n1_r9_c1 n2_r9_c1 n3_r9_c1 n4_r9_c1 n5_r9_c1 n6_r9_c1 n7_r9_c1 n8_r9_c1 n9_r9_c1
!n1_r9_c2 !n2_r9_c2
!n1_r9_c2 !n3_r9_c2
!n1_r9_c2 !n4_r9_c2
!n1_r9_c2 !n5_r9_c2
!n1_r9_c2 !n6_r9_c2
!n1_r9_c2 !n7_r9_c2
!n1_r9_c2 !n8_r9_c2
!n1_r9_c2 !n9_r9_c2
!n1_r9_c2 !n1_r9_c3
!n1_r9_c2 !n1_r9_c4
!n1_r9_c2 !n1_r9_c5
!n1_r9_c2 !n1_r9_c6
!n1_r9_c2 !n1_r9_c7
!n1_r9_c2 !n1_r9_c8
!n1_r9_c2 !n1_r9_c9
!n1_r9_c2 !n1_r7_c1
!n1_r9_c2 !n1_r7_c3
!n1_r9_c2 !n1_r8_c1
!n1_r9_c2 !n1_r8_c3
!n2_r9_c2 !n3_r9_c2
!n2_r9_c2 !n4_r9_c2
!n2_r9_c2 !n5_r9_c2
!n2_r9_c2 !n6_r9_c2
!n2_r9_c2 !n7_r9_c2
!n2_r9_c2 !n8_r9_c2
!n2_r9_c2 !n9_r9_c2
!n2_r9_c2 !n2_r9_c3
!n2_r9_c2 !n2_r9_c4
!n2_r9_c2 !n2_r9_c5
!n2_r9_c2 !n2_r9_c6
!n2_r9_c2 !n2_r9_c7
!n2_r9_c2 !n2_r9_c8
!n2_r9_c2 !n2_r9_c9
!n2_r9_c2 !n2_r7_c1
!n2_r9_c2 !n2_r7_c3
!n2_r9_c2 !n2_r8_c1
!n2_r9_c2 !n2_r8_c3
!n3_r9_c2 !n4_r9_c2
!n3_r9_c2 !n5_r9_c2
!n3_r9_c2 !n6_r9_c2
!n3_r9_c2 !n7_r9_c2
!n3_r9_c2 !n8_r9_c2
!n3_r9_c2 !n9_r9_c2
!n3_r9_c2 !n3_r9_c3
!n3_r9_c2 !n3_r9_c4
!n3_r9_c2 !n3_r9_c5
!n3_r9_c2 !n3_r9_c6
!n3_r9_c2 !n3_r9_c7
!n3_r9_c2 !n3_r9_c8
!n3_r9_c2 !n3_r9_c9
!n3_r9_c2 !n3_r7_c1
!n3_r9_c2 !n3_r7_c3
!n3_r9_c2 !n3_r8_c1
!n3_r9_c2 !n3_r8_c3
!n4_r9_c2 !n5_r9_c2
!n4_r9_c2 !n6_r9_c2
!n4_r9_c2 !n7_r9_c2
!n4_r9_c2 !n8_r9_c2
!n4_r9_c2 !n9_r9_c2
!n4_r9_c2 !n4_r9_c3
!n4_r9_c2 !n4_r9_c4
!n4_r9_c2 !n4_r9_c5
!n4_r9_c2 !n4_r9_c6
!n4_r9_c2 !n4_r9_c7
!n4_r9_c2 !n4_r9_c8
!n4_r9_c2 !n4_r9_c9
!n4_r9_c2 !n4_r7_c1
!n4_r9_c2 !n4_r7_c3
!n4_r9_c2 !n4_r8_c1
!n4_r9_c2 !n4_r8_c3
!n5_r9_c2 !n6_r9_c2
!n5_r9_c2 !n7_r9_c2
!n5_r9_c2 !n8_r9_c2
!n5_r9_c2 !n9_r9_c2
!n5_r9_c2 !n5_r9_c3
!n5_r9_c2 !n5_r9_c4
!n5_r9_c2 !n5_r9_c5
!n5_r9_c2 !n5_r9_c6
!n5_r9_c2 !n5_r9_c7
!n5_r9_c2 !n5_r9_c8
!n5_r9_c2 !n5_r9_c9
!n5_r9_c2 !n5_r7_c1
!n5_r9_c2 !n5_r7_c3
!n5_r9_c2 !n5_r8_c1
!n5_r9_c2 !n5_r8_c3
!n6_r9_c2 !n7_r9_c2
!n6_r9_c2 !n8_r9_c2
!n6_r9_c2 !n9_r9_c2
!n6_r9_c2 !n6_r9_c3
!n6_r9_c2 !n6_r9_c4
!n6_r9_c2 !n6_r9_c5
!n6_r9_c2 !n6_r9_c6
!n6_r9_c2 !n6_r9_c7
!n6_r9_c2 !n6_r9_c8
!n6_r9_c2 !n6_r9_c9
!n6_r9_c2 !n6_r7_c1
!n6_r9_c2 !n6_r7_c3
!n6_r9_c2 !n6_r8_c1
!n6_r9_c2 !n6_r8_c3
!n7_r9_c2 !n8_r9_c2
!n7_r9_c2 !n9_r9_c2
!n7_r9_c2 !n7_r9_c3
!n7_r9_c2 !n7_r9_c4
!n7_r9_c2 !n7_r9_c5
!n7_r9_c2 !n7_r9_c6
!n7_r9_c2 !n7_r9_c7
!n7_r9_c2 !n7_r9_c8
!n7_r9_c2 !n7_r9_c9
!n7_r9_c2 !n7_r7_c1
!n7_r9_c2 !n7_r7_c3
!n7_r9_c2 !n7_r8_c1
!n7_r9_c2 !n7_r8_c3
!n8_r9_c2 !n9_r9_c2
!n8_r9_c2 !n8_r9_c3
!n8_r9_c2 !n8_r9_c4
!n8_r9_c2 !n8_r9_c5
!n8_r9_c2 !n8_r9_c6
!n8_r9_c2 !n8_r9_c7
!n8_r9_c2 !n8_r9_c8
!n8_r9_c2 !n8_r9_c9
!n8_r9_c2 !n8_r7_c1
!n8_r9_c2 !n8_r7_c3
!n8_r9_c2 !n8_r8_c1
!n8_r9_c2 !n8_r8_c3
!n9_r9_c2 !n9_r9_c3
!n9_r9_c2 !n9_r9_c4
!n9_r9_c2 !n9_r9_c5
!n9_r9_c2 !n9_r9_c6
!n9_r9_c2 !n9_r9_c7
!n9_r9_c2 !n9_r9_c8
!n9_r9_c2 !n9_r9_c9
!n9_r9_c2 !n9_r7_c1
!n9_r9_c2 !n9_r7_c3
!n9_r9_c2 !n9_r8_c1
!n9_r9_c2 !n9_r8_c3
n1_r9_c2 n2_r9_c2 n3_r9_c2 n4_r9_c2 n5_r9_c2 n6_r9_c2 n7_r9_c2 n8_r9_c2 n9_r9_c2
!n1_r9_c3 !n2_r9_c3
!n1_r9_c3 !n3_r9_c3
!n1_r9_c3 !n4_r9_c3
!n1_r9_c3 !n5_r9_c3
!n1_r9_c3 !n6_r9_c3
!n1_r9_c3 !n7_r9_c3
!n1_r9_c3 !n8_r9_c3
!n1_r9_c3 !n9_r9_c3
!n1_r9_c3 !n1_r9_c4
!n1_r9_c3 !n1_r9_c5
!n1_r9_c3 !n1_r9_c6
!n1_r9_c3 !n1_r9_c7
!n1_r9_c3 !n1_r9_c8
!n1_r9_c3 !n1_r9_c9
!n1_r9_c3 !n1_r7_c1
!n1_r9_c3 !n1_r7_c2
!n1_r9_c3 !n1_r8_c1
!n1_r9_c3 !n1_r8_c2
!n2_r9_c3 !n3_r9_c3
!n2_r9_c3 !n4_r9_c3
!n2_r9_c3 !n5_r9_c3
!n2_r9_c3 !n6_r9_c3
!n2_r9_c3 !n7_r9_c3
!n2_r9_c3 !n8_r9_c3
!n2_r9_c3 !n9_r9_c3
!n2_r9_c3 !n2_r9_c4
!n2_r9_c3 !n2_r9_c5
!n2_r9_c3 !n2_r9_c6
!n2_r9_c3 !n2_r9_c7
!n2_r9_c3 !n2_r9_c8
!n2_r9_c3 !n2_r9_c9
!n2_r9_c3 !n2_r7_c1
!n2_r9_c3 !n2_r7_c2
!n2_r9_c3 !n2_r8_c1
!n2_r9_c3 !n2_r8_c2
!n3_r9_c3 !n4_r9_c3
!n3_r9_c3 !n5_r9_c3
!n3_r9_c3 !n6_r9_c3
!n3_r9_c3 !n7_r9_c3
!n3_r9_c3 !n8_r9_c3
!n3_r9_c3 !n9_r9_c3
!n3_r9_c3 !n3_r9_c4
!n3_r9_c3 !n3_r9_c5
!n3_r9_c3 !n3_r9_c6
!n3_r9_c3 !n3_r9_c7
!n3_r9_c3 !n3_r9_c8
!n3_r9_c3 !n3_r9_c9
!n3_r9_c3 !n3_r7_c1
!n3_r9_c3 !n3_r7_c2
!n3_r9_c3 !n3_r8_c1
!n3_r9_c3 !n3_r8_c2
!n4_r9_c3 !n5_r9_c3
!n4_r9_c3 !n6_r9_c3
!n4_r9_c3 !n7_r9_c3
!n4_r9_c3 !n8_r9_c3
!n4_r9_c3 !n9_r9_c3
!n4_r9_c3 !n4_r9_c4
!n4_r9_c3 !n4_r9_c5
!n4_r9_c3 !n4_r9_c6
!n4_r9_c3 !n4_r9_c7
!n4_r9_c3 !n4_r9_c8
!n4_r9_c3 !n4_r9_c9
!n4_r9_c3 !n4_r7_c1
!n4_r9_c3 !n4_r7_c2
!n4_r9_c3 !n4_r8_c1
!n4_r9_c3 !n4_r8_c2
!n5_r9_c3 !n6_r9_c3
!n5_r9_c3 !n7_r9_c3
!n5_r9_c3 !n8_r9_c3
!n5_r9_c3 !n9_r9_c3
!n5_r9_c3 !n5_r9_c4
!n5_r9_c3 !n5_r9_c5
!n5_r9_c3 !n5_r9_c6
!n5_r9_c3 !n5_r9_c7
!n5_r9_c3 !n5_r9_c8
!n5_r9_c3 !n5_r9_c9
!n5_r9_c3 !n5_r7_c1
!n5_r9_c3 !n5_r7_c2
!n5_r9_c3 !n5_r8_c1
!n5_r9_c3 !n5_r8_c2
!n6_r9_c3 !n7_r9_c3
!n6_r9_c3 !n8_r9_c3
!n6_r9_c3 !n9_r9_c3
!n6_r9_c3 !n6_r9_c4
!n6_r9_c3 !n6_r9_c5
!n6_r9_c3 !n6_r9_c6
!n6_r9_c3 !n6_r9_c7
!n6_r9_c3 !n6_r9_c8
!n6_r9_c3 !n6_r9_c9
!n6_r9_c3 !n6_r7_c1
!n6_r9_c3 !n6_r7_c2
!n6_r9_c3 !n6_r8_c1
!n6_r9_c3 !n6_r8_c2
!n7_r9_c3 !n8_r9_c3
!n7_r9_c3 !n9_r9_c3
!n7_r9_c3 !n7_r9_c4
!n7_r9_c3 !n7_r9_c5
!n7_r9_c3 !n7_r9_c6
!n7_r9_c3 !n7_r9_c7
!n7_r9_c3 !n7_r9_c8
!n7_r9_c3 !n7_r9_c9
!n7_r9_c3 !n7_r7_c1
!n7_r9_c3 !n7_r7_c2
!n7_r9_c3 !n7_r8_c1
!n7_r9_c3 !n7_r8_c2
!n8_r9_c3 !n9_r9_c3
!n8_r9_c3 !n8_r9_c4
!n8_r9_c3 !n8_r9_c5
!n8_r9_c3 !n8_r9_c6
!n8_r9_c3 !n8_r9_c7
!n8_r9_c3 !n8_r9_c8
!n8_r9_c3 !n8_r9_c9
!n8_r9_c3 !n8_r7_c1
!n8_r9_c3 !n8_r7_c2
!n8_r9_c3 !n8_r8_c1
!n8_r9_c3 !n8_r8_c2
!n9_r9_c3 !n9_r9_c4
!n9_r9_c3 !n9_r9_c5
!n9_r9_c3 !n9_r9_c6
!n9_r9_c3 !n9_r9_c7
!n9_r9_c3 !n9_r9_c8
!n9_r9_c3 !n9_r9_c9
!n9_r9_c3 !n9_r7_c1
!n9_r9_c3 !n9_r7_c2
!n9_r9_c3 !n9_r8_c1
!n9_r9_c3 !n9_r8_c2
n1_r9_c3 n2_r9_c3 n3_r9_c3 n4_r9_c3 n5_r9_c3 n6_r9_c3 n7_r9_c3 n8_r9_c3 n9_r9_c3
!n1_r9_c4 !n2_r9_c4
!n1_r9_c4 !n3_r9_c4
!n1_r9_c4 !n4_r9_c4
!n1_r9_c4 !n5_r9_c4
!n1_r9_c4 !n6_r9_c4
!n1_r9_c4 !n7_r9_c4
!n1_r9_c4 !n8_r9_c4
!n1_r9_c4 !n9_r9_c4
!n1_r9_c4 !n1_r9_c5
!n1_r9_c4 !n1_r9_c6
!n1_r9_c4 !n1_r9_c7
!n1_r9_c4 !n1_r9_c8
!n1_r9_c4 !n1_r9_c9
!n1_r9_c4 !n1_r7_c5
!n1_r9_c4 !n1_r7_c6
!n1_r9_c4 !n1_r8_c5
!n1_r9_c4 !n1_r8_c6
!n2_r9_c4 !n3_r9_c4
!n2_r9_c4 !n4_r9_c4
!n2_r9_c4 !n5_r9_c4
!n2_r9_c4 !n6_r9_c4
!n2_r9_c4 !n7_r9_c4
!n2_r9_c4 !n8_r9_c4
!n2_r9_c4 !n9_r9_c4
!n2_r9_c4 !n2_r9_c5
!n2_r9_c4 !n2_r9_c6
!n2_r9_c4 !n2_r9_c7
!n2_r9_c4 !n2_r9_c8
!n2_r9_c4 !n2_r9_c9
!n2_r9_c4 !n2_r7_c5
!n2_r9_c4 !n2_r7_c6
!n2_r9_c4 !n2_r8_c5
!n2_r9_c4 !n2_r8_c6
!n3_r9_c4 !n4_r9_c4
!n3_r9_c4 !n5_r9_c4
!n3_r9_c4 !n6_r9_c4
!n3_r9_c4 !n7_r9_c4
!n3_r9_c4 !n8_r9_c4
!n3_r9_c4 !n9_r9_c4
!n3_r9_c4 !n3_r9_c5
!n3_r9_c4 !n3_r9_c6
!n3_r9_c4 !n3_r9_c7
!n3_r9_c4 !n3_r9_c8
!n3_r9_c4 !n3_r9_c9
!n3_r9_c4 !n3_r7_c5
!n3_r9_c4 !n3_r7_c6
!n3_r9_c4 !n3_r8_c5
!n3_r9_c4 !n3_r8_c6
!n4_r9_c4 !n5_r9_c4
!n4_r9_c4 !n6_r9_c4
!n4_r9_c4 !n7_r9_c4
!n4_r9_c4 !n8_r9_c4
!n4_r9_c4 !n9_r9_c4
!n4_r9_c4 !n4_r9_c5
!n4_r9_c4 !n4_r9_c6
!n4_r9_c4 !n4_r9_c7
!n4_r9_c4 !n4_r9_c8
!n4_r9_c4 !n4_r9_c9
!n4_r9_c4 !n4_r7_c5
!n4_r9_c4 !n4_r7_c6
!n4_r9_c4 !n4_r8_c5
!n4_r9_c4 !n4_r8_c6
!n5_r9_c4 !n6_r9_c4
!n5_r9_c4 !n7_r9_c4
!n5_r9_c4 !n8_r9_c4
!n5_r9_c4 !n9_r9_c4
!n5_r9_c4 !n5_r9_c5
!n5_r9_c4 !n5_r9_c6
!n5_r9_c4 !n5_r9_c7
!n5_r9_c4 !n5_r9_c8
!n5_r9_c4 !n5_r9_c9
!n5_r9_c4 !n5_r7_c5
!n5_r9_c4 !n5_r7_c6
!n5_r9_c4 !n5_r8_c5
!n5_r9_c4 !n5_r8_c6
!n6_r9_c4 !n7_r9_c4
!n6_r9_c4 !n8_r9_c4
!n6_r9_c4 !n9_r9_c4
!n6_r9_c4 !n6_r9_c5
!n6_r9_c4 !n6_r9_c6
!n6_r9_c4 !n6_r9_c7
!n6_r9_c4 !n6_r9_c8
!n6_r9_c4 !n6_r9_c9
!n6_r9_c4 !n6_r7_c5
!n6_r9_c4 !n6_r7_c6
!n6_r9_c4 !n6_r8_c5
!n6_r9_c4 !n6_r8_c6
!n7_r9_c4 !n8_r9_c4
!n7_r9_c4 !n9_r9_c4
!n7_r9_c4 !n7_r9_c5
!n7_r9_c4 !n7_r9_c6
!n7_r9_c4 !n7_r9_c7
!n7_r9_c4 !n7_r9_c8
!n7_r9_c4 !n7_r9_c9
!n7_r9_c4 !n7_r7_c5
!n7_r9_c4 !n7_r7_c6
!n7_r9_c4 !n7_r8_c5
!n7_r9_c4 !n7_r8_c6
!n8_r9_c4 !n9_r9_c4
!n8_r9_c4 !n8_r9_c5
!n8_r9_c4 !n8_r9_c6
!n8_r9_c4 !n8_r9_c7
!n8_r9_c4 !n8_r9_c8
!n8_r9_c4 !n8_r9_c9
!n8_r9_c4 !n8_r7_c5
!n8_r9_c4 !n8_r7_c6
!n8_r9_c4 !n8_r8_c5
!n8_r9_c4 !n8_r8_c6
!n9_r9_c4 !n9_r9_c5
!n9_r9_c4 !n9_r9_c6
!n9_r9_c4 !n9_r9_c7
!n9_r9_c4 !n9_r9_c8
!n9_r9_c4 !n9_r9_c9
!n9_r9_c4 !n9_r7_c5
!n9_r9_c4 !n9_r7_c6
!n9_r9_c4 !n9_r8_c5
!n9_r9_c4 !n9_r8_c6
n1_r9_c4 n2_r9_c4 n3_r9_c4 n4_r9_c4 n5_r9_c4 n6_r9_c4 n7_r9_c4 n8_r9_c4 n9_r9_c4
!n1_r9_c5 !n2_r9_c5
!n1_r9_c5 !n3_r9_c5
!n1_r9_c5 !n4_r9_c5
!n1_r9_c5 !n5_r9_c5
!n1_r9_c5 !n6_r9_c5
!n1_r9_c5 !n7_r9_c5
!n1_r9_c5 !n8_r9_c5
!n1_r9_c5 !n9_r9_c5
!n1_r9_c5 !n1_r9_c6
!n1_r9_c5 !n1_r9_c7
!n1_r9_c5 !n1_r9_c8
!n1_r9_c5 !n1_r9_c9
!n1_r9_c5 !n1_r7_c4
!n1_r9_c5 !n1_r7_c6
!n1_r9_c5 !n1_r8_c4
!n1_r9_c5 !n1_r8_c6
!n2_r9_c5 !n3_r9_c5
!n2_r9_c5 !n4_r9_c5
!n2_r9_c5 !n5_r9_c5
!n2_r9_c5 !n6_r9_c5
!n2_r9_c5 !n7_r9_c5
!n2_r9_c5 !n8_r9_c5
!n2_r9_c5 !n9_r9_c5
!n2_r9_c5 !n2_r9_c6
!n2_r9_c5 !n2_r9_c7
!n2_r9_c5 !n2_r9_c8
!n2_r9_c5 !n2_r9_c9
!n2_r9_c5 !n2_r7_c4
!n2_r9_c5 !n2_r7_c6
!n2_r9_c5 !n2_r8_c4
!n2_r9_c5 !n2_r8_c6
!n3_r9_c5 !n4_r9_c5
!n3_r9_c5 !n5_r9_c5
!n3_r9_c5 !n6_r9_c5
!n3_r9_c5 !n7_r9_c5
!n3_r9_c5 !n8_r9_c5
!n3_r9_c5 !n9_r9_c5
!n3_r9_c5 !n3_r9_c6
!n3_r9_c5 !n3_r9_c7
!n3_r9_c5 !n3_r9_c8
!n3_r9_c5 !n3_r9_c9
!n3_r9_c5 !n3_r7_c4
!n3_r9_c5 !n3_r7_c6
!n3_r9_c5 !n3_r8_c4
!n3_r9_c5 !n3_r8_c6
!n4_r9_c5 !n5_r9_c5
!n4_r9_c5 !n6_r9_c5
!n4_r9_c5 !n7_r9_c5
!n4_r9_c5 !n8_r9_c5
!n4_r9_c5 !n9_r9_c5
!n4_r9_c5 !n4_r9_c6
!n4_r9_c5 !n4_r9_c7
!n4_r9_c5 !n4_r9_c8
!n4_r9_c5 !n4_r9_c9
!n4_r9_c5 !n4_r7_c4
!n4_r9_c5 !n4_r7_c6
!n4_r9_c5 !n4_r8_c4
!n4_r9_c5 !n4_r8_c6
!n5_r9_c5 !n6_r9_c5
!n5_r9_c5 !n7_r9_c5
!n5_r9_c5 !n8_r9_c5
!n5_r9_c5 !n9_r9_c5
!n5_r9_c5 !n5_r9_c6
!n5_r9_c5 !n5_r9_c7
!n5_r9_c5 !n5_r9_c8
!n5_r9_c5 !n5_r9_c9
!n5_r9_c5 !n5_r7_c4
!n5_r9_c5 !n5_r7_c6
!n5_r9_c5 !n5_r8_c4
!n5_r9_c5 !n5_r8_c6
!n6_r9_c5 !n7_r9_c5
!n6_r9_c5 !n8_r9_c5
!n6_r9_c5 !n9_r9_c5
!n6_r9_c5 !n6_r9_c6
!n6_r9_c5 !n6_r9_c7
!n6_r9_c5 !n6_r9_c8
!n6_r9_c5 !n6_r9_c9
!n6_r9_c5 !n6_r7_c4
!n6_r9_c5 !n6_r7_c6
!n6_r9_c5 !n6_r8_c4
!n6_r9_c5 !n6_r8_c6
!n7_r9_c5 !n8_r9_c5
!n7_r9_c5 !n9_r9_c5
!n7_r9_c5 !n7_r9_c6
!n7_r9_c5 !n7_r9_c7
!n7_r9_c5 !n7_r9_c8
!n7_r9_c5 !n7_r9_c9
!n7_r9_c5 !n7_r7_c4
!n7_r9_c5 !n7_r7_c6
!n7_r9_c5 !n7_r8_c4
!n7_r9_c5 !n7_r8_c6
!n8_r9_c5 !n9_r9_c5
!n8_r9_c5 !n8_r9_c6
!n8_r9_c5 !n8_r9_c7
!n8_r9_c5 !n8_r9_c8
!n8_r9_c5 !n8_r9_c9
!n8_r9_c5 !n8_r7_c4
!n8_r9_c5 !n8_r7_c6
!n8_r9_c5 !n8_r8_c4
!n8_r9_c5 !n8_r8_c6
!n9_r9_c5 !n9_r9_c6
!n9_r9_c5 !n9_r9_c7
!n9_r9_c5 !n9_r9_c8
!n9_r9_c5 !n9_r9_c9
!n9_r9_c5 !n9_r7_c4
!n9_r9_c5 !n9_r7_c6
!n9_r9_c5 !n9_r8_c4
!n9_r9_c5 !n9_r8_c6
n1_r9_c5 n2_r9_c5 n3_r9_c5 n4_r9_c5 n5_r9_c5 n6_r9_c5 n7_r9_c5 n8_r9_c5 n9_r9_c5
!n1_r9_c6 !n2_r9_c6
!n1_r9_c6 !n3_r9_c6
!n1_r9_c6 !n4_r9_c6
!n1_r9_c6 !n5_r9_c6
!n1_r9_c6 !n6_r9_c6
!n1_r9_c6 !n7_r9_c6
!n1_r9_c6 !n8_r9_c6
!n1_r9_c6 !n9_r9_c6
!n1_r9_c6 !n1_r9_c7
!n1_r9_c6 !n1_r9_c8
!n1_r9_c6 !n1_r9_c9
!n1_r9_c6 !n1_r7_c4
!n1_r9_c6 !n1_r7_c5
!n1_r9_c6 !n1_r8_c4
!n1_r9_c6 !n1_r8_c5
!n2_r9_c6 !n3_r9_c6
!n2_r9_c6 !n4_r9_c6
!n2_r9_c6 !n5_r9_c6
!n2_r9_c6 !n6_r9_c6
!n2_r9_c6 !n7_r9_c6
!n2_r9_c6 !n8_r9_c6
!n2_r9_c6 !n9_r9_c6
!n2_r9_c6 !n2_r9_c7
!n2_r9_c6 !n2_r9_c8
!n2_r9_c6 !n2_r9_c9
!n2_r9_c6 !n2_r7_c4
!n2_r9_c6 !n2_r7_c5
!n2_r9_c6 !n2_r8_c4
!n2_r9_c6 !n2_r8_c5
!n3_r9_c6 !n4_r9_c6
!n3_r9_c6 !n5_r9_c6
!n3_r9_c6 !n6_r9_c6
!n3_r9_c6 !n7_r9_c6
!n3_r9_c6 !n8_r9_c6
!n3_r9_c6 !n9_r9_c6
!n3_r9_c6 !n3_r9_c7
!n3_r9_c6 !n3_r9_c8
!n3_r9_c6 !n3_r9_c9
!n3_r9_c6 !n3_r7_c4
!n3_r9_c6 !n3_r7_c5
!n3_r9_c6 !n3_r8_c4
!n3_r9_c6 !n3_r8_c5
!n4_r9_c6 !n5_r9_c6
!n4_r9_c6 !n6_r9_c6
!n4_r9_c6 !n7_r9_c6
!n4_r9_c6 !n8_r9_c6
!n4_r9_c6 !n9_r9_c6
!n4_r9_c6 !n4_r9_c7
!n4_r9_c6 !n4_r9_c8
!n4_r9_c6 !n4_r9_c9
!n4_r9_c6 !n4_r7_c4
!n4_r9_c6 !n4_r7_c5
!n4_r9_c6 !n4_r8_c4
!n4_r9_c6 !n4_r8_c5
!n5_r9_c6 !n6_r9_c6
!n5_r9_c6 !n7_r9_c6
!n5_r9_c6 !n8_r9_c6
!n5_r9_c6 !n9_r9_c6
!n5_r9_c6 !n5_r9_c7
!n5_r9_c6 !n5_r9_c8
!n5_r9_c6 !n5_r9_c9
!n5_r9_c6 !n5_r7_c4
!n5_r9_c6 !n5_r7_c5
!n5_r9_c6 !n5_r8_c4
!n5_r9_c6 !n5_r8_c5
!n6_r9_c6 !n7_r9_c6
!n6_r9_c6 !n8_r9_c6
!n6_r9_c6 !n9_r9_c6
!n6_r9_c6 !n6_r9_c7
!n6_r9_c6 !n6_r9_c8
!n6_r9_c6 !n6_r9_c9
!n6_r9_c6 !n6_r7_c4
!n6_r9_c6 !n6_r7_c5
!n6_r9_c6 !n6_r8_c4
!n6_r9_c6 !n6_r8_c5
!n7_r9_c6 !n8_r9_c6
!n7_r9_c6 !n9_r9_c6
!n7_r9_c6 !n7_r9_c7
!n7_r9_c6 !n7_r9_c8
!n7_r9_c6 !n7_r9_c9
!n7_r9_c6 !n7_r7_c4
!n7_r9_c6 !n7_r7_c5
!n7_r9_c6 !n7_r8_c4
!n7_r9_c6 !n7_r8_c5
!n8_r9_c6 !n9_r9_c6
!n8_r9_c6 !n8_r9_c7
!n8_r9_c6 !n8_r9_c8
!n8_r9_c6 !n8_r9_c9
!n8_r9_c6 !n8_r7_c4
!n8_r9_c6 !n8_r7_c5
!n8_r9_c6 !n8_r8_c4
!n8_r9_c6 !n8_r8_c5
!n9_r9_c6 !n9_r9_c7
!n9_r9_c6 !n9_r9_c8
!n9_r9_c6 !n9_r9_c9
!n9_r9_c6 !n9_r7_c4
!n9_r9_c6 !n9_r7_c5
!n9_r9_c6 !n9_r8_c4
!n9_r9_c6 !n9_r8_c5
n1_r9_c6 n2_r9_c6 n3_r9_c6 n4_r9_c6 n5_r9_c6 n6_r9_c6 n7_r9_c6 n8_r9_c6 n9_r9_c6
!n1_r9_c7 !n2_r9_c7
!n1_r9_c7 !n3_r9_c7
!n1_r9_c7 !n4_r9_c7
!n1_r9_c7 !n5_r9_c7
!n1_r9_c7 !n6_r9_c7
!n1_r9_c7 !n7_r9_c7
!n1_r9_c7 !n8_r9_c7
!n1_r9_c7 !n9_r9_c7
!n1_r9_c7 !n1_r9_c8
!n1_r9_c7 !n1_r9_c9
!n1_r9_c7 !n1_r7_c8
!n1_r9_c7 !n1_r7_c9
!n1_r9_c7 !n1_r8_c8
!n1_r9_c7 !n1_r8_c9
!n2_r9_c7 !n3_r9_c7
!n2_r9_c7 !n4_r9_c7
!n2_r9_c7 !n5_r9_c7
!n2_r9_c7 !n6_r9_c7
!n2_r9_c7 !n7_r9_c7
!n2_r9_c7 !n8_r9_c7
!n2_r9_c7 !n9_r9_c7
!n2_r9_c7 !n2_r9_c8
!n2_r9_c7 !n2_r9_c9
!n2_r9_c7 !n2_r7_c8
!n2_r9_c7 !n2_r7_c9
!n2_r9_c7 !n2_r8_c8
!n2_r9_c7 !n2_r8_c9
!n3_r9_c7 !n4_r9_c7
!n3_r9_c7 !n5_r9_c7
!n3_r9_c7 !n6_r9_c7
!n3_r9_c7 !n7_r9_c7
!n3_r9_c7 !n8_r9_c7
!n3_r9_c7 !n9_r9_c7
!n3_r9_c7 !n3_r9_c8
!n3_r9_c7 !n3_r9_c9
!n3_r9_c7 !n3_r7_c8
!n3_r9_c7 !n3_r7_c9
!n3_r9_c7 !n3_r8_c8
!n3_r9_c7 !n3_r8_c9
!n4_r9_c7 !n5_r9_c7
!n4_r9_c7 !n6_r9_c7
!n4_r9_c7 !n7_r9_c7
!n4_r9_c7 !n8_r9_c7
!n4_r9_c7 !n9_r9_c7
!n4_r9_c7 !n4_r9_c8
!n4_r9_c7 !n4_r9_c9
!n4_r9_c7 !n4_r7_c8
!n4_r9_c7 !n4_r7_c9
!n4_r9_c7 !n4_r8_c8
!n4_r9_c7 !n4_r8_c9
!n5_r9_c7 !n6_r9_c7
!n5_r9_c7 !n7_r9_c7
!n5_r9_c7 !n8_r9_c7
!n5_r9_c7 !n9_r9_c7
!n5_r9_c7 !n5_r9_c8
!n5_r9_c7 !n5_r9_c9
!n5_r9_c7 !n5_r7_c8
!n5_r9_c7 !n5_r7_c9
!n5_r9_c7 !n5_r8_c8
!n5_r9_c7 !n5_r8_c9
!n6_r9_c7 !n7_r9_c7
!n6_r9_c7 !n8_r9_c7
!n6_r9_c7 !n9_r9_c7
!n6_r9_c7 !n6_r9_c8
!n6_r9_c7 !n6_r9_c9
!n6_r9_c7 !n6_r7_c8
!n6_r9_c7 !n6_r7_c9
!n6_r9_c7 !n6_r8_c8
!n6_r9_c7 !n6_r8_c9
!n7_r9_c7 !n8_r9_c7
!n7_r9_c7 !n9_r9_c7
!n7_r9_c7 !n7_r9_c8
!n7_r9_c7 !n7_r9_c9
!n7_r9_c7 !n7_r7_c8
!n7_r9_c7 !n7_r7_c9
!n7_r9_c7 !n7_r8_c8
!n7_r9_c7 !n7_r8_c9
!n8_r9_c7 !n9_r9_c7
!n8_r9_c7 !n8_r9_c8
!n8_r9_c7 !n8_r9_c9
!n8_r9_c7 !n8_r7_c8
!n8_r9_c7 !n8_r7_c9
!n8_r9_c7 !n8_r8_c8
!n8_r9_c7 !n8_r8_c9
!n9_r9_c7 !n9_r9_c8
!n9_r9_c7 !n9_r9_c9
!n9_r9_c7 !n9_r7_c8
!n9_r9_c7 !n9_r7_c9
!n9_r9_c7 !n9_r8_c8
!n9_r9_c7 !n9_r8_c9
n1_r9_c7 n2_r9_c7 n3_r9_c7 n4_r9_c7 n5_r9_c7 n6_r9_c7 n7_r9_c7 n8_r9_c7 n9_r9_c7
!n1_r9_c8 !n2_r9_c8
!n1_r9_c8 !n3_r9_c8
!n1_r9_c8 !n4_r9_c8
!n1_r9_c8 !n5_r9_c8
!n1_r9_c8 !n6_r9_c8
!n1_r9_c8 !n7_r9_c8
!n1_r9_c8 !n8_r9_c8
!n1_r9_c8 !n9_r9_c8
!n1_r9_c8 !n1_r9_c9
!n1_r9_c8 !n1_r7_c7
!n1_r9_c8 !n1_r7_c9
!n1_r9_c8 !n1_r8_c7
!n1_r9_c8 !n1_r8_c9
!n2_r9_c8 !n3_r9_c8
!n2_r9_c8 !n4_r9_c8
!n2_r9_c8 !n5_r9_c8
!n2_r9_c8 !n6_r9_c8
!n2_r9_c8 !n7_r9_c8
!n2_r9_c8 !n8_r9_c8
!n2_r9_c8 !n9_r9_c8
!n2_r9_c8 !n2_r9_c9
!n2_r9_c8 !n2_r7_c7
!n2_r9_c8 !n2_r7_c9
!n2_r9_c8 !n2_r8_c7
!n2_r9_c8 !n2_r8_c9
!n3_r9_c8 !n4_r9_c8
!n3_r9_c8 !n5_r9_c8
!n3_r9_c8 !n6_r9_c8
!n3_r9_c8 !n7_r9_c8
!n3_r9_c8 !n8_r9_c8
!n3_r9_c8 !n9_r9_c8
!n3_r9_c8 !n3_r9_c9
!n3_r9_c8 !n3_r7_c7
!n3_r9_c8 !n3_r7_c9
!n3_r9_c8 !n3_r8_c7
!n3_r9_c8 !n3_r8_c9
!n4_r9_c8 !n5_r9_c8
!n4_r9_c8 !n6_r9_c8
!n4_r9_c8 !n7_r9_c8
!n4_r9_c8 !n8_r9_c8
!n4_r9_c8 !n9_r9_c8
!n4_r9_c8 !n4_r9_c9
!n4_r9_c8 !n4_r7_c7
!n4_r9_c8 !n4_r7_c9
!n4_r9_c8 !n4_r8_c7
!n4_r9_c8 !n4_r8_c9
!n5_r9_c8 !n6_r9_c8
!n5_r9_c8 !n7_r9_c8
!n5_r9_c8 !n8_r9_c8
!n5_r9_c8 !n9_r9_c8
!n5_r9_c8 !n5_r9_c9
!n5_r9_c8 !n5_r7_c7
!n5_r9_c8 !n5_r7_c9
!n5_r9_c8 !n5_r8_c7
!n5_r9_c8 !n5_r8_c9
!n6_r9_c8 !n7_r9_c8
!n6_r9_c8 !n8_r9_c8
!n6_r9_c8 !n9_r9_c8
!n6_r9_c8 !n6_r9_c9
!n6_r9_c8 !n6_r7_c7
!n6_r9_c8 !n6_r7_c9
!n6_r9_c8 !n6_r8_c7
!n6_r9_c8 !n6_r8_c9
!n7_r9_c8 !n8_r9_c8
!n7_r9_c8 !n9_r9_c8
!n7_r9_c8 !n7_r9_c9
!n7_r9_c8 !n7_r7_c7
!n7_r9_c8 !n7_r7_c9
!n7_r9_c8 !n7_r8_c7
!n7_r9_c8 !n7_r8_c9
!n8_r9_c8 !n9_r9_c8
!n8_r9_c8 !n8_r9_c9
!n8_r9_c8 !n8_r7_c7
!n8_r9_c8 !n8_r7_c9
!n8_r9_c8 !n8_r8_c7
!n8_r9_c8 !n8_r8_c9
!n9_r9_c8 !n9_r9_c9
!n9_r9_c8 !n9_r7_c7
!n9_r9_c8 !n9_r7_c9
!n9_r9_c8 !n9_r8_c7
!n9_r9_c8 !n9_r8_c9
n1_r9_c8 n2_r9_c8 n3_r9_c8 n4_r9_c8 n5_r9_c8 n6_r9_c8 n7_r9_c8 n8_r9_c8 n9_r9_c8
!n1_r9_c9 !n2_r9_c9
!n1_r9_c9 !n3_r9_c9
!n1_r9_c9 !n4_r9_c9
!n1_r9_c9 !n5_r9_c9
!n1_r9_c9 !n6_r9_c9
!n1_r9_c9 !n7_r9_c9
!n1_r9_c9 !n8_r9_c9
!n1_r9_c9 !n9_r9_c9
!n1_r9_c9 !n1_r7_c7
!n1_r9_c9 !n1_r7_c8
!n1_r9_c9 !n1_r8_c7
!n1_r9_c9 !n1_r8_c8
!n2_r9_c9 !n3_r9_c9
!n2_r9_c9 !n4_r9_c9
!n2_r9_c9 !n5_r9_c9
!n2_r9_c9 !n6_r9_c9
!n2_r9_c9 !n7_r9_c9
!n2_r9_c9 !n8_r9_c9
!n2_r9_c9 !n9_r9_c9
!n2_r9_c9 !n2_r7_c7
!n2_r9_c9 !n2_r7_c8
!n2_r9_c9 !n2_r8_c7
!n2_r9_c9 !n2_r8_c8
!n3_r9_c9 !n4_r9_c9
!n3_r9_c9 !n5_r9_c9
!n3_r9_c9 !n6_r9_c9
!n3_r9_c9 !n7_r9_c9
!n3_r9_c9 !n8_r9_c9
!n3_r9_c9 !n9_r9_c9
!n3_r9_c9 !n3_r7_c7
!n3_r9_c9 !n3_r7_c8
!n3_r9_c9 !n3_r8_c7
!n3_r9_c9 !n3_r8_c8
!n4_r9_c9 !n5_r9_c9
!n4_r9_c9 !n6_r9_c9
!n4_r9_c9 !n7_r9_c9
!n4_r9_c9 !n8_r9_c9
!n4_r9_c9 !n9_r9_c9
!n4_r9_c9 !n4_r7_c7
!n4_r9_c9 !n4_r7_c8
!n4_r9_c9 !n4_r8_c7
!n4_r9_c9 !n4_r8_c8
!n5_r9_c9 !n6_r9_c9
!n5_r9_c9 !n7_r9_c9
!n5_r9_c9 !n8_r9_c9
!n5_r9_c9 !n9_r9_c9
!n5_r9_c9 !n5_r7_c7
!n5_r9_c9 !n5_r7_c8
!n5_r9_c9 !n5_r8_c7
!n5_r9_c9 !n5_r8_c8
!n6_r9_c9 !n7_r9_c9
!n6_r9_c9 !n8_r9_c9
!n6_r9_c9 !n9_r9_c9
!n6_r9_c9 !n6_r7_c7
!n6_r9_c9 !n6_r7_c8
!n6_r9_c9 !n6_r8_c7
!n6_r9_c9 !n6_r8_c8
!n7_r9_c9 !n8_r9_c9
!n7_r9_c9 !n9_r9_c9
!n7_r9_c9 !n7_r7_c7
!n7_r9_c9 !n7_r7_c8
!n7_r9_c9 !n7_r8_c7
!n7_r9_c9 !n7_r8_c8
!n8_r9_c9 !n9_r9_c9
!n8_r9_c9 !n8_r7_c7
!n8_r9_c9 !n8_r7_c8
!n8_r9_c9 !n8_r8_c7
!n8_r9_c9 !n8_r8_c8
!n9_r9_c9 !n9_r7_c7
!n9_r9_c9 !n9_r7_c8
!n9_r9_c9 !n9_r8_c7
!n9_r9_c9 !n9_r8_c8
n1_r9_c9 n2_r9_c9 n3_r9_c9 n4_r9_c9 n5_r9_c9 n6_r9_c9 n7_r9_c9 n8_r9_c9 n9_r9_c9
Easy Case, Pure Literal n4_r1_c1 = True
Easy Case, Unit Literal n1_r1_c1
Easy Case, Pure Literal n2_r1_c2 = True
Easy Case, Unit Literal n2_r1_c1
Easy Case, Pure Literal n8_r1_c4 = True
Easy Case, Unit Literal n3_r1_c1
Easy Case, Pure Literal n7_r1_c5 = True
Easy Case, Unit Literal n5_r1_c1
Easy Case, Pure Literal n5_r1_c6 = True
Easy Case, Unit Literal n6_r1_c1
Easy Case, Pure Literal n3_r1_c7 = True
Easy Case, Unit Literal n7_r1_c1
Easy Case, Pure Literal n6_r1_c9 = True
Easy Case, Unit Literal n8_r1_c1
Easy Case, Pure Literal n3_r2_c1 = True
Easy Case, Unit Literal n9_r1_c1
Easy Case, Pure Literal n9_r2_c2 = True
Easy Case, Unit Literal n1_r1_c2
Easy Case, Pure Literal n8_r2_c3 = True
Easy Case, Unit Literal n3_r1_c2
Easy Case, Pure Literal n4_r2_c5 = True
Easy Case, Unit Literal n4_r1_c2
Easy Case, Pure Literal n6_r2_c6 = True
Easy Case, Unit Literal n5_r1_c2
Easy Case, Pure Literal n7_r2_c7 = True
Easy Case, Unit Literal n6_r1_c2
Easy Case, Pure Literal n1_r2_c9 = True
Easy Case, Unit Literal n7_r1_c2
Easy Case, Pure Literal n5_r3_c3 = True
Easy Case, Unit Literal n8_r1_c2
Easy Case, Pure Literal n8_r3_c7 = True
Easy Case, Unit Literal n9_r1_c2
Easy Case, Pure Literal n2_r3_c9 = True
Easy Case, Unit Literal n1_r1_c4
Easy Case, Pure Literal n9_r4_c1 = True
Easy Case, Unit Literal n2_r1_c4
Easy Case, Pure Literal n6_r4_c3 = True
Easy Case, Unit Literal n3_r1_c4
Easy Case, Pure Literal n4_r4_c4 = True
Easy Case, Unit Literal n4_r1_c4
Easy Case, Pure Literal n5_r4_c5 = True
Easy Case, Unit Literal n5_r1_c4
Easy Case, Pure Literal n1_r5_c6 = True
Easy Case, Unit Literal n6_r1_c4
Easy Case, Pure Literal n8_r5_c8 = True
Easy Case, Unit Literal n7_r1_c4
Easy Case, Pure Literal n1_r6_c1 = True
Easy Case, Unit Literal n9_r1_c4
Easy Case, Pure Literal n4_r6_c2 = True
Easy Case, Unit Literal n1_r1_c5
Easy Case, Pure Literal n6_r6_c7 = True
Easy Case, Unit Literal n2_r1_c5
Easy Case, Pure Literal n2_r7_c5 = True
Easy Case, Unit Literal n3_r1_c5
Easy Case, Pure Literal n4_r7_c6 = True
Easy Case, Unit Literal n4_r1_c5
Easy Case, Pure Literal n5_r7_c7 = True
Easy Case, Unit Literal n5_r1_c5
Easy Case, Pure Literal n3_r7_c8 = True
Easy Case, Unit Literal n6_r1_c5
Easy Case, Pure Literal n3_r8_c2 = True
Easy Case, Unit Literal n8_r1_c5
Easy Case, Pure Literal n1_r8_c4 = True
Easy Case, Unit Literal n9_r1_c5
Easy Case, Pure Literal n8_r8_c5 = True
Easy Case, Unit Literal n1_r1_c6
Easy Case, Pure Literal n6_r8_c8 = True
Easy Case, Unit Literal n2_r1_c6
Easy Case, Pure Literal n9_r9_c3 = True
Easy Case, Unit Literal n3_r1_c6
Easy Case, Pure Literal n4_r9_c7 = True
Easy Case, Unit Literal n4_r1_c6
Easy Case, Pure Literal n4_r1_c3 = False
Easy Case, Unit Literal n6_r1_c6
Easy Case, Pure Literal n4_r1_c7 = False
Easy Case, Unit Literal n7_r1_c6
Easy Case, Pure Literal n4_r1_c8 = False
Easy Case, Unit Literal n8_r1_c6
Easy Case, Pure Literal n4_r1_c9 = False
Easy Case, Unit Literal n9_r1_c6
Easy Case, Pure Literal n4_r2_c1 = False
Easy Case, Unit Literal n1_r1_c7
Easy Case, Pure Literal n4_r3_c1 = False
Easy Case, Unit Literal n2_r1_c7
Easy Case, Pure Literal n4_r4_c1 = False
Easy Case, Unit Literal n5_r1_c7
Easy Case, Pure Literal n4_r5_c1 = False
Easy Case, Unit Literal n6_r1_c7
Easy Case, Pure Literal n4_r6_c1 = False
Easy Case, Unit Literal n7_r1_c7
Easy Case, Pure Literal n4_r7_c1 = False
Easy Case, Unit Literal n8_r1_c7
Easy Case, Pure Literal n4_r8_c1 = False
Easy Case, Unit Literal n9_r1_c7
Easy Case, Pure Literal n4_r9_c1 = False
Easy Case, Unit Literal n1_r1_c9
Easy Case, Pure Literal n4_r2_c2 = False
Easy Case, Unit Literal n2_r1_c9
Easy Case, Pure Literal n4_r2_c3 = False
Easy Case, Unit Literal n3_r1_c9
Easy Case, Pure Literal n4_r3_c2 = False
Easy Case, Unit Literal n5_r1_c9
Easy Case, Pure Literal n4_r3_c3 = False
Easy Case, Unit Literal n7_r1_c9
Easy Case, Pure Literal n2_r1_c3 = False
Easy Case, Unit Literal n8_r1_c9
Easy Case, Pure Literal n2_r1_c8 = False
Easy Case, Unit Literal n9_r1_c9
Easy Case, Pure Literal n2_r2_c2 = False
Easy Case, Unit Literal n1_r2_c1
Easy Case, Pure Literal n2_r3_c2 = False
Easy Case, Unit Literal n2_r2_c1
Easy Case, Pure Literal n2_r4_c2 = False
Easy Case, Unit Literal n5_r2_c1
Easy Case, Pure Literal n2_r5_c2 = False
Easy Case, Unit Literal n6_r2_c1
Easy Case, Pure Literal n2_r6_c2 = False
Easy Case, Unit Literal n7_r2_c1
Easy Case, Pure Literal n2_r7_c2 = False
Easy Case, Unit Literal n8_r2_c1
Easy Case, Pure Literal n2_r8_c2 = False
Easy Case, Unit Literal n9_r2_c1
Easy Case, Pure Literal n2_r9_c2 = False
Easy Case, Unit Literal n1_r2_c2
Easy Case, Pure Literal n2_r2_c3 = False
Easy Case, Unit Literal n3_r2_c2
Easy Case, Pure Literal n2_r3_c1 = False
Easy Case, Unit Literal n5_r2_c2
Easy Case, Pure Literal n2_r3_c3 = False
Easy Case, Unit Literal n6_r2_c2
Easy Case, Pure Literal n3_r1_c3 = False
Easy Case, Unit Literal n7_r2_c2
Easy Case, Pure Literal n5_r1_c3 = False
Easy Case, Unit Literal n8_r2_c2
Easy Case, Pure Literal n6_r1_c3 = False
Easy Case, Unit Literal n1_r2_c3
Easy Case, Pure Literal n7_r1_c3 = False
Easy Case, Unit Literal n3_r2_c3
Easy Case, Pure Literal n8_r1_c3 = False
Easy Case, Unit Literal n5_r2_c3
Easy Case, Pure Literal n9_r1_c3 = False
Easy Case, Unit Literal n6_r2_c3
Easy Case, Pure Literal n1_r1_c3 = True
Easy Case, Unit Literal n7_r2_c3
Easy Case, Pure Literal n1_r1_c8 = False
Easy Case, Unit Literal n9_r2_c3
Easy Case, Pure Literal n1_r3_c3 = False
Easy Case, Unit Literal n1_r2_c5
Easy Case, Pure Literal n1_r4_c3 = False
Easy Case, Unit Literal n2_r2_c5
Easy Case, Pure Literal n1_r5_c3 = False
Easy Case, Unit Literal n3_r2_c5
Easy Case, Pure Literal n1_r6_c3 = False
Easy Case, Unit Literal n5_r2_c5
Easy Case, Pure Literal n1_r7_c3 = False
Easy Case, Unit Literal n6_r2_c5
Easy Case, Pure Literal n1_r8_c3 = False
Easy Case, Unit Literal n7_r2_c5
Easy Case, Pure Literal n1_r9_c3 = False
Easy Case, Unit Literal n8_r2_c5
Easy Case, Pure Literal n1_r3_c1 = False
Easy Case, Unit Literal n9_r2_c5
Easy Case, Pure Literal n1_r3_c2 = False
Easy Case, Unit Literal n1_r2_c6
Easy Case, Pure Literal n8_r1_c8 = False
Easy Case, Unit Literal n2_r2_c6
Easy Case, Pure Literal n8_r2_c4 = False
Easy Case, Unit Literal n3_r2_c6
Easy Case, Pure Literal n8_r3_c4 = False
Easy Case, Unit Literal n4_r2_c6
Easy Case, Pure Literal n8_r4_c4 = False
Easy Case, Unit Literal n5_r2_c6
Easy Case, Pure Literal n8_r5_c4 = False
Easy Case, Unit Literal n7_r2_c6
Easy Case, Pure Literal n8_r6_c4 = False
Easy Case, Unit Literal n8_r2_c6
Easy Case, Pure Literal n8_r7_c4 = False
Easy Case, Unit Literal n9_r2_c6
Easy Case, Pure Literal n8_r8_c4 = False
Easy Case, Unit Literal n1_r2_c7
Easy Case, Pure Literal n8_r9_c4 = False
Easy Case, Unit Literal n2_r2_c7
Easy Case, Pure Literal n8_r3_c5 = False
Easy Case, Unit Literal n3_r2_c7
Easy Case, Pure Literal n8_r3_c6 = False
Easy Case, Unit Literal n4_r2_c7
Easy Case, Pure Literal n7_r1_c8 = False
Easy Case, Unit Literal n5_r2_c7
Easy Case, Pure Literal n7_r3_c5 = False
Easy Case, Unit Literal n6_r2_c7
Easy Case, Pure Literal n7_r4_c5 = False
Easy Case, Unit Literal n8_r2_c7
Easy Case, Pure Literal n7_r5_c5 = False
Easy Case, Unit Literal n9_r2_c7
Easy Case, Pure Literal n7_r6_c5 = False
Easy Case, Unit Literal n2_r2_c9
Easy Case, Pure Literal n7_r7_c5 = False
Easy Case, Unit Literal n3_r2_c9
Easy Case, Pure Literal n7_r8_c5 = False
Easy Case, Unit Literal n4_r2_c9
Easy Case, Pure Literal n7_r9_c5 = False
Easy Case, Unit Literal n5_r2_c9
Easy Case, Pure Literal n7_r2_c4 = False
Easy Case, Unit Literal n6_r2_c9
Easy Case, Pure Literal n7_r3_c4 = False
Easy Case, Unit Literal n7_r2_c9
Easy Case, Pure Literal n7_r3_c6 = False
Easy Case, Unit Literal n8_r2_c9
Easy Case, Pure Literal n5_r1_c8 = False
Easy Case, Unit Literal n9_r2_c9
Easy Case, Pure Literal n5_r3_c6 = False
Easy Case, Unit Literal n3_r3_c3
Easy Case, Pure Literal n5_r4_c6 = False
Easy Case, Unit Literal n6_r3_c3
Easy Case, Pure Literal n5_r5_c6 = False
Easy Case, Unit Literal n7_r3_c3
Easy Case, Pure Literal n5_r6_c6 = False
Easy Case, Unit Literal n8_r3_c3
Easy Case, Pure Literal n5_r7_c6 = False
Easy Case, Unit Literal n9_r3_c3
Easy Case, Pure Literal n5_r8_c6 = False
Easy Case, Unit Literal n1_r3_c7
Easy Case, Pure Literal n5_r9_c6 = False
Easy Case, Unit Literal n2_r3_c7
Easy Case, Pure Literal n5_r2_c4 = False
Easy Case, Unit Literal n3_r3_c7
Easy Case, Pure Literal n5_r3_c4 = False
Easy Case, Unit Literal n4_r3_c7
Easy Case, Pure Literal n5_r3_c5 = False
Easy Case, Unit Literal n5_r3_c7
Easy Case, Pure Literal n3_r1_c8 = False
Easy Case, Unit Literal n6_r3_c7
Easy Case, Pure Literal n3_r4_c7 = False
Easy Case, Unit Literal n7_r3_c7
Easy Case, Pure Literal n3_r5_c7 = False
Easy Case, Unit Literal n9_r3_c7
Easy Case, Pure Literal n3_r6_c7 = False
Easy Case, Unit Literal n1_r3_c9
Easy Case, Pure Literal n3_r7_c7 = False
Easy Case, Unit Literal n3_r3_c9
Easy Case, Pure Literal n3_r8_c7 = False
Easy Case, Unit Literal n4_r3_c9
Easy Case, Pure Literal n3_r9_c7 = False
Easy Case, Unit Literal n5_r3_c9
Easy Case, Pure Literal n3_r2_c8 = False
Easy Case, Unit Literal n6_r3_c9
Easy Case, Pure Literal n3_r3_c8 = False
Easy Case, Unit Literal n7_r3_c9
Easy Case, Pure Literal n6_r1_c8 = False
Easy Case, Unit Literal n8_r3_c9
Easy Case, Pure Literal n9_r1_c8 = True
Easy Case, Unit Literal n9_r3_c9
Easy Case, Pure Literal n9_r2_c8 = False
Easy Case, Unit Literal n1_r4_c1
Easy Case, Pure Literal n9_r3_c8 = False
Easy Case, Unit Literal n2_r4_c1
Easy Case, Pure Literal n9_r4_c8 = False
Easy Case, Unit Literal n3_r4_c1
Easy Case, Pure Literal n9_r5_c8 = False
Easy Case, Unit Literal n5_r4_c1
Easy Case, Pure Literal n9_r6_c8 = False
Easy Case, Unit Literal n6_r4_c1
Easy Case, Pure Literal n9_r7_c8 = False
Easy Case, Unit Literal n7_r4_c1
Easy Case, Pure Literal n9_r8_c8 = False
Easy Case, Unit Literal n8_r4_c1
Easy Case, Pure Literal n9_r9_c8 = False
Easy Case, Unit Literal n2_r4_c3
Easy Case, Pure Literal n6_r4_c9 = False
Easy Case, Unit Literal n3_r4_c3
Easy Case, Pure Literal n6_r5_c9 = False
Easy Case, Unit Literal n4_r4_c3
Easy Case, Pure Literal n6_r6_c9 = False
Easy Case, Unit Literal n5_r4_c3
Easy Case, Pure Literal n6_r7_c9 = False
Easy Case, Unit Literal n7_r4_c3
Easy Case, Pure Literal n6_r8_c9 = False
Easy Case, Unit Literal n8_r4_c3
Easy Case, Pure Literal n6_r9_c9 = False
Easy Case, Unit Literal n9_r4_c3
Easy Case, Pure Literal n6_r2_c8 = False
Easy Case, Unit Literal n1_r4_c4
Easy Case, Pure Literal n6_r3_c8 = False
Easy Case, Unit Literal n2_r4_c4
Easy Case, Pure Literal n3_r2_c4 = False
Easy Case, Unit Literal n3_r4_c4
Easy Case, Pure Literal n3_r3_c1 = False
Easy Case, Unit Literal n5_r4_c4
Easy Case, Pure Literal n3_r5_c1 = False
Easy Case, Unit Literal n6_r4_c4
Easy Case, Pure Literal n3_r6_c1 = False
Easy Case, Unit Literal n7_r4_c4
Easy Case, Pure Literal n3_r7_c1 = False
Easy Case, Unit Literal n9_r4_c4
Easy Case, Pure Literal n3_r8_c1 = False
Easy Case, Unit Literal n1_r4_c5
Easy Case, Pure Literal n3_r9_c1 = False
Easy Case, Unit Literal n2_r4_c5
Easy Case, Pure Literal n3_r3_c2 = False
Easy Case, Unit Literal n3_r4_c5
Easy Case, Pure Literal n9_r2_c4 = False
Easy Case, Unit Literal n4_r4_c5
Easy Case, Pure Literal n9_r3_c2 = False
Easy Case, Unit Literal n6_r4_c5
Easy Case, Pure Literal n9_r4_c2 = False
Easy Case, Unit Literal n8_r4_c5
Easy Case, Pure Literal n9_r5_c2 = False
Easy Case, Unit Literal n9_r4_c5
Easy Case, Pure Literal n9_r6_c2 = False
Easy Case, Unit Literal n2_r5_c6
Easy Case, Pure Literal n9_r7_c2 = False
Easy Case, Unit Literal n3_r5_c6
Easy Case, Pure Literal n9_r8_c2 = False
Easy Case, Unit Literal n4_r5_c6
Easy Case, Pure Literal n9_r9_c2 = False
Easy Case, Unit Literal n6_r5_c6
Easy Case, Pure Literal n9_r3_c1 = False
Easy Case, Unit Literal n7_r5_c6
Easy Case, Pure Literal n8_r2_c8 = False
Easy Case, Unit Literal n8_r5_c6
Easy Case, Pure Literal n8_r5_c3 = False
Easy Case, Unit Literal n9_r5_c6
Easy Case, Pure Literal n8_r6_c3 = False
Easy Case, Unit Literal n1_r5_c8
Easy Case, Pure Literal n8_r7_c3 = False
Easy Case, Unit Literal n2_r5_c8
Easy Case, Pure Literal n8_r8_c3 = False
Easy Case, Unit Literal n3_r5_c8
Easy Case, Pure Literal n8_r9_c3 = False
Easy Case, Unit Literal n4_r5_c8
Easy Case, Pure Literal n8_r3_c1 = False
Easy Case, Unit Literal n5_r5_c8
Easy Case, Pure Literal n8_r3_c2 = False
Easy Case, Unit Literal n6_r5_c8
Easy Case, Pure Literal n1_r2_c4 = False
Easy Case, Unit Literal n7_r5_c8
Easy Case, Pure Literal n4_r2_c4 = False
Easy Case, Unit Literal n2_r6_c1
Easy Case, Pure Literal n6_r2_c4 = False
Easy Case, Unit Literal n5_r6_c1
Easy Case, Pure Literal n2_r2_c4 = True
Easy Case, Unit Literal n6_r6_c1
Easy Case, Pure Literal n2_r2_c8 = False
Easy Case, Unit Literal n7_r6_c1
Easy Case, Pure Literal n2_r3_c4 = False
Easy Case, Unit Literal n8_r6_c1
Easy Case, Pure Literal n2_r5_c4 = False
Easy Case, Unit Literal n9_r6_c1
Easy Case, Pure Literal n2_r6_c4 = False
Easy Case, Unit Literal n1_r6_c2
Easy Case, Pure Literal n2_r7_c4 = False
Easy Case, Unit Literal n3_r6_c2
Easy Case, Pure Literal n2_r8_c4 = False
Easy Case, Unit Literal n5_r6_c2
Easy Case, Pure Literal n2_r9_c4 = False
Easy Case, Unit Literal n6_r6_c2
Easy Case, Pure Literal n2_r3_c5 = False
Easy Case, Unit Literal n7_r6_c2
Easy Case, Pure Literal n2_r3_c6 = False
Easy Case, Unit Literal n8_r6_c2
Easy Case, Pure Literal n4_r2_c8 = False
Easy Case, Unit Literal n1_r6_c7
Easy Case, Pure Literal n4_r3_c5 = False
Easy Case, Unit Literal n2_r6_c7
Easy Case, Pure Literal n4_r5_c5 = False
Easy Case, Unit Literal n4_r6_c7
Easy Case, Pure Literal n4_r6_c5 = False
Easy Case, Unit Literal n5_r6_c7
Easy Case, Pure Literal n4_r7_c5 = False
Easy Case, Unit Literal n7_r6_c7
Easy Case, Pure Literal n4_r8_c5 = False
Easy Case, Unit Literal n8_r6_c7
Easy Case, Pure Literal n4_r9_c5 = False
Easy Case, Unit Literal n9_r6_c7
Easy Case, Pure Literal n4_r3_c4 = False
Easy Case, Unit Literal n1_r7_c5
Easy Case, Pure Literal n4_r3_c6 = False
Easy Case, Unit Literal n3_r7_c5
Easy Case, Pure Literal n6_r3_c6 = False
Easy Case, Unit Literal n5_r7_c5
Easy Case, Pure Literal n6_r4_c6 = False
Easy Case, Unit Literal n6_r7_c5
Easy Case, Pure Literal n6_r6_c6 = False
Easy Case, Unit Literal n8_r7_c5
Easy Case, Pure Literal n6_r7_c6 = False
Easy Case, Unit Literal n9_r7_c5
Easy Case, Pure Literal n6_r8_c6 = False
Easy Case, Unit Literal n1_r7_c6
Easy Case, Pure Literal n6_r9_c6 = False
Easy Case, Unit Literal n2_r7_c6
Easy Case, Pure Literal n6_r3_c4 = False
Easy Case, Unit Literal n3_r7_c6
Easy Case, Pure Literal n6_r3_c5 = False
Easy Case, Unit Literal n7_r7_c6
Easy Case, Pure Literal n7_r2_c8 = False
Easy Case, Unit Literal n8_r7_c6
Easy Case, Pure Literal n7_r4_c7 = False
Easy Case, Unit Literal n9_r7_c6
Easy Case, Pure Literal n7_r5_c7 = False
Easy Case, Unit Literal n1_r7_c7
Easy Case, Pure Literal n7_r7_c7 = False
Easy Case, Unit Literal n2_r7_c7
Easy Case, Pure Literal n7_r8_c7 = False
Easy Case, Unit Literal n4_r7_c7
Easy Case, Pure Literal n7_r9_c7 = False
Easy Case, Unit Literal n6_r7_c7
Easy Case, Pure Literal n7_r3_c8 = False
Easy Case, Unit Literal n8_r7_c7
Easy Case, Pure Literal n1_r2_c8 = False
Easy Case, Unit Literal n9_r7_c7
Easy Case, Pure Literal n5_r2_c8 = True
Easy Case, Unit Literal n1_r7_c8
Easy Case, Pure Literal n5_r3_c8 = False
Easy Case, Unit Literal n2_r7_c8
Easy Case, Pure Literal n5_r4_c8 = False
Easy Case, Unit Literal n4_r7_c8
Easy Case, Pure Literal n5_r6_c8 = False
Easy Case, Unit Literal n5_r7_c8
Easy Case, Pure Literal n5_r8_c8 = False
Easy Case, Unit Literal n6_r7_c8
Easy Case, Pure Literal n5_r9_c8 = False
Easy Case, Unit Literal n7_r7_c8
Easy Case, Pure Literal n1_r4_c9 = False
Easy Case, Unit Literal n8_r7_c8
Easy Case, Pure Literal n1_r5_c9 = False
Easy Case, Unit Literal n1_r8_c2
Easy Case, Pure Literal n1_r6_c9 = False
Easy Case, Unit Literal n4_r8_c2
Easy Case, Pure Literal n1_r7_c9 = False
Easy Case, Unit Literal n5_r8_c2
Easy Case, Pure Literal n1_r8_c9 = False
Easy Case, Unit Literal n6_r8_c2
Easy Case, Pure Literal n1_r9_c9 = False
Easy Case, Unit Literal n7_r8_c2
Easy Case, Pure Literal n1_r3_c8 = False
Easy Case, Unit Literal n8_r8_c2
Easy Case, Pure Literal n5_r3_c1 = False
Easy Case, Unit Literal n3_r8_c4
Easy Case, Pure Literal n5_r3_c2 = False
Easy Case, Unit Literal n4_r8_c4
Easy Case, Pure Literal n5_r5_c3 = False
Easy Case, Unit Literal n5_r8_c4
Easy Case, Pure Literal n5_r6_c3 = False
Easy Case, Unit Literal n6_r8_c4
Easy Case, Pure Literal n5_r7_c3 = False
Easy Case, Unit Literal n7_r8_c4
Easy Case, Pure Literal n5_r8_c3 = False
Easy Case, Unit Literal n9_r8_c4
Easy Case, Pure Literal n5_r9_c3 = False
Easy Case, Unit Literal n1_r8_c5
Easy Case, Pure Literal n1_r3_c4 = False
Easy Case, Unit Literal n2_r8_c5
Easy Case, Pure Literal n1_r3_c6 = False
Easy Case, Unit Literal n3_r8_c5
Easy Case, Pure Literal n8_r3_c8 = False
Easy Case, Unit Literal n5_r8_c5
Easy Case, Pure Literal n8_r4_c7 = False
Easy Case, Unit Literal n6_r8_c5
Easy Case, Pure Literal n8_r5_c7 = False
Easy Case, Unit Literal n9_r8_c5
Easy Case, Pure Literal n8_r8_c7 = False
Easy Case, Unit Literal n1_r8_c8
Easy Case, Pure Literal n8_r9_c7 = False
Easy Case, Unit Literal n2_r8_c8
Easy Case, Pure Literal n2_r3_c8 = False
Easy Case, Unit Literal n3_r8_c8
Easy Case, Pure Literal n4_r3_c8 = True
Easy Case, Unit Literal n4_r8_c8
Easy Case, Pure Literal n4_r4_c8 = False
Easy Case, Unit Literal n7_r8_c8
Easy Case, Pure Literal n4_r6_c8 = False
Easy Case, Unit Literal n8_r8_c8
Easy Case, Pure Literal n4_r9_c8 = False
Easy Case, Unit Literal n2_r9_c3
Easy Case, Pure Literal n2_r4_c9 = False
Easy Case, Unit Literal n3_r9_c3
Easy Case, Pure Literal n2_r5_c9 = False
Easy Case, Unit Literal n4_r9_c3
Easy Case, Pure Literal n2_r6_c9 = False
Easy Case, Unit Literal n6_r9_c3
Easy Case, Pure Literal n2_r7_c9 = False
Easy Case, Unit Literal n7_r9_c3
Easy Case, Pure Literal n2_r8_c9 = False
Easy Case, Unit Literal n1_r9_c7
Easy Case, Pure Literal n2_r9_c9 = False
Easy Case, Unit Literal n2_r9_c7
Easy Case, Pure Literal n9_r4_c6 = False
Easy Case, Unit Literal n5_r9_c7
Easy Case, Pure Literal n9_r4_c7 = False
Easy Case, Unit Literal n6_r9_c7
Easy Case, Pure Literal n9_r4_c9 = False
Easy Case, Unit Literal n9_r9_c7
Easy Case, Pure Literal n9_r5_c1 = False
Easy Case, Pure Literal n9_r7_c1 = False
Easy Case, Pure Literal n9_r8_c1 = False
Easy Case, Pure Literal n9_r9_c1 = False
Easy Case, Pure Literal n9_r5_c3 = False
Easy Case, Pure Literal n9_r6_c3 = False
Easy Case, Pure Literal n1_r4_c2 = False
Easy Case, Pure Literal n3_r4_c2 = False
Easy Case, Pure Literal n4_r4_c2 = False
Easy Case, Pure Literal n5_r4_c2 = False
Easy Case, Pure Literal n6_r4_c2 = False
Easy Case, Pure Literal n6_r4_c7 = False
Easy Case, Pure Literal n6_r4_c8 = False
Easy Case, Pure Literal n6_r5_c3 = False
Easy Case, Pure Literal n6_r6_c3 = False
Easy Case, Pure Literal n6_r7_c3 = False
Easy Case, Pure Literal n6_r8_c3 = False
Easy Case, Pure Literal n6_r5_c1 = False
Easy Case, Pure Literal n6_r5_c2 = False
Easy Case, Pure Literal n4_r4_c6 = False
Easy Case, Pure Literal n4_r4_c7 = False
Easy Case, Pure Literal n4_r4_c9 = False
Easy Case, Pure Literal n4_r5_c4 = False
Easy Case, Pure Literal n4_r6_c4 = False
Easy Case, Pure Literal n4_r7_c4 = False
Easy Case, Pure Literal n4_r9_c4 = False
Easy Case, Pure Literal n4_r6_c6 = False
Easy Case, Pure Literal n5_r4_c7 = False
Easy Case, Pure Literal n5_r4_c9 = False
Easy Case, Pure Literal n5_r5_c5 = False
Easy Case, Pure Literal n5_r6_c5 = False
Easy Case, Pure Literal n5_r9_c5 = False
Easy Case, Pure Literal n5_r5_c4 = False
Easy Case, Pure Literal n5_r6_c4 = False
Easy Case, Pure Literal n1_r4_c6 = False
Easy Case, Pure Literal n3_r4_c8 = False
Easy Case, Pure Literal n8_r4_c8 = False
Easy Case, Pure Literal n8_r4_c9 = False
Easy Case, Pure Literal n1_r5_c1 = False
Easy Case, Pure Literal n8_r5_c1 = False
Easy Case, Pure Literal n1_r5_c2 = False
Easy Case, Pure Literal n3_r5_c2 = False
Easy Case, Pure Literal n4_r5_c2 = False
Easy Case, Pure Literal n8_r5_c2 = False
Easy Case, Pure Literal n4_r5_c3 = False
Easy Case, Pure Literal n1_r5_c4 = False
Easy Case, Pure Literal n1_r5_c5 = False
Easy Case, Pure Literal n2_r5_c5 = False
Easy Case, Pure Literal n8_r5_c5 = False
Easy Case, Pure Literal n1_r5_c7 = False
Easy Case, Pure Literal n1_r6_c6 = False
Easy Case, Pure Literal n1_r8_c6 = False
Easy Case, Pure Literal n1_r9_c6 = False
Easy Case, Pure Literal n1_r6_c4 = False
Easy Case, Pure Literal n1_r6_c5 = False
Easy Case, Pure Literal n4_r5_c7 = False
Easy Case, Pure Literal n5_r5_c7 = False
Easy Case, Pure Literal n6_r5_c7 = False
Easy Case, Pure Literal n8_r5_c9 = False
Easy Case, Pure Literal n8_r6_c8 = False
Easy Case, Pure Literal n8_r9_c8 = False
Easy Case, Pure Literal n8_r6_c9 = False
Easy Case, Pure Literal n1_r6_c8 = False
Easy Case, Pure Literal n1_r7_c1 = False
Easy Case, Pure Literal n1_r8_c1 = False
Easy Case, Pure Literal n1_r9_c1 = False
Easy Case, Pure Literal n4_r6_c3 = False
Easy Case, Pure Literal n4_r6_c9 = False
Easy Case, Pure Literal n4_r7_c2 = False
Easy Case, Pure Literal n4_r9_c2 = False
Easy Case, Pure Literal n6_r6_c4 = False
Easy Case, Pure Literal n2_r6_c5 = False
Easy Case, Pure Literal n6_r6_c5 = False
Easy Case, Pure Literal n8_r6_c5 = False
Easy Case, Pure Literal n6_r6_c8 = False
Easy Case, Pure Literal n6_r8_c7 = False
Easy Case, Pure Literal n3_r6_c8 = False
Easy Case, Pure Literal n2_r7_c1 = False
Easy Case, Pure Literal n5_r7_c1 = False
Easy Case, Pure Literal n3_r7_c2 = False
Easy Case, Pure Literal n5_r7_c2 = False
Easy Case, Pure Literal n2_r7_c3 = False
Easy Case, Pure Literal n3_r7_c3 = False
Easy Case, Pure Literal n4_r7_c3 = False
Easy Case, Pure Literal n9_r7_c3 = False
Easy Case, Pure Literal n7_r7_c3 = True
Easy Case, Pure Literal n7_r5_c3 = False
Easy Case, Pure Literal n7_r6_c3 = False
Easy Case, Pure Literal n7_r7_c1 = False
Easy Case, Pure Literal n7_r7_c2 = False
Easy Case, Pure Literal n7_r7_c4 = False
Easy Case, Pure Literal n7_r7_c9 = False
Easy Case, Pure Literal n7_r8_c3 = False
Easy Case, Pure Literal n7_r8_c1 = False
Easy Case, Pure Literal n7_r9_c1 = False
Easy Case, Pure Literal n7_r9_c2 = False
Easy Case, Pure Literal n1_r7_c4 = False
Easy Case, Pure Literal n3_r7_c4 = False
Easy Case, Pure Literal n5_r7_c4 = False
Easy Case, Pure Literal n2_r9_c5 = False
Easy Case, Pure Literal n2_r8_c6 = False
Easy Case, Pure Literal n2_r9_c6 = False
Easy Case, Pure Literal n4_r7_c9 = False
Easy Case, Pure Literal n4_r8_c6 = False
Easy Case, Pure Literal n4_r9_c6 = False
Easy Case, Pure Literal n5_r7_c9 = False
Easy Case, Pure Literal n5_r8_c7 = False
Easy Case, Pure Literal n5_r8_c9 = False
Easy Case, Pure Literal n5_r9_c9 = False
Easy Case, Pure Literal n3_r7_c9 = False
Easy Case, Pure Literal n3_r9_c8 = False
Easy Case, Pure Literal n3_r8_c9 = False
Easy Case, Pure Literal n3_r9_c9 = False
Easy Case, Pure Literal n6_r8_c1 = False
Easy Case, Pure Literal n8_r8_c1 = False
Easy Case, Pure Literal n3_r8_c3 = False
Easy Case, Pure Literal n3_r8_c6 = False
Easy Case, Pure Literal n3_r9_c2 = False
Easy Case, Pure Literal n9_r8_c3 = False
Easy Case, Pure Literal n1_r8_c7 = False
Easy Case, Pure Literal n1_r9_c4 = False
Easy Case, Pure Literal n1_r9_c5 = False
Easy Case, Pure Literal n8_r8_c6 = False
Easy Case, Pure Literal n8_r8_c9 = False
Easy Case, Pure Literal n8_r9_c5 = False
Easy Case, Pure Literal n8_r9_c6 = False
Easy Case, Pure Literal n4_r8_c7 = False
Easy Case, Pure Literal n6_r9_c8 = False
Easy Case, Pure Literal n4_r8_c9 = False
Easy Case, Pure Literal n9_r9_c4 = False
Easy Case, Pure Literal n9_r9_c5 = False
Easy Case, Pure Literal n9_r9_c6 = False
Easy Case, Pure Literal n9_r9_c9 = False
Easy Case, Pure Literal n4_r9_c9 = False
Hard Case, Guess: n6_r3_c1 = True
Easy Case, Unit Literal n7_r3_c1
Easy Case, Pure Literal n6_r3_c2 = False
Easy Case, Pure Literal n6_r7_c1 = False
Easy Case, Pure Literal n6_r9_c1 = False
Easy Case, Pure Literal n7_r3_c2 = True
Easy Case, Pure Literal n7_r4_c2 = False
Easy Case, Pure Literal n7_r5_c2 = False
Easy Case, Pure Literal n8_r4_c2 = True
Easy Case, Pure Literal n8_r4_c6 = False
Easy Case, Pure Literal n8_r7_c2 = False
Easy Case, Pure Literal n8_r9_c2 = False
Easy Case, Pure Literal n5_r5_c2 = True
Easy Case, Pure Literal n5_r5_c1 = False
Easy Case, Pure Literal n5_r5_c9 = False
Easy Case, Pure Literal n5_r9_c2 = False
Easy Case, Pure Literal n8_r7_c1 = True
Easy Case, Pure Literal n8_r7_c9 = False
Easy Case, Pure Literal n8_r9_c1 = False
Easy Case, Pure Literal n9_r7_c9 = True
Easy Case, Pure Literal n9_r5_c9 = False
Easy Case, Pure Literal n9_r6_c9 = False
Easy Case, Pure Literal n9_r7_c4 = False
Easy Case, Pure Literal n6_r7_c4 = True
Easy Case, Pure Literal n6_r5_c4 = False
Easy Case, Pure Literal n6_r7_c2 = False
Easy Case, Pure Literal n1_r7_c2 = True
Easy Case, Pure Literal n1_r9_c2 = False
Easy Case, Pure Literal n6_r9_c4 = False
Easy Case, Pure Literal n6_r9_c5 = False
Easy Case, Unit Literal n6_r9_c2
Easy Case, Pure Literal n9_r8_c9 = False
Easy Case, Pure Literal n9_r8_c7 = False
Easy Case, Pure Literal n2_r8_c7 = True
Easy Case, Pure Literal n2_r4_c7 = False
Easy Case, Pure Literal n1_r4_c7 = True
Easy Case, Pure Literal n1_r4_c8 = False
Easy Case, Pure Literal n2_r5_c7 = False
Easy Case, Pure Literal n9_r5_c7 = True
Easy Case, Pure Literal n9_r5_c4 = False
Easy Case, Pure Literal n9_r5_c5 = False
Easy Case, Pure Literal n2_r8_c1 = False
Easy Case, Pure Literal n5_r8_c1 = True
Easy Case, Pure Literal n5_r9_c1 = False
Easy Case, Pure Literal n2_r8_c3 = False
Easy Case, Unit Literal n4_r8_c3
Easy Case, Pure Literal n2_r9_c8 = False
Easy Case, Pure Literal n7_r8_c9 = True
Easy Case, Pure Literal n7_r4_c9 = False
Easy Case, Pure Literal n3_r4_c9 = True
Easy Case, Pure Literal n3_r4_c6 = False
Easy Case, Pure Literal n3_r5_c9 = False
Easy Case, Pure Literal n3_r6_c9 = False
Easy Case, Pure Literal n7_r5_c9 = False
Easy Case, Unit Literal n4_r5_c9
Easy Case, Pure Literal n7_r6_c9 = False
Easy Case, Unit Literal n5_r6_c9
Easy Case, Pure Literal n7_r8_c6 = False
Easy Case, Pure Literal n9_r8_c6 = True
Easy Case, Pure Literal n9_r3_c6 = False
Easy Case, Pure Literal n3_r3_c6 = True
Easy Case, Pure Literal n3_r3_c4 = False
Easy Case, Pure Literal n9_r3_c4 = True
Easy Case, Pure Literal n9_r3_c5 = False
Easy Case, Pure Literal n9_r6_c4 = False
Easy Case, Pure Literal n3_r3_c5 = False
Easy Case, Unit Literal n1_r3_c5
Easy Case, Pure Literal n3_r6_c6 = False
Easy Case, Pure Literal n3_r9_c6 = False
Easy Case, Pure Literal n9_r6_c6 = False
Easy Case, Pure Literal n7_r9_c9 = False
Easy Case, Unit Literal n8_r9_c9
Easy Case, Pure Literal n7_r9_c8 = False
Easy Case, Unit Literal n1_r9_c8
Easy Case, Pure Literal n2_r9_c1 = True
Easy Case, Pure Literal n2_r5_c1 = False
Easy Case, Pure Literal n7_r5_c1 = True
Easy Case, Pure Literal n7_r5_c4 = False
Easy Case, Pure Literal n3_r5_c4 = True
Easy Case, Pure Literal n3_r5_c3 = False
Easy Case, Pure Literal n2_r5_c3 = True
Easy Case, Pure Literal n2_r6_c3 = False
Easy Case, Pure Literal n3_r5_c5 = False
Easy Case, Unit Literal n6_r5_c5
Easy Case, Pure Literal n3_r6_c4 = False
Easy Case, Pure Literal n3_r9_c4 = False
Easy Case, Pure Literal n3_r6_c5 = False
Easy Case, Unit Literal n3_r6_c3
Easy Case, Pure Literal n7_r6_c4 = True
Easy Case, Unit Literal n9_r6_c5
Easy Case, Pure Literal n7_r4_c6 = False
Easy Case, Unit Literal n3_r9_c5
Easy Case, Pure Literal n2_r4_c6 = True
Easy Case, Pure Literal n2_r4_c8 = False
Easy Case, Pure Literal n2_r6_c6 = False
Easy Case, Pure Literal n7_r4_c8 = True
Easy Case, Pure Literal n7_r6_c8 = False
Easy Case, Unit Literal n2_r6_c8
Easy Case, Pure Literal n7_r6_c6 = False
Easy Case, Unit Literal n8_r6_c6
Easy Case, Pure Literal n7_r9_c4 = False
Easy Case, Unit Literal n5_r9_c4
Easy Case, Pure Literal n7_r9_c6 = True
SUCCESS
n1_r1_c1 = false
n2_r1_c1 = false
n3_r1_c1 = false
n4_r1_c1 = true
n5_r1_c1 = false
n6_r1_c1 = false
n7_r1_c1 = false
n8_r1_c1 = false
n9_r1_c1 = false
n1_r1_c2 = false
n2_r1_c2 = true
n3_r1_c2 = false
n4_r1_c2 = false
n5_r1_c2 = false
n6_r1_c2 = false
n7_r1_c2 = false
n8_r1_c2 = false
n9_r1_c2 = false
n1_r1_c3 = true
n2_r1_c3 = false
n3_r1_c3 = false
n4_r1_c3 = false
n5_r1_c3 = false
n6_r1_c3 = false
n7_r1_c3 = false
n8_r1_c3 = false
n9_r1_c3 = false
n1_r1_c4 = false
n2_r1_c4 = false
n3_r1_c4 = false
n4_r1_c4 = false
n5_r1_c4 = false
n6_r1_c4 = false
n7_r1_c4 = false
n8_r1_c4 = true
n9_r1_c4 = false
n1_r1_c5 = false
n2_r1_c5 = false
n3_r1_c5 = false
n4_r1_c5 = false
n5_r1_c5 = false
n6_r1_c5 = false
n7_r1_c5 = true
n8_r1_c5 = false
n9_r1_c5 = false
n1_r1_c6 = false
n2_r1_c6 = false
n3_r1_c6 = false
n4_r1_c6 = false
n5_r1_c6 = true
n6_r1_c6 = false
n7_r1_c6 = false
n8_r1_c6 = false
n9_r1_c6 = false
n1_r1_c7 = false
n2_r1_c7 = false
n3_r1_c7 = true
n4_r1_c7 = false
n5_r1_c7 = false
n6_r1_c7 = false
n7_r1_c7 = false
n8_r1_c7 = false
n9_r1_c7 = false
n1_r1_c8 = false
n2_r1_c8 = false
n3_r1_c8 = false
n4_r1_c8 = false
n5_r1_c8 = false
n6_r1_c8 = false
n7_r1_c8 = false
n8_r1_c8 = false
n9_r1_c8 = true
n1_r1_c9 = false
n2_r1_c9 = false
n3_r1_c9 = false
n4_r1_c9 = false
n5_r1_c9 = false
n6_r1_c9 = true
n7_r1_c9 = false
n8_r1_c9 = false
n9_r1_c9 = false
n1_r2_c1 = false
n2_r2_c1 = false
n3_r2_c1 = true
n4_r2_c1 = false
n5_r2_c1 = false
n6_r2_c1 = false
n7_r2_c1 = false
n8_r2_c1 = false
n9_r2_c1 = false
n1_r2_c2 = false
n2_r2_c2 = false
n3_r2_c2 = false
n4_r2_c2 = false
n5_r2_c2 = false
n6_r2_c2 = false
n7_r2_c2 = false
n8_r2_c2 = false
n9_r2_c2 = true
n1_r2_c3 = false
n2_r2_c3 = false
n3_r2_c3 = false
n4_r2_c3 = false
n5_r2_c3 = false
n6_r2_c3 = false
n7_r2_c3 = false
n8_r2_c3 = true
n9_r2_c3 = false
n1_r2_c4 = false
n2_r2_c4 = true
n3_r2_c4 = false
n4_r2_c4 = false
n5_r2_c4 = false
n6_r2_c4 = false
n7_r2_c4 = false
n8_r2_c4 = false
n9_r2_c4 = false
n1_r2_c5 = false
n2_r2_c5 = false
n3_r2_c5 = false
n4_r2_c5 = true
n5_r2_c5 = false
n6_r2_c5 = false
n7_r2_c5 = false
n8_r2_c5 = false
n9_r2_c5 = false
n1_r2_c6 = false
n2_r2_c6 = false
n3_r2_c6 = false
n4_r2_c6 = false
n5_r2_c6 = false
n6_r2_c6 = true
n7_r2_c6 = false
n8_r2_c6 = false
n9_r2_c6 = false
n1_r2_c7 = false
n2_r2_c7 = false
n3_r2_c7 = false
n4_r2_c7 = false
n5_r2_c7 = false
n6_r2_c7 = false
n7_r2_c7 = true
n8_r2_c7 = false
n9_r2_c7 = false
n1_r2_c8 = false
n2_r2_c8 = false
n3_r2_c8 = false
n4_r2_c8 = false
n5_r2_c8 = true
n6_r2_c8 = false
n7_r2_c8 = false
n8_r2_c8 = false
n9_r2_c8 = false
n1_r2_c9 = true
n2_r2_c9 = false
n3_r2_c9 = false
n4_r2_c9 = false
n5_r2_c9 = false
n6_r2_c9 = false
n7_r2_c9 = false
n8_r2_c9 = false
n9_r2_c9 = false
n1_r3_c1 = false
n2_r3_c1 = false
n3_r3_c1 = false
n4_r3_c1 = false
n5_r3_c1 = false
n6_r3_c1 = true
n7_r3_c1 = false
n8_r3_c1 = false
n9_r3_c1 = false
n1_r3_c2 = false
n2_r3_c2 = false
n3_r3_c2 = false
n4_r3_c2 = false
n5_r3_c2 = false
n6_r3_c2 = false
n7_r3_c2 = true
n8_r3_c2 = false
n9_r3_c2 = false
n1_r3_c3 = false
n2_r3_c3 = false
n3_r3_c3 = false
n4_r3_c3 = false
n5_r3_c3 = true
n6_r3_c3 = false
n7_r3_c3 = false
n8_r3_c3 = false
n9_r3_c3 = false
n1_r3_c4 = false
n2_r3_c4 = false
n3_r3_c4 = false
n4_r3_c4 = false
n5_r3_c4 = false
n6_r3_c4 = false
n7_r3_c4 = false
n8_r3_c4 = false
n9_r3_c4 = true
n1_r3_c5 = true
n2_r3_c5 = false
n3_r3_c5 = false
n4_r3_c5 = false
n5_r3_c5 = false
n6_r3_c5 = false
n7_r3_c5 = false
n8_r3_c5 = false
n9_r3_c5 = false
n1_r3_c6 = false
n2_r3_c6 = false
n3_r3_c6 = true
n4_r3_c6 = false
n5_r3_c6 = false
n6_r3_c6 = false
n7_r3_c6 = false
n8_r3_c6 = false
n9_r3_c6 = false
n1_r3_c7 = false
n2_r3_c7 = false
n3_r3_c7 = false
n4_r3_c7 = false
n5_r3_c7 = false
n6_r3_c7 = false
n7_r3_c7 = false
n8_r3_c7 = true
n9_r3_c7 = false
n1_r3_c8 = false
n2_r3_c8 = false
n3_r3_c8 = false
n4_r3_c8 = true
n5_r3_c8 = false
n6_r3_c8 = false
n7_r3_c8 = false
n8_r3_c8 = false
n9_r3_c8 = false
n1_r3_c9 = false
n2_r3_c9 = true
n3_r3_c9 = false
n4_r3_c9 = false
n5_r3_c9 = false
n6_r3_c9 = false
n7_r3_c9 = false
n8_r3_c9 = false
n9_r3_c9 = false
n1_r4_c1 = false
n2_r4_c1 = false
n3_r4_c1 = false
n4_r4_c1 = false
n5_r4_c1 = false
n6_r4_c1 = false
n7_r4_c1 = false
n8_r4_c1 = false
n9_r4_c1 = true
n1_r4_c2 = false
n2_r4_c2 = false
n3_r4_c2 = false
n4_r4_c2 = false
n5_r4_c2 = false
n6_r4_c2 = false
n7_r4_c2 = false
n8_r4_c2 = true
n9_r4_c2 = false
n1_r4_c3 = false
n2_r4_c3 = false
n3_r4_c3 = false
n4_r4_c3 = false
n5_r4_c3 = false
n6_r4_c3 = true
n7_r4_c3 = false
n8_r4_c3 = false
n9_r4_c3 = false
n1_r4_c4 = false
n2_r4_c4 = false
n3_r4_c4 = false
n4_r4_c4 = true
n5_r4_c4 = false
n6_r4_c4 = false
n7_r4_c4 = false
n8_r4_c4 = false
n9_r4_c4 = false
n1_r4_c5 = false
n2_r4_c5 = false
n3_r4_c5 = false
n4_r4_c5 = false
n5_r4_c5 = true
n6_r4_c5 = false
n7_r4_c5 = false
n8_r4_c5 = false
n9_r4_c5 = false
n1_r4_c6 = false
n2_r4_c6 = true
n3_r4_c6 = false
n4_r4_c6 = false
n5_r4_c6 = false
n6_r4_c6 = false
n7_r4_c6 = false
n8_r4_c6 = false
n9_r4_c6 = false
n1_r4_c7 = true
n2_r4_c7 = false
n3_r4_c7 = false
n4_r4_c7 = false
n5_r4_c7 = false
n6_r4_c7 = false
n7_r4_c7 = false
n8_r4_c7 = false
n9_r4_c7 = false
n1_r4_c8 = false
n2_r4_c8 = false
n3_r4_c8 = false
n4_r4_c8 = false
n5_r4_c8 = false
n6_r4_c8 = false
n7_r4_c8 = true
n8_r4_c8 = false
n9_r4_c8 = false
n1_r4_c9 = false
n2_r4_c9 = false
n3_r4_c9 = true
n4_r4_c9 = false
n5_r4_c9 = false
n6_r4_c9 = false
n7_r4_c9 = false
n8_r4_c9 = false
n9_r4_c9 = false
n1_r5_c1 = false
n2_r5_c1 = false
n3_r5_c1 = false
n4_r5_c1 = false
n5_r5_c1 = false
n6_r5_c1 = false
n7_r5_c1 = true
n8_r5_c1 = false
n9_r5_c1 = false
n1_r5_c2 = false
n2_r5_c2 = false
n3_r5_c2 = false
n4_r5_c2 = false
n5_r5_c2 = true
n6_r5_c2 = false
n7_r5_c2 = false
n8_r5_c2 = false
n9_r5_c2 = false
n1_r5_c3 = false
n2_r5_c3 = true
n3_r5_c3 = false
n4_r5_c3 = false
n5_r5_c3 = false
n6_r5_c3 = false
n7_r5_c3 = false
n8_r5_c3 = false
n9_r5_c3 = false
n1_r5_c4 = false
n2_r5_c4 = false
n3_r5_c4 = true
n4_r5_c4 = false
n5_r5_c4 = false
n6_r5_c4 = false
n7_r5_c4 = false
n8_r5_c4 = false
n9_r5_c4 = false
n1_r5_c5 = false
n2_r5_c5 = false
n3_r5_c5 = false
n4_r5_c5 = false
n5_r5_c5 = false
n6_r5_c5 = true
n7_r5_c5 = false
n8_r5_c5 = false
n9_r5_c5 = false
n1_r5_c6 = true
n2_r5_c6 = false
n3_r5_c6 = false
n4_r5_c6 = false
n5_r5_c6 = false
n6_r5_c6 = false
n7_r5_c6 = false
n8_r5_c6 = false
n9_r5_c6 = false
n1_r5_c7 = false
n2_r5_c7 = false
n3_r5_c7 = false
n4_r5_c7 = false
n5_r5_c7 = false
n6_r5_c7 = false
n7_r5_c7 = false
n8_r5_c7 = false
n9_r5_c7 = true
n1_r5_c8 = false
n2_r5_c8 = false
n3_r5_c8 = false
n4_r5_c8 = false
n5_r5_c8 = false
n6_r5_c8 = false
n7_r5_c8 = false
n8_r5_c8 = true
n9_r5_c8 = false
n1_r5_c9 = false
n2_r5_c9 = false
n3_r5_c9 = false
n4_r5_c9 = true
n5_r5_c9 = false
n6_r5_c9 = false
n7_r5_c9 = false
n8_r5_c9 = false
n9_r5_c9 = false
n1_r6_c1 = true
n2_r6_c1 = false
n3_r6_c1 = false
n4_r6_c1 = false
n5_r6_c1 = false
n6_r6_c1 = false
n7_r6_c1 = false
n8_r6_c1 = false
n9_r6_c1 = false
n1_r6_c2 = false
n2_r6_c2 = false
n3_r6_c2 = false
n4_r6_c2 = true
n5_r6_c2 = false
n6_r6_c2 = false
n7_r6_c2 = false
n8_r6_c2 = false
n9_r6_c2 = false
n1_r6_c3 = false
n2_r6_c3 = false
n3_r6_c3 = true
n4_r6_c3 = false
n5_r6_c3 = false
n6_r6_c3 = false
n7_r6_c3 = false
n8_r6_c3 = false
n9_r6_c3 = false
n1_r6_c4 = false
n2_r6_c4 = false
n3_r6_c4 = false
n4_r6_c4 = false
n5_r6_c4 = false
n6_r6_c4 = false
n7_r6_c4 = true
n8_r6_c4 = false
n9_r6_c4 = false
n1_r6_c5 = false
n2_r6_c5 = false
n3_r6_c5 = false
n4_r6_c5 = false
n5_r6_c5 = false
n6_r6_c5 = false
n7_r6_c5 = false
n8_r6_c5 = false
n9_r6_c5 = true
n1_r6_c6 = false
n2_r6_c6 = false
n3_r6_c6 = false
n4_r6_c6 = false
n5_r6_c6 = false
n6_r6_c6 = false
n7_r6_c6 = false
n8_r6_c6 = true
n9_r6_c6 = false
n1_r6_c7 = false
n2_r6_c7 = false
n3_r6_c7 = false
n4_r6_c7 = false
n5_r6_c7 = false
n6_r6_c7 = true
n7_r6_c7 = false
n8_r6_c7 = false
n9_r6_c7 = false
n1_r6_c8 = false
n2_r6_c8 = true
n3_r6_c8 = false
n4_r6_c8 = false
n5_r6_c8 = false
n6_r6_c8 = false
n7_r6_c8 = false
n8_r6_c8 = false
n9_r6_c8 = false
n1_r6_c9 = false
n2_r6_c9 = false
n3_r6_c9 = false
n4_r6_c9 = false
n5_r6_c9 = true
n6_r6_c9 = false
n7_r6_c9 = false
n8_r6_c9 = false
n9_r6_c9 = false
n1_r7_c1 = false
n2_r7_c1 = false
n3_r7_c1 = false
n4_r7_c1 = false
n5_r7_c1 = false
n6_r7_c1 = false
n7_r7_c1 = false
n8_r7_c1 = true
n9_r7_c1 = false
n1_r7_c2 = true
n2_r7_c2 = false
n3_r7_c2 = false
n4_r7_c2 = false
n5_r7_c2 = false
n6_r7_c2 = false
n7_r7_c2 = false
n8_r7_c2 = false
n9_r7_c2 = false
n1_r7_c3 = false
n2_r7_c3 = false
n3_r7_c3 = false
n4_r7_c3 = false
n5_r7_c3 = false
n6_r7_c3 = false
n7_r7_c3 = true
n8_r7_c3 = false
n9_r7_c3 = false
n1_r7_c4 = false
n2_r7_c4 = false
n3_r7_c4 = false
n4_r7_c4 = false
n5_r7_c4 = false
n6_r7_c4 = true
n7_r7_c4 = false
n8_r7_c4 = false
n9_r7_c4 = false
n1_r7_c5 = false
n2_r7_c5 = true
n3_r7_c5 = false
n4_r7_c5 = false
n5_r7_c5 = false
n6_r7_c5 = false
n7_r7_c5 = false
n8_r7_c5 = false
n9_r7_c5 = false
n1_r7_c6 = false
n2_r7_c6 = false
n3_r7_c6 = false
n4_r7_c6 = true
n5_r7_c6 = false
n6_r7_c6 = false
n7_r7_c6 = false
n8_r7_c6 = false
n9_r7_c6 = false
n1_r7_c7 = false
n2_r7_c7 = false
n3_r7_c7 = false
n4_r7_c7 = false
n5_r7_c7 = true
n6_r7_c7 = false
n7_r7_c7 = false
n8_r7_c7 = false
n9_r7_c7 = false
n1_r7_c8 = false
n2_r7_c8 = false
n3_r7_c8 = true
n4_r7_c8 = false
n5_r7_c8 = false
n6_r7_c8 = false
n7_r7_c8 = false
n8_r7_c8 = false
n9_r7_c8 = false
n1_r7_c9 = false
n2_r7_c9 = false
n3_r7_c9 = false
n4_r7_c9 = false
n5_r7_c9 = false
n6_r7_c9 = false
n7_r7_c9 = false
n8_r7_c9 = false
n9_r7_c9 = true
n1_r8_c1 = false
n2_r8_c1 = false
n3_r8_c1 = false
n4_r8_c1 = false
n5_r8_c1 = true
n6_r8_c1 = false
n7_r8_c1 = false
n8_r8_c1 = false
n9_r8_c1 = false
n1_r8_c2 = false
n2_r8_c2 = false
n3_r8_c2 = true
n4_r8_c2 = false
n5_r8_c2 = false
n6_r8_c2 = false
n7_r8_c2 = false
n8_r8_c2 = false
n9_r8_c2 = false
n1_r8_c3 = false
n2_r8_c3 = false
n3_r8_c3 = false
n4_r8_c3 = true
n5_r8_c3 = false
n6_r8_c3 = false
n7_r8_c3 = false
n8_r8_c3 = false
n9_r8_c3 = false
n1_r8_c4 = true
n2_r8_c4 = false
n3_r8_c4 = false
n4_r8_c4 = false
n5_r8_c4 = false
n6_r8_c4 = false
n7_r8_c4 = false
n8_r8_c4 = false
n9_r8_c4 = false
n1_r8_c5 = false
n2_r8_c5 = false
n3_r8_c5 = false
n4_r8_c5 = false
n5_r8_c5 = false
n6_r8_c5 = false
n7_r8_c5 = false
n8_r8_c5 = true
n9_r8_c5 = false
n1_r8_c6 = false
n2_r8_c6 = false
n3_r8_c6 = false
n4_r8_c6 = false
n5_r8_c6 = false
n6_r8_c6 = false
n7_r8_c6 = false
n8_r8_c6 = false
n9_r8_c6 = true
n1_r8_c7 = false
n2_r8_c7 = true
n3_r8_c7 = false
n4_r8_c7 = false
n5_r8_c7 = false
n6_r8_c7 = false
n7_r8_c7 = false
n8_r8_c7 = false
n9_r8_c7 = false
n1_r8_c8 = false
n2_r8_c8 = false
n3_r8_c8 = false
n4_r8_c8 = false
n5_r8_c8 = false
n6_r8_c8 = true
n7_r8_c8 = false
n8_r8_c8 = false
n9_r8_c8 = false
n1_r8_c9 = false
n2_r8_c9 = false
n3_r8_c9 = false
n4_r8_c9 = false
n5_r8_c9 = false
n6_r8_c9 = false
n7_r8_c9 = true
n8_r8_c9 = false
n9_r8_c9 = false
n1_r9_c1 = false
n2_r9_c1 = true
n3_r9_c1 = false
n4_r9_c1 = false
n5_r9_c1 = false
n6_r9_c1 = false
n7_r9_c1 = false
n8_r9_c1 = false
n9_r9_c1 = false
n1_r9_c2 = false
n2_r9_c2 = false
n3_r9_c2 = false
n4_r9_c2 = false
n5_r9_c2 = false
n6_r9_c2 = true
n7_r9_c2 = false
n8_r9_c2 = false
n9_r9_c2 = false
n1_r9_c3 = false
n2_r9_c3 = false
n3_r9_c3 = false
n4_r9_c3 = false
n5_r9_c3 = false
n6_r9_c3 = false
n7_r9_c3 = false
n8_r9_c3 = false
n9_r9_c3 = true
n1_r9_c4 = false
n2_r9_c4 = false
n3_r9_c4 = false
n4_r9_c4 = false
n5_r9_c4 = true
n6_r9_c4 = false
n7_r9_c4 = false
n8_r9_c4 = false
n9_r9_c4 = false
n1_r9_c5 = false
n2_r9_c5 = false
n3_r9_c5 = true
n4_r9_c5 = false
n5_r9_c5 = false
n6_r9_c5 = false
n7_r9_c5 = false
n8_r9_c5 = false
n9_r9_c5 = false
n1_r9_c6 = false
n2_r9_c6 = false
n3_r9_c6 = false
n4_r9_c6 = false
n5_r9_c6 = false
n6_r9_c6 = false
n7_r9_c6 = true
n8_r9_c6 = false
n9_r9_c6 = false
n1_r9_c7 = false
n2_r9_c7 = false
n3_r9_c7 = false
n4_r9_c7 = true
n5_r9_c7 = false
n6_r9_c7 = false
n7_r9_c7 = false
n8_r9_c7 = false
n9_r9_c7 = false
n1_r9_c8 = true
n2_r9_c8 = false
n3_r9_c8 = false
n4_r9_c8 = false
n5_r9_c8 = false
n6_r9_c8 = false
n7_r9_c8 = false
n8_r9_c8 = false
n9_r9_c8 = false
n1_r9_c9 = false
n2_r9_c9 = false
n3_r9_c9 = false
n4_r9_c9 = false
n5_r9_c9 = false
n6_r9_c9 = false
n7_r9_c9 = false
n8_r9_c9 = true
n9_r9_c9 = false
 4  2  1  8  7  5  3  9  6 
 3  9  8  2  4  6  7  5  1 
 6  7  5  9  1  3  8  4  2 
 9  8  6  4  5  2  1  7  3 
 7  5  2  3  6  1  9  8  4 
 1  4  3  7  9  8  6  2  5 
 8  1  7  6  2  4  5  3  9 
 5  3  4  1  8  9  2  6  7 
 2  6  9  5  3  7  4  1  8 

