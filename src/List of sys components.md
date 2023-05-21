# Generating a Genie IIIs CP/M 3.0 System

Generate a new CP/M 3 with BOOTGEN.SUB and CPM3.SUB

# System Components Holte CP/M 3.0

## src/driver.mac

D R I V E R  *  C P M S Y S 1  *  T h o m a s   H o l t e  *  8 6 0 1 1 3

``` as
DRIVER.MAC  *8 6 0 1 1 3*
Funktion: I/O-DRIVERS FOR THE GENIE IIIs  *
Autor/Version:
Thomas Holte/Version 1.1*
```

## src/font12.mac

F O N T 1 2  *  C P M S Y S 1 a  *  T h o m a s   H o l t e * 8 5 1 1 2 6

``` as
FONT12.MAC *8 5 1 1 2 6*
Funktion: FONTSET FOR MONITORS WITH 12 SCANLINES PER TEXTLINE
Autor/Version:
Thomas Holte/Version 1.0  *
```

## src/booter.mac

B O O T E R  *  C P M S Y S 2  *  T h o m a s   H o l t e  *  8 5 1 1 2 0 

``` as
BOOTER.MAC  *8 5 1 1 2 0*
Funktion: BOOTSTRAP LOADER FOR CP/M 3
Autor/Version: 
Thomas Holte/Version 1.1
```

## src/ldrbios.mac

L D R B I O S  *  C P M S Y S 3  *  T h o m a s   H o l t e * 8 5 1 1 2 0 

``` as
LDRBIOS.MAC  *8 5 1 1 2 0*
Funktion: MINIMUM BIOS FOR CPMLDR
Autor/Version:
Thomas Holte/Version 1.0  *
```

## src/bnkbios.mac

B N K B I O S  *  C P M S Y S 4  *  T h o m a s   H o l t e * 8 5 0 7 1 7

``` as
BNKBIOS.MAC  *8 5 0 7 1 7*
Funktion: ROOT MODULE OF RELOCATABLE BIOS
Autor/Version:
Thomas Holte/Version 1.0  *
```
## src/scb.mac

S C B  *  C P M S Y S 4 a  *  T h o m a s   H o l t e   *   8 5 0 2 0 5

``` as
SCB.MAC  *8 5 0 2 0 5*
Funktion: SYSTEM CONTROL BLOCK DEFINITION
Autor/Version:
Thomas Holte/Version 1.0  *
```

## src/boot.mac

B O O T  *  C P M S Y S 4 b  *  T h o m a s   H o l t e   *  8 5 1 0 2 3
```as
BOOT.MAC  *8 5 1 0 2 3*
Funktion: BOOT LOADER MODULE FOR CP/M 3.0
Autor/Version:
Thomas Holte/Version 1.0
```

## src/chario.mac

C H A R I O  *  C P M S Y S 4 c  *  T h o m a s   H o l t e * 8 5 0 7 1 7

``` as
CHARIO.MAC  *8 5 0 7 1 7*
Funktion: CHARACTER I/O HANDLER FOR Z80 CHIP BASED SYSTEM  
Autor/Version:
Thomas Holte/Version 1.0 
```

## src/move.mac

M O V E  *  C P M S Y S 4 d  *  T h o m a s   H o l t e   *  8 5 0 7 1 7 

``` as
MOVE.MAC *8 5 0 7 1 7*
Funktion: MOVE MODULE FOR CP/M3 LINKED BIOS
Autor/Version:
Thomas Holte/Version 1.0
```

## src/drvtbl.mac

D R V T B L  *  C P M S Y S 4 e  *  T h o m a s   H o l t e * 8 5 0 7 2 3

``` as
DRVTBL.MAC  *8 5 0 9 2 5*
Funktion: DRIVE TABLE   
Autor/Version:
Thomas Holte/Version 1.0  *
```

## src/diskio.mac

D I S K I O  *  C P M S Y S 4 f  *  T h o m a s   H o l t e * 8 5 0 7 2 3

``` as
DISKIO.MAC *8 5 0 9 2 5*
Funktion: DISK HANDLER
Autor/Version:
Thomas Holte/Version 1.0
```

## src/modebaud.mac

M O D E B A U D * C P M S Y S 4 g * T h o m a s   H o l t e * 8 4 1 0 0 5

``` as
MODEBAUD.MAC *8 4 1 0 0 5*
Funktion: EQUATES FOR MODE BYTE BIT FIELDS
Autor/Version:
Thomas Holte/Version 1.0
```

## src/initw.c

I N I T W  *  C P M S Y S 5  *  T h o m a s   H o l t e  *   8 6 0 9 1 5

``` as
INITW.MAC  *8 5 1 1 2 6*
Funktion: UTILITY FOR GENERATING A GENIE IIIs
CP/M Ver.3.0 HARDDISK SYSTEM
Autor/Version:
Thomas Holte/Version 2.0 
```

## Harddisk specific subroutines

## src/basf6188.h

B A S F 6 1 8 8  * C P M S Y S 5 a * T h o m a s   H o l t e * 8 5 0 7 0 2

## src/necd5124.h

N E C D 5 1 2 4  * C P M S Y S 5 a * T h o m a s   H o l t e * 8 5 0 9 2 2

## src/necd5126.h

N E C D 5 1 2 6  * C P M S Y S 5 a * T h o m a s   H o l t e * 8 6 0 5 3 1

## src/sq306.h

S Q 3 0 6  *  C P M S Y S 5 a  *  T h o m a s   H o l t e  *  8 5 0 7 0 2

## src/st412.h

S T 4 1 2  *  C P M S Y S 5 a  *  T h o m a s   H o l t e  *  8 5 0 7 0 2

## Language specific subroutines for INITW.C

## src/initwe.mac

I N I T W E  *  C P M S Y S 5 b  *  T h o m a s   H o l t e * 8 5 1 1 3 0

## src/initwg.mac

I N I T W G  *  C P M S Y S 5 b  *  T h o m a s   H o l t e * 8 5 0 7 0 2 
