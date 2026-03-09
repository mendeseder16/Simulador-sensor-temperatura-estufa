# Simulador-sensor-temperatura-estufa

Esse projeto de simulação de uma estufa de hortaliças no TinkerCad é uma ótima maneira de integrar sensores e atuadores com um Arduino. Para implementar a funcionalidade de IoT, você pode seguir as etapas abaixo:

# Componentes Necessários
1. Arduino Uno
2. Sensor de Temperatura (DHT11 ou LM35)
3. Buzina (ou Buzzer)
4. LED
5. Motor (Motor DC ou Servo Motor)
6. Resistores (para o LED)**
7. Fios de Conexão
8. Fonte de Alimentação (se necessário)

# Esquema de Conexão
1. Sensor de Temperatura
   - Conecte o pino de saída do sensor ao pino digital no Arduino (por exemplo, pino 2).
   - Conecte o VCC e GND do sensor ao VCC e GND do Arduino.

2. LED
   - Conecte o anodo do LED a um pino digital (por exemplo, pino 3) e o catodo a um resistor, que deve ir ao GND.

3. Buzina
   - Conecte a buzina a um pino digital (por exemplo, pino 4) e o outro ao GND.

4. Motor
   - Se for um motor DC, você pode precisar de um módulo controlador de motor. Para um servo, ligue o sinal a um pino digital (por exemplo, pino 5).

# Código Arduino
Aqui está um exemplo básico de como o código pode ser estruturado:

```cpp
#include <DHT.h>

#define DHTPIN 2            // Pino onde o sensor está conectado
#define DHTTYPE DHT11       // Defina o tipo do seu sensor (DHT11 ou DHT22)
#define BUZZER_PIN 4
#define LED_PIN 3
#define MOTOR_PIN 5

DHT dht(DHTPIN, DHTTYPE);

void setup() {
  pinMode(LED_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(MOTOR_PIN, OUTPUT);
  dht.begin();
  Serial.begin(9600);
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();

  if (isnan(h) || isnan(t)) {
    Serial.println("Falha na leitura do sensor!");
    return;
  }

  Serial.print("Umidade: "); Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperatura: "); Serial.print(t);
  Serial.println(" *C");
  
  // Condições para ativar os dispositivos
  if (t > 30) { // Se a temperatura estiver acima de 30ºC
    digitalWrite(LED_PIN, HIGH);
    digitalWrite(BUZZER_PIN, HIGH);
    // Ligar motor para aumentar ventilação
    digitalWrite(MOTOR_PIN, HIGH);
  } else {
    digitalWrite(LED_PIN, LOW);
    digitalWrite(BUZZER_PIN, LOW);
    digitalWrite(MOTOR_PIN, LOW);
  }

  delay(2000); // Aguarde um pouco antes da próxima leitura
}
```

# Funcionalidade de IoT
Para integrar com IoT, você poderia usar um módulo Wi-Fi (como o ESP8266) para enviar dados para um servidor ou aplicativo. Assim, você pode monitorar a temperatura e umidade da sua estufa em tempo real.

# Considerações Finais
1. Testes: Depois de construir o circuito e programar o Arduino, faça vários testes para garantir que tudo funcione corretamente.
2. Expansão: Considere práticas de segurança e automação adicional, como controle de irrigação, por exemplo.

Esse projeto não só é educativo, mas também ajuda na compreensão prática de sensores e atuadores em um ambiente de IoT.
