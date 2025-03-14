# **Linguagens de Programação para IoT**  
## **Introdução ao C++ para Dispositivos Embarcados**  

Se você nunca programou antes, não se preocupe! Vamos aprender juntos como dar os primeiros passos na programação para **dispositivos IoT**, usando **C++** e simulando tudo no **Tinkercad**.  

---

# 📌 **O que é uma Linguagem de Programação?**  

Uma **linguagem de programação** é um conjunto de regras que permite dar instruções a um computador ou microcontrolador. No caso da **IoT**, usamos essas instruções para que dispositivos interajam com o mundo real, como:  

✅ Ligar e desligar LEDs 🚦  
✅ Ler sensores de temperatura 🌡  
✅ Controlar motores ⚙  
✅ Enviar dados para a internet 🌐  

No nosso curso, vamos usar a linguagem **C++**, que é muito eficiente para **dispositivos embarcados**, como **Arduino e ESP32**.  

---

# 🔹 **Primeiros Passos no C++ para IoT**  

Antes de programar no Tinkercad, precisamos entender alguns conceitos básicos do C++.  

### 🖥 **1. Criando Nosso Primeiro Código**  

Todo programa em C++ para IoT tem **duas partes principais**:  

1️⃣ `setup()`: Executa apenas uma vez, no início do programa.  
2️⃣ `loop()`: Roda continuamente, repetindo as ações.  

Exemplo de um código simples que **liga e desliga um LED**:  

```cpp
void setup() {
    pinMode(13, OUTPUT); // Define o pino 13 como saída
}

void loop() {
    digitalWrite(13, HIGH); // Liga o LED
    delay(1000); // Espera 1 segundo
    digitalWrite(13, LOW); // Desliga o LED
    delay(1000); // Espera 1 segundo
}
```

---

# 🛠 **Passo a Passo no Tinkercad**  

Agora, vamos simular esse código no **Tinkercad**!  

### 1️⃣ **Acesse o site do Tinkercad:**  
🔗 [https://www.tinkercad.com/](https://www.tinkercad.com/)  

### 2️⃣ **Crie um novo circuito:**  
- Clique em **"Criar Novo Circuito"**  
- No menu à direita, pesquise por **"Arduino Uno"** e arraste para a área de trabalho  

### 3️⃣ **Adicione um LED e um resistor:**  
- No menu à direita, pesquise **"LED"** e arraste para a tela  
- Pegue um **resistor** (de 220Ω) e conecte ao **pino 13 do Arduino**  

### 4️⃣ **Conecte o LED ao resistor:**  
- O **lado longo do LED** vai no resistor  
- O **lado curto do LED** vai no **GND do Arduino**  

### 5️⃣ **Clique em "Iniciar Simulação"** e veja o LED piscando!  

---

# 🔹 **Variáveis e Operadores em C++**  

**Variáveis** armazenam informações, como **números** e **textos**.  

```cpp
int temperatura = 25; // Guarda um valor numérico
char letra = 'A'; // Guarda um caractere
```

**Operadores** permitem fazer cálculos e comparações.  

```cpp
int resultado = 10 + 5;  // Soma
bool maior = (10 > 5);   // Comparação (verdadeiro)
```

---

# 🔹 **Estruturas Condicionais (if-else)**  

Os sistemas IoT tomam decisões usando `if-else`.  

📌 **Exemplo: Se a temperatura for maior que 30°C, liga um ventilador**.  

```cpp
int temperatura = 32;

void setup() {
    pinMode(9, OUTPUT); // Pino do ventilador
}

void loop() {
    if (temperatura > 30) {
        digitalWrite(9, HIGH); // Liga o ventilador
    } else {
        digitalWrite(9, LOW); // Desliga o ventilador
    }
}
```

---

# 🔹 **Estruturas de Repetição (for, while)**  

Os **loops** permitem repetir ações sem precisar escrever o mesmo código várias vezes.  

📌 **Exemplo: Piscar um LED 5 vezes usando um loop `for`**  

```cpp
void setup() {
    pinMode(13, OUTPUT);
}

void loop() {
    for (int i = 0; i < 5; i++) {
        digitalWrite(13, HIGH);
        delay(500);
        digitalWrite(13, LOW);
        delay(500);
    }
}
```

---

# 🔹 **Funções em C++**  

Funções organizam o código e evitam repetições.  

📌 **Exemplo: Criando uma função para piscar um LED**  

```cpp
void piscarLED(int pino, int tempo) {
    digitalWrite(pino, HIGH);
    delay(tempo);
    digitalWrite(pino, LOW);
    delay(tempo);
}

void setup() {
    pinMode(13, OUTPUT);
}

void loop() {
    piscarLED(13, 1000); // Chama a função
}
```

---

# **Técnicas de Programação e APIs**  

### ✅ **Técnicas de Programação**  

Algumas boas práticas para programar IoT:  
✅ **Organizar o código com funções**  
✅ **Usar variáveis para evitar números fixos no código**  
✅ **Testar cada parte separadamente**  

### 🔌 **O que são APIs?**  

Uma **API (Interface de Programação de Aplicações)** permite que dispositivos IoT troquem informações com outros sistemas.  

📌 **Exemplo:** Um sensor pode enviar dados para a **API do Google Sheets**, registrando medições automaticamente.  

---

# **Bibliotecas para Sensores e Comunicação**  

### 📚 **O que são Bibliotecas?**  
Bibliotecas são **códigos prontos** que facilitam o uso de sensores e comunicação.  

📌 **Exemplo:** Em vez de escrever um código complexo para um sensor de temperatura, podemos usar a biblioteca `DHT.h`.  

---

# 🔹 **Passo a Passo: Lendo um Sensor de Temperatura no Tinkercad**  

Agora, vamos aprender como **ler a temperatura** no Tinkercad usando um **sensor DHT11**.  

### 1️⃣ **Monte o circuito:**  
- No **Tinkercad**, adicione:  
  ✅ **Arduino Uno**  
  ✅ **Sensor DHT11**  
  ✅ **Resistor de 10KΩ** (entre VCC e o pino de dados do sensor)  

### 2️⃣ **Conecte os pinos:**  
- **VCC do sensor → 5V do Arduino**  
- **GND do sensor → GND do Arduino**  
- **Pino de dados do sensor → Pino 2 do Arduino**  

### 3️⃣ **Código para ler a temperatura**  

```cpp
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

void setup() {
    Serial.begin(9600);
    dht.begin();
}

void loop() {
    float temperatura = dht.readTemperature();
    Serial.print("Temperatura: ");
    Serial.print(temperatura);
    Serial.println("°C");
    delay(2000);
}
```

