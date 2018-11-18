1. Projete o hardware necessário para o MSP430 controlar um motor DC de 12V e 4A. Utilize transistores bipolares de junção (TBJ) com Vbe = 0,7 V, beta = 100 e Vce(saturação) = 0,2 V. Além disso, considere que Vcc = 3 V para o MSP430, e que este não pode fornecer mais do que 10 mA por porta digital.

************************************************************************************************************

Vbe = 0,7V, beta = 100 e Vce(saturação) = 0,2 V. 
CONSIDERAÇÕES:
Vcc = 3 V para o MSP430, não pode fornecer mais do que 10 mA por porta digital.

O MOTOR DC EH O TRANSISTOR SUPORTE A CORRENTE DO COLETOR
A CORRENTE NECESSÁRIA AO COLETOR EH 4A. ENTÃO:

Ib = Ic 
100 = 4 
100 = 40 mA.

SABEMOS QUE DEVEREMOS USAR DOIS TRANSISTORES POIS, A CORRENTE DE BASE ULTRAPASSA A CORRENTE QUE A PORTA DIGITAL TEM CAPACIDADE DE FORNECER NA FORMA DO PAR DARLINGTON.
LOGO A CORRENTE DE BASE EH:

Ib = Ic
ß2 = 4 
10000 = 0,4 mA

O resistor Rb = (Vcc – 2*Vbe)
Ib = (3 – 2*0,7)
0,4*10^(-3) = 4 Kohm

************************************************************************************************************

2. Projete o hardware necessário para o MSP430 controlar um motor DC de 10V e 1A. Utilize transistores bipolares de junção (TBJ) com Vbe = 0,7 V e beta = 120. Além disso, considere que Vcc = 3,5 V para o MSP430, e que este não pode fornecer mais do que 10 mA por porta digital.

************************************************************************************************************

Ic = B*Ib 
1 = 120*Ib 
Ib = 8.33 mA	Como Ib é menor que Ib pode-se usar o transistor bipolar.

Calculo da resistência:

Rb = (Vcc-Vbe)/Ib 
Rb = (3.5-0.7)/8.33*10^(-3) 
Rb = 336 Ohm

************************************************************************************************************

3. Projete o hardware utilizado para controlar 6 LEDs utilizando charlieplexing. Apresente os pinos utilizados no MSP430 e os LEDs, nomeados L1-L6.

************************************************************************************************************

Utilizaremos 3 pinos: P1.1,P1.2,P1.4;
P1DIR &= ~ P1.1  , P1.2=1 , P1.4=0 , -> LED 1 (L1) //P1.1 ESTÁ COMO ENTRADA DIGITAL
P1DIR &= ~ P1.1 , P1.2=0 , P1.4=1 , -> LED 2 (L2)  //P1.1 ESTÁ COMO ENTRADA DIGITAL
P1.1=1 , P1.2=0 ,P1DIR &= ~ P1.4 , -> LED 3 (L3)   //P1.4 ESTÁ COMO ENTRADA DIGITAL
P1.1=0 , P1.2=1 ,P1DIR &= ~ P1.4 , -> LED 4 (L4)   //P1.4 ESTÁ COMO ENTRADA DIGITAL
P1.1=1 ,P1DIR &= ~ P1.2 , P1.4=0 , -> LED 5 (L5)   //P1.2 ESTÁ COMO ENTRADA DIGITAL
P1.1=0 ,P1DIR &= ~ P1.2 , P1.4=1 , -> LED 6 (L6)   //P1.2 ESTÁ COMO ENTRADA DIGITAL

************************************************************************************************************

4. Defina a função `void main(void){}` para controlar 6 LEDs de uma árvore de natal usando o hardware da questão anterior. Acenda os LEDs de forma que um ser humano veja todos acesos ao mesmo tempo.

************************************************************************************************************

