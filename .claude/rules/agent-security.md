# Rule: Segurança para agentes de IA

## Permissões
- Use o princípio do menor privilégio.
- Agente de pesquisa deve ter apenas leitura e busca.
- Agente de segurança deve poder ler e auditar, mas não aplicar mudanças sem plano.

## Segredos
- Nunca leia, copie, resuma ou imprima arquivos `.env`, chaves privadas, tokens, certificados ou secrets.
- Nunca cole segredo em código, logs, testes, documentação ou mensagens.
- Use `.env.example` apenas com nomes de variáveis e valores fictícios.

## Comandos perigosos
- Proibido executar `rm -rf`, `chmod 777`, `curl | bash`, `wget | sh`, `git push --force`, `docker system prune -a`, `DROP DATABASE`, `TRUNCATE`, `DELETE` sem `WHERE`.
- Proibido rodar migration em produção sem runbook.
