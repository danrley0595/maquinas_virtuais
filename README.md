Guia de Estudos: Linux e Docker
1. Comandos Essenciais do Linux
Navegação e Visualização
`pwd`: Mostra o caminho completo do diretório atual.
`ls`: Lista arquivos e pastas.
`ls -l`: Mostra lista detalhada (permissões, tamanho, data).
`ls -la`: Mostra tudo, incluindo arquivos ocultos (que começam com `.`).
`cd`: Troca de diretório.
`cd pasta`: Entra na pasta especificada.
`cd ..`: Sobe um nível (volta para a pasta anterior).
`cd ~`: Vai direto para a pasta pessoal (home) do usuário.
`cd -`: Alterna para o último diretório em que você estava.
Criação, Cópia e Remoção
`mkdir nome`: Cria uma nova pasta.
`mkdir -p a/b/c`: Cria várias pastas uma dentro da outra de uma só vez.
`rmdir nome`: Remove uma pasta vazia.
`rm -r nome`: Remove uma pasta e todo o conteúdo dentro dela (não vai para a lixeira).
`cp -r origem destino`: Copia uma pasta e seu conteúdo para outro local.
`mv origem destino`: Move ou renomeia um arquivo/pasta.
Leitura e Texto
`echo "texto"`: Exibe uma mensagem na tela.
`cat arquivo.txt`: Lê e exibe o conteúdo de um arquivo direto no terminal.
---
2. Comandos Essenciais do Docker
Conceitos Rápidos (`run` vs `exec`)
`docker run`: Cria um novo contêiner a partir de uma imagem.
`docker exec`: Entra em um contêiner que já está rodando.
Entrando no Terminal de um Contêiner
Sistemas completos (Ubuntu/Debian):
```bash
  docker run -it ubuntu /bin/bash
  ```
Sistemas leves (Alpine — não possui Bash):
```bash
  docker run -it alpine /bin/sh
  ```
Contêiner temporário (apaga sozinho ao digitar `exit`):
```bash
  docker run -it --rm nginx:alpine /bin/sh
  ```
Acessar um contêiner que já está rodando em segundo plano:
```bash
  docker exec -it alice /bin/sh
  ```
Rodando Servidor Web (Nginx) na Prática
Para subir um site em segundo plano, liberar a porta de acesso e conectar uma pasta do seu computador:
```bash
docker run -d -p 8080:80 --name alice -v "/mnt/c/Users/EFG Bittencourt/huang:/usr/share/nginx/html" nginx:alpine
```
Entendendo cada parte do comando:
`-d` (detached): Roda o contêiner em segundo plano sem travar o terminal.
`-p 8080:80`: Conecta a porta `8080` do seu computador à porta `80` do contêiner (acesse via `http://localhost:8080`).
`--name alice`: Dá um nome fácil para gerenciar o contêiner depois.
`-v "origem:destino"`: Mapeia uma pasta do seu computador para dentro do contêiner.
Atenção: Use aspas duplas sempre que o caminho tiver espaços (como `EFG Bittencourt`).
`/usr/share/nginx/html` é a pasta padrão onde o Nginx busca o `index.html`.
`nginx:alpine`: Nome da imagem e sua versão (tag) separadas por dois-pontos (`:`).
Dicas de Ouro e Erros Comuns
Separador de versão: Use dois-pontos (`nginx:alpine`), nunca espaço.
Nomes duplicados: Se tentar criar um contêiner com um nome que já existe, remova o antigo antes com `docker rm -f alice`.
Flags interativas: Sempre use `-it` junto com `/bin/bash` ou `/bin/sh` para conseguir digitar comandos dentro do contêiner.
