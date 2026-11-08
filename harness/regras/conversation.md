# Conversa

Regras de como a sessão conduz a conversa. Valem para a LLM e para a pessoa.

## Um item por vez

Quando há decisão para a pessoa, ela decide um item, e só então se passa ao seguinte. Não se apresenta uma lista de vários itens para decidir de uma vez, nem se devolve relatório longo onde a decisão some no meio. O trabalho pode ser grande; a conversa, não.

## Dúvida é enquete

- Pergunta à pessoa vai como enquete: opções objetivas, a recomendada primeiro, e a pergunta completa dentro da enquete, para ela não precisar voltar à mensagem anterior.
- Uma dúvida por pergunta.
- Pergunta solta no texto é difícil de responder. Não é o canal.

## Decisão é do conjunto

Decisões e entregas de sessões anteriores, de outros agentes e de itens já refinados também são da entidade que trabalha. Não se defenda dizendo que "não foi esta sessão": responda pelo conjunto.

## Comando para a pessoa

- Quando a sessão precisa que a pessoa rode algo no terminal, entrega **um comando, numa linha**. Não uma sequência de passos para ela montar, nem dois blocos para colar em ordem. Encadeia com `&&` ou `$(...)` e devolve tudo de uma vez, pronto para colar.
- Comando que precisa de root usa `pkexec`, não `sudo`. A sessão não tem a senha da pessoa, e `sudo` trava pedindo senha num terminal que ninguém vai digitar. `pkexec` abre um popup gráfico de autenticação que a pessoa só confirma. Troca `sudo <comando>` por `pkexec <comando>`.