void main(void)
{
    while(1)
    {
        //ACENDENDO D1
		P1DIR = (P1DIR | (BIT1 + BIT2)) & (~BIT0)
		P1OUT = (P1OUT | BIT1) & (~BIT2);
        //ACENDENDO D2
		P1DIR = (P1DIR|(BIT1+BIT2))&(~BIT0)
		P1OUT = (P1OUT|BIT2)&(~BIT1);
        //ACENDENDO D3
		P1DIR = (P1DIR|(BIT0+BIT1))&(~BIT2)
		P1OUT = (P1OUT|BIT0)&(~BIT1);
        //ACENDENDO D4
		P1DIR = (P1DIR|(BIT0+BIT1))&(~BIT2)
		P1OUT = (P1OUT|BIT1)&(~BIT0);
        //ACENDENDO D5
		P1DIR = (P1DIR|(BIT0+BIT2))&(~BIT1)
		P1OUT = (P1OUT|BIT0)&(~BIT2);
        //ACENDENDO D6
		P1DIR = (P1DIR|(BIT0+BIT2))&(~BIT1)
		P1OUT = (P1OUT|BIT2)&(~BIT0);
	}
}

************************************************************************************************************

5. Defina a função `void main(void){}` para controlar 6 LEDs de uma árvore de natal usando o hardware da questão 3. Acenda os LEDs de forma que um ser humano veja os LEDs L1 e L2 acesos juntos por um tempo, depois os LEDs L3 e L4 juntos, e depois os LEDs L5 e L6 juntos.

************************************************************************************************************

#include<msp430g2553.h>
#define LED_DLY 5000      // VALOR A SER DECREMENTADO

void delay( volatile unsigned long int t)
{
	while(t--);
}

int main(void){
WDTCTL = WDTPW | WDTHOLD; //Parar o whatdog timer
while(1)
{       //LED 1
	P1DIR &= ~ BIT1;        //Entrada digital
	P1DIR|= (BIT2+ ~BIT4); //combinações de saida digitais
	P1REN|= (BIT2+ ~BIT4);
	P1OUT|= (BIT2+ ~BIT4);
//LED2
	P1DIR &= ~ BIT1;        //Entrada digital
	P1DIR|= (BIT4+ ~BIT2); //combinações de saida digitais
	P1REN|= (BIT4+ ~BIT2);
	P1OUT|= (BIT4+ ~BIT2);
delay(1);
//LED 3
	P1DIR &= ~ BIT4;       //Entrada digital
	P1DIR|= (BIT1+ ~BIT2); //combinações de saida digitais
	P1REN|= (BIT1+ ~BIT2);
	P1OUT|= (BIT1+ ~BIT2);
//LED4
	P1DIR &= ~ BIT4;       //Entrada digital
	P1DIR|= (BIT2+ ~BIT1); //combinações de saida digitais
	P1REN|= (BIT2+ ~BIT1);
	P1OUT|= (BIT2+ ~BIT1);
delay(1);
//LED5
	P1DIR &= ~ BIT2;       //Entrada digital
	P1DIR|= (BIT1+ ~BIT4); //combinações de saida digitais
	P1REN|= (BIT1+ ~BIT4);
	P1OUT|= (BIT1+ ~BIT4);
//LED6
	P1DIR &= ~ BIT2;       //Entrada digital
	P1DIR|= (BIT4+ ~BIT1); //combinações de saida digitais
	P1REN|= (BIT4+ ~BIT1);
	P1OUT|= (BIT4+ ~BIT1); 
delay(1);
}
}

************************************************************************************************************

6. Defina a função `void EscreveDigito(volatile char dig);` que escreve um dos dígitos 0x0-0xF em um único display de 7 segmentos via porta P1, baseado na figura abaixo. Considere que em outra parte do código os pinos P1.0-P1.6 já foram configurados para corresponderem aos LEDs A-G, e que estes LEDs possuem resistores externos para limitar a corrente.

```
        ---  ==> A
       |   |
 F <== |   | ==> B
       |   |
        ---  ==> G
       |   |
 E <== |   | ==> C
       |   |
        ---  ==> D
```

************************************************************************************************************

#define SA BIT0
#define SB BIT1
#define SC BIT2
#define SD BIT3
#define SE BIT4
#define SF BIT5
#define SG BIT6

