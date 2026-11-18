Guia de Estudos: Linux e Docker
---
1. Comandos Essenciais do Linux
1.1. Onde estou e o que tem aqui? (Navegação e Visualização)
`pwd` (Print Working Directory)
O que faz: Mostra o caminho completo da pasta onde você está agora.
Exemplo: `pwd` -> `/home/usuario/projetos`
`ls` (List)
O que faz: Lista os arquivos e pastas do diretório atual.
Variações úteis:
`ls -l`: Mostra detalhes (permissões, dono, tamanho e data).
`ls -la`: Mostra tudo, inclusive arquivos ocultos (que começam com `.`).
`cd` (Change Directory)
O que faz: Muda de pasta.
Variações úteis:
`cd nome_da_pasta`: Entra na pasta indicada.
`cd ..`: Volta um nível (sobe para a pasta anterior).
`cd ~`: Vai direto para a sua pasta pessoal (home).
`cd -`: Volta para o último diretório que você visitou.
---
1.2. Criando e Apagando Pastas
`mkdir` (Make Directory)
O que faz: Cria uma nova pasta.
Variações úteis:
`mkdir projetos`: Cria a pasta `projetos`.
`mkdir -p a/b/c`: Cria várias pastas uma dentro da outra de uma só vez.
`rmdir` (Remove Directory)
O que faz: Apaga uma pasta, mas apenas se ela estiver vazia.
Exemplo: `rmdir pasta_vazia`
`rm -r` (Remove Recursive)
O que faz: Apaga uma pasta e tudo o que estiver dentro dela (arquivos e subpastas).
Atenção: No terminal do Linux não existe lixeira; apagou, sumiu.
Exemplo: `rm -r pasta_cheia`
---
1.3. Copiando, Movendo e Renomeando
`cp -r` (Copy Recursive)
O que faz: Copia uma pasta inteira e seu conteúdo para outro lugar.
Exemplo: `cp -r pasta_origem pasta_destino`
`mv` (Move)
O que faz: Serve tanto para mover quanto para renomear arquivos e pastas.
Exemplos:
Mover: `mv arquivo.txt /home/usuario/documentos/`
Renomear: `mv nome_antigo.txt nome_novo.txt`
---
1.4. Exibindo Mensagens e Lendo Arquivos
`echo`
O que faz: Exibe um texto na tela do terminal.
Exemplo: `echo "Estudando Linux e Docker"`
`cat` (Concatenate)
O que faz: Abre e mostra todo o conteúdo de um arquivo de texto direto no terminal.
Exemplo: `cat anotacoes.txt`
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
