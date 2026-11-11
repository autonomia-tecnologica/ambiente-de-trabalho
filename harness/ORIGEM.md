# Origem

Tabela regra de origem → destino. Cada regra das fontes atuais aparece uma vez.

Os caminhos `regras/*.md` e `CLAUDE.md` citados abaixo são históricos: o destino atual de toda
regra é o `HARNESS.md`; a tabela preserva de onde cada uma veio.

Fontes (rótulos, sem caminho):

- `global`: o CLAUDE.md do usuário, para qualquer repo.
- `instância`: o CLAUDE.md da pasta de trabalho pessoal.
- `Harness/<arquivo>`: os arquivos de regra da pasta de trabalho pessoal.
- `hook`: a configuração de hooks de sessão e o script que a executa.

Situações:

- **mantida**: vai para o harness, com o mesmo sentido (pode estar generalizada).
- **movida**: vai para outro lugar do harness, ou vira o padrão de entrada (`HARNESS.md`).
- **mecanismo**: é como o hook faz valer a regra; não mora aqui (ver `HARNESS.md`, seção Harness).
- **cortada**: sai por ser histórico, já aposentado ou duplicado.
- **pessoal**: fica fora deste repositório, com quem usa o ambiente.

| id | origem | regra (resumo) | situação | destino | nota |
|---|---|---|---|---|---|
| R01 | global, Conversa | "você" fala da entidade IA e do harness, não da sessão; decisões e entregas anteriores são do conjunto | mantida | `CLAUDE.md` (Conversa); `regras/conversation.md` (Decisão é do conjunto) | generalizada: o sujeito "você" virou "a pessoa" |
| R02 | global, Conversa | "workspace" é a instância pessoal do ai-tmux-workspace e os repos que ela gere | pessoal | fica fora | define um termo da pessoa; o papel "organiza" está em `CLAUDE.md` (Papéis) |
| R03 | global, Conversa | dúvida para a pessoa é enquete, opções objetivas, recomendada primeiro, uma dúvida por pergunta | mantida | `CLAUDE.md` (Conversa); `regras/conversation.md` | liga-se a R18 |
| R04 | global, Código | identificadores, arquivos, pastas, comentários, logs, documentação, testes e commits em en-US | mantida | `CLAUDE.md` (Código); `regras/code.md` (Escopo e Idioma) | vale só para repositório de código; a pasta principal da pessoa fica fora (D2). O texto de regra deste repo é pt-BR |
| R05 | global, Código | texto que a pessoa final lê passa por i18n; pt-BR principal; app novo nasce com i18n | mantida | `CLAUDE.md` (Código); `regras/code.md` (Idioma) | |
| R06 | global, Regras de agente | regras de agente ficam num arquivo próprio; repo fora da pasta de trabalho lê esse arquivo antes de mexer em backlog ou commit | movida | `CLAUDE.md` (topo e Backlog/Commits); `regras/harness.md` (último item) | o caminho do arquivo de origem não entra |
| R07 | instância, topo | esta pasta é a instância: os dados de quem a usa | pessoal | fica fora | descreve a pasta da pessoa |
| R08 | instância, leitura | ordem de leitura: README, depois `Harness/` começando pela conduta de sessão, depois o backlog da raiz | movida | `CLAUDE.md` (ponto de entrada) | o padrão vira a entrada do harness; a lista de arquivos da pessoa não entra |
| R09 | instância, repos externos | repos de código fora da pasta são geridos pelo workspace | pessoal | fica fora | a lista de repos e perfis é setup da pessoa; o papel "organiza" está em `CLAUDE.md` |
| R10 | instância, regras gerais | regras gerais de código ficam no CLAUDE.md global | movida | `CLAUDE.md` deste repo | |
| R11 | Harness/README | índice dos arquivos da pasta pessoal | pessoal | fica fora | o padrão de índice é `CLAUDE.md` |
| R12 | Harness/README | "Onde eu encosto": a pasta pessoal só lê outras como dado; a regra que governa é a da própria instância | pessoal | fica fora | o princípio de dado vs regra está em `regras/harness.md` |
| R13 | Harness/como-conduzir | um item por vez, sem parede de resposta | mantida | `regras/conversation.md` (Um item por vez) | |
| R14 | Harness/como-conduzir | o backlog é o fio da meada: decisão gravada na hora, ler antes de perguntar, deixar o aberto, tirar o fechado | mantida | `regras/backlog.md` (Fio da meada) | |
| R15 | Harness/como-conduzir | o "estoque da sessão" do README foi aposentado | cortada | fica fora | histórico |
| R16 | Harness/como-conduzir | o backlog do produto é um arquivo no produto, não tarefas na pasta pessoal | pessoal | fica fora | caminho e decisão do produto da pessoa |
| R17 | Harness/como-conduzir | "remover" é o comando de exclusão real do produto, não cancelar | pessoal | fica fora | comando de produto específico |
| R18 | Harness/como-conduzir | pergunta à pessoa é sempre enquete, nunca texto no prompt | mantida | `regras/conversation.md` (Dúvida é enquete) | repete R03 |
| R19 | Harness/como-conduzir | o harness guarda só o estável; log, medição, diário e estado da máquina são dado | mantida | `regras/harness.md` (primeiro item) | os lugares de dado da pessoa ficam fora |
| R20 | Harness/como-conduzir | tudo é arquivo comum; só o CLAUDE.md é da ferramenta; regra que só funciona numa ferramenta não é regra da pasta | mantida | `regras/harness.md` (Tudo é arquivo comum; Exceção) | |
| R21 | Harness/como-conduzir | comando para a pessoa rodar: uma linha, encadeada | mantida | `regras/conversation.md` (Comando para a pessoa); `CLAUDE.md` (Conversa) | preferência virou regra geral do ambiente |
| R22 | Harness/como-conduzir | comando de root usa `pkexec`, não `sudo`, porque a sessão não tem a senha e o popup é que confirma | mantida | `regras/conversation.md`; `CLAUDE.md`; forçada em `forcado.yaml` (F03) | |
| R23 | Harness/commit-e-backup | git local é histórico; backup é HD e nuvem, com script próprio; o remoto foi deletado | pessoal | fica fora | infra de backup da pessoa |
| R24 | Harness/commit-e-backup | scan de segredos antes de todo commit; achado diz onde, nunca o valor; arquivo ignorado não viaja mas segredo em claro fica fora | mantida | `regras/commits.md`; `CLAUDE.md` (Commits); forçada (F01) | o comando de scan da ferramenta da pessoa não entra |
| R25 | Harness/regras-de-agente | backlog é `BACKLOG.md` na raiz, sem outro caminho; repo sem ele não tem tarefas; não criar vazio | mantida | `regras/backlog.md` (Arquivo) | |
| R26 | Harness/regras-de-agente | `BACKLOG.md` nunca commitado: `/BACKLOG.md` no `.gitignore` | mantida | `regras/backlog.md`; forçada (F04) | |
| R27 | Harness/regras-de-agente | backlog escrito em pt-BR, em todo repo | mantida | `regras/backlog.md` (Arquivo) | |
| R28 | Harness/regras-de-agente | só o que está aberto; item feito sai ao fechar a fatia | mantida | `regras/backlog.md` (Arquivo) | |
| R29 | Harness/regras-de-agente | id inteiro, incremental, sem prefixo; topo guarda o próximo id; id nunca se reusa | mantida | `regras/backlog.md` (Arquivo) | |
| R30 | Harness/regras-de-agente | três seções na ordem `Refinadas`, `A refinar`, `A validar` | mantida | `regras/backlog.md` (Arquivo) | |
| R31 | Harness/regras-de-agente | `Categoria` obrigatória, reaproveitada dentro do repo; o painel agrupa | mantida | `regras/backlog.md` (Categoria e dependência) | |
| R32 | Harness/regras-de-agente | `Depende de` só entre ids da mesma categoria; ciclo é erro de formato | mantida | `regras/backlog.md` (Categoria e dependência) | |
| R33 | Harness/regras-de-agente | formato dos itens de `Refinadas` e `A refinar` (quatro campos); de `A validar` (Conferir, Como, Decide) | mantida | `regras/backlog.md` (blocos de formato) | |
| R34 | Harness/regras-de-agente | refinada é item com categoria, quatro campos e nenhuma pergunta aberta; só refinado é despachado | mantida | `regras/backlog.md` (Refinada e despacho) | |
| R35 | Harness/regras-de-agente | tarefa nova: não refinar nem perguntar antes de salvar; dúvida vai para `A refinar` sem enquete; só refina quando avisado | mantida | `regras/backlog.md` (Refinada e despacho) | |
| R36 | Harness/regras-de-agente | `A validar` é só o que a pessoa confere; nunca despachado; entra com a entrega e sai quando validado | mantida | `regras/backlog.md` (A validar) | |
| R37 | Harness/regras-de-agente | fatia de no máximo 65k de contexto; se não cabe, quebra em itens com id novo | mantida | `regras/backlog.md` (Tamanho) | |
| R38 | Harness/regras-de-agente | item curto, em torno de 15 linhas; decisão e histórico não moram nele | mantida | `regras/backlog.md` (Tamanho) | |
| R39 | Harness/regras-de-agente | `(PAUSADA)` no título tira o item das prontas | mantida | `regras/backlog.md` (Tamanho) | |
| R40 | Harness/regras-de-agente | papéis: refinar é modelo forte em conversa; implementar fatia é de subagente de escopo fechado, que não commita nem fecha, e devolve a pergunta se for ambíguo | mantida | `regras/agents.md` (Papéis) | o nome do subagente da pessoa não entra |
| R41 | Harness/regras-de-agente | ao despachar, a descrição do subagente é `implementador-<id>` | mantida | `regras/agents.md` (Papéis) | generalizada: é convenção de nome de execução |
| R42 | Harness/regras-de-agente | fechar é da sessão que despachou: confere, tira do backlog, commita | mantida | `regras/agents.md`; `CLAUDE.md` (Agentes) | |
| R43 | Harness/regras-de-agente | revisão profunda só quando a pessoa pedir, nunca por fatia | mantida | `regras/agents.md` (Papéis); `CLAUDE.md` (Agentes) | |
| R44 | Harness/regras-de-agente | despacho: a pessoa diz o modo; a sessão não adivinha | mantida | `regras/agents.md` (Despacho) | |
| R45 | Harness/regras-de-agente | modo uma fatia: implementa, fecha e para; é o padrão | mantida | `regras/agents.md` (Despacho) | |
| R46 | Harness/regras-de-agente | modo tudo refinado: paralelo só com `Caminhos` disjuntos e sem `Depende de` entre si; um worktree e uma branch por fatia; um integrador mescla uma por vez e roda a validação a cada merge; worker não resolve conflito | mantida | `regras/agents.md` (Despacho) | |
| R47 | Harness/regras-de-agente | mensagem de commit: última linha `AI Generated`, sem coautoria; checada por um hook global de commit | mantida | `regras/commits.md`; forçada (F02) | o hook global é mecanismo; o valor da linha é do ambiente |
| R48 | Harness/regras-de-agente | worktree não é sandbox; o limite são as permissões do agente | mantida | `regras/agents.md` (Worktrees); `CLAUDE.md` | |
| R49 | Harness/regras-de-agente | worktree novo não traz `.env` nem dependências; o repo tem passo de preparação | mantida | `regras/agents.md` (Worktrees) | |
| R50 | Harness/regras-de-agente | a pessoa não cria worktrees; quem cria remove, só o que criou | mantida | `regras/agents.md` (Worktrees) | |
| R51 | Harness/regras-de-agente | custo: ao fechar, registrar tokens e retrabalho no corpo do commit | mantida | `regras/agents.md` (Custo); `CLAUDE.md` (Commits) | |
| R52 | Harness/codigos | tudo em inglês em pastas e arquivos de código, em todos os níveis; o texto que o script mostra e os comentários podiam ficar em português | mantida | `regras/code.md` (Idioma) | a parte de nomes entra; a exceção de comentário em português foi cortada (conflita com R04, D2); o texto mostrado segue R05 |
| R53 | Harness/codigos | código o mais genérico possível; nada de caminho, disco, título ou parâmetro fixo; um script por tarefa | mantida | `regras/code.md` (Genérico) | |
| R54 | Harness/codigos | dados da demanda em `config.json` na pasta do código; argumentos de linha de comando sobrepõem | mantida | `regras/code.md` (Genérico) | |
| R55 | Harness/codigos | a pasta de scripts tem README com uma linha por pasta; mudança atualiza no mesmo passo | mantida | `regras/code.md` (Escrever) | generalizada: vale para a pasta de scripts de qualquer repo |
| R56 | Harness/repositorios-externos | gestão dos repos externos pelo workspace; lista e perfis em arquivo de configuração | pessoal | fica fora | setup da pessoa; o papel fica em R09 |
| R57 | Harness/repositorios-externos | regras gerais de código e conversa no global; backlog, commit, despacho e worktree no arquivo de regras de agente | movida | `CLAUDE.md`; `regras/` | |
| R58 | Harness/repositorios-externos | a pasta pessoal só tem o que governa o trabalho nela; o que acontece em repos externos entra como dado | mantida | `regras/harness.md` | |
| R59 | Harness/reportar-bug | a pasta pessoal usa o produto: não conserta nele; reporta no backlog do produto; não abre o produto para mexer | pessoal | fica fora | fluxo com o produto da pessoa |
| R60 | Harness/reportar-bug | formato de bug (título, prioridade, evidência, reprodução) | pessoal | fica fora | formato do produto da pessoa |
| R61 | Harness/segredos | segredo em claro não mora na pasta, nem ignorado; cifrado dentro ou em claro fora; estado derivado vai para cache | mantida | `regras/secrets.md` | os caminhos e a ferramenta de cifra são exemplos genéricos |
| R62 | Harness/segredos | segredo novo entra pelo popup; a sessão não pede para colar; o valor não passa pela sessão, argv nem histórico | mantida | `regras/secrets.md` (Segredo novo entra por canal seguro) | o script e o popup da pessoa ficam fora (mecanismo pessoal) |
| R63 | Harness/infra | toda definição de infra em pasta por stack; nada de arquivo solto | mantida | `regras/infra.md` | caminhos da pessoa ficam fora |
| R64 | Harness/infra | `.env` com segredo fora do repo, recebido por `--env-file` | mantida | `regras/infra.md`; `regras/secrets.md` | |
| R65 | Harness/infra | dado grande fica onde faz sentido; só a definição vem para o repo | mantida | `regras/infra.md` | |
| R66 | Harness/infra | imagem com tag exata; nunca `latest`, `release` nem tag de major; digest é bem-vindo; tag que quebra se testa antes de fixar | mantida | `regras/infra.md`; forçada (F06) | |
| R67 | Harness/infra | histórico da migração de stacks (datas e origens) | cortada | fica fora | registro de migração, não regra |
| R68 | Harness/biblioteca-e-scripts | a biblioteca de dados é lida por demanda, nunca inteira | pessoal | fica fora | regra do dado da pessoa |
| R69 | Harness/biblioteca-e-scripts | nome de pasta dentro da biblioteca é capitalizado | pessoal | fica fora | padrão da biblioteca da pessoa |
| R70 | Harness/biblioteca-e-scripts | script novo: olhar antes se já existe um; script que pode servir fica na pasta de scripts, não em diretório temporário | mantida | `regras/code.md` (Escrever) | |
| R71 | Harness/biblioteca-e-scripts | acervo de música: arquivos por faixa e nomes de pasta | pessoal | fica fora | domínio pessoal |
| R72 | Harness/biblioteca-e-scripts | para entrar na biblioteca, a música precisa de áudio, capa e gênero | pessoal | fica fora | domínio pessoal |
| R73 | Harness/maquina-e-disco | serviço systemd é de usuário por padrão; sem serviço de sistema sem pedido; linger se precisar | mantida | `regras/operation.md` | |
| R74 | Harness/maquina-e-disco | root pequeno: download e dado pesado vão para o disco de dados; conferir espaço antes de baixar | mantida | `regras/operation.md`; `regras/infra.md` | os caminhos da máquina ficam fora |
| R75 | Harness/maquina-e-disco | antes de criar pasta, ver se já existe uma | mantida | `regras/operation.md` | |
| R76 | Harness/maquina-e-disco | modelos de LLM num lugar só, com links simbólicos dos caminhos antigos | pessoal | fica fora | decisão de máquina da pessoa |
| R77 | Harness/maquina-e-disco | ferramenta de dev pesada num diretório de config do disco de dados, com link | pessoal | fica fora | decisão de máquina da pessoa |
| R78 | Harness/maquina-e-disco | job longo com limite de memória, nunca `nohup` ou `setsid`; sem swap | mantida | `regras/operation.md` | a decisão "sem swap" é da máquina e fica fora |
| R79 | Harness/teste-de-karaoke | o padrão de teste de um projeto de karaokê | pessoal | fica fora | domínio pessoal |
| R80 | Harness/rip-de-disco | rip copia o disco inteiro antes de converter; formato de guarda de áudio | pessoal | fica fora | domínio de mídia da pessoa |
| R81 | Harness/capa-de-musica | capa é arquivo separado ao lado do áudio | pessoal | fica fora | domínio de mídia |
| R82 | Harness/legenda-e-bazarr | legenda é de um gerenciador, o servidor só lê; regra de filme com idioma estrangeiro | pessoal | fica fora | domínio de mídia da pessoa |
| R83 | Harness/karaoke-modo-festa | procedimento de pôr música num aparelho, com letra vinda da pessoa | pessoal | fica fora | domínio pessoal |
| R84 | Harness/identificar-dvd-coletanea | identificar faixas de disco pelo áudio, sem OCR; regra de canal de áudio | pessoal | fica fora | domínio de mídia |
| R85 | Harness/playlist-de-disco | playlist de disco nasce não listada e sem colaboradores | pessoal | fica fora | domínio pessoal |
| R86 | Harness/karaoke-cdg-na-biblioteca | trazer arquivos de karaokê de uma fonte, ignorando o que já existe | pessoal | fica fora | domínio pessoal |
| R87 | Harness/(arquivo de pipeline de vídeo) | pipeline de vídeo: vinheta gerada nos parâmetros do vídeo; identidade visual num lugar só | pessoal | fica fora | produto e identidade da pessoa |
| R88 | Harness/pipeline-de-conteudo | pipeline de conteúdo: o texto é da pessoa; a LLM só corrige ortografia e divide parágrafos | pessoal | fica fora | autoria da pessoa |
| R89 | Harness/nova-reflexao | reflexão vira nota, nunca tarefa | pessoal | fica fora | fluxo da pessoa |
| R90 | Harness/metodologia | repo de livro: a IA não escreve o texto; commit só da pessoa | pessoal | fica fora | regra de um repo específico |
| R91 | Harness/creditos-e-licencas | licenças e créditos dos repos da pessoa e da identidade visual | pessoal | fica fora | decisão de licença da pessoa; a regra de terceiros é de cada repo |
| R92 | hook, sessão | o hook de início de sessão injeta o harness no contexto | mecanismo | fica fora (item de mecanismo) | |
| R93 | hook, a cada mensagem | o hook injeta um lembrete de leitura do harness em cada mensagem | mecanismo | fica fora (item de mecanismo) | |
| R94 | hook, leitura | o hook marca que um arquivo do harness foi lido na sessão | mecanismo | fica fora (item de mecanismo) | |
| R95 | hook, escrita | o hook nega escrita na pasta até o harness ser lido na sessão; isento para subagentes | movida | `forcado.yaml` (F07, como lembrete) | ver D1 |
| R96 | hook, geral | qualquer erro no hook libera a ação (falha aberta) | movida | `forcado.yaml` (F07, campo `falha`) | ver D1 |

