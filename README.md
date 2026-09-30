# ⚡ Miniguia de Estudos: Arquitetura de APIs RESTful e Webhooks
> **Bootcamp Santander 2026 - Automação com N8N | Desafio de Projeto: NotebookLM**

Este repositório contém a documentação e os resultados do desafio prático de uso do **NotebookLM** como ferramenta de aprendizagem e consulta técnica, focado em **APIs RESTful, Webhooks e Integração de Sistemas**.

---

## 🎯 1. Contexto e Objetivos

### Contexto
No ecossistema de automação de processos (RPA, N8N, sistemas legados e microsserviços), o consumo de APIs REST e a recepção de eventos assíncronos via Webhooks são a espinha dorsal de qualquer comunicação entre aplicações. Mapear estes conceitos com clareza é fundamental para construir fluxos robustos.

### Objetivos de Estudo
1. Compreender o funcionamento do protocolo HTTP, métodos REST e estruturas de dados JSON.
2. Diferenciar arquiteturas baseadas em *Polling* (síncronas) e *Webhooks* (assíncronas/baseadas em eventos).
3. Entender padrões de autenticação seguros (Bearer Token, API Key, OAuth2) para conectores de automação.
4. Testar a capacidade do **NotebookLM (RAG)** em resumir padrões da especificação OpenAPI / Swagger e manuais de integração.

---

## 📑 2. Curadoria de Fontes

Para alimentar o NotebookLM com dados técnicos confiáveis, foram selecionadas **4 fontes abertas e oficiais**:

| Fonte | Instituição / Origem | Descrição / Tipo de Conteúdo |
| :--- | :--- | :--- |
| **1. Guia Oficial HTTP/REST** | Mozilla Developer Network (MDN) | Métodos HTTP, Headers, Status Codes (2xx, 4xx, 5xx) e padrões de API REST. |
| **2. Documentação de Webhooks & Triggers** | N8N / Webhook Docs | Conceitos de endpoints receptores, escuta de payloads JSON em tempo real e respostas `200 OK`. |
| **3. Padrão de Autenticação em APIs** | OETF / Auth Specifications | Guia de implementação de Bearer Tokens, API Keys e headers de autorização. |
| **4. Especificação OpenAPI v3.0** | OpenAPI Initiative | Estruturação de esquemas JSON, parâmetros de requisição e contratos de resposta. |

---

## 🧪 3. Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Registro dos testes de prompts aplicados no NotebookLM para extração e organização de conhecimento técnico sobre APIs.

### Teste 1: Diferenciação entre REST e Webhooks (Prompt V1 vs V2)
* **Prompt V1 (Superficial)**: *"Qual a diferença entre API e Webhook?"*
  * **Resultado**: A IA respondeu de forma genérica, sem citar a diferença de arquitetura de rede (*pull* vs *push*) nem o comportamento dos servidores.
* **Prompt V2 (Técnico e Delimitado)**: *"Com base nas fontes fornecidas (MDN e N8N Docs), compare o modelo de comunicação síncrono (Polling via HTTP Request) com o modelo assíncrono (Webhooks). Explique a diferença no consumo de recursos do servidor e na latência de dados."*
  * **Resultado**: A IA gerou uma resposta precisa, destacando que Webhooks evitam requisições desnecessárias (event-driven), reduzindo a latência a zero para acionamento de workflows.

### 🛠️ Cicatrizes & Troubleshooting (Dificuldades Encontradas)
1. **Tratamento de Códigos de Erro HTTP**:
   * *Problema*: Inicialmente a IA misturou erros do cliente (`401 Unauthorized`, `403 Forbidden`) com erros do servidor (`500 Internal Server Error`, `503 Service Unavailable`).
   * *Solução*: Foi aplicado um prompt de filtragem exigindo que a IA agrupasse os códigos de status HTTP por família de dígitos (2xx, 4xx e 5xx) com exemplos de uso em workflows do N8N.
2. **Formatação de Payloads JSON**:
   * *Ajuste*: Solicitou-se explicitamente a exibição das respostas no formato de código `json` para validação de sintaxe (chaves, arrays e objetos aninhados).

---

## 📖 4. Miniguia de Estudo (Entrega Final)

### Resumo Estruturado do Assunto
- **Mapeamento de Verbos HTTP**:
  - `GET`: Recupera dados de um recurso (idempotente e seguro).
  - `POST`: Envia novos dados para criação de recurso.
  - `PUT`: Atualiza integralmente um recurso existente.
  - `PATCH`: Atualiza parcialmente um recurso.
  - `DELETE`: Remove um recurso da aplicação.
- **Webhooks vs Polling**:
  - **Polling (HTTP GET continuo)**: O cliente pergunta repetidamente ao servidor se há novos dados. Gera tráfego desnecessário e alta latência.
  - **Webhook (HTTP POST orientado a eventos)**: O servidor envia uma requisição automaticamente para a URL do cliente assim que um evento acontece. Ideal para gatilhos no N8N.
- **Autenticação**:
  - `API Key`: Enviada via Header ou Query Parameter para identificação simples.
  - `Bearer Token (JWT)`: Token encriptado passado no Header `Authorization: Bearer <token>` para sessão segura.

---

### 📌 Glossário Técnico
* **Payload**: O corpo útil dos dados transmitidos na requisição ou resposta HTTP (geralmente formatado em JSON).
* **Endpoint**: A URL específica onde uma API pode acessar os recursos do servidor (Ex: `https://api.exemplo.com/v1/pedidos`).
* **Header (Cabeçalho)**: Metadados enviados na requisição, informando tipo de conteúdo (`Content-Type: application/json`), credenciais de acesso, etc.
* **Status Code**: Código numérico retornado pelo servidor indicando o resultado da requisição:
  * `200 OK` / `201 Created`: Sucesso.
  * `400 Bad Request`: Erro de sintaxe nos dados enviados.
  * `401 Unauthorized` / `403 Forbidden`: Problemas de credencial/permissão.
  * `404 Not Found`: Endpoint ou recurso inexistente.
  * `500 Internal Server Error`: Erro interno no servidor de destino.
* **Rate Limit**: Limite de requisições que um cliente pode fazer a uma API em um determinado intervalo de tempo (Ex: 100 requisições/minuto).

---

### 🔄 Prompts Reutilizáveis para Revisões Futuras

```text
[Prompt de Análise de Erros de API]:
"Atuando como um Analista de Integrações, analise o payload JSON e o status code retornado [Inserir erro] e diagnostique se o problema ocorreu na autenticação, no formato do corpo da requisição ou no servidor de destino."

[Prompt de Mapeamento para N8N]:
"Com base na especificação da API fornecida no texto, me diga quais os headers obrigatórios e qual o método HTTP correto para configurar o nó 'HTTP Request' do N8N para criar um novo registro."

[Prompt de Validação de JSON Schema]:
"Gere um exemplo de payload JSON válido contendo um array de objetos com campos de ID, data, status e valor, pronto para ser manipulado via JavaScript."
