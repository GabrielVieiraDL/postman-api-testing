# Rule: Clean Code universal

## Clareza e Intenção
- Nomes de variáveis, testes e coleções devem revelar intenção direta.
- Testes de API devem declarar claramente o que está sendo asserido (`pm.test("Status code é 200 OK")`).
- Evite abreviações obscuras.

## Tamanho e Complexidade
- Scripts de pré-requisição e testes devem ser curtos e modulares.
- Funções reutilizáveis devem ser mantidas em scripts compartilhados ou na coleção.
- Não engolir exceções com `try/catch` vazio.
