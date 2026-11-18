# postman-api-testing — CLAUDE.md

## Idioma de trabalho
- Responder e documentar em Português do Brasil.

## Objetivo do Projeto
- Hub central de coleções, automação de testes com Newman, validação de contratos OpenAPI/JSON Schema e portfólio de Technical Support Engineer (TSE) e QA.

## Workflow Obrigatório
1. **RESEARCH**: Analisar especificação da API (`spec/openapi.yaml`), contratos e requisições no Postman Console antes de alterar coleções.
2. **PLAN**: Planejar os casos de teste (positivos, negativos, SLAs de resposta, headers de correlação e schemas).
3. **IMPLEMENT**: Estruturar variáveis dinâmicas em `Pre-request` e asserções BDD em `Tests`.
4. **VERIFY**: Executar coleções localmente via Newman ou Postman Runner.
5. **REPORT**: Criar relatório em `docs/tasks/` com o resumo do que foi validado.

## Regras Ativas
- @.claude/rules/agent-security.md
- @.claude/rules/backend-security.md
- @.claude/rules/clean-code.md
- @.claude/rules/task-report.md

## Regras Invioláveis
- NUNCA versionar tokens reais, API Keys de produção ou credenciais nos arquivos `.json` de ambiente (usar sempre placeholders como `REPLACE_WITH_YOUR_KEY`).
- Todas as asserções devem ter mensagens de erro explícitas e legíveis.
- Monitorar e validar tempos de resposta (SLA) e integridade de headers essenciais (`Content-Type`, `X-Request-ID`).
