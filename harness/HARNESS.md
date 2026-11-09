# HARNESS.md

Regras que governam quem trabalha no ambiente: a pessoa e a LLM. Este é o arquivo geral e único do
harness; as categorias abaixo são as seções dele. A parte forçada está em `forcado.yaml`; a origem
de cada regra, em `ORIGEM.md`. Os termos usados aqui estão no Glossário, no fim.

## Por que separar contextos

O ambiente existe para dar controle sobre a LLM, não para juntar tudo num contexto só. Confusão de
LLM é um problema real: quando as fronteiras somem, o custo sobe (mais tokens, mais retrabalho), a
segurança cai (regra e permissão se misturam) e a interação fica menos significativa (o agente
perde o foco).

Por isso o ambiente separa contextos por repositório, por sessão e por ferramenta. O `HARNESS.md` é
a camada geral; cada repositório tem o seu contexto; a home é o cliente que atravessa os repos
quando faz sentido. Não se trata o harness como um bolo só: regra específica fica no contexto
específico, regra geral sobe para cá, e o freio fica na segurança.

Definição do projeto: **um ambiente de trabalho para sentir que se tem o controle da LLM, com
interações mais significativas sem abrir mão da segurança.**

## O que é harness

- **Harness é a camada de texto:** regra escrita para a LLM seguir. É uma tentativa de controle, não
  uma garantia; a LLM pode ignorá-la.
- **Freio determinístico não é harness.** Allowlist, hook, política, sandbox e usuário de SO são
  features de segurança, regidas em "Isolamento e segurança".
- **O adversário do freio determinístico é a própria LLM**, intencional ou não. Por isso o que
  precisa valer é código, não recomendação.
- **Camadas:** este harness é a regra master, comum a Claude e OpenCode; o workspace implementa o
  ambiente e este arquivo aponta para o código; o que é pessoal de quem usa fica na main
  (`~/Codes/home`). Nenhuma camada repete a de cima; em conflito, vale a de cima.
- **Precedência:** nesta fase, nenhum harness da home ou de repo se sobrepõe ao `HARNESS.md`; o
  harness local só aponta ou detalha, nunca contradiz. O que pode se sobrepor será definido ao
  longo do tempo e registrado aqui.
- **Papéis:** a LLM executa; o workspace organiza (backlog, painel, repos); este harness regra.
  Regra que nasce num repo e vale para os outros sobe para cá. Repo que contraria uma regra daqui
  diz a contradição e o motivo, nunca em silêncio.
- **Entry points:** `CLAUDE.md` e a configuração do OpenCode só apontam para este arquivo. Quando a
  instalação existir, o harness instalado fica em
  `~/.local/share/ai-tmux-workspace/harness/HARNESS.md` e a cópia instalada é somente leitura para
  o agente.
- **Leitura do código:** o `HARNESS.md` diz o que é o ambiente e aponta para o código instalado do
  ai-tmux-workspace (em `~/.local/share/ai-tmux-workspace/`). Qualquer agente pode ler esse código
  para entender como o ambiente funciona, mas não precisa ler sempre: lê quando perceber ganho de
  integração. Os ganhos ficam explícitos aqui quando estiverem claros.
- **Antes de mexer em backlog ou commit, ler a regra correspondente.**

## Ambiente

O ambiente de trabalho é composto por **repositórios** (código) e **pastas** (arquivos livres).

- **Repositórios:** cada um é um repo de código; o workspace e a home os organizam, um por janela.
- **Pastas livres:** arquivos que não são repositório — por exemplo `Documentos`, `Imagens`,
  `Vídeos`. Ainda não estão implementadas. Quando existirem, entram na navegação do workspace sem
  virar repo, sem entrar na home e sem receber agente: a pessoa navega e age nos arquivos pelo mesmo
  mecanismo dos arquivos de repo, com outra interface. **Não se abre agente em pasta.**
- **A home é o cliente do ai-tmux-workspace:** pode ler e editar qualquer repositório do ambiente.
  O recomendado é trabalhar em cada pasta individualmente; a home serve para o que cruza repos,
  configuração e navegação.
