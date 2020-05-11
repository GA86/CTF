# Romance Dawn

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

```console
$ xxd -g 1 7uffy.png | grep IHDR 
00000000: 89 50 4e 47 0d 0a 1a 0a 00 00 00 0d 49 48 44 52  .PNG........IHDR
$ xxd -g 1 7uffy.png | grep PLTE 
$ xxd -g 1 7uffy.png | grep IDAT 
$ xxd -g 1 7uffy.png | grep IHDR 
00000000: 89 50 4e 47 0d 0a 1a 0a 00 00 00 0d 49 48 44 52  .PNG........IHDR
```
