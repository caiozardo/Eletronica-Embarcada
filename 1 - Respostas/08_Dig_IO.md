Para todas as questões, utilize os LEDs e/ou os botões da placa Launchpad do MSP430.

1. Escreva um código em C que pisca os LEDs ininterruptamente.

***********************************************************************

#include <msp430g2553.h>     // Biblioteca da MSP430 MODELO G2553
#define BTN BIT2             // btn=0000 00010
#define LED1 BIT0            // LED=0000 0001
#define LED2 BIT6            // LED2= 0010 0000
#define LED_DLY 5000         // VaLOR PARA SER DECREMENTADO

void delay( volatile unsigned long int t)
{
	while(t--);
}

void led_acende (){
  while(1)
  {
    P1OUT|= LED1+ LED2;
    break;    
    }  
  }

void led_apaga(){
  while(1)
  {
    P1OUT&=~(LED1+LED2);
    break;
    }  
  }

void main(void){
WDTCTL = WDTPW | WDTHOLD; //Parar o whatdog timer
P1OUT = 0x00;                 // Desligar os Leds no inicio
P1DIR = LED1 + LED2;       // Deixar disponiveis os valores
for(;;)
{
led_apaga();
delay(LED_DLY);
led_acende();
delay(LED_DLY);
}
}

***********************************************************************

2. Escreva um código em C que pisca os LEDs ininterruptamente. No ciclo que pisca os LEDs, o tempo que os LEDs ficam ligados deve ser duas vezes maior do que o tempo que eles ficam desligados.

***********************************************************************

void piscaV2(volatile unsigned int Tdelay)
{
    P1OUT &= ~LEDS;
    while(true)
    {
    	P1OUT |= LEDS;
    	atraso(2 * Tdelay);
    	P1OUT &= ~LEDS;
    	atraso(Tdelay);
    }
}

***********************************************************************

3. Escreva um código em C que acende os LEDs quando o botão é pressionado.

***********************************************************************

#include <msp430g2553.h>
#define LEDS (BIT0|BIT6)
#degine BTN BIT3

int main(void)
{
WDTCTL = WDTPW | WDTHOLD;
P1OUT |= LEDS;				
P1DIR |= LEDS;
P1DIR &= ~BTN;
P1REN |= BTN;				Resistor de pull-up
P1OUT |= BTN;				Resistor de pull-up
while(1)
{
while (P1IN&BTN)>0);
P1OUT |= LEDS;
while (P1IN&BTN)==0);
P1OUT &=~LEDS;
}
return 0;
}

***********************************************************************

4. Escreva um código em C que pisca os LEDs ininterruptamente somente se o botão for pressionado.

***********************************************************************

#include <msp430g2553.h>             
#define BTN BIT3                  
#define LED1 BIT0        
#define LED2 BIT6         
#define LED_DLY 100000      
#define DUAS_X_LED_DLY 50000 
  
void delay( volatile unsigned long int t)
{
	while(t--);
}

void led_acende (){
  while(1)
  {
    P1OUT|= LED1+ LED2;
    break;    
    }  
  }

void led_apaga(){
  while(1)
  {
    P1OUT&=~(LED1+LED2);
    break;
    }  
  }

int main( void )
{
  volatile unsigned int i;
  // Stop watchdog timer to prevent time out reset
  WDTCTL = WDTPW + WDTHOLD;
  P1OUT = 0x00;
  P1REN |= BTN;                 // Habilitando o resistor de pull-up ou pull-down[Depende da saída]
  P1DIR |= LED1 + LED2;
  P1OUT = BTN;               //Habilitando o botão, será pull-up
    for(;;)
    {
      if((P1IN & BTN) == 0)
      {
        led_acende ();
       delay(LED_DLY);
       led_apaga();
       delay(LED_DLY);
      }
      else
      {
       led_apaga();
      }
    } 
}

***********************************************************************

5. Escreva um código em C que acende os LEDs quando o botão é pressionado. Deixe o MSP430 em modo de baixo consumo, e habilite a interrupção do botão.

***********************************************************************

int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;
	P1DIR |= LEDS;
	P1DIR &= ~BTN;
	P1REN |= BTN;
	P1OUT |= BTN;
	P1IES |= BTN;
	P1IE |= BTN;
	_BIS_SR(GIE + LPM4_bits);
	return 0;
}

interrupt(PORT1_VECTOR) Interrupcao_P1(void)
{
	P1OUT |= LEDS;
	while((P1IN&BTN)==0);
	P1OUT &= ~LEDS;
	P1IFG &= ~BTN;
}

***********************************************************************