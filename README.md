#Teste de conhecimentos do módulo de Nuvem com Microsoft Azure, da trilha .NET, neste desafio, desenvolver um sistema de RH. 
# 🏢 Sistema de RH — Trilha .NET + Microsoft Azure
[![.NET](https://img.shields.io/badge/.NET-8.0-purple)](https://dotnet.microsoft.com/)
[![Azure](https://img.shields.io/badge/Azure-App%20Service-blue)](https://azure.microsoft.com/)
[![DIO](https://img.shields.io/badge/DIO-Desafio%20de%20Projeto-orange)](https://www.dio.me/)

> Desafio de projeto da Trilha .NET da DIO — módulo de Nuvem com Microsoft Azure.
> Sistema de cadastro de funcionários com CRUD completo, banco de dados SQL e log de alterações no Azure Table Storage.

---

## 📋 Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [Arquitetura](#arquitetura)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Pré-requisitos](#pré-requisitos)
- [Configuração do Ambiente Azure](#configuração-do-ambiente-azure)
- [Configuração do Projeto](#configuração-do-projeto)
- [Deploy no Azure](#deploy-no-azure)
- [Endpoints da API](#endpoints-da-api)
- [Testando a API](#testando-a-api)
- [Logs no Azure Table Storage](#logs-no-azure-table-storage)
- [Prints do Projeto](#prints-do-projeto)

---

## 📖 Sobre o Projeto

Sistema de RH desenvolvido como Web API em .NET, permitindo o cadastro completo de funcionários com as seguintes funcionalidades:

- CRUD completo de funcionários
- Armazenamento em Azure SQL Database
- Log automático de todas as alterações no Azure Table Storage
- Documentação automática via Swagger
- Deploy na nuvem com Azure App Service

---

## 🏗️ Arquitetura

```
┌─────────────────┐         ┌──────────────────────┐
│   Cliente/      │         │   Azure App Service   │
│   Swagger UI    │────────▶│   (rhdiolab)          │
└─────────────────┘         │   Web API .NET        │
                            └──────────┬────────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │                  │                   │
                    ▼                  ▼                   │
        ┌───────────────────┐  ┌───────────────────┐      │
        │  Azure SQL        │  │  Azure Table      │      │
        │  Database         │  │  Storage          │      │
        │  (RhDioLab)       │  │  (FuncionarioLog) │      │
        │                   │  │                   │      │
        │  1. Salva dados   │  │  2. Salva logs    │      │
        └───────────────────┘  └───────────────────┘      │
```

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Uso |
|-----------|--------|-----|
| .NET | 8.0 | Framework principal |
| ASP.NET Core Web API | 8.0 | API REST |
| Entity Framework Core | 6.x | ORM / Migrations |
| Azure App Service | - | Hospedagem da API |
| Azure SQL Database | - | Banco de dados relacional |
| Azure Table Storage | - | Armazenamento de logs (NoSQL) |
| Swagger / OpenAPI | 3.0 | Documentação da API |
| Azure CLI | - | Deploy via linha de comando |

---

## ✅ Pré-requisitos

- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- Conta no [Microsoft Azure](https://azure.microsoft.com/)
- [Git](https://git-scm.com/)

---

## ☁️ Configuração do Ambiente Azure

### Recursos criados no Azure

| Nome | Tipo | Região |
|------|------|--------|
| `ASP-desafiorh-887e` | Plano do Serviço de Aplicativo (F1) | West US 2 |
| `desafio-rh-azure` | SQL Server | West US 2 |
| `logrhdiolab` | Conta de Armazenamento | West US 2 |
| `RhDioLab` | Banco de Dados SQL | West US 2 |
| `rhdiolab` | Serviço de Aplicativo | West US 2 |

### Grupo de Recursos

```
Nome: desafiorh
Região: West US 2
```

### Print — Grupo de Recursos no Portal Azure

![Texto Alternativo](/imagens/versao_final/Print_Meu_Dashboard_Microsoft_Azure_Chrome_2026-06-14_220234.png)

---

## ⚙️ Configuração do Projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/seu-usuario/trilha-net-azure-desafio.git
cd trilha-net-azure-desafio
```

### 2. Configurar as Connection Strings

Edite o arquivo `appsettings.json` com suas credenciais:

```json
{
  "ConnectionStrings": {
    "ConexaoPadrao": "Server=tcp:desafio-rh-azure.database.windows.net,1433;Initial Catalog=RhDioLab;User ID=SEU_USUARIO;Password=SUA_SENHA;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    "SAConnectionString": "DefaultEndpointsProtocol=https;AccountName=logrhdiolab;AccountKey=SUA_CHAVE;EndpointSuffix=core.windows.net",
    "AzureTableName": "FuncionarioLog"
  }
}
```

> ⚠️ **Nunca suba credenciais reais para o GitHub!**
> Em produção, use as variáveis de ambiente do App Service.

### 3. Onde encontrar as credenciais no Azure

**ConexaoPadrao** (SQL):
```
Portal Azure → RhDioLab (Banco de dados SQL) → Cadeias de conexão → ADO.NET
```

**SAConnectionString** (Storage):
```
Portal Azure → logrhdiolab (Conta de armazenamento) → Segurança + rede → Chaves de acesso → key1
```

### 4. Rodar a Migration

```bash
dotnet ef database update \
  --connection "Server=tcp:desafio-rh-azure.database.windows.net,1433;Initial Catalog=RhDioLab;User ID=SEU_USUARIO;Password=SUA_SENHA;Encrypt=True;"
```

---

## 🚀 Deploy no Azure

### 1. Login no Azure CLI

```bash
az login
```

![Texto Alternativo](/imagens/versao_final/Print_pendrive_ms2h_02_study_TECH_Cursos_DIO_git_copilot_2026-06-14_193345.png)

### 2. Compilar para produção

```bash
dotnet publish -c Release -o ./publish
```

### 3. Zipar os arquivos

```bash
cd publish && zip -r ../app.zip .
```

![Texto Alternativo](/imagens/versao_final/Print_pendrive_ms2h_02_study_TECH_Cursos_DIO_git_copilot_2026-06-14_193823.png)

### 4. Fazer o deploy

```bash
az webapp deployment source config-zip \
  --resource-group desafiorh \
  --name rhdiolab \
  --src ../app.zip
```

### 5. Resultado esperado

```
Status: Building the app...        ⏳
Status: Build successful.          ✅
Status: Site started successfully. ✅
```
![Texto Alternativo](/imagens/versao_final/Print_pendrive_ms2h_02_study_TECH_Cursos_DIO_git_copilot_2026-06-14_193809.png)

### 6. Configurar Connection Strings no App Service

```
Portal Azure
  → rhdiolab (Serviço de Aplicativo)
    → Configurações
      → Variáveis de ambiente
        → Cadeias de conexão
          → + Adicionar
```

| Nome | Tipo |
|------|------|
| `ConexaoPadrao` | SQLAzure |
| `SAConnectionString` | Custom |
| `AzureTableName` | Custom |

### Print — Connection Strings configuradas no App Service

![Texto Alternativo](/imagens/versao_final/Print_rhdiolab_Microsoft_Azure_Chrome_2026-06-14_195401.png)

---

## 📡 Endpoints da API

**URL Base:**
```
https://rhdiolab-f9byeseedwh7ckaq.westus2-01.azurewebsites.net
```

| Verbo | Endpoint | Descrição |
|-------|----------|-----------|
| `GET` | `/Funcionario/{id}` | Buscar funcionário por ID |
| `POST` | `/Funcionario` | Criar novo funcionário |
| `PUT` | `/Funcionario/{id}` | Atualizar funcionário |
| `DELETE` | `/Funcionario/{id}` | Deletar funcionário |

### Schema do Funcionário

```json
{
  "nome": "Nome do Funcionário",
  "endereco": "Rua 1234",
  "ramal": "1234",
  "emailProfissional": "email@empresa.com",
  "departamento": "TI",
  "salario": 5000,
  "dataAdmissao": "2024-01-15T00:00:00.000Z"
}
```

### Modelo de Classes

```
Funcionario
├── Id : int
├── Nome : string
├── Endereco : string
├── Ramal : string
├── EmailProfissional : string
├── Departamento : string
├── Salario : decimal
└── DataAdmissao : DateTimeOffset

FuncionarioLog (herda de Funcionario)
├── TipoAcao : Enum (Inclusao, Atualizacao, Remocao)
├── PartitionKey : string
├── RowKey : string
├── Timestamp : DateTimeOffset
├── ETag : Struct
└── JSON : string
```

---

## 🧪 Testando a API

### Via Swagger

Acesse:
```
https://rhdiolab-f9byeseedwh7ckaq.westus2-01.azurewebsites.net/swagger
```

### Print — Swagger UI

![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_200224.png)

### Via cURL

**Criar funcionário (POST):**
```bash
curl -X 'POST' \
  'https://rhdiolab-f9byeseedwh7ckaq.westus2-01.azurewebsites.net/Funcionario' \
  -H 'Content-Type: application/json' \
  -d '{
  "nome": "João Silva",
  "endereco": "Rua 1234",
  "ramal": "1234",
  "emailProfissional": "joao@email.com",
  "departamento": "TI",
  "salario": 5000,
  "dataAdmissao": "2024-01-15T00:00:00.000Z"
}'
```
![Texto Alternativo](/imagens/versao_final/print_2026-06-14_193730.jpg)
![Texto Alternativo](/imagens/versao_final/Print_pendrive_ms2h_02_study_TECH_Cursos_DIO_git_copilot_2026-06-14_205535.png)
![Texto Alternativo](/imagens/versao_final/Print_pendrive_ms2h_02_study_TECH_Cursos_DIO_git_copilot_2026-06-14_205524.png)


**Resposta esperada (201 Created):**
```json
{
  "id": 1,
  "nome": "João Silva",
  "endereco": "Rua 1234",
  "ramal": "1234",
  "emailProfissional": "joao@email.com",
  "departamento": "TI",
  "salario": 5000,
  "dataAdmissao": "2024-01-15T00:00:00+00:00"
}
```
![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_204907.png)

### Print — POST com resposta 201

![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_201143.png)
![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_205151.png)
![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_204907.png)

**Buscar funcionário (GET):**
```bash
curl -X 'GET' \
  'https://rhdiolab-f9byeseedwh7ckaq.westus2-01.azurewebsites.net/Funcionario/1'
```

**Atualizar funcionário (PUT):**
```bash
curl -X 'PUT' \
  'https://rhdiolab-f9byeseedwh7ckaq.westus2-01.azurewebsites.net/Funcionario/1' \
  -H 'Content-Type: application/json' \
  -d '{
  "nome": "João Silva Atualizado",
  "endereco": "Rua 5678",
  "ramal": "5678",
  "emailProfissional": "joao.novo@email.com",
  "departamento": "RH",
  "salario": 7000,
  "dataAdmissao": "2024-01-15T00:00:00.000Z"
}'
```

**Deletar funcionário (DELETE):**
```bash
curl -X 'DELETE' \
  'https://rhdiolab-f9byeseedwh7ckaq.westus2-01.azurewebsites.net/Funcionario/1'
```

---

## 📊 Logs no Azure Table Storage

Toda alteração gera um log automático na tabela `FuncionarioLog`:

| TipoAcao | Gerado quando |
|----------|--------------|
| `Inclusao (0)` | POST — novo funcionário criado |
| `Atualizacao (1)` | PUT — funcionário atualizado |
| `Remocao (2)` | DELETE — funcionário removido |

### Visualizar os logs

```
Portal Azure
  → logrhdiolab (Conta de armazenamento)
    → Navegador de armazenamento
      → Tabelas
        → FuncionarioLog
```

Ou via terminal:
```bash
CHAVE=$(az storage account keys list \
  --account-name logrhdiolab \
  --resource-group desafiorh \
  --query [0].value \
  --output tsv)

az storage entity query \
  --table-name FuncionarioLog \
  --account-name logrhdiolab \
  --account-key $CHAVE \
  --output table
```

### Print — Tabela FuncionarioLog com registros

![Texto Alternativo](/imagens/versao_final/print_2026-06-14_044705_Gallery.jpg)
![Texto Alternativo](/imagens/versao_final/Print_logrhdiolab_Microsoft_Azure_Chrome_2026-06-14_215824.png.jpg)
![Texto Alternativo](/imagens/versao_final/print_2026-06-14_044720_Gallery.jpg)

### Print — Conta de armazenamento no Portal

![Texto Alternativo](/imagens/versao_final/print_2026-06-14_044658_Gallery.jpg)

---

## 🖼️ Prints do Projeto

| # | Descrição | Print |
|---|-----------|-------|
| 1 | Grupo de recursos no Portal Azure | ![Texto Alternativo](/imagens/versao_final/Print_Resource_Manager_Microsoft_Azure_Chrome_2026-06-14_185724.png) |
| 2 | App Service rhdiolab em execução | ![Texto Alternativo](/imagens/versao_final/Print_RhDioLab_desafio_rh_azure_RhDioLab_Microsoft_Azure_Goo_2026-06-14_220722.png)|
|3 | POST retornando 201 Created | ![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_204907.png) |
| 4 | GET retornando o funcionário | ![Texto Alternativo](/imagens/versao_final/Print_Swagger_UI_Chrome_2026-06-14_205151.png) |
| 5 | Log Atividades | ![Texto Alternativo](/imagens/versao_final/Print_logrhdiolab_Microsoft_Azure_Chrome_2026-06-14_204552.png) |
| 6 | FuncionarioLog no Table Storage | ![Texto Alternativo](/imagens/versao_final/Print_logrhdiolab_Microsoft_Azure_Chrome_2026-06-14_215824.png) |
| 7 | Conta gratuita | ![Texto Alternativo](/imagens/versao_final/print_2026-06-14_044658_Gallery) |
| 8 | Tudo Apagado | ![Texto Alternativo](/imagens/versao_final/print_2026-06-14_044737_Gallery)|
---

## 👩‍💻 Autora

Feito com durante a **Trilha .NET — Nuvem com Microsoft Azure** da [DIO](https://www.dio.me/)
AI melhor editor de md que eu já vi!
Joga o texto e ele estrutura tudo, corrigi ortografia, indentação!

---

## 📄 Licença

Este projeto está sob a licença MIT.
