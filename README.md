<h1 align ="center"> Projeto Final de Sistemas Embarcados</h1>

# Medidor de Nível de Água com Arduino na Plataforma Wokwi

Este projeto é um medidor de nível de água feito com Arduino, utilizando a plataforma online do Wokwi para simulação e teste. O objetivo do projeto é monitorar e exibir o nível de água em um recipiente por meio de sensores conectados ao Arduino.

## Funcionalidades

- Mede o nível de água em tempo real.
- Exibe o nível de água em uma interface gráfica.
- Permite ajustar os limites de nível de água desejados.
- Alerta visual quando o nível de água atinge um valor crítico.

## Recursos Utilizados

- Arduino Uno (ou outra placa compatível).
- Sensores de nível de água.
- Display LCD para exibição dos resultados.
- Bibliotecas do Arduino para comunicação e controle.

## Instruções de Uso

1. Clone ou faça o download do repositório.
2. Abra o arquivo do projeto no Arduino IDE.
3. Conecte os sensores de nível de água à placa Arduino.
4. Faça as devidas configurações dos limites de nível de água no código.
5. Carregue o programa no Arduino.
6. Execute a simulação na plataforma Wokwi.

## Exemplo de Código

Aqui está um trecho de código que ilustra como ler os valores dos sensores de nível de água e atualizar a exibição no display LCD:

```C
#include <LiquidCrystal.h>

// Definição dos pinos para o display LCD
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

// Pinos dos sensores de nível de água
const int sensorPin1 = A0;
const int sensorPin2 = A1;

void setup() {
  lcd.begin(16, 2);
}

void loop() {
  // Leitura dos valores dos sensores de nível de água
  int sensorValue1 = analogRead(sensorPin1);
  int sensorValue2 = analogRead(sensorPin2);

  // Cálculos e lógica para determinar o nível de água

  // Atualização do display LCD com o valor do nível de água
  lcd.clear();
  lcd.print("Nivel de Agua:");
  lcd.setCursor(0, 1);
  lcd.print("Valor: " + String(nivelAgua));

  // Lógica para alerta visual em caso de nível crítico

  // Aguarda um intervalo de tempo antes da próxima leitura
  delay(1000);
}
```