- **Sessão de repo** escreve só no próprio repo e em `/tmp`. Para mexer em outro repo, ela manda
  mensagem ao repo irmão; não edita. O repo irmão recebe o pedido e decide na própria sessão (ver
  "Sessões").
- **Harness e skills na home:** o harness vale na home como em qualquer sessão, e a home pode
  instalar skills. A cópia instalada do `HARNESS.md` é somente leitura; a regra muda no canônico,
  em `ambiente-de-trabalho`.
- **O ambiente roda em tmux:** o workspace abre um repo por janela. O agente pode abrir telas
  separadas (`display-popup`/`new-window`) para comandos interativos ou privilegiados, acompanhar a
  saída e devolver o código de saída; não mata sessões ou janelas que não criou.
- **Comando privilegiado nunca é chamado pela sessão:** a sessão pede ao ambiente (tmux), que abre
  a tela separada e usa `pkexec`, com a pessoa autorizando. A sessão não chama `sudo` nem
  `pkexec` diretamente; ela chama o ambiente de LLM.
- **Código do workspace é caminho protegido:** sessão de repo, home e repo irmão não escrevem no
  código do ai-tmux-workspace; só a sessão cujo root é o próprio código do workspace escreve.
  Leitura é permitida.
- **A separação entre repositórios é regra de harness.** O repositório não é fronteira de
  segurança a esse nível. A trava em código (política, sandbox, usuário de SO) é feature de
  segurança e entra em degraus.

## Conversa

Regras de como a sessão conduz a conversa. Valem para a LLM e para a pessoa.

### Um item por vez

Quando há decisão para a pessoa, ela decide um item, e só então se passa ao seguinte. Não se
apresenta uma lista de vários itens para decidir de uma vez, nem se devolve relatório longo onde a
decisão some no meio. O trabalho pode ser grande; a conversa, não.

### Dúvida é enquete

- Pergunta à pessoa vai como enquete: opções objetivas, a recomendada primeiro, e a pergunta
  completa dentro da enquete, para ela não precisar voltar à mensagem anterior.
- Uma dúvida por pergunta.
- Pergunta solta no texto é difícil de responder. Não é o canal.

### Decisão é do conjunto

Decisões e entregas de sessões anteriores, de outros agentes e de itens já refinados também são da
entidade que trabalha. Não se defenda dizendo que "não foi esta sessão": responda pelo conjunto.

### Comando para a pessoa

- Quando a sessão precisa que a pessoa rode algo no terminal, entrega **um comando, numa linha**.
  Não uma sequência de passos para ela montar, nem dois blocos para colar em ordem. Encadeia com
  `&&` ou `$(...)` e devolve tudo de uma vez, pronto para colar.
- **Comando privilegiado:** a sessão não chama `sudo` nem `pkexec`. Ela pede ao ambiente para abrir
  a tela separada (tmux), com o comando completo visível; a tela usa `pkexec` e a pessoa autoriza.
  Fora do tmux, a sessão entrega o comando de uma linha para a pessoa rodar. `sudo` nunca, porque a
  sessão não tem a senha e ele trava num terminal que ninguém vai digitar.

## Código

Regras para quem escreve, move ou altera código em repositório de código.

### Escopo

- Esta regra vale só para repositório de código. A pasta principal da pessoa (sua pasta de
  trabalho) não é repositório de código: esta regra não diz nada sobre ela, e quem a gerencia trata
  o idioma dela.

### Idioma

- Em repositório de código, em inglês (en-US): nomes de pasta e de arquivo em todos os níveis,
  identificadores, comentários, logs, documentação, nomes de teste e mensagens de commit.
- Texto que a pessoa final lê (texto, rótulo, interface, o que o programa mostra) passa por i18n,
  nunca fixo no código. Idioma principal: pt-BR. Outro idioma entra como tradução só por decisão da
  pessoa.
- App novo nasce com a camada de i18n. Sistema com i18n nativo usa a dele.

### Genérico

- Código sai o mais genérico possível, mesmo para uma demanda específica. Nada de caminho, disco,
  título, lista de faixas ou parâmetro da demanda fixo no código: entra por argumento ou por
  `config.json`.
- Um script por tarefa, não um por caso. Nomes como `script_2.py` ou `script_<pessoa>.py` são o
  contraexemplo.
