1. Dada uma variável a do tipo char (um byte), escreva os trechos de códigos em C para:
(a) Somente setar o bit menos significativo de a:

***********************************************************

char byte = 0x00;  //Exemplo apenas para testar
char mask = 0x01;  //Mascara (00000001)
byte |= mask;
for (i=7 ; 0<=i ; i--)
                  printf("%d", (byte >> i) & 0x01);

***********************************************************

(b) Somente setar dois bits de a: o menos significativo e o segundo menos significativo:

***********************************************************

char byte = 0x00;  //Exemplo apenas para testar
char mask = 0x03;  //Mascara (00000011)
byte |= mask;
for (i=7 ; 0<=i ; i--)
                  printf("%d", (byte >> i) & 0x01);

***********************************************************

(c) Somente zerar o terceiro bit menos significativo de a:

***********************************************************

char byte = 0xFF;  //Exemplo apenas para testar
char mask = 0xFB;  //Mascara (11111011)
byte &= mask;
for (i=7 ; 0<=i ; i--)
                  printf("%d", (byte >> i) & 0x01);

***********************************************************

(d) Somente zerar o terceiro e o quarto bit menos significativo de a:

***********************************************************

char byte = 0xFF;  //Exemplo apenas para testar
char mask = 0xF3;  //Mascara (11110011)
byte &= mask;
for (i=7 ; 0<=i ; i--)
                  printf("%d", (byte >> i) & 0x01);

***********************************************************

(e) Somente inverter o bit mais significativo de a:

***********************************************************

char byte = 0xFF;  //Exemplo apenas para testar
char mask = 0x01;  //Mascara (00000001)
int i;
byte ^= mask;
for (i=7 ; 0<=i ; i--)
                  printf("%d", (byte >> i) & 0x01);

***********************************************************

(f) Inverter o nibble mais significativo de a, e setar o nibble menos significativo de a:

***********************************************************

char byte = 0xAA;  //Exemplo apenas para testar
char mask1 = 0x0F;  //Mascara (00001111)
char mask2 = 0xF0;  //Mascara (11110000)
int i;
byte |= mask1;
byte ^= mask2;
for (i=7 ; 0<=i ; i--)
                  printf("%d", (byte >> i) & 0x01);

***********************************************************

2. Considerando a placa Launchpad do MSP430, escreva o código em C para piscar os dois LEDs ininterruptamente.

***********************************************************

#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6

void main (void) {
  int contadorEspera = 0;

  WDTCTL = WDTPW | WDTHOLD; //Reseta o Watch dog timer de acordo com o registrador de Hold
  P1DIR = LED1 + LED2;
  P1OUT = LED1 + LED2;
  while (true) {
         P1OUT ^= LED1 + LED2;
      for (contadorEspera = 0; contadorEspera <= 10000; contadorEspera++);
  }
}

***********************************************************

3. Considerando a placa Lauchpad do MSP430, escreva o código em C para piscar duas vezes os dois LEDs sempre que o usuário pressionar o botão.

***********************************************************

#include <msp430g2553.h>
#define BUT BIT2
#define LED1 BIT0
#define LED2 BIT1

void main (void) {
  WDTCTL = WDTPW + WDTHOLD;
  P1OUT = 0;
  P1DIR = (LED1+LED2);
  for (;;){
  if (P1IN & BUT ==0){
 	P1OUT |= (LED1+LED2);
 	P1OUT = 0;
 	P1OUT |= (LED1+LED2);
 	P1OUT = 0;
 	while (P1IN & BTN == 0 ) {}
 	}	
 else {
 	P1OUT &= ~(LED1+LED2);}	
 	}
 }

***********************************************************

4. Considerando a placa Launchpad do MSP430, faça uma função em C que pisca os dois LEDs uma vez.

***********************************************************

#define COUNTER 32768

void blink () {
  int auxCount;
  P1OUT = LED1 + LED2;
  for (auxCount = 0; auxCount < COUNTER; auxCount++);
  P1OUT ^= (LED1 + LED2);
  for (auxCount = 0; auxCount < COUNTER; auxCount++);
}

***********************************************************

5. Reescreva o código da questão 2 usando a função da questão 4.

***********************************************************

#include <msp430g2553.h>
#define LED1 BIT0
#define LED2 BIT6

void main (void) {
  int contadorEspera = 0;

  WDTCTL = WDTPW | WDTHOLD; //Reseta o Watch dog timer de acordo com o registrador de Hold
  P1DIR = LED1 + LED2;
  P1OUT = LED1 + LED2;
  while (true) {
     blink();
  }
}

***********************************************************

6. Reescreva o código da questão 3 usando a função da questão 4.

***********************************************************

#include <msp430g2553.h>
#define BUT BIT2
#define LED1 BIT0
#define LED2 BIT1

 void main (void) {
 WDTCTL = WDTPW + WDTHOLD;
 P1OUT = 0;
 P1DIR = (LED1+LED2);
 for (;;){
 if (P1IN & BUT ==0){
 	blink();
 	while (P1IN & BTN == 0 ) {}
 	}
 else {
 	P1OUT &= ~(LED1+LED2);}
 	}
 }

***********************************************************
