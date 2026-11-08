# Infra

- **Definição de infra** (por exemplo um compose) fica em pasta própria por stack, versionada. Nada de arquivo de definição solto fora do repo.
- **Dado pesado** (biblioteca de mídia, banco de dados, downloads) fica onde faz sentido no disco. Só a definição vem para o repo.
- **`.env` com segredo** fica fora do repo, em diretório de configuração (`~/.config/<stack>/.env`), e entra no compose por `--env-file`. A varredura de segredos lê a pasta crua, e um `.env` ignorado ainda aparece nela.
- **Imagem sempre com tag exata de versão** (ex.: `4.1.3`, `6.8`), para evitar que uma tag que muda por baixo traga código de terceiro sem aviso. Nunca `latest`, `release`, nem tag só de major (`caddy:2`). Digest (`@sha256:`) é bem-vindo além da tag, nunca no lugar dela.
- Tag com incompatibilidade conhecida (ex.: imagem que traz runtime de versão diferente da que a aplicação exige) se testa antes de fixar.