- `config.json`, na pasta do código, guarda a informação da demanda. Quando existe, o script lê os
  padrões dele, e os argumentos de linha de comando sobrepõem. O que muda por caso fica no arquivo
  de dados; o código não muda. Isso facilita migrar o código para algo mais definitivo.

### Escrever

- Script novo: antes de escrever do zero, procurar se já existe um que faz a mesma coisa ou algo
  perto, e estender esse em vez de duplicar.
- Script que nasce numa sessão e pode servir de novo fica na pasta de scripts do repo, não solto em
  diretório temporário.
- Pasta de scripts tem um README com uma linha por pasta. Pasta nova, renomeada ou removida atualiza
  o README no mesmo passo.

## Backlog

Regras do `BACKLOG.md`, o arquivo de tarefas de cada repo. O painel do workspace lê esse arquivo:
quem o formata segue este padrão.

### Arquivo

- `BACKLOG.md` na raiz de todo repo, sem outro caminho (nada de `TODO.md` nem `docs/BACKLOG.md`).
  Repo sem `BACKLOG.md` não tem tarefas: não se cria um vazio.
- Nunca é commitado: `/BACKLOG.md` no `.gitignore` de todo repo.
- Escrito em pt-BR, em todo repo. O código segue em inglês.
- Só o que está aberto. Fatia entregue vai para `A validar`; o item só sai do arquivo depois que a
  pessoa valida (o arquivo não é versionado).
- O topo guarda `Próximo id: N`. Id é inteiro e incremental (1, 2, 3...), sem letra, prefixo ou
  código. Item novo recebe o próximo número e o contador sobe. Id nunca se reusa, nem o de item
  feito ou removido.
- Três seções, nesta ordem: `## Refinadas`, `## A refinar` e `## A validar`.

### Fio da meada

- Decisão nova vai para o arquivo assim que aparece, sem esperar o fim da sessão.
- Antes de perguntar de novo, ler o arquivo: o que já foi decidido não se pergunta outra vez.
- O que ficou em aberto fica nele, para a próxima sessão continuar do ponto.
- O que fechou sai dele. Regra que já virou regra mora no harness, não na fila.

### Categoria e dependência

- Todo item, em qualquer seção, tem `Categoria:` obrigatória: o assunto a que a pendência pertence,
  nome curto e reaproveitado dentro do repo. O painel agrupa por categoria.
- **`Segurança`** é categoria fixa e transversal: toda pendência que reduz risco e cria ou endurece
  um freio **em código** (isolamento, permissão, credencial, escrita indevida, auditoria, policy,
  hook, sandbox, usuário de SO) fica nela, mesmo quando nasce junto de uma feature. O nome não muda
  nem é absorvido por outra categoria; é por ela que a pessoa vê o que ainda é melhoria de
  segurança.
- **`HARNESS.md`** é categoria fixa: toda pendência de regra escrita para a LLM seguir (HARNESS.md,
  entry point, shim, texto de controle) fica nela. O freio correspondente, quando existir, é item de
  `Segurança`.
- `Harness inicial` não existe mais. Instalação e ambiente vão para `Ambiente`.
- Categorias não dependem entre si. `Depende de` só lista ids da mesma categoria; dependência entre
  categorias é erro de formato.
- `Depende de:` é uma lista de ids separados por vírgula e espaço (`12, 15`), ou `—`. Item bloqueado
  é o que tem algum id listado ainda presente no arquivo. Id ausente conta como cumprido. Id maior
  ou igual ao `Próximo id` é provável erro de digitação.
- Ciclo, inclusive autodependência, é erro de formato: os itens do ciclo ficam inválidos e
  bloqueados até alguém desfazê-lo.

### Refinada e despacho

- Refinada é o item com `Categoria` e os quatro campos preenchidos e nenhuma pergunta em aberto: é
  o item **despachável**, não o item concluído. Item ambíguo fica em `A refinar`.
- **Não existe status "pronto".** Fatia entregue não é apagada nem marcada como concluída: vai para
  `A validar`, e só sai do arquivo depois que a pessoa valida.
