# Agentes

Papéis, despacho, worktrees e custo.

## Papéis

- Refinar é trabalho de modelo forte, em conversa com a pessoa.
- Implementar fatia refinada é trabalho de um subagente de implementação. Ele lê só o item e os caminhos de `Caminhos`, faz só o que o item pede, roda a `Validação` e devolve curto: arquivos mudados, números reais, dúvidas. Não commita e não fecha a fatia. Se o item for ambíguo, para e devolve a pergunta.
- Ao despachar, a descrição da chamada do subagente é `implementador-<id>`, com o id do item (ex.: `implementador-61`), para distinguir os workers na lista.
- Fechar é da sessão que despachou: confere a validação, tira o item do `BACKLOG.md` e commita.
- Revisão profunda só quando a pessoa pedir, nunca por fatia.

## Despacho

A pessoa diz o modo; a sessão não adivinha.

- **Uma fatia** ("faz o 17", "próxima"): implementa uma, fecha e para. É o padrão e o mais barato.
- **Tudo que está refinado** ("toca tudo"): vários workers em paralelo. Gasta mais token no total e compra tempo. Só entram fatias refinadas com `Caminhos` disjuntos e sem `Depende de` entre si. Cada worker roda num worktree próprio, com uma branch por fatia. Um integrador só, a sessão, mescla uma branch por vez, a mais fundamental primeiro, e roda a `Validação` a cada merge. Worker não resolve conflito.

## Worktrees

- Worktree não é sandbox; o limite real são as permissões do agente.
- Worktree novo não traz `.env` nem dependências: o repo tem um passo de preparação.
- A pessoa não cria worktrees. Quem cria, remove: o agente (ou a sessão que despachou) apaga o worktree e a branch que ele mesmo criou, depois de mesclar ou descartar. Nunca mexe nos que não criou.

## Custo

Ao fechar uma fatia, se o custo estiver disponível, registrar no corpo do commit os tokens gastos e se houve retrabalho.
