Para cada questão, escreva funções em C e/ou sub-rotinas na linguagem Assembly do MSP430. Reaproveite funções e sub-rotinas de uma questão em outra, se assim desejar. Leve em consideração que as sub-rotinas são utilizadas em um código maior, portanto utilize adequadamente os registradores R4 a R11. As instruções da linguagem Assembly do MSP430 se encontram ao final deste texto.

1. (a) Escreva uma função em C que calcule a raiz quadrada `x` de uma variável `S` do tipo float, utilizando o seguinte algoritmo: após `n+1` iterações, a raiz quadrada de `S` é dada por

```
x(n+1) = (x(n) + S/x(n))/2
```

O protótipo da função é:

```C
unsigned int Raiz_Quadrada(unsigned int S);
```

*********************************************

float Raiz_Quadrada(float S, float x)
{
	float y;
	int i;
	if (x == 0)
	{
		x +=1;
	}
    do
	{	
		x= (x + S/x)/2;
		y=(x + y)/2;
	}while (y-x!=0);

	return x;
}

*********************************************

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. A variável `S` é fornecida pelo registrador R15, e a raiz quadrada de `S` (ou seja, a variável `x`) é fornecida pelo registrador R15 também.

*******************************************************************

S-> R15, X->R11, RES -> R10, AUX->R8

  ;*******RAIZ EM ASSEMBLY*****************************************

  ;*******DIVISÃO ASSEMBLY*****************************************

DIV:
clr.w R10              ; R1O=0
MOV.W R11,R8           ; R8=R11

COND_DIV:
comp r15,r11           ; r15>r11 ou r15=r11
jn   SAIR              ; se NÃO satisfazer a condição acima termina o programa
INC.W R10              ; R10++1
ADD.W R11,R8           ; R9=R11+R8
JUMP COND_DIV          ; SAIR DA CONDIÇÃO

SAIR:
;PUSH.W R10            ; COLOCOU NA PILHA O VALOR DE 
RET                    ; RETORNA AO ENDEREÇO QUE A CHAMOU

raiz_babilonico:       ;Rótulo da função
mov.w #1,R11           ; x=1
mov.w R15,R9           ;aux=p=s

WHILE:
test R9                ; R9-0
jeq  EXIT              ;
DEC.W R9               ; P-=1

EXIT:
PUSH.W R15             ; guarde S na pilha
PUSH.W R14             ; guarde x na pilha
CALL #DIV              ; S/X
ADD.W  R11 ,R10        ;(s/x)+x)
RRA.W R10              ;(((s/x)+x)/2)
MOV.W R10,R14;         ;x=(((s/x)+x)/2)
PUSH.W R14

RET                     ; retorna aonde a função foi chamada

2. (a) Escreva uma função em C que calcule `x` elevado à `N`-ésima potência, seguindo o seguinte protótipo: 

```C
int Potencia(int x, int N);
```

********************************************************

int x,N,i;

for(i<n; i=0; i++){
x*=x;
                  }
return x;
}*/

#include <stdio.h>

int MULT_unsigned(int a, int b)
{
if(b==0) return 0;
else
return (a+MULT_unsigned(a, b-1));
}
int MULT_signed(int a, int b)
{
	if(a<0 && b<0)
	{	
                a = -a;
		b = -b;
		return MULT_unsigned(a,b);
	}
	else if (a<0 && b>0)
	{	a = -a;
		return -(MULT_unsigned(a,b));
	}
	else if (a>0 && b<0)
	{	b = -b;
		return -(MULT_unsigned(a,b));
	}
	else
	{	return MULT_unsigned(a,b);
	}
}

int Potencia(int x, int N){
    if(N==0) return 1;
    else   return(MULT_signed(x,Potencia(x,N-1)));
}
int main(){
    int N,x,resultado=0;
    printf("Escreva os fatores do produto:\n");
    scanf("%d\n%d",&x,&N);
    resultado=Potencia(x,N);
    printf("%d\n",resultado);
}