- Item bloqueado não é despachado, mesmo em `Refinadas`.
- Ao registrar tarefa nova, não refinar por conta própria nem perguntar antes de salvar. Tarefa bem
  definida entra direto em `Refinadas`, com os quatro campos. Qualquer dúvida, por menor que seja,
  entra em `A refinar` do jeito que está, sem enquete.
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

### A validar

- Seção do que só a pessoa confere (escuta, olho, uso real): trabalho já entregue, ou decisão que
  depende de percepção humana.
- Nunca é despachado, e não entra em `Depende de`.
- Entra assim que a entrega existe (a sessão que entregou registra) e sai quando a pessoa valida.
  Existe para ela ver o que falta conferir.
- **Não existe status "pronto":** a entrega entra aqui, não é apagada nem marcada como concluída.

Cada item:

```
### 18. Título curto
- Categoria: Vídeo
- Conferir: o que a pessoa precisa ver ou ouvir, e onde
- Como: os passos ou o material (trechos, comandos, caminhos)
- Decide: o que a conferência destrava ou muda, e em qual item ou repo
```

### Tamanho

- Fatia de no máximo 65k de contexto. Se o item não cabe, quebra em itens menores, cada um com id
  novo, e o original sai.
- Item curto, em torno de 15 linhas. Decisão e histórico de investigação não moram nele.
- `(PAUSADA)` no título tira o item das despacháveis.

## Agentes

Papéis, despacho, worktrees, comunicação e custo.

### Papéis

- Refinar é trabalho de modelo forte, em conversa com a pessoa.
- Implementar fatia refinada é trabalho de um subagente de implementação. Ele lê só o item e os
  caminhos de `Caminhos`, faz só o que o item pede, roda a `Validação` e devolve curto: arquivos
  mudados, números reais, dúvidas. Não commita e não fecha a fatia. Se o item for ambíguo, para e
  devolve a pergunta.
- Ao despachar, a descrição da chamada do subagente é `implementador-<id>`, com o id do item
  (ex.: `implementador-61`), para distinguir os workers na lista.
- Fechar é da sessão que despachou: confere a validação, move o item para `A validar` (não apaga) e
  commita; o item sai do arquivo quando a pessoa valida.
- Revisão profunda só quando a pessoa pedir, nunca por fatia.

### Permissões

- A política é única e mora em "Isolamento e segurança"; vale igual no Claude e no OpenCode.
- Negado por padrão; permissão é por forma de uso (capacidade), nunca por comando solto.
- A sessão principal fica em `acceptEdits`; modo automático só em subagente delimitado, com escopo
  fechado, e dentro da caixa de SO.
- Permissão é de um comando concreto, nunca de um guarda-chuva (`Bash`, interpretador inteiro).
- `auto` é permitido dentro da caixa de SO de cada sessão; sem caixa, não é permitido.
- `bypassPermissions` só por escolha da pessoa, com o aviso de segurança; `deny` vale em todo modo.
- Ação que exige sair da caixa não roda na sessão: vai para uma função explícita, com aprovação da
  pessoa.

### Comunicação entre sessões

- Uma sessão pode mandar mensagem a outra: a um repo irmão, para pedir informação, análise ou uma
  edição; ou à home, para uma operação do ambiente (criar pasta de repo, atualizar os repos).
- **Repo não edita repo irmão.** Quem edita é a sessão do repo B, na própria caixa.
- **Toda comunicação é autorizada pela pessoa no agente que recebe**, inclusive conversa; nada é
  lido, incorporado ou executado sem essa autorização. A home só executa depois da autorização
  dela. O detalhe está em "Isolamento e segurança > Sessões".

### Despacho

A pessoa diz o modo; a sessão não adivinha.

- **Uma fatia** ("faz o 17", "próxima"): implementa uma, fecha e para. É o padrão e o mais barato.
- **Tudo que está refinado** ("toca tudo"): vários workers em paralelo. Gasta mais token no total e
  compra tempo. Só entram fatias refinadas com `Caminhos` disjuntos e sem `Depende de` entre si.
  Cada worker roda num worktree próprio, com uma branch por fatia. Um integrador só, a sessão,
  mescla uma branch por vez, a mais fundamental primeiro, e roda a `Validação` a cada merge. Worker
  não resolve conflito.

### Worktrees

