# Apple II Eval Function
An Apple II extension to Applesoft allow user input to be evaluated.

# Forward
I wrote this small utility to extend Applesoft with an eval function back in 1988.  I intended to submit it for publication to one of the various Apple II magazines publishing in those days, but I never did.

On the off chance that you, dear retrocomputerist, find it useful, I publish it here, complete with source code for armchair enjoyment or your own fabulous retro project.  The utility and demo are quite short and so, in the spirit of the computer magazines of yore, if you want to try it out, you will actually have to key it in. 

__Warning: Is this eval function evil?  Does it allow your user to input aribitrary Applesoft?  I honestly don't remember.  Use with caution.__

The text of the article is a first draft and could use a good edit.  I left it as-is for an authenticity's sake.

# Original Article
The following routine will make life easier both for the Applesoft programmer and user. _The Eval Routine_ is an extended version of the VAL function. It allows the user to enter numerical input in expression form. For example the user may enter something like SIN(1.5)+2-COS(3.14)/5 and the routine will evaluate the expression and store the result in a specified variable. The user is no longer forced to calculate any values when entering numbers.

The routine lies in the $300 area of memory and may be acessed through CALL statements. The routine is very easy to use even if you do not have any machine language experience. To use the routine, you will have to enter the machine language code in listing one.

To use this routine in your program include this statement: `CALL 768,A$,N`.

`A$` may be any string variable and N may be any real variable. The above statement evaluates the expression in A$ and stores the result in N. The following short program illustrates how to use the routine:
```
     10 INPUT"ENTER AN EXPRESSION:";A$
     20 CALL 768,A$,N
     30 PRINT A$;" = ";N
     40 GOTO 10
 ```
The user may use variables in your program as part of the expression. For example, you could include a `PI=3.1416` statement in the beginning of your program. Then the user can use the variable PI as part of an expression for example: `SIN(PI/4)^2+COS(PI/4)^2`. The user may enter any expression as long as it is in standard Applesoft format.

Listing two, a program that graphs user entered functions, shows a very interesting use of the Eval Routine. The graph program asks the user to enter a expression containing the variable X. The program then evaluates the user's expression over and over allowing X to vary over a pre-selected range and graphs the result.

The following input will graph a sine function:

     X-axis range low: -2*PI
     X-axis range high: 2*PI
     Y-axis range low: -1
     Y-axis range high: 1
     F(X) = SIN (X)
     X range low: -2*PI
     X range high: 2*PI
     X increment: .1

Run the graph program and try this input. The program will draw a sine wave. Then try some functions of your own.

The Eval Routine is a useful and convenient routine for both the user and the programmer. It will save time and exasperation on the user's part and give the programmer more flexibility.