********************************************************

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. `x` e `n` são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida no registrador R15.

********************************************************

Potencia:
cmp #0, R14		; N == 0
jeq Potencia_1
mov.w #0, R12		; i = 0
dec.w R14		; N--
mov.w R15, R13          ; R13 = X

loop_potencia:
cmp R14, R12		; i < N-1
jeq Fim_potencia	; Pular se i == N-1
push R14
push R12
push R13
mov.w R13, R14		; R14 = x
call #Mult_unsigned	; R15 = R15*x
pop R13
pop R12
pop R14
inc.w R12	        ; i++
jmp loop_potencia

Potencia_1:
mov.w #1, R15

Fim_potencia:
ret

********************************************************

3. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula a divisão de `a` por `b`, onde `a`, `b` e o valor de saída são inteiros de 16 bits. `a` e `b` são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida através do registrador R15.

********************************************************

DIV: NOP
cmp #0, R14
jne DI2
mov #-1, R15
ret

DIV2: cmp R14, R15
jge DIVL
mov #0, R15
ret

DIVL: NOP
push R13 ; R13 é i
push R12 ; R12 é res
mov #1, R13

DIVT: NOP
push R15
mov R13, R15
call #MULT
mov R15, R12
pop R15
cmp R15, R12
jge DIVE
inc R13
jmp DIVT

DIVE: NOP
mov R13, R15
dec R15
pop R12
pop R13

FIM:
ret

********************************************************

4. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula o resto da divisão de `a` por `b`, onde `a`, `b` e o valor de saída são inteiros de 16 bits. `a` e `b` são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida através do registrador R15.

********************************************************

em c:

#include <stdio.h>

int MULT_unsigned(int a, int b)
{
if(b==0) return 0;
else
return (a + MULT_unsigned (a, b-1));
}
int MULT_signed(int a, int b)
{
	if(a<0 && b<0)
	{	a = -a;
		b = -b;
		return MULT_unsigned(a, b);
	}
	else if (a<0 && b>0)
	{	a = -a;
		return -(MULT_unsigned(a,b));
	}
	else if (a>0 && b<0)
	{	b = -b;
		return -(MULT_unsigned(a,b));
	}
	else
	{	return MULT_unsigned(a,b);
	}
}

int div(int D, int q){
    int i=0, aux=q;
while(aux < D || aux==D){
    i++;
    aux=aux+q;
    }
    return i;
}
int res_div(int D, int d,int q){
    return (D-MULT_signed(q,d));
}
int main(){
    int D,q,d,resto;
    printf("Escreva o Divisor,quociente:\n");
    scanf("%i\n",&D);
    scanf("%i\n",&q);
    d=div(D,q);
    printf("%d\n",d);
    resto=res_div(D,d,q);
    printf("%d",resto);
}

;*******DIVISÃO ASSEMBLY********************************

DIV:
clr.w R10               ; R1O=0
MOV.W R11,R8            ; R8=R11
COND_DIV:
comp r15,r11            ; r15>r11 ou r15=r11
jn   SAIR               ; se NÃO satisfazer a condição acima termina o programa
INC.W R10               ; R10++1
ADD.W R11,R8            ; R9=R11+R8
JUMP COND_DIV           ; SAIR DA CONDIÇÃO

SAIR:
;PUSH.W R10             ; COLOCOU NA PILHA O VALOR DE 
RET                     ; RETORNA AO ENDEREÇO QUE A CHAMOU

RESTO:                  ; Rótulo da função
PUSH.W R15              ; guarde S na pilha
PUSH.W R14              ; guarde x na pilha
CALL #DIV               ; A/B
MOV.W R10,R15           ;
POP.W R10               ;A
POP.W R14               ;B
MOV.W R10,R15           ;
CALL #MULT_signed       ;RES*B
SUB.W R15,R10           ; A - RES*B ARMAZENADO EM R10[RESTO]
RET

********************************************************

