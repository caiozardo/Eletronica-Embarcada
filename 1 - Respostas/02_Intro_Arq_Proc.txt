1. Quais as diferenças entre os barramentos de dados e de endereços?
O barramento de dados funcionam de maneira bidirecional entre a CPU e periféricos carregando dados e instruções, já os barramentos de endereços, diferentemente, interagem com memórias e periféricos funcionando de maneira unidirecional. 
O tamanho dos barramentos de endereço variam de acordo com a necessidade, de forma que os bits podem apresentar indereços diferentes, porém, o barramento de dados tem um tamanho definido de acordo com a capacidade de cada CPU.

2. Quais são as diferenças entre as memórias RAM e ROM?
A memória RAM permite a leitura e escrita de dados, é volátil, ou seja, perde os dados salvos assim que o sistema é desligado, logo, na mamória RAM não são salvos programas que podem comprometer o pleno funcionamento do Sistema Operacional. Então ela é usada para guardar dados os programas que podem ser recarregados para um rápido acesso.
A memória ROM permite, diferentemente da RAM, somente a leitura de dados, após serem escritos. Também se diferencia por ser uma memória não volátil, ou seja, lugar onde é posto programas de Controle do Sistema, essenciais ao funcionamento do sistema, sabendo que esses dados não serão perdidos caso o sistema seja desligado.

3. Considere o código abaixo:

#include <stdio.h>
int main(void)
{
         int i;
         printf("Insira um número inteiro: ");
         scanf("%d", &i);
         if(i%2)
                  printf("%d eh impar.\n");
         else
                  printf("%d eh par.\n");
         return 0;
}

Para este código, responda:

(a) A variável i é armazenada na memória RAM ou ROM? Por quê?
A variável i é armazenada na memória RAM, pois ela não é essencial para o sistema, ou seja, caso o sistema seja desligado, o valor armazenado nela será perdido e somente basta colocar outro valor para o sistema voltar a funcionar, ademais, esta variável permite a escrita e leitura de dados.

(b) O programa compilado a partir deste código é armazenado na memória RAM ou ROM? Por quê?
O programa compilado será armazenado na memória ROM, pois é um dado não volátil, o que deve ser salvo.

4. Quais são as diferenças, vantagens e desvantagens das arquiteturas Harvard e Von Neumann?
Na arquitetura Harvard tem-se a presença de barramentos separados para dados e endereços, podendo ter tamanhos diversificados. Esse tipo de arquitetura é mais elaborada e apresenta maior complexidade com memórias separadas.
Já, na arquitetura Von Neumann, tem-se um único barramento para dados e endereços, o que possibilita juntamente o acesso a memória de programas e dados juntamente. Pelo fato de ser utilizado apenas um único barramento, a vatagem é que esse tipo de arquitetura tem uma menor complexidade de projeto de microprocessador.

5. Considere a variável inteira i, armazenando o valor 0x8051ABCD. Se i é armazenada na memória a partir do endereço 0x0200, como fica este byte e os seguintes, considerando que a memória é:
(a) Little-endian:

***********************************************************

0x0200 - CD
0x0201 - AB
0x0202 - 51
0x0203 - 80

***********************************************************

(b) Big-endian:

***********************************************************

0x0200 - 80
0x0201 - 51
0x0202 - AB
0x0203 - CD

***********************************************************

6. Sabendo que o processador do MSP430 tem registradores de 16 bits, como ele soma duas variáveis de 32 bits?
O MSP pode fazer essa operação fazendo uso dos registradores de "carry", para transferir o excesso da soma para outro registrador. E pode utilizar o registrador de "overflow" para indicar se houve "overflow" na execução da operação de soma.