- Worktree não é sandbox; o limite real são as permissões do agente.
- Worktree novo não traz `.env` nem dependências: o repo tem um passo de preparação.
- A pessoa não cria worktrees. Quem cria, remove: o agente (ou a sessão que despachou) apaga o
  worktree e a branch que ele mesmo criou, depois de mesclar ou descartar. Nunca mexe nos que não
  criou.

### Custo

Ao fechar uma fatia, se o custo estiver disponível, registrar no corpo do commit os tokens gastos e
se houve retrabalho.

## Isolamento e segurança

Premissa: `docs/pt_BR/filosofia.md`, tópico 4. Esta é a regra operacional única: o Claude e o
OpenCode implementam a mesma decisão, com a mesma política.

Segurança é o viés principal do ambiente: o ambiente de trabalho e o ai-tmux-workspace controlam a
segurança do ambiente de trabalho, com ou sem IA, por tipo de arquivo, por comando do sistema
operacional ou com o AT como ponte.

### Harness e segurança

- **Harness é texto; freio determinístico é feature de segurança.** Allowlist, hook, política,
  sandbox e usuário de SO não são harness; são o que faz a regra valer contra a LLM.
- **O adversário é a própria LLM**, intencional ou não. A regra escrita não substitui o freio; o
  freio é que dá a garantia.
- **A separação entre repos é regra de harness na primeira versão.** O repositório não protege a
  esse nível: a LLM pode desobedecer. A trava em código entra em degraus, como feature de
  segurança, sem tirar a autonomia do agente.
- **O código do ai-tmux-workspace é a exceção:** caminho protegido (ver "Ambiente"); só a sessão
  cujo root é o próprio código do workspace escreve. Leitura é permitida.

### Padrão fechado

- Tudo negado por padrão. O ambiente declara os repositórios e as pastas livres; o que não está
  declarado não é acessível. Decisão desconhecida ou ambígua falha fechada.
- Quando a caixa de SO existir, a LLM roda com usuário próprio, separado da pessoa, e a allowlist
  vale por usuário, sessão e pasta. Na primeira versão, a caixa é a política de harness (ver
  "Status"). Não existe permissão implícita.

### Caixa da sessão

- Sessão aberta num repo escreve só nesse repo e em `/tmp`. A pasta principal (home) pode escrever
  em qualquer repo do ambiente. Configuração da máquina é somente leitura para sessão de repo; quem
  altera é a home. Mudar configuração de máquina por função é trabalho futuro, fora desta regra.
- `/tmp` é a única área global liberada; o resto do sistema é leitura, e só quando a forma de uso
  exigir.
- A caixa é imposta pelo SO (usuário, ACL, namespace) quando a plataforma permitir; na primeira
  versão, a caixa é a política de harness (pasta do repo + `/tmp`, configuração read-only),
  declarada como atrito. Windows e ambiente corporativo podem não permitir usuário à parte; a
  política de harness continua sendo a caixa disponível.

### Formas de uso

- Permissão é por **forma de uso**, não por comando: ler repo, editar repo, ler git, escrever git,
  rodar teste, rodar build, rede, instalar, administrar, configuração, `/tmp`.
- Cada forma de uso tem a própria allowlist; comandos entram dentro dela, nunca soltos (`git` e
  `python` não são permissão).
- Classificador determinístico: comando desconhecido ou ambíguo cai em `ask`; sem pessoa para
  responder, em `deny`. A mesma ação tem a mesma decisão no Claude e no OpenCode.
- Só entra configuração de harness que dê para centralizar. Configuração que só existe numa
  ferramenta não vira regra do ambiente.

### Auto mode

- `auto` é permitido em qualquer harness, mas cada sessão dentro da própria caixa. Ele só dispensa
  o prompt dentro do que a caixa já permite; não amplia capacidade.
- `auto` é permitido dentro da caixa vigente: hoje a caixa é a política de harness; quando houver
  usuário/ACL, passa a ser imposta pelo SO. Sem caixa nenhuma, `auto` não é permitido.
- `bypassPermissions` só por escolha da pessoa, com o aviso de segurança; `deny` vale em todo modo.

### Sessões

