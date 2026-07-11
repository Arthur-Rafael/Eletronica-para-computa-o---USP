# Semáforo Inteligente com Arduino e Sensor Ultrassônico

Este projeto é a simulação de um semáforo que altera suas luzes (Verde, Amarelo e Vermelho) com base na distância detectada por um sensor ultrassônico HC-SR04.

## 🛠️ Link para o Projeto
Você pode visualizar e testar a simulação interativa diretamente no Tinkercad pelo link abaixo:
(https://www.tinkercad.com/things/ecrbJNapSmC-semaforo/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard)

## 🔌 Componentes Utilizados
* 1x Arduino Uno R3
* 1x Sensor Ultrassônico HC-SR04
* 3x LEDs (Vermelho, Amarelo e Verde)
* 3x Resistores
* Protoboard e Jumpers

##  Vídeo explicativo do projeto
            
https://github.com/user-attachments/assets/c43e8320-d0ec-4950-aea0-06bcf43d3724

## Código do projeto (utilizamos a linguagem C)
```
// ==========================================
// DEFINIÇÃO DOS PINOS
// ==========================================

// Pinos dos LEDs do Semáforo
const int pinoVerde = 2;
const int pinoAmarelo = 3;
const int pinoVermelho = 4;

// Pinos do Sensor Ultrassônico
const int pinoTrig = 9;
const int pinoEcho = 10;

// Configuração da distância em centímetros
const int distanciaAtivacao = 15;

// ==========================================
// CONFIGURAÇÃO INICIAL (Roda apenas uma vez)
// ==========================================
void setup() {
  // Configura os pinos dos LEDs para enviar energia (saída)
  pinMode(pinoVerde, OUTPUT);
  pinMode(pinoAmarelo, OUTPUT);
  pinMode(pinoVermelho, OUTPUT);
 
  // Configura os pinos do Sensor (Trig envia som, Echo escuta)
  pinMode(pinoTrig, OUTPUT);
  pinMode(pinoEcho, INPUT);
 
  // Define o estado inicial da rua: Sinal Aberto (Verde)
  digitalWrite(pinoVerde, HIGH);
  digitalWrite(pinoAmarelo, LOW);
  digitalWrite(pinoVermelho, LOW);
}

// ==========================================
// LOOP PRINCIPAL (Roda repetidamente)
// ==========================================
void loop() {
  long duracao;
  int distancia;

  // 1. Manda o sensor emitir um pulso sonoro
  digitalWrite(pinoTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinoTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinoTrig, LOW);

  // 2. Calcula o tempo que o som demorou para ir e voltar
  duracao = pulseIn(pinoEcho, HIGH);
 
  // 3. Converte esse tempo em distância (centímetros)
  distancia = duracao * 0.034 / 2;

  // 4. Lógica de decisão do semáforo
  // Se detectar algo entre 1cm e 15cm, inicia a mudança de sinal
  if (distancia > 0 && distancia <= distanciaAtivacao) {
   
    // Sinal Amarelo (Atenção)
    digitalWrite(pinoVermelho, LOW);
    digitalWrite(pinoVerde, LOW);
    digitalWrite(pinoAmarelo, HIGH);
    delay(3500); // Fica 3,5 segundos no amarelo

    // Sinal verde (siga)
    digitalWrite(pinoVermelho, LOW);
    digitalWrite(pinoAmarelo, LOW);
    digitalWrite(pinoVerde, HIGH);
    delay(8000); // Fica 8 segundos aberto para os carros passarem

    // Volta para o Sinal Vermelho (Pare)
    digitalWrite(pinoVermelho, HIGH);
    digitalWrite(pinoAmarelo, LOW);
    digitalWrite(pinoVerde, LOW);
   
    // Pausa de 3 segundos antes de permitir que o sensor feche o sinal de novo
    delay(5000);
  }

  // Pequena pausa para o sensor não bugar com leituras rápidas demais
  delay(100);
}
```

