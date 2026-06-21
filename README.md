# Cronômetro Digital TTL/CMOS

Projeto de um cronômetro digital utilizando circuitos TTL/CMOS, displays de 7 segmentos e alimentação diretamente da rede elétrica da CEMIG.

## Licença

Este projeto é licenciado sob a CERN-OHL-S v2.

## Recursos do projeto

- Contagem de minutos e segundos
- Clock derivado da rede elétrica de 60 Hz
- Displays de 7 segmentos
- Circuitos TTL/CMOS
- Projeto desenvolvido no KiCad
## 
1.	OBJETIVO
   
Projetar e implementar um cronômetro digital capaz de contar os segundos e minutos. O dispositivo deverá ser alimentado exclusivamente a partir da rede elétrica da CEMIG. O cronômetro deverá possuir dois botões, sendo um para iniciar e parar a contagem do tempo (start/stop) e outro para zerar a contagem (reset). Os segundos e minutos serão mostrados em quatro displays de sete segmentos, sendo dois para os segundos (dezena e unidade) e dois para os minutos (dezena e unidade).

## 
2.	DESENVOLVIMENTO
   
O sistema foi dividido em três subsistemas distintos e interligados, sendo eles : 

•	Circuito para a alimentação e geração de clock;

•	Circuito lógico de contagem;

•	Circuito para mostrar a contagem.

A seguir será apresentado o desenvolvimento de cada um destes subsistemas. Todos os diagramas foram montados no software KiCAD.

2.1.	Circuito para a Alimentação e Geração de Clock

De acordo com o meu objetivo ao realizar esse projeto, o cronômetro digital deverá ser alimentado exclusivamente a partir da rede elétrica da CEMIG, a qual possui uma tensão senoidal com tensão eficaz de 127 V e 60 Hz. Considerando que os circuitos integrados a serem utilizados serão do tipo TTL ou CMOS, alimentados em 5 V, tensão contínua, será necessário converter a tensão alternada de 127 VAC em 5 VCC. Por isso, utilizei um retificador de onda completa com um regulador linear de tensão.
O circuito lógico de contagem requer um sinal de clock com frequência fixa e níveis adequados de tensão para uma contagem precisa. Sei que a rede elétrica da CEMIG provê uma senoide com frequência de 60 Hz. Portanto, optei por condicionar a senoide fornecida pela CEMIG para gerar um clock com os mesmos 60 Hz, o qual será posteriormente utilizado no circuito lógico de contagem.
A Figura 1 apresenta o circuito desenvolvido para a alimentação e geração de clock.

<img width="886" height="456" alt="image" src="https://github.com/user-attachments/assets/3845c028-412a-41dc-b940-a2e88f050af0" />

Figura 1 – Circuito para alimentação e geração de clock.

Para um adequado funcionamento do circuito de geração do clock e circuito de alimentação, será utilizado um transformador de três enrolamentos de 127 V/7,5 V/7,5 V. Entretanto, considerando que não foi encontrado este tipo de transformador no comércio local, na montagem, foram utilizados dois transformadores distintos, um de 1 A e outro de 0,5 A.

Os componentes para o circuito de geração de clock foram escolhidos conforme explicado a seguir:

Tensão de saída do enrolamento do transformador de 7,5 V, 500 mA.

D9: diodo zenner. Foi escolhido um diodo zenner de 5,1 Volts quando é reversamente polarizado. Quando diretamente polarizado sua tensão é de 1,5 V. Vou considerar que ali vai passar metade da corrente máxima(200mA) suportada por ele, ou seja, projetei o resistor R35 para 100mA.

R35: utilizado para limitar a corrente do diodo zenner. Considerando a corrente máxima no zenner em função de sua potência máxima (de acordo com o datasheet) e a tensão de pico da senoide na saída do transformador (pior caso da tensão), quando o zenner estiver diretamente polarizado, tem-se:
R35=(10,6-1,5)/0,1
R35=91 Ω

Para este valor de resistência, haverá uma potência dissipada de:
P=(7,5-1,5)*0,1
P=0,6 W
Comercialmente foi escolhido um resistor de 100 Ω e 1 W.