- Sessão é relativa a um repo ou à home; pasta livre não recebe sessão.
- **Toda comunicação entre agentes é autorizada pela pessoa no agente que recebe.** Nenhuma
  mensagem de outra sessão é lida, incorporada ou executada sem essa autorização no recebedor; a
  conferência é iniciada e acompanhada pela pessoa.
- **Qualquer sessão pode mandar um pedido à home.** A home é quem executa operações do ambiente —
  criar pasta de repo, atualizar os repos do ambiente de trabalho — sempre depois da autorização da
  pessoa na home. Recusa, expiração ou ausência de pessoa não executa nem deixa efeito.
- A home não executa pedido de repo; repo não executa pedido da home; cada um só executa o que a
  pessoa aprovou na própria sessão.

### Rastro e degraus

- Toda ação deixa rastro: comando, forma de uso, decisão, sessão e pasta.
- Pendência que reduz risco e vira freio em código fica no `BACKLOG.md` sempre sob a categoria fixa
  `Segurança`, mesmo quando nasce dentro de outra frente: é o recorte que a pessoa usa para ver o
  que falta endurecer.
- A modelagem de ameaças é contínua; o que não se mede se declara. A segurança sobe em degraus,
  reversíveis e com teste, sem tirar a autonomia do agente.
- A implementação é gradual e em ordem: cada tarefa de segurança fecha aos poucos, para conter o
  risco da própria mudança.

### Status

- Rascunho; nada instalado ainda. A implementação mora no `ai-tmux-workspace` (itens 63–67 e
  75–84).
- A primeira versão é a caixa de harness; o usuário dedicado/ACL é endurecimento posterior (itens
  35 e 40 do `ai-tmux-workspace`), não bloqueia esta fase. Enquanto não houver SO, a caixa é atrito
  declarado, não garantia.
- Windows e ambiente corporativo podem não permitir usuário à parte; ali a caixa é a política de
  harness, com o mesmo limite declarado.

## Commits

- **Mensagem:** a última linha é `AI Generated`. Sem coautoria de ferramenta e sem rastro de
  ferramenta no texto. A checagem automática é proposta em `forcado.yaml`, não existe aqui.
- **Scan de segredos antes de todo commit**, da raiz do repo. Só se commita com o scan limpo. O scan
  vale para cada commit, inclusive o de fechar a sessão. Ele lê os arquivos como estão, sem
  respeitar `.gitignore`, e a história do git.
- **Achado do scan:** a sessão diz **onde** (regra, arquivo, linha), nunca o valor, e não abre o
  arquivo para "conferir".
- Achado em arquivo que o git ignora não viaja no commit, mas segredo em claro continua fora da
  pasta de trabalho (ver "Segredos").
- **Custo:** registrar tokens e retrabalho no corpo do commit, ver "Agentes".

## Segredos

- **Segredo em claro não mora na pasta de trabalho, nem ignorado pelo git.** Ele vai para um de dois
  lugares: cifrado dentro do repo (por exemplo com sops), ou em claro fora dele, em diretório de
  configuração do usuário.
- **Estado derivado** (perfil de navegador, cache) não se versiona. Mora em diretório de cache.
- **Segredo novo entra por canal seguro**: popup de entrada ou cofre cifrado. A sessão não pede para
  a pessoa colar segredo no chat, não passa segredo em variável de ambiente na linha de comando e
  não grava segredo em arquivo em claro.
- **Chave de acesso nunca vive dentro do ambiente.** Chave de API, token de provedor e credencial
  equivalente não moram em arquivo do ambiente: nem na pasta de trabalho, nem em repo, nem nas
  settings que o workspace gera ou gerencia para a pessoa. Essas settings podem apontar para onde a
  chave está (um cofre ou um canal externo ao ambiente), nunca conter o valor. A chave entra no
  processo só na hora de subir o agente, vinda de fora.
- O valor não passa pela sessão, nem por argumento de processo, nem pelo histórico do shell. Chave
  que já existe pode ser reescrita pelo mesmo caminho.

## Harness

Regras sobre o próprio harness.

- **Guarda o que é estável e governa o trabalho:** regra, procedimento, preferência.
- **Nunca guarda** log de debug, medição, diário de investigação nem estado da máquina. Isso é dado,
  e mora fora do harness. Teste: se amanhã pode estar errado porque algo mudou, não é harness.