5. (a) Escreva uma função em C que indica a primalidade de uma variável inteira sem sinal, retornando o valor 1 se o número for primo, e 0, caso contrário. Siga o seguinte protótipo:

```C
int Primalidade(unsigned int x);
```

********************************************************

int Primalidade (unsigned int x)
	{
unsigned int d;
if(x<2) return 0;
if((x&1)==0) return 0;	
d=x/2;
while(d>2)
         {
        	if((x%d)==0) return 0;
		d--;
         }
	return 1;
	}

********************************************************

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. A variável de entrada é fornecida pelo registrador R15, e o valor de saída também.

********************************************************

Primalidade:
push R4 	;d
cmp #2, R15
jge Teste_par

Prim_zero:	
pop R4
clr R15
ret

Teste_par:
mov R15, R14
and #1, R14
cmp #0, R14
jeq Prim_zero

Teste_impar:
mov R15, R4
rra R4 		;d=x/2

Prim_while:
cmp #2, R4
jeq Prim_end
push R15
mov R4, R14
call Resto_uns
mov R15, R14
pop R15
cmp #0, R14
jeq Prim_zero
dec R4
jmp Prim_while

Prim_end:
pop R4
mov #1,R15
ret

********************************************************

6. Escreva uma função em C que calcula o duplo fatorial de n, representado por n!!. Se n for ímpar, n!! = 1*3*5*...*n, e se n for par, n!! = 2*4*6*...*n. Por exemplo, 9!! = 1*3*5*7*9 = 945 e 10!! = 2*4*6*8*10 = 3840. Além disso, 0!! = 1!! = 1.
O protótipo da função é:

```C
unsigned long long DuploFatorial(unsigned long long n);
```

********************************************************

unsigned long long duploFatorial(unsigned long long n) //aproveitando
{
    if (n == 0 || n==1)
      return 1;
    return n*doublefactorial(n-2);
}

********************************************************

7. (a) Escreva uma função em C que calcula a função exponencial utilizando a série de Taylor da mesma. Considere o cálculo até o termo n = 20. O protótipo da função é `double ExpTaylor(double x);`

********************************************************

#include <stdio.h>
#include <stdlib.h>
#include <math.h>

// e^x para 20 termos

int fatorial( int n )
{
if ( n <= 1 )
return 1;

else
return  n*fatorial(n-1);
}

double ExpTaylor (int n_term, double x)
{
if (n_term <= 1)
return 1;

else

return ExpTaylor(n_term-1, x) + pow(x, n_term-1)/fatorial(n_term-1);
}

int main()
{
double y,x=2;
y =ExpTaylor(20,x);
printf("taylor eh %f\n", y);
return 0;
}

********************************************************

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430, mas considere que os valores de entrada e de saída são inteiros de 16 bits. A variável de entrada é fornecida pelo registrador R15, e o valor de saída também.

********************************************************

ExpTaylor: ; R15 = x
push R5 ; guarda R5 na pilha
push R6 ; guarda R6 na pilha
clr R5 ; n = 0
clr R6 ; sum = 0

Forloop:
cmp #20, R5
jge For_Loop_End ; se R5 >= #20, sai do loop
mov.w R5, R14 ; R14 = R5 = n
call Potencia ; R15 = x**n
push R15 ; guarda x**n na pilha

mov.w R5, R15 ; R15 = n
call Fatorial ; fatorial de R15 = fatorial(n)
mov.w R15, R14 ; R14 = fatorial(n)
pop R15 ; recupera x**n da pilha em R15

call Div ; R15 = (x**n)/fatorial(n)

add.w R15, R6 ; sum+= (x**n)/fatorial(n)
jmp Forloop

For_Loop_End:
pop R6 ; recupera R6 da pilha
pop R5 ; recuepra R6 da pilha
ret

********************************************************