D12: diodo de germânio. Escolhi um diodo de germânio pois, quando diretamente polarizado, possui uma tensão direta de  0,3 V, e este valor está dentro da faixa de valores permitidos pelo Schimdt trigger. A corrente máxima de diodos comuns como o 1N34A tem como corrente máxima 50 mA, valor que eu utilizei para calcular o valor da resistência R36.

R36: utilizado para limitar a corrente no diodo de germânio. 

R36=(1,5-0,3)/0,05
R36=24 Ω
Para este valor de resistência, haverá uma potência dissipada de:
P=(1,5-0,3)*0,05
P=0,06 W
Comercialmente foi escolhido um resistor de 24 Ω e 1/4 W.

CI U44: Schimdt trigger. Utilizado para o condicionamento final da senoide ceifada. Escolhi um Schimdt trigger alimentado com 5 Volts e cuja tensão de entrada varie de -0,5 até Vcc + 0,5. Segundo o datasheet, ele funciona, para 4,5 V de alimentação, com uma mudança para nível logico alto de 3,15 V e com a mudança para nível lógico baixo de 2,3 V. Escolhi o 74HC14A

Dessa forma, após o condicionamento da senoide entregue pela CEMIG, teremos um CLK1 de 60 Hz, com as tensões adequadas para os níveis lógicos 1 e 0, com relação ao referencial GND.

Os componentes para o circuito de alimentação foram escolhidos conforme explicado a seguir:

Tensão de saída do enrolamento do transformador de 7,5 V (valor eficaz), corrente de 1 A.

D7,D8, D10 e D11: Ponte retificadora em onda completa. Foram definidos os diodos 1N4007, amplamente utilizados neste tipo de circuito. Estes diodos suportam, facilmente, a corrente máxima do regulador de tensão. Foi considerada uma corrente de carga máxima de 1 A.
  
U43: Regulador linear de tensão. Foi escolhido o LM7805, o qual possui uma corrente máxima de 1 A e tensão de saída de 5 V.
  
C4: capacitor de saída da ponte retificadora completa. Utilizado para limitar a tensão de ripple da fonte. O LM7805 requer uma tensão mínima de 7 V para que eu tenha 5V na saída, logo, o meu ripple tem que ser de (10,6-7) = 3,6 V. Assim, minha capacitância é de: 

  C=I/(Vripple*f)
  
  C=1/(3,6*120)

  C=2,31 mF

Esse capacitor deverá ser eletrolítico e sua tensão máxima será de (10,6-1,4) =  9,2 V. 
Considerando valores comerciais, foi escolhido um capacitor eletrolítico de 2200 microF, 25 V.

C5: capacitor de disco de cerâmica de 0,22 microF, indicado no datasheet do LM7805.
  C6: capacitor de disco de cerâmica de 0,1 microF, indicado no datasheet do LM7805.

Desta forma, na saída do circuito de alimentação teremos uma tensão contínua de 5 V, com relação ao referencial GND.

Optou-se por implementar o circuito para alimentação e geração do clock em uma placa de circuito impressa. A Figura 2 apresenta o diagrama de roteamento da placa desenvolvida, vista pelo lado dos componentes. Ressalta-se que as ilhas dos conectores de entrada (transformador) e saída (GND, + 5 V e CLK1) não condizem com a realidade. Da mesma forma, a largura das trilhas também não condiz com a realidade implementada. A numeração dos componentes não possui relação direta com a numeração apresentada na Figura 1.

<img width="886" height="474" alt="image" src="https://github.com/user-attachments/assets/dd7933d5-7cad-49c3-96cf-050a522cd2f1" />

Figura 2: Diagrama de roteamento da placa do circuito para alimentação e geração do clock.

2.2.	Circuito Lógico de Contagem
O circuito lógico de contagem é o circuito responsável pela lógica de contagem do cronômetro. Esse circuito receberá o sinal de CLK1 e a tensão de 5 VDC da placa de alimentação e geração clock.
A Figura 3 apresenta o circuito lógico de contagem.

<img width="947" height="554" alt="image" src="https://github.com/user-attachments/assets/7ce67e79-d3a0-4dc9-b480-abb69d9e565c" />

Figura 3: Diagrama do circuito lógico de contagem.

