# 🚀 Postman API Testing & Automation Portfolio

Repositório central de estudos, coleções, testes automatizados e especificações de APIs desenvolvido ao longo da trilha oficial **Postman API Testing Path** e focado na atuação profissional como **Technical Support Engineer (TSE)** e **QA / API Tester**.

---

## 🎯 Objetivo Profissional (Technical Support Engineer)

Em suporte técnico avançado (L2/L3) e integrações de APIs, o diferencial técnico está na habilidade de:
1. **Isolar a causa raiz (Root Cause Analysis)** de falhas de clientes via contratos HTTP e payloads.
2. **Reproduzir chamadas com fidelidade** utilizando variáveis de ambiente, autenticações dinâmicas e mocks.
3. **Validar SLAs e estabilidade** por meio de asserções em tempo de resposta, cabeçalhos, status code e schemas JSON.
4. **Automatizar diagnósticos e regressões** com CLI (`newman`) e pipelines de CI/CD.

---

## 📁 Estrutura do Repositório

```text
postman-api-testing/
├── .github/
│   └── workflows/
│       └── postman-tests.yml        # CI/CD: Execução automática via Newman no GitHub Actions
├── collections/
│   └── PostAir Weather API.postman_collection.json  # Casos de teste e requisições organizadas
├── environments/
│   └── PostAir Dev.postman_environment.json         # Ambientes com variáveis seguras e URLs base
├── spec/
│   └── openapi.yaml                 # Especificação e contrato da API (OpenAPI 3.0 / Swagger)
├── docs/
│   └── notes.md                     # Guia de estudo, comandos, scripts e anotações para TSE
├── monitors/                        # Configurações de monitoramento recorrente no Postman Cloud
├── scripts/                         # Snippets reutilizáveis de Pre-request e Tests
├── .gitignore                       # Proteção contra vazamento de tokens e arquivos locais
└── README.md
```

---

## 🛠️ Tecnologias & Ferramentas

- **Postman**: Desenvolvimento de coleções, environments, mocks, testes e documentação interativa.
- **Newman**: Postman CLI para execução de testes em terminal e pipelines de integração contínua.
- **JavaScript (Chai / Postman Sandbox)**: Scripts de `Pre-request` e asserções avançadas em `Tests`.
- **Ajv (JSON Schema Validator)**: Validação de contratos de resposta.
- **GitHub Actions**: Automação de testes a cada push e pull request.

---

## 🧪 Como Executar os Testes Localmente via Newman

1. Instale o Newman globalmente via Node.js:
   ```bash
   npm install -g newman newman-reporter-htmlextra
   ```

2. Execute uma coleção com ambiente:
   ```bash
   newman run "collections/PostAir Weather API.postman_collection.json" \
     -e "environments/PostAir Dev.postman_environment.json" \
     --reporters cli,htmlextra \
     --reporter-htmlextra-export ./newman/report.html
   ```

---

## 📋 Casos de Teste e Estudos de Caso

- [x] **PostAir Weather API**: Primeiro case oficial da trilha Postman API Testing (Contratos, query params, asserções de status, headers e schemas).
- [ ] **Fluxos de Autenticação (OAuth 2.0 / API Key / Bearer)**: Scripts de renovação dinâmica de token.
- [ ] **Testes de Regressão e Resiliência**: Validação de rate limits (429), timeouts e payloads malformados (400/422).

---

Feito com dedicação para aprendizado contínuo por [Gabriel Vieira](https://github.com/GabrielVieiraDL).
