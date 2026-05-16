# MCI
MCI EXPERIMENTS

EXP1
(a) Addition Program (8051 Assembly)
MOV A,#45H
MOV B,#35H
ADD A,B
MOV R1,A
MOV 6EH,A
END

(b) Subtraction Program (8051 Assembly)
MOV A,#25H
MOV B,#36H
SUBB A,B
MOV R2,A
MOV 49H,A
MOV P1,A
END

(c) Multiplication Program (8051 Assembly)
MOV A,#25H
MOV B,#36H
MUL AB
MOV R0,B
MOV R1,A
MOV 30H,B
MOV 31H,A
END

(d) Division Program (8051 Assembly)
MOV A,#09H
MOV B,#02H
DIV AB
MOV R0,B
MOV R1,A
MOV 30H,B
MOV 31H,A
END

EXP2
Copy 10 Bytes from Program Memory to Internal RAM (8051 Assembly)
ORG 0300H

DB 56H
DB 12H
DB 10101010B
DB "HI YCCE"

ORG 0000H

MOV DPTR,#0300H
MOV R1,#30H
MOV R2,#0AH

UP: MOVC A,@A+DPTR
    MOV @R1,A
    INC DPTR
    INC R1
    CLR A
    DJNZ R2,UP

END

EXP3
; Write assembly language program to convert a bit packed BCD number
; into equivalent Hexadecimal and store the result in register R6.

MOV A,#69H
MOV B,A
ANL A,#0FH
MOV R0,A
MOV A,B
ANL A,#0F0H
SWAP A
MOV B,#0AH
MUL AB
ADD A,R0
MOV R6,A
END

EXP4
; Write assembly language program to generate 10 random numbers
; and store them in internal RAM from 50H.
; Count the even numbers from this data block
; and store the result at 70H.

MOV R1,#50H
MOV A,#34H
MOV R2,#10

L1: RL A
    MOV @R1,A
    INC R1
    DJNZ R2,L1

MOV R1,#50H
MOV R3,#10
MOV B,#0

L2: MOV A,@R1
    JB ACC.0,DOWN
    INC B

DOWN: INC R1
      DJNZ R3,L2

MOV 70H,B
END

EXP5
; To generate 11 random numbers and store them in internal RAM
; from 60H find out largest number from this data block
; and store the result at 75H

MOV R1,#60H
MOV A,#34H
MOV R2,#11

L1: RR A
    MOV @R1,A
    INC R1
    DJNZ R2,L1

MOV R1,#60H
MOV R3,#10
MOV A,@R1

UP: INC R1
    MOV B,@R1
    CJNE A,B,NEXT

NEXT: JNC DOWN
      MOV A,B

DOWN: DJNZ R3,UP

MOV 75H,A
END

EXP6
/* Interface LED with P1.x pin of 8051 microcontroller
   & write a C-Program to toggle this LED continuously
   with a delay of 100 ms */

#include<reg51.h>

void msdelay(unsigned int);

sbit x = P1^3;

void main(void)
{
    while(1)
    {
        x = 0;
        msdelay(100);

        x = 1;
        msdelay(100);
    }
}

void msdelay(unsigned int y)
{
    unsigned int i,j;

    for(i=0;i<y;i++)
    for(j=0;j<1275;j++);
}

EXP7
/* Write a C language program to display BCD number on
   7 segment display after delay of 100 msec,
   interface 7 segment display using IC 7447 */

#include<reg51.h>

sbit buzzer = P2^2;
sbit disp1  = P2^4;
sbit disp2  = P2^5;
sbit disp3  = P2^6;
sbit disp4  = P2^7;

void msdelay(unsigned int);

void main(void)
{
    unsigned char seg[10] = {
        0x3F,0x06,0x5B,0x4F,0x66,
        0x6D,0x7D,0x07,0x7F,0x67
    };

    P0 = 0x00;

    while(1)
    {
        unsigned x;

        buzzer = 0;
        disp1 = 0;
        disp2 = 0;
        disp3 = 0;
        disp4 = 0;

        for(x=0;x<10;x++)
        {
            P0 = seg[x];
            msdelay(100);
        }
    }
}

void msdelay(unsigned int y)
{
    unsigned int i,j;

    for(i=0;i<y;i++)
    for(j=0;j<1275;j++);
}

EXP8
; Write assembly language program to transmit the characters
; "YCCE" serially at 9600 baud.
; Assume XTAL = 11.0592MHz

MOV TMOD,#20H
MOV TH1,#0FDH
MOV SCON,#50H
SETB TR1

AGAIN: MOV A,#'Y'
       ACALL SrTx

       MOV A,#'C'
       ACALL SrTx

       MOV A,#'C'
       ACALL SrTx

       MOV A,#'E'
       ACALL SrTx

       MOV A,#' '
       ACALL SrTx

       SJMP AGAIN

SrTx: CLR TI
      MOV SBUF,A

LOOP: JNB TI,LOOP
      RET

END

EXP9
; Write an ALP to generate sawtooth waveform
; by using DAC0808 IC and microcontroller 8051.
; Interface DAC0808 to Port-1 of 8051.

MOV A,#00H

L1: MOV P1,A
    INC A
    SJMP L1

END

EXP10
/* Write a C program to display characters "ET DEPT"
   on first line and "YCCE" on second line of 4-bit LCD */

#include<reg51.h>

sfr x = 0x80;

sbit rs = P0^0;
sbit en = P0^1;
sbit rw = P0^2;
sbit bl = P0^3;
sbit buzzer = P2^2;

void msdelay(unsigned int);
void lcd_cmd(unsigned char);
void lcd_data(unsigned char);

void main(void)
{
    P0 = 0x00;
    bl = 1;
    buzzer = 0;

    lcd_cmd(0x28);
    lcd_cmd(0x02);
    lcd_cmd(0x0E);
    lcd_cmd(0x01);
    lcd_cmd(0x82);

    msdelay(10);

    lcd_data('E');
    lcd_data('T');
    lcd_data(' ');
    lcd_data('D');
    lcd_data('E');
    lcd_data('P');
    lcd_data('T');
    lcd_data(' ');

    msdelay(10);

    lcd_cmd(0xC6);

    lcd_data('Y');
    lcd_data('C');
    lcd_data('C');
    lcd_data('E');

    while(1);
}

void lcd_cmd(unsigned char c)
{
    x = (c & 0xF0);

    rs = 0;
    rw = 0;
    en = 1;

    msdelay(10);

    en = 0;

    x = ((c << 4) & 0xF0);

    rs = 0;
    rw = 0;
    en = 1;

    msdelay(10);

    en = 0;
}

void lcd_data(unsigned char d)
{
    x = (d & 0xF0);

    rs = 1;
    rw = 0;
    en = 1;

    msdelay(10);

    en = 0;

    x = ((d << 4) & 0xF0);

    rs = 1;
    rw = 0;
    en = 1;

    msdelay(10);

    en = 0;
}

void msdelay(unsigned int y)
{
    unsigned int i,j;

    for(i=0;i<y;i++)
    for(j=0;j<1275;j++);
}


