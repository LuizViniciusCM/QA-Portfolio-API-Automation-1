# ServeRest API - Automação de Testes com Postman

Switch de testes funcionais automatizados para a API pública [ServeRest](https://serverest.dev), cobrindo **32 casos de teste** (CT-01 a CT-32) organizados em uma collection Postman autocontida e executável a qualquer momento.

> **Autor:** Luiz Vinicius Cunha Maciel  
> **Versão:** 2.0 - Automatizada (baseada no Plano de Testes Manual v1.0)  
> **Data:** 08/08/2026

> **Este repositório é a evolução automatizada do projeto de testes manuais.** A v1.0, com plano de testes manual, casos de teste, matriz de execução, relatório de bugs e Test Summary Report do Ciclo 1, está disponível em [QA-Portfolio-API-Test-1](https://github.com/LuizViniciusCM/QA-Portfolio-API-Test-1).

---

## Sumário

- [Sobre o Projeto](#sobre-o-projeto)
- [Evolução da v1 para a v2](#evolução-da-v1-para-a-v2)
- [Funcionalidades Cobertas](#funcionalidades-cobertas)
- [Estrutura da Collection](#estrutura-da-collection)
- [Casos de Teste](#casos-de-teste)
- [Pré-requisitos](#pré-requisitos)
- [Como Executar](#como-executar)
- [Estratégia de Automação](#estratégia-de-automação)
- [Arquivos do Projeto](#arquivos-do-projeto)
- [Critérios de Entrada e Saída](#critérios-de-entrada-e-saída)

---

## Sobre o Projeto

Este projeto automatiza a validação das principais funcionalidades da API REST ServeRest, que simula o back-end de um e-commerce. A switch cobre os fluxos de **login**, **cadastro e gestão de usuários**, **produtos** e **carrinhos de compra**, verificando status codes, contratos de resposta e regras de negócio a cada execução.

**Destaques da abordagem:**

- Cada requisição é **autocontida**: os dados de teste (usuários, produtos, carrinhos) são criados dinamicamente em runtime via pre-request scripts, sem dependência de estado de execuções anteriores.
- A switch pode ser executada por completo, por pasta ou requisição isolada, **em qualquer ordem**, com resultado determinístico.
- Resolve por construção a restrição do ambiente público, que **reinicia sua base de dados diariamente** - cada execução recria a própria massa de dados.

---

## Evolução da v1 para a v2

A v1.0 ([QA-Portfolio-API-Test-1](https://github.com/LuizViniciusCM/QA-Portfolio-API-Test-1)) cobriu um ciclo completo de **testes manuais**: planejamento, design dos 32 casos de teste, execução via Postman, registro de defeitos e Test Summary Report. Resultados do Ciclo 1:

| Métrica | Valor |
|---|---|
| Casos executados | 32/32 (100%) |
| Casos aprovados | 31 (96,9%) |
| Defeitos encontrados | 1 — BUG-01 (severidade Alta) |
| Cobertura de funcionalidades | 17/17 (100%) |

O BUG-01 identificado na v1 — `GET /usuarios` retornar e-mail e senha de todos os usuários sem exigir autenticação - foi preservado e documentado como **ponto de atenção de segurança** no CT-31 desta switch automatizada.

A v2.0 (este repositório) automatiza os mesmos 32 casos em uma collection Postman **autocontida e determinística**, eliminando a execução manual repetitiva e tornando a switch executável a qualquer momento, inclusive considerando que o ambiente público reinicia sua base de dados diariamente.

---

## Funcionalidades Cobertas

| Funcionalidade | Endpoints | Casos de Teste |
|---|---|---|
| Login | `POST /login` | CT-01 a CT-04 |
| Cadastro de Usuários | `GET`, `POST`, `PUT`, `DELETE /usuarios` | CT-05 a CT-15 |
| Produtos | `GET`, `POST`, `PUT`, `DELETE /produtos` | CT-16 a CT-22 |
| Carrinhos | `GET`, `POST`, `DELETE /carrinhos` | CT-23 a CT-28, CT-32 |
| Segurança | Rotas protegidas diversas | CT-29 a CT-31 |

**Fora do escopo:** testes de interface (front-end), carga/performance em larga escala, segurança avançada e monitoramento de infraestrutura.

---

## Estrutura da Collection

```
Testes - ServeRest (Automatizada)
│
├── 📁 Login               (CT-01 a CT-04)
├── 📁 Cadastro de Usuários (CT-05 a CT-15)
├── 📁 Produtos            (CT-16 a CT-22)
├── 📁 Carrinhos           (CT-23 a CT-28, CT-32)
└── 📁 Segurança           (CT-29 a CT-31)
```

---

## Casos de Teste

### Login

| ID | Título | Tipo | Prioridade |
|---|---|---|---|
| CT-01 | Login com credenciais válidas | Smoke / Funcional | 🔴 Alta |
| CT-02 | Login com senha incorreta | Negativo | 🔴 Alta |
| CT-03 | Login com e-mail inexistente | Negativo | 🔴 Alta |
| CT-04 | Login com campos obrigatórios vazios | Negativo | 🟡 Média |

### 👤 Usuários

| ID | Título | Tipo | Prioridade |
|---|---|---|---|
| CT-05 | Cadastro de novo usuário | Funcional | 🔴 Alta |
| CT-06 | Cadastro com e-mail já existente | Negativo | 🔴 Alta |
| CT-07 | Cadastro com campos obrigatórios vazios | Negativo | 🟡 Média |
| CT-08 | Cadastro com campo "administrador" em tipo incorreto | Contrato/Schema | 🟡 Média |
| CT-09 | Listagem de usuários | Funcional | 🟡 Média |
| CT-10 | Listagem de usuários com filtro por query string | Funcional | 🟡 Média |
| CT-11 | Consulta de usuário por ID existente | Funcional | 🟢 Baixa |
| CT-12 | Consulta de usuário por ID inexistente | Negativo | 🟢 Baixa |
| CT-13 | Edição de usuário existente | Funcional | 🟡 Média |
| CT-14 | Exclusão de usuário sem vínculo com carrinho | Funcional | 🟡 Média |
| CT-15 | Exclusão de usuário vinculado a carrinho ativo | Regra de negócio | 🔴 Alta |

### 📦 Produtos

| ID | Título | Tipo | Prioridade |
|---|---|---|---|
| CT-16 | Cadastro de produto com usuário administrador autenticado | Funcional | 🔴 Alta |
| CT-17 | Cadastro de produto sem autenticação | Autenticação/Negativo | 🔴 Alta |
| CT-18 | Cadastro de produto com usuário não administrador | Autorização/Negativo | 🔴 Alta |
| CT-19 | Listagem de produtos | Funcional | 🟡 Média |
| CT-20 | Consulta de produto por ID | Funcional | 🟢 Baixa |
| CT-21 | Edição de produto com usuário administrador autenticado | Funcional | 🟡 Média |
| CT-22 | Exclusão de produto vinculado a carrinho | Regra de negócio | 🔴 Alta |

### 🛒 Carrinhos

| ID | Título | Tipo | Prioridade |
|---|---|---|---|
| CT-23 | Criação de carrinho vinculado ao usuário autenticado | Smoke / Funcional | 🔴 Alta |
| CT-24 | Impedimento de criar novo carrinho havendo carrinho ativo | Regra de negócio | 🔴 Alta |
| CT-25 | Validação de estoque insuficiente ao adicionar produto | Regra de negócio | 🔴 Alta |
| CT-26 | Consulta de carrinho por ID | Funcional | 🟡 Média |
| CT-27 | Concluir compra do carrinho | Smoke / Funcional | 🔴 Alta |
| CT-28 | Cancelar compra do carrinho | Funcional | 🔴 Alta |
| CT-32 | Consulta de carrinho (listagem) | Funcional | 🟡 Média |

### Segurança

| ID | Título | Tipo | Prioridade |
|---|---|---|---|
| CT-29 | Acesso a rotas protegidas sem token | Autenticação/Negativo | 🔴 Alta |
| CT-30 | Acesso a rotas protegidas com token inválido/expirado | Autenticação/Negativo | 🔴 Alta |
| CT-31 | Listagem de usuários sem autenticação | Autenticação | 🔴 Alta |

---

## Pré-requisitos

- [Postman](https://www.postman.com/downloads/) versão **10 ou superior** (necessário para suporte a `top-level await` nos scripts)
- Acesso à internet (o ambiente `https://serverest.dev` é público)

---

## Como Executar

### 1. Importar os arquivos no Postman

1. Abra o Postman
2. Clique em **Import**
3. Importe os dois arquivos:
   - `Testes_postman_collection_automatizada.json` — a collection
   - `ServeRest_environment.json` — o environment com a variável `URL`

### 2. Selecionar o environment

No canto superior direito do Postman, selecione o environment **"ServeRest - QA Automation"**.

### 3. Executar a switch completa

1. Clique com o botão direito na collection **"Testes - ServeRest (Automatizada)"**
2. Selecione **"Run collection"**
3. Clique em **"Run"** - nenhuma configuração adicional é necessária

### 4. Executar uma pasta ou teste isolado

Clique com o botão direito em qualquer pasta (ex.: `Login`, `Produtos`) ou requisição individual e selecione **"Run"**. Cada item é autocontido e pode ser executado de forma independente.

---

## Estratégia de Automação

### Geração dinâmica de dados

Toda a massa de dados é criada em runtime pelos helpers de pre-request script, sem dados fixos hardcoded:

| Helper | Descrição |
|---|---|
| `randomEmail()` | Gera e-mail único com timestamp + número aleatório |
| `randomNome()` | Gera nome aleatório via variável dinâmica do Postman |
| `randomSenha()` | Gera senha aleatória |
| `criarUsuario(admin)` | Cria usuário comum ou administrador via `POST /usuarios` |
| `login(email, senha)` | Autentica e retorna o bearer token |
| `criarUsuarioAdminComToken()` | Cria admin e retorna token de autenticação já pronto |
| `criarProduto(token, overrides)` | Cadastra produto com dados padrão sobrescritíveis |
| `criarCarrinho(token, idProduto, qtd)` | Cria carrinho com produto especificado |
| `criarUsuarioComCarrinho(idProduto, qtd)` | Monta cenário completo para testes de regra de negócio |

### Camadas de verificação

Cada requisição valida consistentemente quatro aspectos:

1. **Status code** - código HTTP esperado (200, 201, 400, 401, 403)
2. **Tempo de resposta** - limite de `< 2000ms` como guarda-corpo de performance
3. **Contrato/Schema** - presença, tipo e obrigatoriedade dos campos no body de resposta
4. **Regra de negócio** - mensagens de erro e sucesso específicas da API

### Técnicas aplicadas

- **Particionamento de equivalência e análise de valor limite** - e-mails únicos vs. duplicados, quantidade solicitada vs. estoque
- **Tabela de decisão** - variação do parâmetro `administrador: true/false` nos helpers de criação
- **Testes ponta-a-ponta** - fluxo completo: cadastro → login → produto → carrinho → conclusão/cancelamento
- **Teste de contrato** - assertions dedicadas à estrutura do JSON de resposta em cada requisição

---

## Arquivos do Projeto

| Arquivo | Descrição |
|---|---|
| `Testes_postman_collection_automatizada.json` | Collection Postman com os 32 casos de teste automatizados |
| `ServeRest_environment.json` | Environment com as variáveis `URL` e `token` |
| `Plano_de_Teste_Automatizado.pdf` | Plano de testes automatizados (v2.0) |
| `Casos_de_Teste.pdf` | Detalhamento de todos os 32 casos de teste (v1.0 manual) |
| `README.md` | Este documento |

---

## Critérios de Entrada e Saída

### Critérios de entrada
- Ambiente `https://serverest.dev` acessível
- Collection e environment importados e validados no Postman

### Critérios de saída
- 100% dos 32 casos de teste executados com sucesso
- Nenhuma falha (*failure*) ou erro (*error*) reportado no resumo do Postman

### Critério de suspensão e retomada
A execução é suspensa caso o ambiente fique indisponível ou o rate limit da API seja atingido. Como toda a massa é gerada em runtime, a switch pode ser **retomada a qualquer momento** sem necessidade de reconfiguração.

---

## 🔗 Links Úteis

- [v1.0 — Testes Manuais (QA-Portfolio-API-Test-1)](https://github.com/LuizViniciusCM/QA-Portfolio-API-Test-1)
- [ServeRest — Ambiente de testes](https://serverest.dev)
- [Documentação interativa (Swagger)](https://serverest.dev)
- [Postman — Download](https://www.postman.com/downloads/)
