# Filosofia do ambiente de trabalho

> Texto de referência, escrito primariamente para ser lido por LLMs (agentes e subagentes), e legível por humanos. Cada tópico é uma regra ou premissa; em conflito, vale a precedência do tópico 12.

## 1. O que é o ambiente

- Terminal e navegador são a casca: é por eles que se lê, escreve e age.
- Programas gráficos entram para ver: PDF, imagem, o que pede olho.
- O centro é o que a pessoa lê, escreve e entende; o resto é janela.
- Mora sob o guarda-chuva Autonomia Tecnológica; este repo estrutura a parte do ambiente.

## 2. O que une a tríade

- **AT**: o core, o motor de tarefas, o "sistema operacional da pessoa".
- **Produtividade**: a home da pessoa, a Pasta de Trabalho com os dados dela.
- **ai-tmux-workspace**: onde se constrói: repositórios, aplicações, código determinístico.
- Os três seguem este texto; cada um cita, nenhum repete.

## 3. Core único e interfaces

- Um core só, o AT: nele moram as regras, os dados e as decisões.
- WebApp, `cli` e plugin tmux são interfaces, cada uma com suas particularidades.
- Interface pede ao core; não guarda uma segunda verdade nem decide sozinha.
- O site compartilha identidade e filosofia, mas não fala com o AT.

## 4. Segurança

- Segurança é premissa, não opcional: o padrão é o seguro.
- Menos seguro é escolha informada, com o preço dito antes; quem escolhe é a pessoa.
- Menor privilégio para pessoa, programa e agente; rastro de toda ação; segredo nunca em claro.
- O que não se mede se declara, em vez de virar promessa.

## 5. Dados da pessoa

- Os dados são da pessoa e moram numa pasta que ela escolhe.
- A cópia dessa pasta basta para tudo voltar.
- Repositório é produto: não carrega dado pessoal nem nomeia a instância de ninguém.

## 6. Determinístico e LLM

- **LLM na construção, determinístico na execução.**
- A LLM ajuda a escrever código, vocabulário e scripts; o que roda todo dia é código que faz sempre a mesma coisa.
- Modelo na execução é sempre opcional: sem ele o determinístico funciona inteiro; com ele, propõe e o motor executa; escrita pede confirmação.
- Modelo remoto (o Jev) é aceito nesses termos: opcional e desligável.

## 7. Este texto

- O público principal é a LLM: o texto é direto, em tópicos, e pode ser gerado ou revisado por LLM.
- Humanos podem ler, mas não são a medida de forma nem de estilo aqui.
- Quem gera ou altera este texto preserva as decisões do Rafael; na dúvida, pergunta.

## 8. A pessoa decide

- A máquina propõe, mostra o custo e espera; a pessoa decide.
- Agente não aprova a si mesmo; sem ninguém para responder, nada se assume.
- Na dúvida entre decidir e perguntar, pergunta.

## 9. Superfície mínima

- A superfície (rede, processo, porta, dependência) é a mínima possível.
- Superfície aceita conta como o mínimo possível: aceitar é decisão consciente, e dita.
- O que não foi aceito explicitamente não entra.

## 10. Pouco e robusto

- Dependência é decisão com um porquê: pinada e verificável.
- Robustez e simplicidade antes da novidade; nada experimental.
- Reescrever a roda vale quando a roda de fora custa autonomia.

## 11. Para quem

- Para o Rafael primeiro: pensado para o uso dele, honesto sobre o que serve.
- Aberto a quem não programa depois.
- pt-BR primeiro.

## 12. Precedência

- Filosofia (este texto) > documento fundador do AT > `CLAUDE.md`.
- Em conflito, vale o de cima; o de baixo se corrige.

## 13. Como cada repo cita este texto

- O manifesto da Autonomia Tecnológica mora no site (`site/i18n/pt_BR.json`), que é a fonte primária dele; este texto o cita e não o repete.
- Cada repo cita este texto numa linha (README ou documento fundador) e não o repete.
- Regra que nasce num repo e vale para os outros sobe para cá.
- Repo que precisa contrariar uma regra daqui diz a contradição e o porquê; nunca em silêncio.