# Listing One— EVAL
```
0300: 20 be de 20 e3 df a0 00
0308: b1 83 aa c8 b1 83 85 06
0310: c8 b1 83 85 07 8a a8 b1
0318: 06 99 00 02 88 d0 f8 b1
0320: 06 99 00 02 a5 b8 48 a5
0328: b9 48 20 39 d5 e8 86 b8
0330: c8 84 b9 20 59 d5 20 b1
0338: 00 20 67 dd 68 85 b9 68
0340: 85 b8 20 be de 20 e3 df
0348: aa 20 2b eb 20 b7 00 60
```
__BSAVE EVAL A$300, L$50__
# Listing Two— GRAPH
```
 10  REM  GRAPH PROGRAM BY JENNY SCHMIDT. AUGUST 20, 1988.
 20  REM GRAPHING PROGRAM TO DEMONSTRATE EVALUATED INPUT
 30  PRINT  CHR$ (4);"BLOAD EVAL,A$300"
 40  PRINT "THIS PROGRAM GRAPHS MATHEMATICAL        FUNCTIONS. FIRST ENTER THE PORTION OF   AXES YOU WANT ON THE SCREEN. THEN ENTER"
 50  PRINT "THE FUNCTION AS A FUNCTION OF X AND IN  APPLESOFT SYNTAX. (I.E. X+X^2-SIN(X) )"
 60  PRINT "FINALLY ENTER THE RANGE OF THE X        VARIABLE AND THE INCREMENT BETWEEN      PLOTTING POINTS."
 70  PRINT : PRINT 
 80 FR = 768:PI = 3.141592
 90  INPUT "X-AXIS RANGE LOW:";A$: CALL FR,A$,XL
 100  INPUT "X-AXIS RANGE HIGH:";A$: CALL FR,A$,XH
 110  INPUT "Y-AXIS RANGE LOW:";A$: CALL FR,A$,YL
 120  INPUT "Y-AXIS RANGE HIGH:";A$: CALL FR,A$,YH
 130  INPUT "F(X)=";F$
 140  INPUT "X RANGE LOW:";A$: CALL FR,A$,X1
 150  INPUT "X RANGE HIGH:";A$: CALL FR,A$,X2
 160  INPUT "X INCREMENT:";A$: CALL FR,A$,IN
 170 Y0 = 159 + YL / (YH - YL) * 159:X0 =  - XL / (XH - XL) * 199
 180  HGR : HCOLOR= 3: IF YL < 0 AND YH > 0 THEN  HPLOT 0,Y0 TO 199,Y0
 190  IF XL < 0 AND XH > 0 THEN  HPLOT X0,0 TO X0,159
 200 X = X1: CALL FR,F$,N:YP = Y0 - (N / (YH - YL)) * 159:XP = X0 + (X / (XH - XL)) * 199
 210  GOSUB 300
 220  HPLOT XP,YP
 230  FOR X = X1 TO X2 + IN STEP IN
 240  CALL FR,F$,N:YP = Y0 - (N / (YH - YL)) * 159:XP = X0 + (X / (XH - XL)) * 199
 250  GOSUB 300
 260  HPLOT  TO XP,YP
 270  NEXT X
 280  INPUT "MAKE NEW GRAGH?";A$: IF  LEFT$ (A$,1) <  > "N" THEN  TEXT : PRINT : GOTO 40
 290  END 
 300  IF YP < 0 THEN YP = 0
 310  IF YP > 159 THEN YP = 159
 320  RETURN 
```
__SAVE GRAPH__
# Listing Three— EVAL SOURCE CODE
```
;****************************
;*                          *
;*    INPUT EVALUATOR FOR   *
;* THE APPLESOFT PROGRAMMER *
;*                          *
;*     BY JENNY SCHMIDT     *
;*                          *
;*      AUGUST 20, 1988     *
;*                          *
;****************************
;
;ASSEMBLED ON MICROSPARC ASSEMBLER
;
CHKCOM EQU $DEBE ;CHECK FOR COMMA
PTRGET EQU $DFE3 ;GET ADDRESS OF VARIABLE
TXTPTR EQU $B8 ;POINTER TO CURRENT CHARACTER
CHRGET EQU $B1 ;GET NEXT CHARACTER
GETPTR EQU $DFE3
CHRGOT EQU $B7 ;GET CURRENT CHARACTER
FRMNUM EQU $DD67 ;EVALUATE FORMULA AT TXTPTR
BUF    EQU $200 ;INPUT BUFFER
FACTOMEM EQU $EB2B ;STORE NUMBER IN FLOATING POINT ACCUMULATOR IN MEMORY
PARSE EQU $D559 ;TURN INPUT INTO TOKENIZED APPLESOFT CODE
GDBUFS EQU $D539 ;FIX BUFFER
GETARYPTR EQU $F7D9 ;FIND ARRAY VARIABLE
NAMEPTR EQU $9B ;TEMPORARY POINTERS
ENDPTR EQU $6D ;TO MOVE VARIABLE SPACE
MOVE EQU $FE2C ;MONITOR MOVE ROUTINE
A1 EQU $3C ;VARIABLES FOR MOVE
A2 EQU $3E
A4 EQU $42
VARPTR EQU $83
TEMPPTR EQU $06
ORG $300
;EVAL: EVALUATE USER INPUT AND STORE IN VARIABLE#;USE ONLY WITHIN A PROGRAM. FORMAT:
;10 CALL 768,A$,N
;USER'S INPUT WILL BE EVALUATED AND STORED IN N
EVAL JSR CHKCOM ;GET FORMULA STRING
     JSR GETPTR
     LDY #0
     LDA (VARPTR),Y ;LENGTH OF STRING
     TAX
     INY
     LDA (VARPTR),Y ;
     STA TEMPPTR
     INY
     LDA (VARPTR),Y
     STA TEMPPTR+1 ;STORE ADDRESS OF STRING
     TXA
     TAY
LOOP LDA (TEMPPTR),Y
     STA BUF,Y
     DEY
     BNE LOOP
     LDA (TEMPPTR),Y
     STA BUF,Y
     LDA TXTPTR ;SAVE TXTPTR, TXTPTR+1
     PHA
     LDA TXTPTR+1
     PHA
     JSR GDBUFS
;POINT TXTPTR TO BUF
     INX
     STX TXTPTR
     INY
     STY TXTPTR+1
     JSR PARSE ;TOKENIZE BUF FOR FRMNUM7 
     JSR CHRGET ;ADJUST TXTPTR TO POINT TO BEGINNING OF BUFB
     JSR FRMNUM ;EVALUATE EXPRESSION IN BUF AND STORE IN FAC ($9D-$A2)
     PLA ;GET OLD TXTPTR TO FIND OUT WHERE TO STORE RESULT IN FAC
     STA TXTPTR+1
     PLA
     STA TXTPTR
     JSR CHKCOM ;PICK UP COMMA
     JSR PTRGET ;LOCATE VARIABLE
     TAX ;Y,X CONTAIN ADDRESS OF VARIABLE2 JSR FACTOMEM 
     ;TRANSFER CONTENT OF FAC TO VARIABLE
     JSR CHRGOT
     RTS
```