## Decisões registradas

- **D1. O gate de leitura é lembrete, não segurança.** O hook atual nega Write, Edit e NotebookEdit até o harness ser lido, falha aberta em qualquer erro, e não cobre Bash nem outras formas de escrita. A decisão é mantê-lo como lembrete (`tipo: lembrete` em F07), sem apresentá-lo como barreira. A segurança fica nos itens `hook` e `deny` de `forcado.yaml`, a implementar fora deste repositório.
- **D2. Idioma em repositório de código: inglês (en-US), decidido pela pessoa.** Vale para identificadores, nomes de arquivo e pasta, comentários, logs, documentação, nomes de teste e mensagens de commit, em todo repositório de código. A pasta principal da pessoa (sua pasta de trabalho) não é repositório de código: a regra não se aplica a ela, e quem a gerencia trata o idioma dela. A exceção de R52 (comentário em português) sai.
- **D3. Papéis.** A LLM executa, o workspace organiza (backlog, painel, repos), este ambiente regra. O mecanismo (hook, configuração) não mora aqui.
- **D4. Idioma.** Texto de regra em pt-BR, que é o idioma da pessoa. Nomes de arquivo em inglês quando isso faz sentido; a pasta `regras/` segue o nome pedido.
- **D5. Quem usa é "a pessoa".** Nenhuma regra deste repositório nomeia quem a escreveu nem traz dado pessoal dele.
- **D6. Itens forçados decididos.** F03 (`sudo` negado, pkexec no lugar) e F05 (segredo em claro fora do repo, com exceção para modelos `.example`) foram confirmados pela pessoa e marcados `status: decidida` em `forcado.yaml`.
- **D7. Chave de acesso nunca vive dentro do ambiente, decidido pela pessoa.** O workspace pode gerar e gerenciar as settings do agente para a pessoa, exceto as chaves (API, token de provedor): essas nunca moram no ambiente, e as settings só apontam para onde estão. Regra em `regras/secrets.md` e `forcado.yaml` (F08).
- **D8. Modo automático só em subagente, decidido pela pessoa.** A sessão principal fica sempre em `accept edits`; ação não monitorada roda em subagente delimitado, com escopo fechado. Permissão se concede a um comando concreto por vez, nunca a um guarda-chuva que libera outros comandos (`Bash`, interpretador inteiro). Regra em `regras/agents.md`; resumo em `CLAUDE.md`. **Substituída por D10.**
- **D9. Perfil não-Claude sobe com o tool search desligado, por compatibilidade.** Enquanto a ferramenta de harness não for trocada, todo perfil do workspace cujo nome não seja `claude` sobe com `ENABLE_TOOL_SEARCH=false`. O Claude Code grava o estado do tool search no transcript (`tool_addition` com `tool_definition`), e endpoints compatíveis com a Anthropic de outros provedores recusam esse variant (422 em `messages[1].content`), quebrando o `--continue`; desligado por padrão, a sessão nova já nasce compatível. Não é barreira de segurança e não muda o perfil `claude`. A decisão é revista quando o at-harness substituir a ferramenta de harness ou quando o provedor aceitar os blocos.
- **D10. Modo de permissão: `acceptEdits` na primeira vez, último modo salvo por perfil, `auto` só no Claude e nunca aplicado na troca (substitui D8).** A primeira abertura de um perfil usa `acceptEdits`. Se a pessoa mudar o modo, o workspace salva o último modo daquele perfil e o reaplica nas aberturas seguintes do mesmo perfil. Na troca de perfil, o modo salvo não é aplicado de primeira: o perfil novo abre em `acceptEdits`, e a pessoa decide o resto. Perfil de API externa nunca recebe o modo salvo de outro perfil na primeira abertura. `auto` só existe no perfil `claude`, porque o classifier é um modelo Claude; em perfil não-Claude ele nunca é aplicado, nem quando veio salvo do `claude`. `bypassPermissions` só por escolha da pessoa, com o aviso de segurança; `deny` vale em todo modo e o sandbox delimita o Bash. No bypass, `allow` não tem efeito. Fora do bypass, permissão continua sendo de um comando concreto, nunca de um guarda-chuva (`Bash`, interpretador inteiro). Regra em `regras/agents.md`; resumo em `CLAUDE.md`. **Substituída por D11 na parte de modo e permissão; o resto continua válido enquanto não houver caixa de SO.**
- **D11. Isolamento por sessão e política única de capacidades, decidido pela pessoa (substitui D10 na parte de modo e permissão).** Tudo negado por padrão; a LLM roda com usuário próprio, separado da pessoa; sessão de repo só altera o próprio repo e `/tmp`; a home altera as outras; configuração da máquina é da home; permissão por forma de uso (capacidade), nunca por comando solto; a mesma decisão no Claude e no OpenCode; `auto` só dentro da caixa de SO; pedido de execução vindo de outra sessão exige aprovação clara da pessoa. Regra em `regras/isolation.md` e `docs/pt_BR/filosofia.md` (tópico 4); implementação no `ai-tmux-workspace`, itens 63–66. Enquanto não houver caixa de SO, a política de harness é atrito declarado, não garantia.
- **D12. Todo efeito tem um dono único, decidido pela pessoa.** Desenhar na tela, chamar `tmux`, escrever arquivo e formatar caminho têm um dono no código; o resto pede a ele. A pergunta é "quem pode produzir este efeito?", não "este código é igual?". O freio é teste de arquitetura com allowlist por efeito, que só encolhe: arquivo novo que emita o efeito reprova. DRY não serve de freio porque é regra de forma, não é falsificável. A regra mora no harness adicional do projeto (`harness/choke-point.md`, pelo `README.md` da pasta), não no `HARNESS.md` geral: o geral só declara que o projeto pode ter harness próprio. Dívida real: `src/viewer.py` foi feito dono do desenho, mas `panel_tabs.py`, `session_placeholder.py`, `mic.py` e `conference.py` abriram laços próprios; a exceção documentada de `session_placeholder.py` (não importa `lib`/`tmux` para sobreviver ao reload) fica nomeada, não vira segundo dono silencioso.
