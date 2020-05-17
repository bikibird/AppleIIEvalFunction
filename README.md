# Apple II Eval Function
An Apple II extension to Applesoft allow user input to be evaluated.

# Forward
I wrote this small utility to extend Applesoft with an eval function back in 1988.  I intended to submit it for publication to one of the various Apple II magazines publishing in those days, but I never did.  On the off chance that you, dear retrocomputerist, find it useful, I publish it here, complete with source code for armchair enjoyment or your own fabulous retro project.  The utility and demo are quite short and so, in the spirit of the computer magazines of yore, if you want to try it out, you will actually have to key it in. 

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

# Listing Two— GRAPH

# Listing Three— EVAL SOURCE CODE
