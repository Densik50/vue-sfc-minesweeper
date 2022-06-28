# Minesweeper Generator

Build a program which generates and visualises the minesweeper (uncovered) game field.
Input: 3 parameters – number of rows, number of columns, number of mines
Rules: each field is either empty, mine or number (1-8),
 - Mine means there is a mine in that field.
 - Number means there is no mine in that field but the number tells you how many mines lay
hidden in the eight surrounding squares.
 - Empty means there is no mine and there are no mines in surrounding squares.
Generate a rectangular field based on input number of rows and columns. Place mines randomly in
the field – there can be only one mine in each field. Calculate the number clues and display the field.
Output: simple visualisation of uncovered game board
E.g. (15 rows, 15 cols, 30 mines) OR
1X3X1 111
12X21111 2X2
1121212X1 3X3
1X1 1X211 13X2
111 111 1X21
122222112221
1212XX2XX11X11X
X2X222222111111
1322 111
1X1 1X211
1211 1222X1
X1 111 112X1111
11 2X311X322 11
2XX1112X112X
1221 1111X2

TIP: We care about the generating function (mine generation, clue generation). We do not care much
about the input (you can even hardcode the function call but it has to work for ANY positive integer
parameters).
TIP: We do not care too much about visualisation either so please just display it somehow. Try to
make it fancier ONLY IF you have some time left.

Evaluation criteria:
a) Demonstrate understanding of problem/issue
b) Identify what you need to do to solve problem (thought process)
c) Approach and methodology used to solve the problem
d) The result/outcome

How to submit
You can either email us back with zip file and PDF document where you will shortly describe
your reasoning how you approached the problem what programming language, data
structures and algorithms you chose or you can do the same providing a link to your Github
(use README file instead of the PDF doc).
NOTE: The description should not exceed one page if not necessary. “I know this
programming language best” is a completely valid argument for your language choice.

## Problem solutions:
 - generation of mines is done in range <0,width*range) 
 - calculation of value around mines is done during placing mines (goes to all tiles around mine and increments value)
 - tile showing is done recursivly

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```
