# 🌦️ Bot de Clima no Telegram com n8n

Automação para consulta de clima em tempo real via Telegram, utilizando n8n e a API da OpenWeather.

---

## 🚀 Funcionalidades

* Recebe mensagens via Telegram
* Normaliza entrada (acentos, espaços, case)
* Consulta clima em tempo real
* Valida resposta da API
* Trata erros de forma resiliente
* Retorna mensagem formatada ao usuário

---

## 🧠 Arquitetura

```
Telegram Trigger
   ↓
Normalização
   ↓
HTTP Request (OpenWeather)
   ↓
Validação (cod == 200)
   ↓
Validação (temperatura)
   ↓
Resposta ou Erro
```

---

## 🔧 Principais Etapas

### 🔤 Normalização da Entrada

```javascript
($json.message.text || "")
  .trim()
  .toLowerCase()
  .normalize("NFD")
  .replace(/[\u0300-\u036f]/g, "")
  .replace(/\s+/g, " ")
```

---

### 🌐 Requisição OpenWeather

**Endpoint**

```
https://api.openweathermap.org/data/2.5/weather
```

**Parâmetros**

* `q`: {{$json.queue}}
* `units`: metric
* `lang`: pt_br
* `appid`: {{$env.OPENWEATHER_API_KEY}}

---

### ✅ Validação de Sucesso

```javascript
Number($json.cod) === 200
```

---

### 🌡️ Validação de Dados

* `main.temp`
* `main.temp_min`
* `main.temp_max`

---

### 💬 Mensagem de Resposta

```javascript
A previsão em *{{ $json.name }}*: 
*{{ Math.round($json.main.temp) }}°C* 
(mín *{{ Math.round($json.main.temp_min) }}°C*, máx *{{ Math.round($json.main.temp_max) }}°C*). 
*{{ $json.weather[0].description }}*.
```

---

## ❌ Tratamento de Erros

* Cidade não encontrada (API)
* Dados incompletos (temperatura)

---

## 🔐 Variáveis de Ambiente

```
OPENWEATHER_API_KEY=your_api_key_here
```

---

## 📦 Como usar

1. Importar o workflow no n8n
2. Configurar credenciais do Telegram
3. Definir a variável de ambiente
4. Ativar o fluxo
5. Enviar uma cidade no Telegram

---

## 🧪 Exemplo

**Entrada**

```
São Paulo
```

**Saída**

```
A previsão em São Paulo: 21°C (mín 20°C, máx 22°C). algumas nuvens.
```

---

## 🏗️ Boas Práticas

* Separação de responsabilidades
* Normalização de entrada
* Validação defensiva
* Uso de variáveis de ambiente
* Tratamento de erro estruturado

---

## 👨‍💻 Autor

Projeto de automação com foco em integração de APIs e workflows no n8n.
