1. Escreva uma função em C que faz o debounce de botões ligados à porta P1.

*****************************************************************************************************

#include <msp430g2553.h> 
#define BTN BIT2         
#define LED1 BIT0        
#define LED2 BIT6         
#define LED_DLY 5000      

void delay( volatile unsigned long int t)
{
	while(t--);
}

int Debounce(int porta_1)
{
if(porta_1 = 1)
 {
     delay(100);		   // se houver mudança, aguarda um tempo
     if(porta_1 = 1)
       {
          return P1IN & porta_1;   // espera suficiente
        }
    else if(porta_1 = 0)
        {
          return P1IN &~ porta_1;  // não esperou o que deveria
         }
  {

*****************************************************************************************************

2. Escreva um código em C que lê 9 botões multiplexados por 6 pinos, e pisca os LEDs da placa Launchpad de acordo com os botões. Por exemplo, se o primeiro botão é pressionado, os LEDs piscam uma vez; se o segundo botão é pressionado, os LEDs piscam duas vezes; e assim por diante. Se mais de um botão é pressionado, os LEDs não piscam.

*****************************************************************************************************

#include <msp430.h>
#define ROW1 BIT2
#define ROW2 BIT3
#define ROW3 BIT4
#define COL1 BIT5
#define COL2 BIT6
#define COL3 BIT7
#define LED1 BIT0
#define LED2 BIT1
#define LEDS (LED1|LED2)

void pisca(); 
void Atraso_ms(volatile unsigned int ms); 

int numPos(long unsigned int i, long unsigned int j); 

void main(void)
{
int coord[3][3]; 				//[Linhas][Colunas]
long unsigned int i, j;
int q_press, num;
WDTCTL = WDTPW | WDTHOLD;
P1DIR |= LEDS;
P1DIR &= ~(ROW1 + ROW2 + ROW3);         // primeiras entradas
P1DIR |= (COL1 + COL2 + COL3); 		// primeiras saídas
P1REN |= (ROW1 + ROW2 + ROW3); 		// on resist. entrada
P1OUT |= (ROW1 + ROW2 + ROW3); 		// pull-up

    while(1)
    {
q_press = 0;
coord = [ [0,0,0], [0,0,0], [0,0,0] ];

P1DIR |= LEDS;
P1DIR &= ~(ROW1 + ROW2 + ROW3); 	// primeiras entradas
P1DIR |= (COL1 + COL2 + COL3); 		// primeiras saídas
P1REN |= (ROW1 + ROW2 + ROW3); 		// on resist. entrada
P1OUT |= (ROW1 + ROW2 + ROW3); 		// ativa-os como pull-up
P1OUT &= ~(COL1 + COL2 + COL3);

        for (i = 1000; i <= 10000; i*= 10)
            if ((P1IN & i) == 0 )
                for (j = 0; j < 3; j++)
                    coord[i][j] = 1; 		// soma 1 a todas as colunas reduzidas
 
// ROWs são saídas, COLs são entradas
P1DIR |= (ROW1 + ROW2 + ROW3); 		// saídas
P1DIR &= ~(COL1 + COL2 + COL3); 	// entradas
P1REN |= (COL1 + COL2 + COL3); 		// on resist. entrada
P1OUT |= (COL1 + COL2 + COL3); 		// ativa-os como pull-up
P1OUT &= ~(ROW1 + ROW2 + ROW3);

        for (i = 100000; i <= 10000000; i*= 10)
            if ((P1IN & i) == 0 )
                for (j = 0; j < 3; j++)
                    coord[j][i] = 1; 		// soma 1 a todas as linhas reduzidas

        for (i = 0; i < 3; i++)
        {
            for (j = 0; j < 3; j++)
            {
                // quantidade de botões que foram apertados e seguidamente registrados 
                if(coord[i][j] == 2)
                    q_press++;
                      num = coord[i][j] == 2 ? numPos(i,j) : num;
            }
        }
        if (q_press == 1)
            for(i=0; i < num; i++)
                pisca();
    }
}

void pisca()
{
P1OUT |= LEDS;
Atraso_ms(1000);
P1OUT &= ~LEDS;
}

void Atraso_ms(volatile unsigned int ms)
{
TACCR0 = 1000-1;
TACTL = TACLR;
TACTL = TASSEL_2 + ID_0 + MC_1;
while(ms--)
   {
    while((TACTL&TAIFG)==0);
    TACTL &= ~TAIFG;
    }
    TACTL = MC_0;
}

int numPos(long unsigned int i, long unsigned int j)
{
    return (int) 3*j+i+1;
}

*****************************************************************************************************