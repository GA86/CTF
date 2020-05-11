# Romance Dawn

## First steps
This challenge is a forensics png reconstruction based one. The name of the file we get from the server is `7uffy.png` and a console call of pngcheck gives the following output.

```console
$ pngcheck 7uffy.png 
7uffy.png  illegal (unless recently approved) unknown, public chunk EASY
ERROR: 7uffy.png
```

if you are unaware of png files data structure, a rapide look at [this source](http://www.libpng.org/pub/png/spec/1.2/PNG-Chunks.html) tells you that there are four critical chunks in a png file (see below image).

<p align="center">
<img src="https://github.com/GA86/CTF/blob/master/png-chunks.png" width="400">
</p>

These critical chunks are: IHDR, PLTE, IDAT and IEND. There is a good chance that the file `EASY` chunks need to be corrected to one of them. So let's investigate which one of these chunks are already declared in the file. PLTE is mentionned as optionnal, however, we will check for it too as one is never too sure...

## Chunk assessment.

To investigate the  do so we use the `xxd` command. Its output are then piped into `grep`, looking for the appropriate chunk name.

```console
$ xxd -g 1 7uffy.png | grep IHDR 
00000000: 89 50 4e 47 0d 0a 1a 0a 00 00 00 0d 49 48 44 52  .PNG........IHDR
$ xxd -g 1 7uffy.png | grep PLTE 
$ xxd -g 1 7uffy.png | grep IDAT 
$ xxd -g 1 7uffy.png | grep IHDR 
00000000: 89 50 4e 47 0d 0a 1a 0a 00 00 00 0d 49 48 44 52  .PNG........IHDR
```

Findings indicate that chunks IDAT are missing. So let us bet on modifying occurences of EASY to IDAT to fix the file. 

## Occurences of EASY

Before making any corrections on the file, let's have a look on the number of times EASY appear the file.

```console
$ xxd -g 1 7uffy.png | grep EASY
00000080: 64 2e 65 07 00 00 20 00 45 41 53 59 78 da ec dd  d.e... .EASYx...
00002090: 00 00 20 00 45 41 53 59 73 fd ba 35 74 86 d8 d4  .. .EASYs..5t...
000040a0: 45 41 53 59 ba 77 ef ae db 6f 56 b3 7f d8 e3 3a  EASY.w...oV....:
000060a0: b8 8e 27 4f 6e ba a5 31 00 00 0a 18 45 41 53 59  ..'On..1....EASY
```

EASY is found four times on bytes:
- $`9\times 16^0 + 8\times 16 = 137`$
- $`5\times 16^0 + 9\times 16 + 0\times 16^2 + 2\times 16^3 = 8341`$
- $`1\times 16^0 + 10\times 16 + 0\times 16^2 + 4\times 16^3 = 16544`$
- $`13\times 16^0 + 10\times 16 + 0\times 16^2 + 6\times 16^3 = 24748`$

. We will thus make the four 