Conforme dito, o circuito lógico de contagem recebe o sinal de clock CLK1, que possui frequência de 60 Hz. Considerando que a menor medida de tempo a ser contada são os segundos, será necessário dividir essa frequência por 60. Para isso, foi projetado um contador síncrono, módulo-60, que conte de 0 a 59, ou seja, ao chegar em 60, ou 111100 em binário, ele resete. Dessa forma, na saída desse divisor teremos um clock de 1 Hz. Esse circuito possui, ainda, as chaves de start/stop, sendo ela uma chave de duas posições e a chave de reset, do tipo push bottom.

A saída do divisor módulo-60 (com frequencia de 1 Hz) alimentará um circuito com contadores de década utilizado para a contagem dos segundos e minutos.

Os componentes para o circuito de lógica de contagem foram escolhidos conforme explicado a seguir:

  •	CIs U27, U28 e U28. Foram utilizados seis flip-flops do tipo JK, configurados como flip-flop tipo T, para a implementação do divisor módulo-60. Foi escolhido o CI 4027.

  •	CI U38: para a implementação do divisor módulo-60 foi necessário utilizar uma AND de quatro entradas, utilizada para zerar os flip-flops tipo T quando a contagem chega em 111100. Foi escolhido o CI 7421.
  
  •	SW4: chave de duas posições, on/off. Utilizada para implementar o sistema de start/stop do cronômetro. Essa chave habilita ou desabilita a saída do clock do divisor módulo-60 para o circuito contador dos segundos e minutos.
  
  •	CI U35A: porta AND de duas entradas, utilizado para implementar a lógica de habilitação/desabilitação do sinal de clock de saída do divisor módulo-60.
  
  •	CIs U29, U32, U39 e U40: Decidi utilizar contadores de década (contagem BCD) para a a contagem dos segundos e minutos de forma assíncrona. 
     
Os CIs U29 e U32 são utilizados para a contagem dos segundos (contagem de 0 a 59), utilizando o sinal de clock de saída do divisor de módulo-60. O CI U29 é utilizado para a contagem das unidades de segundo e o CI U32 para a contagem das dezenas de segundos, utilizando a saída do CI U29 como sinal de clock, após a divisão por 10. Os dois contadores são zerados quando U32 atinge o número 6, 0110 em binário, o que equivale a chegar ao número 60 nos dois contadores (conta de 0 a 59 segundos).
 
  Os CIs U39 e U40 são utilizados para a contagem dos minutos (contagem de 0 a 59), utilizando como clock a saída do CI U32. Os dois contadores são zerados quando U40 atinge o número 6 (0110 em binário), o que equivale a chegar ao número 60 nos dois contadores (conta de 0 a 59 minutos).

    
  •	SW3: chave NA do tipo push bottom. Utilizada para implementar o sistema de reset do cronômetro. Ao ser pressionada os contadores BCD (CIs U29, U32, U39 e U40, responsáveis por contar os segundos e minutos) são resetados.

  •	CI U36: porta OR de duas entradas. Utilizado para implementar a lógica do botão de reset.
  
  •	R33 e R34: resistores utilizados em série com as chaves SW3 e SW4 para a implementação das lógicas de start/stop e reset. Foram utilizados resistores de 10 kΩ, valor suficiente para limitar a corrente quando as chaves forem acionadas sem prejudicar o valor dos níveis digitais aplicados às entradas das portas lógicas.
  
  •	J1, J2, J3 e J4: conectores de 4 pinos para disponibilizar os quatro bits da saída dos contadores de década (contagem BCD).

A saída do circuito lógico de contagem são quatro conjuntos de quatro bits (contagem BCD), responsáveis pela contagem das unidades de segundos, dezenas de segundos, unidades de minutos e dezenas de minutos. Esses sinais são disponibilizados nos conectores J1 a J4.

  Optou-se por implementar o circuito lógico de contagem em um protoboard, devido a complexidade deste circuito.

2.3.	Circuito para Mostrar a Contagem

