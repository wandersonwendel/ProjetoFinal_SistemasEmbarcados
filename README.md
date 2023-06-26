<h1 align ="center"> Projeto Final de Sistemas Embarcados</h1>

## Introdução
O objetivo deste projeto foi desenvolver um medidor de nível de água utilizando o simulador de Arduino, Wokwi, a fim de monitorar e exibir o nível de água em um recipiente por meio de sensores conectados ao Arduino. O projeto foi realizado por dois estudantes da UFC - Campus de Itapajé, Wanderson Wendel e Samuel Pinheiro, como parte de suas atividades acadêmicas na disciplina de Sistemas Embarcados, ministrada pelo professor Juan Sebastian.

## Funcionalidades

- Mede o nível de água em tempo real.
- Exibe o nível de água em uma interface gráfica.
- Permite ajustar os limites de nível de água desejados.
- Alerta visual quando o nível de água atinge um valor crítico.

## Componentes Utilizados

- Arduino Uno (ou outra placa compatível);
- Sensore de nível de água;
- Potenciômetro;
- Display LCD para exibição dos resultados; e
- Bibliotecas do Arduino para comunicação e controle.

## Instruções de Uso

1. Clone ou faça o download do repositório;
2. Abra o arquivo do projeto no Arduino IDE, ou algum simulador(usamos o Wokwi como exemplo);
3. Conecte os sensores de nível de água à placa Arduino, de acorco com a sessão "Passo a Passo";
4. Faça as devidas configurações dos limites de nível de água no código;
5. Carregue o programa no Arduino, ou simulador; e
6. Execute a simulação.

## Passo a Passo - Código do Projeto

Passo a passo do código que ilustra como ler os valores dos sensores de nível de água e atualizar a exibição no display LCD:

1 - Essas linhas incluem as bibliotecas necessárias para acessar a memória EEPROM (para salvar o valor do nível de água) e para controlar um display de LCD.
````C++
#include <EEPROM.h>
#include <LiquidCrystal.h>
````
<br/>
<br/>

2 - Cria um objeto da classe LiquidCrystal para controlar o display de LCD. Os números (2,3,4,5,6,7) indicam as conexões do display com os pinos do Arduino.
````C++
LiquidCrystal lcd(2,3,4,5,6,7);
````
<br/>
<br/>

3 - Declaração das variáveis utilizadas no código. duration e inches são usadas para armazenar a duração do pulso e a medida em polegadas do nível de água. set_val guarda o valor de referência do nível de água definido pelo usuário. percentage armazena o percentual do nível de água em relação ao valor de referência. state é uma variável de controle e pump indica o estado da bomba de água.
````C++
long duration, inches;
int set_val,percentage;
bool state,pump;
````
<br/>
<br/>

4 - A função setup() é executada uma vez no início do programa. Nela, a configuração inicial é feita. O display de LCD é iniciado com 16 colunas e 2 linhas. Em seguida, algumas mensagens são impressas no display para indicar o estado inicial do sistema. Os pinos 8, 9, 10, 11 e 12 são configurados para diferentes funções: 8 e 12 como saídas e os demais como entradas. O valor de referência do nível de água é lido da memória EEPROM. Se o valor lido for maior que 150, é definido o valor máximo de 150.
````C++
void setup() {
  
  lcd.begin(16, 2);
  lcd.print("WATER LEVEL:");
  lcd.setCursor(0, 1); 
  lcd.print("PUMP:OFF MANUAL");
  
  pinMode(8, OUTPUT);
  pinMode(9, INPUT);
  pinMode(10, INPUT_PULLUP);
  pinMode(11, INPUT_PULLUP);
  pinMode(12, OUTPUT);
  
   set_val=EEPROM.read(0);
   if(set_val>150)set_val=150;
}
````
<br/>
<br/>

5 - A função loop() é executada continuamente após a função setup(). Nela, o código principal é executado em um loop infinito.

Primeiro, um pulso é enviado a um sensor ultrassônico conectado ao pino 9 para medir a distância até a superfície da água. A duração desse pulso é convertida em uma medida em polegadas usando a função microsecondsToInches().

Com base no valor de referência do nível de água (set_val) e na medida atual (inches), é calculado o percentual do nível de água.

O percentual é então exibido no display de LCD. Se o percentual for inferior a 0, é ajustado para 0.

Dependendo do percentual do nível de água e do estado do botão conectado ao pino 11, a variável pump é definida como 1 (liga) ou 0 (desliga). O estado da bomba é controlado pelo pino 12.

A segunda linha do display indica se a bomba está ligada ou desligada (ON ou OFF) e se o sistema está no modo manual ou automático.

Se o botão conectado ao pino 10 for pressionado e solto, e o sistema estiver no modo automático (digitalRead(11) == HIGH), o estado do sistema muda para configurar um novo valor de referência do nível de água (set_val). Esse valor é salvo na memória EEPROM.

Se o botão conectado ao pino 10 for pressionado e solto, e o sistema estiver no modo manual (digitalRead(11) == LOW), o estado do sistema muda para alternar o estado da bomba (pump).

Finalmente, um pequeno atraso de 500 milissegundos é inserido antes de repetir o loop. Isso permite uma taxa de atualização adequada para exibição e controle.
````C++
void loop() {
  
   digitalWrite(3, LOW);
   delayMicroseconds(2);
   digitalWrite(8, HIGH);
   delayMicroseconds(10);
   digitalWrite(8, LOW);
   duration = pulseIn(9, HIGH);
   inches = microsecondsToInches(duration);
   
   percentage=(set_val-inches)*100/set_val;
   
   lcd.setCursor(12, 0); 
   if(percentage<0)percentage=0;
   lcd.print(percentage);
   lcd.print("%   ");
    
   if(percentage<30&digitalRead(11))pump=1;
   if(percentage>99)pump=0;
   digitalWrite(12,!pump);
     
   lcd.setCursor(5, 1);
   if(pump==1)lcd.print("ON ");
   else if(pump==0) lcd.print("OFF");
   
    lcd.setCursor(9, 1);
    if(!digitalRead(11))lcd.print("MANUAL");
    else lcd.print("AUTO   ");
    
    if(!digitalRead(10)&!state&digitalRead(11)){
      state=1;
      set_val=inches;
      EEPROM.write(0, set_val);
      }
      
     if(!digitalRead(10)&!state&!digitalRead(11)){
        state=1;
        pump=!pump;
     
      }
      
    if(digitalRead(10))state=0;
      
    
    delay(500);
}
long microsecondsToInches(long microseconds) {
   return microseconds / 74 / 2;
}
````
