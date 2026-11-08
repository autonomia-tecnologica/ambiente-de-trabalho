# Segredos

- **Segredo em claro não mora na pasta de trabalho, nem ignorado pelo git.** Ele vai para um de dois lugares: cifrado dentro do repo (por exemplo com sops), ou em claro fora dele, em diretório de configuração do usuário.
- **Estado derivado** (perfil de navegador, cache) não se versiona. Mora em diretório de cache.
- **Segredo novo entra por canal seguro**: popup de entrada ou cofre cifrado. A sessão não pede para a pessoa colar segredo no chat, não passa segredo em variável de ambiente na linha de comando e não grava segredo em arquivo em claro.
- O valor não passa pela sessão, nem por argumento de processo, nem pelo histórico do shell. Chave que já existe pode ser reescrita pelo mesmo caminho.
