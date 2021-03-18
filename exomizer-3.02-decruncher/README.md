# Exomizer v3.02 6502 decruncher (Elk/Beeb)

Exomizer v3.02 6502 decruncher in [BeebAsm](https://github.com/stardot/beebasm) format. Specifically for use on an Acorn Electron, BBC Micro or BBC Master.

## Adding the decruncher to your 6502 asm project

In your 6502 asm project, add this to your zeropage variables:

```
include "exomizer302decruncher.h.6502"
```

And this anywhere in your code:

```
include "exomizer302decruncher.6502"

.my_crunched_data
include "mycruncheddata.exo"
```

### Important

This decruncher is hardcoded to use the bottom of the stack (&0100-&01A0, rounded up) as workspace while decompressing data. This is easily changed by modifying `decrunch_table`.

## Preparing compressed data

Download [Exomizer v3.02](https://bitbucket.org/magli143/exomizer/wiki/Home) and compress your data, e.g.:

```
exomizer.exe level -M256 -c mydata.bin@0x0000 -o mycruncheddata.exo
```

## Decrunching data in your 6502 asm project

```
ldx #lo(my_crunched_data)
ldy #hi(my_crunched_data)
lda #hi(&5800) ; destination for decompressed data, only hi byte needed (page)
jsr decrunch_to_page_A
```

## Acknowledgements

[Exomizer](https://bitbucket.org/magli143/exomizer/wiki/Home) is created by Magnus Lind.
This version of the 6502 decruncher was created by the [Bitshifters](https://github.com/bitshifters).