- **Tudo é arquivo comum** (markdown, jsonl, yaml). Uma sessão de outra ferramenta lê tudo e
  continua o trabalho; nada se perde por trocar de agente.
- **Exceção:** o `CLAUDE.md` e a configuração do OpenCode existem por causa de uma ferramenta; por
  isso só apontam para o `HARNESS.md`, em vez de carregar regra.
- **Arquivo único:** `HARNESS.md` é o geral e único; as categorias são seções dele. Antes de
  acrescentar algo, procurar a categoria que mais se encaixa e ver se a regra já não existe.
- Regra que só funciona dentro de uma ferramenta não é regra do harness: tem de valer para quem
  abrir a pasta.
- O que é pessoal de quem usa o ambiente mora fora deste repositório.
- **Mecanismo** (hook, permissão, configuração de ferramenta) não mora aqui. `forcado.yaml` declara
  o que deve virar bloqueio; a implementação fica fora deste repositório.
- Regra que nasce num repo e vale para os outros sobe para cá. Repo que precisa contrariar uma regra
  daqui diz a contradição e o motivo, nunca em silêncio.
- Repo fora desta pasta lê estas regras antes de mexer em backlog ou commit.

## Operação e infra

### Operação

- **Antes de criar pasta, ver se já existe uma** (`ls` ou `find` no disco de destino) e usar a que
  está lá.
- **Antes de baixar algo grande, conferir o espaço livre** (`df -h`). O disco do sistema é pequeno:
  download, build, cache grande e dado pesado vão para o disco de dados, não para o do sistema.
- **Serviço novo é de usuário** (`systemd --user`) por padrão. Serviço de sistema só com pedido
  explícito. Se precisar rodar sem sessão de login ativa, habilitar linger, não subir como serviço
  de sistema.
- **Job longo roda com limite de memória** (`systemd-run --user -p MemoryMax=...`), não com `nohup`
  ou `setsid`. Sem swap, o OOM pode matar o cgroup do terminal inteiro, com as sessões dentro.
- **Comando de root:** a sessão pede ao ambiente (tmux) para abrir a tela separada; a tela usa
  `pkexec`. A sessão não chama `sudo` nem `pkexec` diretamente.

### Infra

- **Definição de infra** (por exemplo um compose) fica em pasta própria por stack, versionada. Nada
  de arquivo de definição solto fora do repo.
- **Dado pesado** (biblioteca de mídia, banco de dados, downloads) fica onde faz sentido no disco. Só
  a definição vem para o repo.
- **`.env` com segredo** fica fora do repo, em diretório de configuração (`~/.config/<stack>/.env`),
  e entra no compose por `--env-file`. A varredura de segredos lê a pasta crua, e um `.env`
  ignorado ainda aparece nela.
- **Imagem sempre com tag exata de versão** (ex.: `4.1.3`, `6.8`), para evitar que uma tag que muda
  por baixo traga código de terceiro sem aviso. Nunca `latest`, `release`, nem tag só de major
  (`caddy:2`). Digest (`@sha256:`) é bem-vindo além da tag, nunca no lugar dela.
- Tag com incompatibilidade conhecida (ex.: imagem que traz runtime de versão diferente da que a
  aplicação exige) se testa antes de fixar.

## Glossário

Termos usados neste arquivo e no ambiente. Termo novo entra aqui antes de virar regra espalhada.

- **Ambiente de trabalho** — o conjunto que a pessoa usa: repositórios (código), pastas livres, a
  home, o workspace que organiza tudo, o `HARNESS.md` e as features de segurança. Definição do
  projeto: um ambiente para sentir que se tem o controle da LLM, com interações mais significativas
  sem abrir mão da segurança.
- **Workspace** — a instância do `ai-tmux-workspace` e os repositórios que ela organiza: o programa
  que abre as janelas, o painel e o agente. É a ferramenta que organiza o ambiente, não o ambiente
  inteiro.
- **Home** — a sessão principal, cliente do workspace. Pode ler e editar qualquer repositório do
  ambiente; o recomendado é trabalhar por pasta. Não edita o código do workspace e não executa
  pedido de repo sem aprovação da pessoa.
