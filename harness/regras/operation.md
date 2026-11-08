# Operação

- **Antes de criar pasta, ver se já existe uma** (`ls` ou `find` no disco de destino) e usar a que está lá.
- **Antes de baixar algo grande, conferir o espaço livre** (`df -h`). O disco do sistema é pequeno: download, build, cache grande e dado pesado vão para o disco de dados, não para o do sistema.
- **Serviço novo é de usuário** (`systemd --user`) por padrão. Serviço de sistema só com pedido explícito. Se precisar rodar sem sessão de login ativa, habilitar linger, não subir como serviço de sistema.
- **Job longo roda com limite de memória** (`systemd-run --user -p MemoryMax=...`), não com `nohup` ou `setsid`. Sem swap, o OOM pode matar o cgroup do terminal inteiro, com as sessões dentro.
- **Comando de root:** `pkexec`, nunca `sudo` (ver `conversation.md`).
