### **Atividade: Simulando um Sensor de Temperatura IoT no Tinkercad**

#### **Objetivo:**
Criar um circuito no Tinkercad que simula um sensor de temperatura (usando um potenciômetro) e envia os dados para o monitor serial, simulando um envio de dados para a nuvem.

---

### **Materiais Necessários:**
1. Arduino Uno
2. Potenciômetro (para simular a leitura de temperatura)
3. Fios de conexão

---

### **Passo a Passo:**

#### **1. Monte o Circuito:**
1. Conecte o **pino 5V** do Arduino ao **pino VCC** do potenciômetro.
2. Conecte o **pino GND** do Arduino ao **pino GND** do potenciômetro.
3. Conecte o **pino central (OUT)** do potenciômetro ao **pino analógico A0** do Arduino.

#### **2. Código Explicativo:**
Aqui está o código que simula a leitura de temperatura e envia os dados para o monitor serial:

```cpp
// Definição do pino onde o potenciômetro está conectado
const int pinoSensor = A0;

void setup() {
  // Inicia a comunicação serial com taxa de 9600 bps
  Serial.begin(9600);
  Serial.println("Iniciando leitura do sensor de temperatura simulado...");
}

void loop() {
  // Lê o valor do potenciômetro (0 a 1023)
  int leituraSensor = analogRead(pinoSensor);

  // Converte o valor lido para uma escala de temperatura simulada (0°C a 100°C)
  float temperatura = map(leituraSensor, 0, 1023, 0, 100);

  // Exibe a temperatura no monitor serial
  Serial.print("Temperatura Simulada: ");
  Serial.print(temperatura);
  Serial.println(" °C");

  // Simula o envio dos dados para a nuvem (IoT)
  Serial.println("Enviando dados para a nuvem...");

  // Aguarda 2 segundos antes da próxima leitura
  delay(2000);
}
```

---

#### **3. Explicação do Código:**
1. **`const int pinoSensor = A0;`**  
   Define o pino analógico A0 como o pino de leitura do potenciômetro.

2. **`Serial.begin(9600);`**  
   Inicia a comunicação serial para exibir os dados no monitor serial.

3. **`int leituraSensor = analogRead(pinoSensor);`**  
   Lê o valor do potenciômetro, que varia de 0 a 1023.

4. **`float temperatura = map(leituraSensor, 0, 1023, 0, 100);`**  
   Converte o valor lido (0 a 1023) para uma escala de temperatura simulada (0°C a 100°C).

5. **`Serial.print()` e `Serial.println()`**  
   Exibem os dados no monitor serial.

6. **`delay(2000);`**  
   Aguarda 2 segundos antes de realizar a próxima leitura.

---

#### **4. Teste o Circuito:**
1. No Tinkercad, clique em "Iniciar Simulação".
2. Abra o monitor serial (no canto inferior direito) para ver os dados sendo exibidos.
3. Gire o potenciômetro e observe a mudança na temperatura simulada.

---

#### **5. Simulação de IoT:**
- Explique aos alunos que, em um sistema IoT real, os dados seriam enviados para a nuvem usando módulos como Wi-Fi (ESP8266/ESP32) ou GSM.
- No Tinkercad, o monitor serial simula o envio de dados para a nuvem.

---

### **Resultado Esperado:**
- No monitor serial, os alunos verão mensagens como:
  ```
  Temperatura Simulada: 25.00 °C
  Enviando dados para a nuvem...
  Temperatura Simulada: 50.00 °C
  Enviando dados para a nuvem...
  ```
- Ao girar o potenciômetro, a temperatura simulada mudará.

---

### **Discussão:**
1. Como o potenciômetro simula a leitura de um sensor de temperatura?
2. O que seria necessário para enviar esses dados para a nuvem em um projeto real?
3. Como poderíamos melhorar a precisão da leitura?
