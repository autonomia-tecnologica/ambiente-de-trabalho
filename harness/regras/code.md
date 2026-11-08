# Código

Regras para quem escreve, move ou altera código em repositório de código.

## Escopo

- Esta regra vale só para repositório de código. A pasta principal da pessoa (sua pasta de trabalho) não é repositório de código: esta regra não diz nada sobre ela, e quem a gerencia trata o idioma dela.

## Idioma

- Em repositório de código, em inglês (en-US): nomes de pasta e de arquivo em todos os níveis, identificadores, comentários, logs, documentação, nomes de teste e mensagens de commit.
- Texto que a pessoa final lê (texto, rótulo, interface, o que o programa mostra) passa por i18n, nunca fixo no código. Idioma principal: pt-BR. Outro idioma entra como tradução só por decisão da pessoa.
- App novo nasce com a camada de i18n. Sistema com i18n nativo usa a dele.

## Genérico

- Código sai o mais genérico possível, mesmo para uma demanda específica. Nada de caminho, disco, título, lista de faixas ou parâmetro da demanda fixo no código: entra por argumento ou por `config.json`.
- Um script por tarefa, não um por caso. Nomes como `script_2.py` ou `script_<pessoa>.py` são o contraexemplo.
- `config.json`, na pasta do código, guarda a informação da demanda. Quando existe, o script lê os padrões dele, e os argumentos de linha de comando sobrepõem. O que muda por caso fica no arquivo de dados; o código não muda. Isso facilita migrar o código para algo mais definitivo.

## Escrever

- Script novo: antes de escrever do zero, procurar se já existe um que faz a mesma coisa ou algo perto, e estender esse em vez de duplicar.
- Script que nasce numa sessão e pode servir de novo fica na pasta de scripts do repo, não solto em diretório temporário.
- Pasta de scripts tem um README com uma linha por pasta. Pasta nova, renomeada ou removida atualiza o README no mesmo passo.