- **Repositório** — pasta de código versionada, aberta numa janela e numa sessão. A sessão de repo
  escreve só nele e em `/tmp`.
- **Repositório irmão** — repositório declarado como relacionado no config do ambiente
  (`siblings` ou equivalente), cuja mudança pode exigir mudança no outro. A relação é declarada,
  nunca inferida. Serve para comunicação entre sessões, separação de harness/contexto e para não
  tratar tudo como um contexto único. Irmão não é permissão de escrita: cada sessão edita o próprio
  repo e pede a edição ao irmão.
- **Pasta livre** — pasta que não é repositório (por exemplo `Documentos`, `Imagens`, `Vídeos`).
  Entra na navegação e na ação em arquivos, não vira repo e não recebe agente. Ainda não está
  implementada.
- **Sessão** — instância de agente com papel e raiz, relativa a um repositório ou à home. Tem uma
  caixa e um contexto. Pasta livre não recebe sessão.
- **Caixa** — área em que a sessão pode agir. No repo: o próprio repo e `/tmp`. Na home: os repos do
  ambiente. Configuração da máquina é somente leitura para sessão de repo. Pode ser imposta pelo SO
  (usuário, ACL, namespace) ou, na primeira versão, pela política de harness; sem SO, é atrito
  declarado, não garantia.
- **Harness** — camada de texto em prosa que define o que o agente pode fazer e como conduzir o
  trabalho. É uma tentativa de controle, não uma garantia; o modelo pode ignorar. Modelos de
  fronteira costumam respeitar, mas isso não é forçado. O que força é o freio determinístico, que é
  segurança, não harness.
- **HARNESS.md** — o arquivo geral e único do harness, no `ambiente-de-trabalho`; as categorias são
  seções dele. A cópia instalada em `~/.local/share/ai-tmux-workspace/harness/HARNESS.md` é somente
  leitura para o agente. Nesta fase, nenhum harness da home ou de repo se sobrepõe a ele.
- **Entry point** — arquivo que existe por causa de uma ferramenta (`CLAUDE.md`, configuração do
  OpenCode) e só aponta para o `HARNESS.md`; não carrega regra.
- **Harness da home** — o harness específico da home/main. Nesta fase só aponta ou detalha, nunca se
  sobrepõe ao `HARNESS.md`; o que pode se sobrepor será definido com o tempo.
- **Segurança** — termo amplo para cybersecurity ou segurança tecnológica. Dentro do ambiente:
  a segurança do ambiente e a proteção dos dados de quem o usa — quem pode ler, escrever e
  executar, onde, com qual identidade e rastro. Não é o harness.
- **Freio determinístico** — mecanismo em código que faz a regra valer: allowlist, hook, política,
  sandbox, usuário de SO. É feature de segurança. O adversário é a própria LLM, intencional ou não.
- **Política** — a política central de capacidades: um motor, dois adaptadores (Claude e OpenCode);
  negado por padrão; permissão por forma de uso; a mesma decisão nos dois. Mora no workspace; a
  regra escrita correspondente está no `HARNESS.md`.
- **Forma de uso / allowlist** — a capacidade (ler repo, editar repo, ler/escrever git, rodar
  teste, rodar build, rede, instalar, administrar, configuração, `/tmp`), nunca o comando solto.
  Cada forma tem a própria allowlist; `git` ou `python` soltos não são permissão.
- **Adversário** — a própria LLM, intencional ou não. É contra ela que o freio determinístico vale;
  o harness é texto e não a contém.
- **Contexto** — o que a sessão sabe e carrega: regras, arquivos lidos, histórico. Separar contextos
  por repo e por sessão evita confusão, custo e perda de segurança.
- **Conferência** — conversa entre o repo da janela e os irmãos, cada um respondendo pelo próprio
  repo. A pessoa inicia e acompanha; toda comunicação entre agentes é autorizada por ela no agente
  que recebe.
- **Participante** — cada repo/sessão que responde numa conferência.
- **Ganho de integração** — quando ler o código instalado ou usar um recurso do ambiente compensa
  para a tarefa; não é leitura padrão. Os ganhos ficam explícitos no `HARNESS.md` quando claros.
