# Romance Dawn

This challenge is a forensics png reconstruction based one. The name of the file we get from the server is `7uffy.png` and a console call of pngcheck gives the following output.

```console
$ pngcheck 7uffy.png 
7uffy.png  illegal (unless recently approved) unknown, public chunk EASY
ERROR: 7uffy.png
```

if you are unaware of png files data structure, a rapide look at [this source](http://www.libpng.org/pub/png/spec/1.2/PNG-Chunks.html) tells you that there are three essential chunks in a png file (see below image).

<p align="center">
<img src="https://github.com/GA86/CTF/blob/master/png-chunks.png" width="400">
</p>

These essential chunks are: IHDR, IDAT and IEND. So let's investigate which one of these chunks are already declared in the file. There is a good chance that the file `EASY` chunks need to be corrected to one of them.
