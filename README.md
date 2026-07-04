# n8n Bot Clima

Bot de Telegram para consulta de previsão do tempo em tempo real, implementado como workflow no [n8n](https://n8n.io) com integração à [OpenWeather API](https://openweathermap.org/api).

## Sobre o projeto

Automação low-code que recebe o nome de uma cidade via Telegram, consulta dados meteorológicos e devolve uma resposta formatada em português. O fluxo inclui normalização de entrada, validação defensiva da resposta da API e mensagens de erro claras para o usuário.

## Funcionalidades

- Recebimento de mensagens via Telegram Bot API
- Normalização de texto (trim, lowercase, remoção de acentos)
- Consulta de clima em tempo real (OpenWeatherMap)
- Validação de status HTTP e integridade dos dados de temperatura
- Tratamento estruturado de erros (cidade inválida, dados incompletos)
- Resposta formatada com temperatura atual, mínima, máxima e condição

## Stack

| Tecnologia | Uso |
|------------|-----|
| n8n | Orquestração do workflow |
| Telegram Bot API | Interface com o usuário |
| OpenWeatherMap API | Dados meteorológicos |

## Arquitetura

```
Telegram Trigger
      ↓
Normalização da entrada
      ↓
HTTP Request → OpenWeatherMap
      ↓
Validação (cod == 200)
      ↓
Validação (temperatura)
      ↓
Resposta formatada ou mensagem de erro
```

## Pré-requisitos

- Instância n8n (local ou cloud)
- Conta no [Telegram BotFather](https://t.me/BotFather) com token de bot
- Chave de API da [OpenWeatherMap](https://openweathermap.org/api)

## Configuração

1. Importe o arquivo `workflow-telegram-chatbot.json` no n8n
2. Configure as credenciais do Telegram no nó **Receber Mensagem**
3. Substitua a API key no nó **HTTP OpenWeatherMap** ou use a variável de ambiente:

```bash
OPENWEATHER_API_KEY=sua_chave_aqui
```

4. Ative o workflow
5. Envie o nome de uma cidade ao bot no Telegram

## Exemplo de uso

**Entrada**

```
São Paulo
```

**Saída**

```
A previsão em São Paulo: 21°C (mín 20°C, máx 22°C). algumas nuvens.
```

## Detalhes técnicos

### Normalização da entrada

```javascript
($json.message.text || "")
  .trim()
  .toLowerCase()
  .normalize("NFD")
  .replace(/[\u0300-\u036f]/g, "")
  .replace(/\s+/g, " ")
```

### Endpoint OpenWeatherMap

```
GET https://api.openweathermap.org/data/2.5/weather
```

| Parâmetro | Valor |
|-----------|-------|
| `q` | Nome da cidade normalizado |
| `units` | `metric` |
| `lang` | `pt_br` |
| `appid` | Chave da API |

### Validações

- Resposta da API com `cod === 200`
- Presença de `main.temp`, `main.temp_min` e `main.temp_max`

## Tratamento de erros

| Cenário | Comportamento |
|---------|---------------|
| Cidade não encontrada | Mensagem informando falha na consulta |
| Dados de temperatura ausentes | Mensagem de erro específica |

## Licença

Projeto open source para fins educacionais e demonstração de integração de APIs com n8n.
