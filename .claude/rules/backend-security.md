# Rule: Segurança backend e APIs

## Endpoints e Contratos
- Todo endpoint deve declarar método, rota, autenticação, autorização, payload e retorno.
- Não expor stack trace para cliente.
- Usar respostas de erro padronizadas (RFC 7807 / ProblemDetails).
- Aplicar rate limit em login, endpoints de teste e recursos custosos.

## Dados e Validação
- Validar todo dado recebido via HTTP, query params, headers ou body.
- Proibido SQL concatenado (usar parâmetros ou ORM).
- Validações de contrato com JSON Schema / OpenAPI.
