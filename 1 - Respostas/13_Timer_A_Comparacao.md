Para todas as quest�es abaixo, utilize o modo de compara��o do Timer A.

1. Para os itens abaixo, confira a diferen�a no brilho do LED.
	(a) Pisque o LED no pino P1.6 numa frequ�ncia de 100 Hz e ciclo de trabalho de 25%.

**********************************************************************************************

#include <msp430g2553.h>
#define LED BIT6
#define PERIODO 1250 // teremos 100 hz
#define ciclo_de_trabalho 312 // seria o 25%
#define freqs 1 

int main(void)
{
WDTCTL = WDTPW + WDTHOLD;
BCSCTL1 = CALBC1_1MHZ;
DCOCTL = CALDCO_1MHZ;
P1DIR |= LED;
P1SEL |= LED;
P1SEL2 &= ~LED;
TACCR0 = PERIODO-1;
TACCR1 = ciclo_de_trabalho-1;
TACCTL1 = OUTMOD_7;
TACTL = TASSEL_2 + ID_3 + MC_1;
_BIS_SR(LPM0_bits);
}

**********************************************************************************************

	(b) Pisque o LED no pino P1.6 numa frequ�ncia de 100 Hz e ciclo de trabalho de 50%.

**********************************************************************************************

#include <msp430g2553.h>
#define LED BIT6
#define PERIODO 1250 // teremos 100 hz
#define ciclo_de_trabalho 625 // seria o 50%
#define freqs 1 

int main(void)
{
WDTCTL = WDTPW + WDTHOLD;
BCSCTL1 = CALBC1_1MHZ;
DCOCTL = CALDCO_1MHZ;
P1DIR |= LED;
P1SEL |= LED;
P1SEL2 &= ~LED;
TACCR0 = PERIODO-1;
TACCR1 = ciclo_de_trabalho-1;
TACCTL1 = OUTMOD_7;
TACTL = TASSEL_2 + ID_3 + MC_1;
_BIS_SR(LPM0_bits);
}

**********************************************************************************************

	(c) Pisque o LED no pino P1.6 numa frequ�ncia de 100 Hz e ciclo de trabalho de 75%.

**********************************************************************************************

#include <msp430g2553.h>
#define LED BIT6
#define PERIODO 1250 // teremos 100 hz
#define ciclo_de_trabalho 938 // seria o 75%
#define freqs 1 

int main(void)
{
WDTCTL = WDTPW + WDTHOLD;
BCSCTL1 = CALBC1_1MHZ;
DCOCTL = CALDCO_1MHZ;
P1DIR |= LED;
P1SEL |= LED;
P1SEL2 &= ~LED;
TACCR0 = PERIODO-1;
TACCR1 = ciclo_de_trabalho-1;
TACCTL1 = OUTMOD_7;
TACTL = TASSEL_2 + ID_3 + MC_1;
_BIS_SR(LPM0_bits);
}

**********************************************************************************************


2. Pisque o LED no pino P1.6 numa frequ�ncia de 1 Hz e ciclo de trabalho de 25%.

**********************************************************************************************

#include <msp430g2553.h>
#define LED BIT6
#define PERIODO 62500 // teremos 2 Hz
#define ciclo_de_trabalho 3907 // seria o 25% de 25 %
#define freqs 1 

atraso(void){
BCSCTL1 = CALBC1_1MHZ;
DCOCTL = CALDCO_1MHZ;
TACCR0 = PERIODO-1;
atraso();
}
int main(void)
{
	WDTCTL = WDTPW + WDTHOLD;
	P1DIR |= LED;
	P1SEL |= LED;
	P1SEL2 &= ~LED;
	atraso();	
        
	TACCR1 = ciclo_de_trabalho-1;
	TACCTL1 = OUTMOD_7;
	TACTL = TASSEL_2 + ID_3 + MC_1;
	_BIS_SR(LPM0_bits);   
}

**********************************************************************************************

3. Pisque o LED no pino P1.6 numa frequ�ncia de 1 Hz e ciclo de trabalho de 50%.

**********************************************************************************************

#include <msp430g2553.h>
#define LED BIT6
#define PERIODO 62500 // teremos 2 Hz
#define ciclo_de_trabalho 15625 // seria o 50% de 50 %
#define freqs 1 

atraso(void){
BCSCTL1 = CALBC1_1MHZ;
DCOCTL = CALDCO_1MHZ;
TACCR0 = PERIODO-1;
atraso();
}

int main(void)
{
	WDTCTL = WDTPW + WDTHOLD;
	P1DIR |= LED;
	P1SEL |= LED;
	P1SEL2 &= ~LED;
	atraso();	
        
	TACCR1 = ciclo_de_trabalho-1;
	TACCTL1 = OUTMOD_7;
	TACTL = TASSEL_2 + ID_3 + MC_1;
	_BIS_SR(LPM0_bits);
	
	
       
        }

**********************************************************************************************

4. Pisque o LED no pino P1.6 numa frequ�ncia de 1 Hz e ciclo de trabalho de 75%.

**********************************************************************************************

O TIMER A EST� CONFIGURADO COM UM MCLK EQUIVALENTE AO CLOCK DE ENTRADA, ONDE � DIVIDIDO POR 8 E COM PER�ODO DE 500MS.
AO LIGAR O MODO TOGGLE, O PER�ODO DA ONDA � DOBRADO E POR INTERM�DIO O REG TA0CCR1 � POSS�VEL CONFIGURAR O CICLO DE TRABALHO PARA O EXERC�CIO.

void ConfigPwm1Hz(volatile unsigned int ciclo)
{
	TACCR0 = 62500-1;
	TACCR1 = (ciclo*62500/100)-1;
	TACCTL1 = OUTMOD_4; //Modo Toggle para dobrar per�odo
	TACTL = TASSEL_2 + ID_3 + MC_3;
}

**********************************************************************************************