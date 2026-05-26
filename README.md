# 🚀 SPACE CONNECT — Missão DevOps

> Pipeline CI/CD automatizado para sistemas de comunicação e monitoramento espacial.

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-3.0.0-black?logo=flask)
![Docker](https://img.shields.io/badge/Docker-containerizado-2496ED?logo=docker)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?logo=jenkins)
![GitHub](https://img.shields.io/badge/GitHub-repositório-181717?logo=github)

---

## 📡 Sobre o Projeto

O **SPACE CONNECT** é uma aplicação de monitoramento desenvolvida para simular a comunicação entre sistemas terrestres e satélites. O projeto foi estruturado com uma esteira CI/CD completa utilizando Jenkins, Docker e GitHub, garantindo automação, rastreabilidade e confiabilidade nas entregas.

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Função |
|---|---|---|
| Python | 3.11 | Linguagem da aplicação |
| Flask | 3.0.0 | Framework web |
| Docker | latest | Containerização |
| Jenkins | LTS | Automação CI/CD |
| GitHub | — | Versionamento |

---

## 📁 Estrutura do Repositório

```
space-connect-app/
├── app.py              # Aplicação Flask
├── requirements.txt    # Dependências Python
├── Dockerfile          # Configuração do container
├── Jenkinsfile         # Pipeline CI/CD
├── README.md           # Documentação
└── evidencias/         # Prints das etapas
```

---

## ▶️ Como Rodar Localmente

**Pré-requisitos:** Docker instalado

```bash
# 1. Clone o repositório
git clone https://github.com/Lamataa/space-connect-app.git
cd space-connect-app

# 2. Build da imagem
docker build -t space-connect-app:1.0 .

# 3. Rodar o container
docker run -d -p 5000:5000 --name space-connect space-connect-app:1.0

# 4. Testar
curl http://localhost:5000/
curl http://localhost:5000/health
```

---

## 🌐 Endpoints da API

| Método | Endpoint | Descrição | Resposta |
|---|---|---|---|
| GET | `/` | Status da missão | `mission`, `version`, `status`, `timestamp` |
| GET | `/health` | Health check | `status`, `mission`, `version` |

**Exemplo de resposta — `GET /`:**
```json
{
  "mission": "SPACE CONNECT",
  "version": "1.0",
  "status": "ONLINE",
  "timestamp": "2026-05-26T00:54:22.491529"
}
```

**Exemplo de resposta — `GET /health`:**
```json
{
  "mission": "SPACE CONNECT",
  "status": "healthy",
  "version": "1.0"
}
```

---

## 🔄 Pipeline CI/CD — Jenkins

O pipeline é composto por 3 estágios declarativos:

```
Build ──► Test ──► Deploy Simulado
```

| Stage | O que faz |
|---|---|
| **Build** | Clona o repositório e constrói a imagem Docker |
| **Test** | Sobe container de teste e valida os endpoints com `curl` |
| **Deploy Simulado** | Para o container anterior e sobe a nova versão na porta 5000 |

### Variáveis de Ambiente

| Variável | Valor | Descrição |
|---|---|---|
| `APP_NAME` | `space-connect-app` | Nome da imagem Docker |
| `APP_PORT` | `5000` | Porta exposta da aplicação |
| `CONTAINER_NAME` | `space-connect-running` | Nome do container em produção |

---

## 🐳 Dockerfile

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## 🔧 Troubleshooting

### Erro: `port is already allocated`

**Causa:** Outro container já está usando a porta 5000.

**Como identificar:**
```bash
docker ps
```

**Como resolver:**
```bash
docker stop space-connect
docker rm space-connect
```

**Como evitar:** O Jenkinsfile já executa `docker stop || true` e `docker rm || true` antes de cada deploy, garantindo que o container anterior seja removido automaticamente.

---

## 🔗 Links

- 📦 Repositório: [github.com/Lamataa/space-connect-app](https://github.com/Lamataa/space-connect-app)