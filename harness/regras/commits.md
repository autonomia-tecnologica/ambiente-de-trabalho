# Commits

- **Mensagem:** a última linha é `AI Generated`. Sem coautoria de ferramenta e sem rastro de ferramenta no texto. A checagem automática é proposta em `forcado.yaml`, não existe aqui.
- **Scan de segredos antes de todo commit**, da raiz do repo. Só se commita com o scan limpo. O scan vale para cada commit, inclusive o de fechar a sessão. Ele lê os arquivos como estão, sem respeitar `.gitignore`, e a história do git.
- **Achado do scan:** a sessão diz **onde** (regra, arquivo, linha), nunca o valor, e não abre o arquivo para "conferir".
- Achado em arquivo que o git ignora não viaja no commit, mas segredo em claro continua fora da pasta de trabalho (`secrets.md`).
- **Custo:** registrar tokens e retrabalho no corpo do commit, ver `agents.md`.
