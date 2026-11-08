# Harness

Regras sobre o próprio harness.

- **Guarda o que é estável e governa o trabalho:** regra, procedimento, preferência.
- **Nunca guarda** log de debug, medição, diário de investigação nem estado da máquina. Isso é dado, e mora fora do harness. Teste: se amanhã pode estar errado porque algo mudou, não é harness.
- **Tudo é arquivo comum** (markdown, jsonl, yaml). Uma sessão de outra ferramenta lê tudo e continua o trabalho; nada se perde por trocar de agente.
- **Exceção:** o `CLAUDE.md`, o único arquivo que existe por causa de uma ferramenta. Por isso ele aponta para o resto, em vez de carregar regra.
- Regra que só funciona dentro de uma ferramenta não é regra do harness: tem de valer para quem abrir a pasta.
- O que é pessoal de quem usa o ambiente mora fora deste repositório.
- **Mecanismo** (hook, permissão, configuração de ferramenta) não mora aqui. `forcado.yaml` declara o que deve virar bloqueio; a implementação fica fora deste repositório.
- Regra que nasce num repo e vale para os outros sobe para cá. Repo que precisa contrariar uma regra daqui diz a contradição e o motivo, nunca em silêncio.
- Repo fora desta pasta lê estas regras antes de mexer em backlog ou commit.
