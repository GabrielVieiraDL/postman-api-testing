# 📘 Guia de Estudos Postman & Dicas para Technical Support Engineer (TSE)

Este documento reúne comandos essenciais, conceitos de baixo nível, macetes de depuração e padrões de testes para atuar com maestria em suporte técnico de APIs, integrações de clientes e QA.

---

## 🧭 O Mindset de um Technical Support Engineer (TSE) para APIs

Quando um cliente ou desenvolvedor reporta: *"A API de vocês está retornando erro 500 ou não está funcionando"*, o papel do TSE não é repassar para o time de desenvolvimento cegamente, mas sim:

1. **Inspecionar a tríade HTTP:**
   - **Method & URL**: Parâmetros de rota (`/users/:id`) vs Query Parameters (`?status=active`).
   - **Headers**: `Content-Type`, `Accept`, `Authorization`, `X-Request-ID`, `User-Agent`.
   - **Body (Payload)**: JSON malformatado, campos obrigatórios ausentes, tipos incorretos (string enviada onde se espera integer).
2. **Reproduzir o cenário no Postman:**
   - Montar a requisição idêntica e checar o **Postman Console** (`Alt + Ctrl + C`).
   - Analisar o status code real:
     - `400 Bad Request`: Payload ou parâmetros inválidos (erro do cliente).
     - `401 Unauthorized`: Token expirado, ausente ou chave inválida.
     - `403 Forbidden`: Token válido, mas sem permissão (RBAC/Scopes) para aquele recurso.
     - `404 Not Found`: Rota inexistente ou entidade de ID não encontrada.
     - `422 Unprocessable Entity`: Erro semântico de validação de regras de negócio.
     - `429 Too Many Requests`: Estouro de rate limit (checar header `Retry-After`).
     - `500/502/503/504`: Erro interno no backend, proxy reverso fora ou gateway timeout.
3. **Validar Contratos (OpenAPI/Schema):**
   - A resposta da API segue a especificação prometida?

---

## ⚡ Comandos e Snippets Essenciais no Postman (pm.* API)

Os scripts rodam em JavaScript na sandbox do Postman.

### 1. `Pre-request Script` (Executado ANTES da chamada)

Usado para configurar variáveis dinâmicas, timestamps, gerar hashes ou renovar tokens:

```javascript
// Gerar timestamp em segundos ou ISO string
pm.environment.set("current_timestamp", Math.floor(Date.now() / 1000));
pm.environment.set("iso_date", new Date().toISOString());

// Gerar UUID aleatório nativo do Postman
pm.environment.set("request_uuid", pm.variables.replaceIn("{{$guid}}"));

// Gerar dados fictícios dinâmicos
const randomEmail = pm.variables.replaceIn("{{$randomEmail}}");
pm.environment.set("test_email", randomEmail);
```

### 2. `Tests Script` (Executado APÓS a resposta)

A biblioteca de asserções do Postman usa a sintaxe do **Chai BDD** (`pm.expect`).

#### A) Checagem de Status Code e Tempo de Resposta (SLA)
```javascript
pm.test("Status code é 200 OK", function () {
    pm.response.to.have.status(200);
});

pm.test("Tempo de resposta menor que 500ms (SLA)", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});
```

#### B) Checagem de Headers
```javascript
pm.test("Header Content-Type é application/json", function () {
    pm.response.to.have.header("Content-Type");
    pm.expect(pm.response.headers.get("Content-Type")).to.include("application/json");
});

// Essencial para TSE: checar IDs de correlação de logs
pm.test("Possui Header de rastreabilidade (X-Request-ID ou similar)", function () {
    pm.expect(pm.response.headers.has("X-Request-ID") || pm.response.headers.has("traceparent")).to.be.true;
});
```

#### C) Parse de JSON e Validação de Conteúdo
```javascript
const jsonData = pm.response.json();

pm.test("Propriedades essenciais existem no corpo", function () {
    pm.expect(jsonData).to.be.an("object");
    pm.expect(jsonData).to.have.property("status");
    pm.expect(jsonData.status).to.eql("success");
});

// Guardar ID recebido para a próxima requisição na coleção (Chaining)
if (jsonData.id) {
    pm.environment.set("entity_id", jsonData.id);
    console.log("Variável entity_id atualizada para:", jsonData.id);
}
```

#### D) Validação Completa de Schema JSON (Ajv)
Garante que a resposta não quebrou o contrato, mesmo que o status seja 200:
```javascript
const schema = {
    "type": "object",
    "required": ["city", "temperature", "conditions"],
    "properties": {
        "city": { "type": "string" },
        "temperature": { "type": "number" },
        "conditions": { "type": "string" },
        "humidity": { "type": "integer", "minimum": 0, "maximum": 100 }
    }
};

pm.test("Resposta adere ao Schema JSON contratado", function () {
    pm.response.to.have.jsonSchema(schema);
});
```

---

## 💡 Dicas de Ouro para Aprendizado Profundo

1. **Abra o Postman Console Sempre (`Ctrl + Alt + C`)**:
   - É lá que você vê a requisição crua (*raw request*), os headers reais enviados e as mensagens de `console.log()`.
2. **Entenda o Escopo de Variáveis**:
   - `Global` > `Collection` > `Environment` > `Data (CSV/JSON no Runner)` > `Local`.
   - Regra de ouro: Variáveis de configuração de servidor (URLs, credenciais) ficam no **Environment**. Variáveis de fluxo temporárias ficam no **Collection** ou **Local**.
3. **Nunca versione segredos em arquivos de ambiente exportados**:
   - Ao exportar o arquivo JSON de environment, verifique se campos de chaves privadas, senhas e tokens reais não estão preenchidos na coluna `Initial Value` (o Postman só sincroniza e exporta o `Initial Value`, mantendo o `Current Value` local no seu app).
4. **Pratique o Collection Runner**:
   - Use o runner embutido do Postman para rodar batches de requisições e carregar arquivos CSV ou JSON com dados de teste.
5. **Aprenda a Gerar cURL**:
   - No painel lateral direito do Postman (botão `</>`), copie o comando em formato `cURL` para testar no terminal ou compartilhar com clientes para reproduzir erros.
