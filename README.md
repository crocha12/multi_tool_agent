# Multi Tool Agent

Agente de exemplo construído com o [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/) e o Gemini. Ele responde perguntas sobre **hora** e **clima** de uma cidade usando duas ferramentas (tools) em Python.

## Como funciona

O agente `weather_time_agent` está definido em [agent.py](agent.py) e usa o modelo `gemini-flash-latest`. Ele decide sozinho qual ferramenta chamar:

| Ferramenta | Descrição |
| --- | --- |
| `get_weather(city)` | Retorna o clima atual da cidade informada. |
| `get_current_time(city)` | Retorna a data e a hora atuais no fuso horário da cidade. |

> As ferramentas são um exemplo simplificado: por enquanto só respondem para **New York**. Para qualquer outra cidade retornam `status: "error"`.

## Pré-requisitos

- Python 3.10 ou superior
- Uma chave de API do Gemini, que pode ser criada no [Google AI Studio](https://aistudio.google.com/apikey)

## Instalação

```bash
git clone https://github.com/crocha12/multi_tool_agent.git
cd multi_tool_agent

python -m venv .venv
source .venv/bin/activate

pip install google-adk
```

## Configuração

Copie o arquivo de exemplo e coloque a sua chave:

```bash
cp .env.example .env
```

```env
GOOGLE_API_KEY="SUA_CHAVE_DO_GEMINI"
```

O `.env` está no `.gitignore` e não deve ser commitado.

### Usando o Vertex AI (opcional)

Em vez da API key, é possível autenticar pelo Google Cloud:

```bash
gcloud auth application-default login
gcloud config set project SEU_PROJECT_ID
```

No `.env`, use:

```env
GOOGLE_GENAI_USE_VERTEXAI=TRUE
GOOGLE_CLOUD_PROJECT="SEU_PROJECT_ID"
GOOGLE_CLOUD_LOCATION="us-central1"
```

## Executando

O ADK espera que o agente esteja em uma pasta com `__init__.py` e `agent.py`, então rode os comandos a partir da pasta **pai** do projeto:

```bash
cd ..

# Interface web de desenvolvimento
adk web

# Ou direto no terminal
adk run multi_tool_agent
```

Se você clonou o repositório com outro nome de pasta, use esse nome no `adk run`.

Exemplos de perguntas:

- `What is the weather in New York?`
- `What time is it in New York?`
- `What is the weather in Paris?` (retorna erro, pois a cidade não é suportada)

## Estrutura

```
multi_tool_agent/
├── __init__.py      # expõe o módulo agent para o ADK
├── agent.py         # ferramentas e definição do root_agent
├── .env.example     # modelo de configuração
└── .gitignore
```

## Licença

O código-base segue o cabeçalho Apache License 2.0 presente em [agent.py](agent.py).