void EscreveDigito(volatile char dig)
{
	P1OUT = 0;
	switch(dig)
	{
case 'A':
P1OUT |= (SA + SB + SC + SE + SF);
break;

case 'B':
P1OUT |= (SF + SE + SC + SD + SG);
break;

case 'C':
P1OUT |= (SA + SF + SE + SD);
break;

case 'D':
P1OUT |= ~(SA + SF);
break;

case 'E':
P1OUT |= ~(SB + SC);
break;

case 'F':
P1OUT |= ~(SB + SC + SD);
break;

case '9':
P1OUT |= ~(SD + SE);
break;

case '8':
P1OUT ˆ= (P1OUT);
break;

case '7':
P1OUT |= (SA + SB + SC);
break;

case '6':
P1OUT |= ~(SB);
break;
		
case '5':
P1OUT |= ~(SB + SE);
break;
		
case '4':
P1OUT |= ~(SA + SE + SD);
break;
		
case '3':
P1OUT |= ~(SF + SE);
break;
		
case '2':
P1OUT |= ~(SF + SC);
break;
		
case '1':
P1OUT |= (SB + SC);
break;
		
case '0':
P1OUT |= ~(SG);
break;	

default:
break;
	}
}

************************************************************************************************************

7. Multiplexe 2 displays de 7 segmentos para apresentar a seguinte sequência em loop:
	00 - 11 - 22 - 33 - 44 - 55 - 66 - 77 - 88 - 99 - AA - BB - CC - DD - EE - FF

************************************************************************************************************

DEVEMOS CONSIDERAR QUE A LIGAÇÃO DOS 2 DISPLAYS SERÃO FEITAS NO MESMO NÓ.
RESULTANDO QUE AO SE MANIPULAR O HARDWARE A LÓGICA FEITA PARA UM DISPLAY TAMBÉM SERVIRÁ PARA O OUTRO

#include<msp430g2553.h>
#define LED_DLY 5000      // VaLOR PARA SER DECREMENTADO

volatile unsigned long int i;
void delay( volatile unsigned long int t)
{
	while(t--);
}
void escrevadigito(volatile unsigned long int dig){
P1DIR=0xff;
if(dig == 0){
P1OUT= BIT0+BIT1+BIT2+BIT3+BIT4+BIT5;
}
if(dig == 1){
P1OUT= BIT1+BIT2;
}
if(dig == 2){
P1OUT= BIT0+BIT1+BIT3+BIT4+BIT6;
}
if(dig == 3){
P1OUT= BIT0+BIT1+BIT2+BIT3+BIT6;
}
if(dig == 4){
P1OUT= BIT0+BIT1+BIT2+BIT3+BIT4+BIT5;
}
if(dig == 5){
P1OUT= BIT1+BIT2+BIT5+BIT6;
}
if(dig == 6){
P1OUT= BIT0+BIT2+BIT3+BIT4+BIT5+BIT6;
}
if(dig == 7){
P1OUT= BIT0+BIT1+BIT2;
}
if(dig == 8){
P1OUT= BIT0+BIT1+BIT2+BIT3+BIT4+BIT5+BIT6;
}
if(dig == 9){
P1OUT= BIT0+BIT1+BIT2+BIT3+BIT5+BIT6;
}
if(dig ==11){
P1OUT= BIT0+BIT1+BIT2+BIT4+BIT5+BIT6;
}
if(dig ==11){
P1OUT= BIT2+BIT3+BIT4+BIT5+BIT6;
}
if(dig ==12){
P1OUT= BIT0+BIT3+BIT4+BIT5;
}
if(dig ==13){
P1OUT= BIT1+BIT2+BIT3+BIT4+BIT6;
}
if(dig ==14){
P1OUT= BIT0+BIT3+BIT4+BIT5+BIT6;
}
if(dig ==15){
P1OUT= BIT0+BIT4+BIT5+BIT6;
}
}
int main(void){
WDTCTL = WDTPW | WDTHOLD; //Parar o whatdog timer

while(1)
{ 
	for(i=15;i>0; i--){
	 escrevadigito(i);
	 delay(LED_DLY);
	}
}
}

************************************************************************************************************








