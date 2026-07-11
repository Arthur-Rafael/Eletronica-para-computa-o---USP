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
# Componentes utilizados e suas funções no projeto
---

## Placa Arduino Uno R3
A placa Arduino Uno R3 é o "cérebro" ou o microcontrolador do projeto. Ele processa as informações e toma as decisões.

* **Função no projeto:** Ele roda o código escrito. O Arduino lê os dados de distância enviados pelo sensor ultrassônico, calcula se há um objeto perto ou longe e envia ordens para ligar ou desligar os LEDs corretos nos pinos digitais `3`, `4` e `5`.

---

## Sensor Ultrassônico (HC-SR04)
Mede a distância entre ele e um objeto usando ondas sonoras de alta frequência.

* **Função no projeto:** Ele atua como o "olho" do semáforo. O pino `TRIG` (Gatilho) dispara o som, o som bate em um obstáculo (como um carro se aproximando) e volta. O pino `ECHO` (Eco) recebe esse som de volta. O Arduino mede esse tempo e calcula a distância para decidir se o semáforo deve mudar de cor.

---

## LEDs (Diodos Emissores de Luz)
São componentes semicondutores que emitem luz visível quando uma corrente elétrica passa por eles. Eles possuem polaridade, ou seja, a corrente só flui do terminal positivo (Anodo) para o negativo (Catodo).

* **Função no projeto:** São os indicadores visuais do trânsito (Vermelho, Amarelo e Verde). Eles mudam de estado de acordo com a distância calculada pelo sensor para simular o controle de tráfego de uma rua.

---

## Resistores
Como o próprio nome diz, eles oferecem "resistência" à passagem da corrente elétrica, limitando a quantidade de energia que passa pelo circuito.

* **Função no projeto:** Proteção. Os pinos do Arduino fornecem uma corrente elétrica que pode ser forte demais para os LEDs. Os resistores ficam no caminho da energia para diminuir essa corrente, impedindo que os seus LEDs queimem ou que a placa do Arduino seja danificada por sobrecarga.

---

## Protoboard (Placa de Ensaio)
É uma placa cheia de furos com conexões de metal internas que permitem conectar componentes eletrônicos uns aos outros sem a necessidade de solda.

* **Função no projeto:** Serve como a base física para montar o seu circuito. Ela conecta as trilhas de energia positivo (`5V`) e negativo (`GND`) do Arduino para o sensor e os resistores de forma organizada.

---

## Jumpers (Fios de Conexão)
São fios condutores elétricos encapados com plástico de proteção nas pontas.

* **Função no projeto:** São as "veias" do circuito. Eles transportam os sinais elétricos e de dados de um ponto a outro — como levar o sinal de comando do pino digital do Arduino até o LED correspondente na protoboard.

## Membros do grupo

* Rayan Fernandes Macarele Da Silva<br>
* Arthur Rafael de Souza Jesus<br>
* Yann Rodrigues dos Santos


