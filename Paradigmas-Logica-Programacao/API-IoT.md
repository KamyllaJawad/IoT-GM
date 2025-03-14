# **Criando uma API para Enviar Dados do Arduino para o Google Sheets** 📊🚀  

Vamos criar uma API que permitirá ao **Arduino (simulado no Tinkercad)** enviar dados de sensores diretamente para o **Google Sheets**. Dessa forma, podemos armazenar informações como **temperatura, umidade, luminosidade, etc.** para análise.  

---

## **🛠 O Que Precisamos?**  
✅ **Conta Google** (para acessar o Google Sheets e Google Apps Script)  
✅ **Google Sheets** (Planilha onde os dados serão armazenados)  
✅ **Google Apps Script** (Para criar a API)  
✅ **Arduino (no Tinkercad)** (Para enviar os dados para a API)  

---

# **📌 Passo 1: Criar a Planilha no Google Sheets**  

1️⃣ Acesse [Google Sheets](https://docs.google.com/spreadsheets/).  
2️⃣ Crie uma nova planilha e renomeie-a (exemplo: **"Dados IoT"**).  
3️⃣ Na **célula A1**, escreva `Temperatura`.  
4️⃣ Na **célula B1**, escreva `Data/Hora`.  

A tabela ficará assim:  

| Temperatura (°C) | Data/Hora          |  
|-----------------|-------------------|  
| 25.3           | 05/03/2025 14:00  |  
| 27.1           | 05/03/2025 14:05  |  

---

# **📌 Passo 2: Criar a API no Google Apps Script**  

Agora, vamos criar um **script que receberá os dados enviados pelo Arduino e os salvará na planilha**.  

1️⃣ No **Google Sheets**, clique em `Extensões` → `Apps Script`.  
2️⃣ Apague qualquer código existente e cole o seguinte código:  

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  
  var temperatura = e.parameter.temperatura;
  var dataHora = new Date();
  
  sheet.appendRow([temperatura, dataHora]);
  
  return ContentService.createTextOutput("Dado recebido com sucesso!");
}
```

**Explicação do código:**  
✅ O script recebe um **parâmetro chamado `temperatura`** na URL da requisição.  
✅ Adiciona o **valor recebido** e a **data/hora atual** na próxima linha da planilha.  
✅ Retorna uma mensagem de sucesso.  

---

# **📌 Passo 3: Implantar a API**  

Agora, precisamos disponibilizar essa função na internet para que o **Arduino possa enviar dados para ela**.  

1️⃣ Clique em **"Implantar"** → **"Nova Implantação"**  
2️⃣ Em **Tipo de implantação**, selecione `Aplicativo da Web`.  
3️⃣ Em **Acessível por**, escolha `Qualquer pessoa`.  
4️⃣ Clique em **Implantar**.  
5️⃣ O Google pedirá **permissões**. Clique em `Avançado` → `Ir para seu projeto` → `Permitir`.  
6️⃣ Copie a **URL do aplicativo da web** que será gerada (exemplo):  

```
https://script.google.com/macros/s/SEU-LINK-UNICO/exec
```

---

# **📌 Passo 4: Testar a API Manualmente**  

Antes de programar o Arduino, vamos testar a API manualmente.  

1️⃣ Abra o navegador e digite a URL da API **adicionando um parâmetro** na frente, assim:  

```
https://script.google.com/macros/s/SEU-LINK-UNICO/exec?temperatura=28.5
```

2️⃣ Pressione **Enter**. Você verá a mensagem:  

```
Dado recebido com sucesso!
```

3️⃣ Vá para o Google Sheets e veja se o valor **28.5** foi salvo! 🎉  

---

# **📌 Passo 5: Programar o Arduino para Enviar Dados**  

Agora, vamos fazer o **Arduino enviar automaticamente os dados do sensor para a API**.  

### **1️⃣ Monte o Circuito no Tinkercad**  
- **Arduino Uno**  
- **Sensor DHT11 (Temperatura e Umidade)**  
- Conecte o **VCC do DHT11 → 5V do Arduino**  
- Conecte o **GND do DHT11 → GND do Arduino**  
- Conecte o **Pino de dados do DHT11 → Pino 2 do Arduino**  

### **2️⃣ Código para o Arduino (C++)**  

```cpp
#include <DHT.h>
#include <WiFi.h>
#include <HTTPClient.h>

#define DHTPIN 2
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

const char* ssid = "NOME_DA_SUA_WIFI";  // Substitua pelo nome da sua rede WiFi
const char* password = "SENHA_DA_SUA_WIFI";  // Substitua pela senha da sua rede WiFi
const char* serverName = "https://script.google.com/macros/s/SEU-LINK-UNICO/exec";

void setup() {
    Serial.begin(115200);
    dht.begin();
    
    WiFi.begin(ssid, password);
    Serial.print("Conectando ao WiFi...");
    
    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.print(".");
    }
    Serial.println("\nWiFi Conectado!");
}

void loop() {
    if (WiFi.status() == WL_CONNECTED) {
        HTTPClient http;
        
        float temperatura = dht.readTemperature();
        String url = String(serverName) + "?temperatura=" + String(temperatura);
        
        http.begin(url);
        int httpResponseCode = http.GET();
        
        if (httpResponseCode > 0) {
            Serial.print("Resposta da API: ");
            Serial.println(http.getString());
        } else {
            Serial.print("Erro na requisição: ");
            Serial.println(httpResponseCode);
        }
        
        http.end();
    }
    
    delay(5000);  // Envia os dados a cada 5 segundos
}
```

---

# **📌 Explicação do Código Arduino**  

✅ **Conecta ao WiFi** utilizando as credenciais configuradas.  
✅ **Lê a temperatura** do sensor DHT11.  
✅ **Envia os dados** para a API do Google Sheets.  
✅ **Recebe uma resposta** da API confirmando que os dados foram salvos.  

