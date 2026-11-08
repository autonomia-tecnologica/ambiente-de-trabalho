# Backlog

Regras do `BACKLOG.md`, o arquivo de tarefas de cada repo. O painel do workspace lê esse arquivo: quem o formata segue este padrão.

## Arquivo

- `BACKLOG.md` na raiz de todo repo, sem outro caminho (nada de `TODO.md` nem `docs/BACKLOG.md`). Repo sem `BACKLOG.md` não tem tarefas: não se cria um vazio.
- Nunca é commitado: `/BACKLOG.md` no `.gitignore` de todo repo.
- Escrito em pt-BR, em todo repo. O código segue em inglês.
- Só o que está aberto. Item feito sai do arquivo ao fechar a fatia (sem commit, o arquivo não é versionado).
- O topo guarda `Próximo id: N`. Id é inteiro e incremental (1, 2, 3...), sem letra, prefixo ou código. Item novo recebe o próximo número e o contador sobe. Id nunca se reusa, nem o de item feito ou removido.
- Três seções, nesta ordem: `## Refinadas`, `## A refinar` e `## A validar`.

## Fio da meada

- Decisão nova vai para o arquivo assim que aparece, sem esperar o fim da sessão.
- Antes de perguntar de novo, ler o arquivo: o que já foi decidido não se pergunta outra vez.
- O que ficou em aberto fica nele, para a próxima sessão continuar do ponto.
- O que fechou sai dele. Regra que já virou regra mora no harness, não na fila.

## Categoria e dependência

- Todo item, em qualquer seção, tem `Categoria:` obrigatória: o assunto a que a pendência pertence, nome curto e reaproveitado dentro do repo. O painel agrupa por categoria.
- Categorias não dependem entre si. `Depende de` só lista ids da mesma categoria; dependência entre categorias é erro de formato.
- `Depende de:` é uma lista de ids separados por vírgula e espaço (`12, 15`), ou `—`. Item bloqueado é o que tem algum id listado ainda presente no arquivo. Id ausente conta como cumprido. Id maior ou igual ao `Próximo id` é provável erro de digitação.
- Ciclo, inclusive autodependência, é erro de formato: os itens do ciclo ficam inválidos e bloqueados até alguém desfazê-lo.

## Refinada e despacho

- Refinada é o item com `Categoria` e os quatro campos preenchidos e nenhuma pergunta em aberto. Só item refinado é despachado. Item ambíguo fica em `A refinar`.
- Item bloqueado não é despachado, mesmo em `Refinadas`.
- Ao registrar tarefa nova, não refinar por conta própria nem perguntar antes de salvar. Tarefa bem definida entra direto em `Refinadas`, com os quatro campos. Qualquer dúvida, por menor que seja, entra em `A refinar` do jeito que está, sem enquete.
- Só se refina quando a pessoa avisar que quer.

Itens de `Refinadas` e `A refinar`:

```
### 17. Título curto
- Categoria: Vídeo
- Caminhos: o que a fatia toca (arquivos e pastas)
- Depende de: 12 | —
- Aceite: o que tem de ser verdade quando acabar
- Validação: o comando ou a conferência, com o resultado esperado
```

## A validar

- Seção do que só a pessoa confere (escuta, olho, uso real): trabalho já entregue, ou decisão que depende de percepção humana.
- Nunca é despachado, e não entra em `Depende de`.
- Entra assim que a entrega existe (a sessão que entregou registra) e sai quando a pessoa valida. Existe para ela ver o que falta conferir.

Cada item:

```
### 18. Título curto
- Categoria: Vídeo
- Conferir: o que a pessoa precisa ver ou ouvir, e onde
- Como: os passos ou o material (trechos, comandos, caminhos)
- Decide: o que a conferência destrava ou muda, e em qual item ou repo
```

## Tamanho

- Fatia de no máximo 65k de contexto. Se o item não cabe, quebra em itens menores, cada um com id novo, e o original sai.
- Item curto, em torno de 15 linhas. Decisão e histórico de investigação não moram nele.
- `(PAUSADA)` no título tira o item das prontas.