O circuito para mostrar a contagem é o circuito responsável por mostrar, ao usuário, a contagem do cronômetro.  Esse circuito receberá a tensão de 5 VDC da placa de alimentação e geração clock e os quatro conjuntos de quatro bits (contagem BCD) do circuito lógico de contagem.

  O valor da contagem dos segundos e minutos será apresentado em displays de sete segmentos, totalizando quatro display, dois para os segundos (unidades e dezenas) e dois para os minutos (unidades e dezenas). Para a implementação foram utilizados conversores BCD para 7 segmentos.
  
A Figura 4 apresenta o diagrama do circuito para mostrar o valor da contagem.

<img width="886" height="371" alt="image" src="https://github.com/user-attachments/assets/254d944d-ff5b-408f-9859-dcc0790a543f" />

Figura 4: Diagrama do circuito para mostrar a contagem.

Os componentes do circuito para mostrar a contagem foram escolhidos conforme explicado a seguir:

J5, J6, J7 e J8: conectores de 4 pinos para receber os quatros bits da contagem da unidade dos segundos, dezena dos segundos, unidade dos minutos e dezena dos minutos.
  
U30, U33, U41 e U45: conversores de BCD para 7 segmentos. Foram utilizados o CI 4511.
  
R37 a R64 (28 resistores): resistores utilizados para limitar a corrente fornecida pelos conversores de BCD para 7 segmentos, considerando a corrente necessária para o adequado funcionamento dos displays de 7 segmentos.
  
Para os leds dos displays acenderem em intensidade média, o datasheet e indica que é necessário 10 mA, logo, usarei 12mA. O datasheet informa, também, que a tensão direta dos leds é de 2 V. Considerando a saída em 5 V do conversor BCD para 7 segmentos, temos:

R=(5-2)/0,012

R=250 Ω

Para esse valor de resistência, a potência será de:

P=3*0,012

P=0,036 W

Será utilizado o valor comercial de R = 250 Ω e ¼ W.

U31, U34, U37 e U47: displays de 7 segmentos de alto brilho, na cor vermelha. Foram utilizados displays de catodo comum, tendo sido utilizado o modelo 5161AS, disponível no comércio local.

SW5: chave NA do tipo push bottom utilizada para implementar o teste dos displays.

R65: resistor utilizado em série com a chave SW5 para a implementação das lógicas teste dos displays. Foi utilizado um resistor de 1,0 kΩ., valor suficiente para limitar a corrente quando a chave for acionada sem prejudicar o valor dos níveis digitais aplicados às entradas dos conversores BCD para 7 segmentos.

Optou-se por implementar o circuito para mostrar a contagem em uma placa de circuito impressa. A Figura 5 apresenta o diagrama de roteamento da placa desenvolvida, vista pelo lado dos componentes. Ressalta-se que a largura das trilhas não condiz com a realidade implementada. A numeração dos componentes não possui relação direta com a numeração apresentada na Figura 4.

<img width="886" height="512" alt="image" src="https://github.com/user-attachments/assets/8fba03ea-8f64-4488-857a-fdc14d026750" />

Figura 5 - Diagrama de roteamento da placa do circuito para mostrar a contagem.

## 
3.	CONCLUSÃO

Neste trabalho foi projetado um cronômetro digital capaz de marcar os segundos e minutos a partir do comando do usuário, dados por chaves. O cronômetro digital é alimentado a partir da rede elétrica da CEMIG, sendo que o sinal de clock é gerado a partir do condicionamento da senoide de tensão da alimentação, o que garante estabilidade na contagem.

O valor da contagem é apresentado em quatro displays de sete segmentos na cor vermelha e alto brilho.

O projeto foi segmentado em três subsistemas, o primeiro dedicado à geração de tensão contínua de 5 V, com relação ao GND, a ser utilizada em todo o projeto e à geração do sinal de clock de 60 Hz a partir da própria senoide da tensão de alimentação. O segundo dedicado à contagem propriamente dita do cronômetro. O terceiro dedicado a apresentar o valor da contagem ao usuário.

O primeiro e terceiro subsistemas foram implementados em placas de circuito impresso. O segundo sistema foi implementado em um protoboard.

Infelizmente, devido a complexidade do projeto pretendido e à falta de tempos disponível, não foi possível concluir satisfatoriamente o segundo sistema, não sendo possível, portanto, testar todo o sistema proposto. Entende-se, entretanto, que após a finalização do segundo sistema o projeto proposto funcionará a contento.

