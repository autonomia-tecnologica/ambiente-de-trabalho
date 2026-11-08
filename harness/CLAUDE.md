# CLAUDE.md

Regras que governam quem trabalha no ambiente: a pessoa e a LLM. Este arquivo é o resumo e o ponto de entrada. O detalhe está em `regras/`, a parte forçada em `forcado.yaml`, e a origem de cada regra em `ORIGEM.md`.

Estado: rascunho. Nada aqui é instalado nem carregado por ferramenta ainda.

Antes de mexer em backlog ou commit, ler a regra correspondente.

## Papéis

- A LLM executa. O workspace organiza (backlog, painel, repos). Este harness regra.
- Regra que nasce num repo e vale para os outros sobe para cá. Repo que contraria uma regra daqui diz a contradição e o motivo, nunca em silêncio.

## Conversa (`regras/conversation.md`)

- Um item por vez, na ordem. Sem parede de resposta.
- Dúvida para a pessoa vai como enquete: opções objetivas, a recomendada primeiro, uma dúvida por pergunta. Nunca só no texto.
- Decisões e entregas de sessões e agentes anteriores são do conjunto: responda por elas, não se defenda com "não foi esta sessão".
- Comando para a pessoa rodar: uma linha, encadeada com `&&` ou `$(...)`. Root usa `pkexec`, nunca `sudo`.

## Código (`regras/code.md`)

- Em repositório de código: identificadores, nomes de arquivo e pasta, comentários, logs, documentação, nomes de teste e mensagens de commit em inglês (en-US). A pasta principal da pessoa não é repositório de código e fica fora desta regra.
- Texto que a pessoa final lê passa por i18n, nunca fixo no código. Idioma principal: pt-BR. Outro idioma só como tradução decidida pela pessoa. App novo nasce com a camada de i18n.
- Código genérico: o que muda por caso entra por argumento ou `config.json`, nunca fixo no código.
- Antes de escrever script novo, procurar um que já faz o mesmo ou algo perto.

## Backlog (`regras/backlog.md`)

- `BACKLOG.md` na raiz de todo repo, nunca commitado (`/BACKLOG.md` no `.gitignore`). Só itens abertos, em pt-BR.
- Id inteiro, incremental, nunca reusado. Seções `Refinadas`, `A refinar` e `A validar`, nesta ordem.
- Despachado só o que está refinado (quatro campos e nenhuma dúvida aberta) e sem dependência aberta.

## Agentes (`regras/agents.md`)

- Uma fatia por vez, por padrão. Implementar fatia refinada é trabalho de subagente de escopo fechado, que não commita nem fecha a fatia.
- Fechar a fatia é da sessão que despachou: confere a validação, tira o item do backlog e commita.
- Revisão profunda só quando a pessoa pedir.
- Worktree não é sandbox. Quem cria, remove, e só o que criou.

## Commits (`regras/commits.md`)

- Scan de segredos antes de todo commit. Só se commita com o scan limpo.
- Mensagem sem coautoria nem rastro de ferramenta; última linha `AI Generated`.
- Ao fechar uma fatia, registrar no corpo do commit os tokens gastos, se disponíveis, e se houve retrabalho.

## Segredos (`regras/secrets.md`)

- Segredo em claro não fica na pasta de trabalho, nem ignorado pelo git. Entra por canal seguro, nunca colado no chat, em argumento de linha de comando ou em arquivo em claro.

## Harness (`regras/harness.md`)

- O harness guarda o que é estável e governa o trabalho. Log, medição, diário e estado da máquina são dado, não regra.
- Nada depende de uma ferramenta específica. O `CLAUDE.md` é o único arquivo que existe por causa de uma ferramenta, e por isso só aponta para o resto.

## Operação e infra (`regras/operation.md`, `regras/infra.md`)

- Antes de criar pasta, ver se já existe uma. Antes de baixar algo grande, conferir o espaço livre.
- Serviço novo é de usuário, não de sistema. Job longo roda com limite de memória.
- Imagem de container com tag exata de versão, nunca `latest`. Definição de infra versionada por stack; segredo fora do repo.
