🌦️ Automação n8n: Consulta de Clima via Telegram
Este projeto é uma automação desenvolvida no n8n que permite consultar a previsão do tempo de qualquer cidade do mundo através de um bot no Telegram. O workflow integra as APIs do Telegram e do OpenWeatherMap.

🚀 Como Funciona

Gatilho de Entrada: A automação inicia ao receber qualquer mensagem no Telegram.
Pergunta Interativa: O bot responde perguntando: "Olá, qual cidade gostaria de saber a previsão do tempo?" e aguarda a resposta do usuário (função sendAndWait).
Consulta Meteorológica: O nome da cidade enviado é processado pela API do OpenWeatherMap em português (ptbr).

Fluxo de Resposta:
✅ Sucesso: Se a cidade for encontrada, o bot retorna uma mensagem formatada com a temperatura atual, mínima e máxima (formatadas para o padrão brasileiro).
❌ Falha: Caso a cidade não exista ou a API retorne erro, o bot informa: "A cidade [nome] não existe" e encerra a execução.

🛠️ Configuração Necessária
Para rodar este workflow, você precisará configurar as seguintes credenciais:
Telegram Bot API: Crie um bot via @BotFather para obter seu Token.
OpenWeatherMap API Key: Gere sua chave gratuita no portal oficial do OpenWeatherMap.
