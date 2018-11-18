<b>1. O que são sistemas embarcados?</b>
São sistemas microprocessados em que o computador é completamente encapsulado ou dedicado ao dispositivo ou sistema que ele controla. São sistemas "invisíveis" a quem olha o todo.
Em suma, uma combinação de hardware e software e eventualmente outras partes elétricas ou mecânicas contendo processamento de informações embarcadas nos produtos.

<b>2. O que são sistemas microprocesssados?</b>
São sistemas que possuem uma CPU, memória RAN e RON além de outros periféricos, ou seja, são sistemas controlados por microcontroladores. 

<b>3. Apresente aplicações de sistemas embarcados.</b>
<b>(a) para a indústria automotiva:</b>
Sensor que identifica se há alguém sentado no banco e se esta pessoa esta fazendo uso do cinto de segurança. Caso não, emite-se um sinal sonoro;
Sensor para saber se o motorista está dormindo;
Sensor que regula altura do som e dos bancos;
Computador de Bordo.

<b>(b) para eletrodomésticos:</b>
Programar um horário para que a cafeteira prepare o café;
Geladeira com tela de programação;
Smart TV's;
Maquina de lavar roupa;

<b>(c) para automação industrial:</b>
Robótica Industrial;
CNC (Comando Numérico Computadorizado);
Acionamentos Elétricos (Soft-Start, Inversor de Frequência e Servoacionamentos);
Braço Mecânico.

<b>4. Cite arquiteturas possíveis e as diferenças entre elas.</b>
Arquiteturas para memória
>Harvard Architecture (PICs, Intel 8051, ARM9): possui circuitos separadamente para sinais e armazenamento de dados e instruções independentes em termos de barramento, acesso a memória de dados separados em relação a memoria do programa.
>Von Neumann Architecture: possui separação no barramento de dados da memoria de instrução de dados o que permite que um processador possa acessa-las simultaneamente obtendo melhor desempenho.

<b>5. Por que usamos o MSP430 na disciplina, ao invés de outro microcontrolador?</b>
Porque esse microprocessador possui arquitetura von Neumann (mais simples quando comparada com outras), aplicações de baixo consumo de energia (facilmente colocado em modo de baixo consumo, através de bits no registrador de estado),
possui processador, barramentos de dados e de memória e registradores de 16 bits, também permite aritmética diretamente com valores da memória, várias opções de CLOCKS, vários periféricos sem a necessidade de CPU para funcionarem,	
além de possuir uma CPU pequena e muito eficiente, pois possui muitos registradores, etc.
Ou seja, a MSP430 ultrapassa os outros microcontroladores pela soma de variadas funções com o custo da mesma, suprindo bem a necessidade da disciplina e, em suma, por ser um componente ideal para a execução da disciplica de Microcontroladores
e Microprocessadores com capacidades técnicas para o desenvolvimento de aplicações compatíveis com as metas da matéria e não se assemelha ao arduino por não ser direcionado a prototipação e hobbies.

 