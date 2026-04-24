# Sistema de Alarme IoT com ESP32

## Descrição

Este projeto implementa um sistema de alarme utilizando ESP32 com integração via MQTT. O sistema é capaz de detectar movimento, acionar dispositivos físicos (buzzer, LED e servo motor) e enviar notificações em tempo real para um broker MQTT.

---

## Funcionalidades

* Conexão Wi-Fi com reconexão automática
* Comunicação MQTT (publicação e assinatura)
* Detecção de movimento com sensor PIR
* Alarme sonoro intermitente com buzzer
* Indicação visual com LED
* Controle manual por botão
* Controle remoto via MQTT
* Acionamento de servo motor

---

## Componentes Utilizados

* ESP32
* Sensor PIR
* Servo motor
* Buzzer
* Botão
* LED

---

## Pinagem

| Componente | Pino |
| ---------- | ---- |
| Servo      | 26   |
| Buzzer     | 25   |
| Botão      | 13   |
| LED        | 14   |
| Sensor PIR | 27   |

---

## Configuração

### Wi-Fi

```cpp
const char* ssid = "SEU_WIFI";
const char* password = "SUA_SENHA";
```

### MQTT

```cpp
const char* mqtt_server = "test.mosquitto.org";
const int mqtt_port = 1883;
```

---

## Tópicos MQTT

### Publicação

* `AlarmeIoT` → alerta de invasão
* `estadoAlarme` → estado do sistema

### Assinatura

* `testeIoT` → controle remoto do sistema

---

## Funcionamento Geral

O sistema dispara o alarme quando:

* O sensor PIR detecta movimento
* O sistema está ativado manualmente
* O sistema está ativado via MQTT

---

## Função callback()

Responsável por receber mensagens MQTT e controlar o sistema remotamente.

### Funcionamento

1. Converte a mensagem recebida (array de bytes) em string
2. Exibe a mensagem no monitor serial
3. Verifica o tópico recebido
4. Interpreta os comandos

### Comandos suportados

| Mensagem | Ação               |
| -------- | ------------------ |
| ON   | Ativa o sistema    |
| OFF      | Desativa o sistema |

---

## Função alarme()

Responsável pelo controle do buzzer de forma intermitente.

### Funcionamento

* Utiliza a função millis() para alternar o estado do buzzer
* Evita o uso de delay(), permitindo execução não bloqueante

### Lógica

* A cada 250 ms o estado do buzzer é invertido
* O PWM é aplicado para gerar o som intermitente

---

## Conectividade

### Wi-Fi

* Tentativa de reconexão a cada 2 segundos

### MQTT

* Reconexão automática ao broker
* Comunicação contínua com client.loop()

---

## Controle de Tempo

O sistema utiliza millis() para:

* Debounce do botão
* Controle do buzzer
* Intervalo de envio MQTT
* Tentativas de reconexão

Isso permite execução não bloqueante e melhor desempenho.

---

## Como Usar

1. Instalar as bibliotecas:

   * WiFi.h
   * PubSubClient.h
   * ESP32Servo.h

2. Configurar credenciais Wi-Fi e MQTT

3. Fazer upload do código para o ESP32

4. Utilizar um cliente MQTT para:

   * Monitorar mensagens
   * Enviar comandos (ARMADO ou OFF)

---

## Possíveis Melhorias

* Implementar autenticação no MQTT
* Criar interface de controle (aplicativo ou web)
* Integrar com dashboards (Node-RED, ThingsBoard)
* Adicionar notificações remotas
* Integrar com câmera

---

## Estrutura do Código

* setup() → inicialização
* loop() → lógica principal
* callback() → comunicação MQTT
* alarme() → controle do buzzer

---

## Observações

* O sistema depende de conexão com a internet para funcionamento MQTT
* Recomenda-se o uso de um broker seguro
* O sensor PIR deve estar corretamente calibrado

---

## Autor

Projeto desenvolvido para fins de estudo em IoT com ESP32 e MQTT.