8. Escreva uma sub-rotina na linguagem Assembly do MSP430 que indica se um vetor esta ordenado de forma decrescente. Por exemplo:
[5 4 3 2 1] e [90 23 20 10] estão ordenados de forma decrescente.
[1 2 3 4 5] e [1 2 3 2] não estão.
O primeiro endereço do vetor é fornecido pelo registrador R15, e o tamanho do vetor é fornecido pelo registrador R14. A saída deverá ser fornecida no registrador R15, valendo 1 quando o vetor estiver ordenado de forma decrescente, e valendo 0 em caso contrário.

*******************em c**********************

int vetor_Dec(int x[], int N)
{ 
int i;
	for (i=0; i<(N-1); i++)
{
	if (x[i]<x[i+1])
	{
		return 0;
		{
	}
	return 1;
}
return 0;
***************OU*******************

int vetor_Dec(int x[], int N)
{ 
int i;
for (i=0; i<(N-1); i++, x++)
	{
	if ((*x)<(*(x+1)))
	{
return 0;
	{
}
return 1;
}
return 0;

******************em assembly*********************

Vetor_Dec:
push R4
clr R4    		;i=0
mov R14, R13
dec R13			;R13=N-1	 

Vetor_Dec_For:
cmp R13, R4			
jge Vetor_Dec_End	; Se sim pula pra funçao Vetor_Dec_End, senao segue o baile
cmp 2(R15), 0(R15)
jge Vetor_Dec_Inc
pop R4
clr R15
ret

Vetor_Dec_Inc:
inc R4
incd R15
jmp Vetor_Dec_For

Vetor_Dec_End:
pop R4	mov #1, R15
ret

***************************************************

9. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula o produto escalar de dois vetores, `a` e `b`. O primeiro endereço do vetor `a` deverá ser passado através do registrador R15, o primeiro endereço do vetor `b` deverá ser passado através do registrador R14, e o tamanho do vetor deverá ser passado pelo registrador R13. A saída deverá ser fornecida no registrador R15.

***************************************************

Prod_Int:
push R5 ; guarda R5 na pilha
mov.w R14, R5 ; R5 = &b(0)
push R6 ; guarda R6 na pilha
mov.w R15, R6 ; R6 = &a(0)
clr R12 ; R12 = 0

PI_Loop:
mov.w 0(R6), R15
mov.w 0(R5), R14
call Multiplica ; r15 = R15 * R14
add.w R15, R12
incd.w R6
incd.w R5
dec.w R13

Primo_Loop_End:
mov.w R12, R15
pop R6
pop R5
ret

10. (a) Escreva uma função em C que indica se um vetor é palíndromo. Por exemplo:
	[1 2 3 2 1] e [0 10 20 20 10 0] são palíndromos.
	[5 4 3 2 1] e [1 2 3 2] não são.
Se o vetor for palíndromo, retorne o valor 1. Caso contrário, retorne o valor 0. O protótipo da função é:

```C
int Palindromo(int vetor[ ], int tamanho);
```

*********************CONTINUANDO**************************

int Palindromo(int *p,int tamanho)
{
	int i=0;
	while(i<tamanho)
	{
		if(p[i]!=p[(tamanho-1)-i])
	{
return 0;
	}
i++;

	}
	return 1;
}

**********************************************************

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. O endereço do vetor de entrada é dado pelo registrador R15, o tamanho do vetor é dado pelo registrador R14, e o resultado é dado pelo registrador R15.

**********************************************************

Palindromo:
push R4
push R5
clr R4		;i=0
mov R14, R5
dec R5		;k=N-1
rra R14		;k/2
dec R14		;k/2-1

Palin_test:	
cmp R4, R14
jl Palin_end
mov R4, R12
rla R12
add R15, R12
mov R5, R13
rla R13
add R15, R13
cmp 0(R12), 0(R13)
jeq Palin_inc
pop R5
pop R4
clr R15
ret

Palin_inc:
inc R4
dec R5
jmp Palin_test

Palin_end:
pop R5
pop R4
mov #1, R15
ret

*****************************************************