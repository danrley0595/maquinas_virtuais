# Guia de Estudos: Linux e Docker

---

## 1. Comandos Essenciais do Linux

### 1.1. Onde estou e o que tem aqui? (Navegação e Visualização)

* **`pwd`** (*Print Working Directory*)

  Mostra o caminho completo da pasta onde você está no momento.

  * Exemplo:
    ```bash
    pwd
    ```

<br>

* **`ls`** (*List*)

  Lista os arquivos e pastas do diretório atual.

  * `ls`: Lista simples dos nomes.

  * `ls -l`: Lista detalhada (mostra permissões, dono, tamanho e data).

  * `ls -la`: Lista tudo, incluindo arquivos ocultos (que começam com ponto `.`).

<br>

* **`cd`** (*Change Directory*)

  Serve para entrar ou sair de pastas.

  * `cd nome_da_pasta`: Entra na pasta escolhida.

  * `cd ..`: Sobe um nível (volta para a pasta anterior).

  * `cd ~`: Vai direto para a sua pasta pessoal (*home*).

  * `cd -`: Volta para o último diretório em que você estava antes do atual.

---

### 1.2. Criando Pastas

* **`mkdir`** (*Make Directory*)

  Cria uma nova pasta no local atual.

  * `mkdir projetos`: Cria a pasta chamada `projetos`.

  * `mkdir -p a/b/c`: Cria várias pastas uma dentro da outra de uma única vez.

---

### 1.3. Copiando, Movendo e Renomeando

* **`cp`** (*Copy*)

  Copia arquivos ou pastas para outro lugar.

  * `cp arquivo.txt copia.txt`: Copia um arquivo.

  * `cp -r pasta_origem pasta_destino`: Copia uma pasta inteira com tudo o que tem dentro (o `-r` significa *recursivo*).

<br>

* **`mv`** (*Move*)

  Move um arquivo/pasta de lugar ou muda o nome dele.

  * Mover: `mv arquivo.txt /home/usuario/documentos/`

  * Renomear: `mv nome_antigo.txt nome_novo.txt`

---

### 1.4. Apagando Arquivos e Pastas

* **`rmdir`** (*Remove Directory*)

  Apaga uma pasta **somente se ela estiver totalmente vazia**.

  * Exemplo:
    ```bash
    rmdir pasta_vazia
    ```

<br>

* **`rm`** (*Remove*)

  Apaga arquivos e pastas permanentemente (não vai para a lixeira).

  * `rm arquivo.txt`: Apaga um arquivo.

  * `rm -r pasta`: Apaga uma pasta e todo o conteúdo dentro dela.

---

### 1.5. Lendo e Exibindo Textos

* **`echo`**

  Exibe uma mensagem de texto na tela do terminal.

  * Exemplo:
    ```bash
    echo "Olá, Linux!"
    ```

<br>

* **`cat`** (*Concatenate*)

  Abre e mostra o conteúdo completo de um arquivo de texto direto no terminal.

  * Exemplo:
    ```bash
    cat arquivo.txt
    ```

---

## 2. Comandos Essenciais do Docker

### Conceitos Rápidos (`run` vs `exec`)

* **`docker run`**: Cria um **novo** contêiner a partir de uma imagem.

* **`docker exec`**: Entra em um contêiner que **já está rodando**.

---

### Entrando no Terminal de um Contêiner

* **Sistemas completos (Ubuntu/Debian):**
  ```bash
  docker run -it ubuntu /bin/bash
  ```

* **Sistemas leves (Alpine — não possui Bash):**
  ```bash
  docker run -it alpine /bin/sh
  ```

* **Contêiner temporário (apaga sozinho ao digitar `exit`):**
  ```bash
  docker run -it --rm nginx:alpine /bin/sh
  ```

* **Acessar um contêiner que já está rodando em segundo plano:**
  ```bash
  docker exec -it alice /bin/sh
  ```

---

### Rodando Servidor Web (Nginx) na Prática

Para subir um site em segundo plano, liberar a porta de acesso e conectar uma pasta do seu computador:

```bash
docker run -d -p 8080:80 --name alice -v "/mnt/c/Users/EFG Bittencourt/huang:/usr/share/nginx/html" nginx:alpine
```

**Entendendo cada parte do comando:**

* **`-d`** (*detached*): Roda o contêiner em segundo plano sem travar o terminal.

* **`-p 8080:80`**: Conecta a porta `8080` do seu computador à porta `80` do contêiner (acesse via `http://localhost:8080`).

* **`--name alice`**: Dá um nome fácil para gerenciar o contêiner depois.

* **`-v "origem:destino"`**: Mapeia uma pasta do seu computador para dentro do contêiner.

  * *Atenção:* Use **aspas duplas** sempre que o caminho tiver espaços (como `EFG Bittencourt`).

  * `/usr/share/nginx/html` é a pasta padrão onde o Nginx busca o `index.html`.

* **`nginx:alpine`**: Nome da imagem e sua versão (*tag*) separadas por dois-pontos (`:`).

---

### Dicas de Ouro e Erros Comuns

* **Separador de versão:** Use dois-pontos (`nginx:alpine`), nunca espaço.

* **Nomes duplicados:** Se tentar criar um contêiner com um nome que já existe, remova o antigo antes com `docker rm -f alice`.

* **Flags interativas:** Sempre use `-it` junto com `/bin/bash` ou `/bin/sh` para conseguir digitar comandos dentro do contêiner.






Aula 6

1 - cloudflare 
docker run -d -p 8081:80 --name cloudflare -v "/mnt/c/Users/EFG Bittencourt/projetos/site1:/usr/share/nginx/html" nginx:alpine

/mnt/c/Users/EFG Bittencourt/projetos/site1

echo "cloudflared tunned" >> index.html


http://localhost:8081/

cloudflared tunnel --url http://localhost:8081


2 - ngrok
docker run -d -p 8082:80 --name ngrok -v "/mnt/c/Users/EFG Bittencourt/projetos/site2:/usr/share/nginx/html" nginx:alpine

/mnt/c/Users/EFG Bittencourt/projetos/site2

echo "ngrok tunned" >> index.html

http://localhost:8082/

ngrok instalar e gerar link

-------------------------------------------


3 - tailscale
docker run -d -p 8083:80 --name tailscale -v "/mnt/c/Users/EFG Bittencourt/projetos/site3:/usr/share/nginx/html" nginx:alpine

/mnt/c/Users/EFG Bittencourt/projetos/site3

echo "tailscale tunned" >> index.html

http://localhost:8083/

tailscale instalar e gerar link

nano script.sh

cat script.sh


bash script.


Aula 8 - DockerFile


1. Estrutura do Dockerfile

O arquivo que serve de "receita" para criar sua imagem:
Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
RUN mkdir -p /var/www/uploads
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

Use o código com cuidado.
• FROM nginx:alpine: Define a base da imagem (Nginx leve).
• COPY . /usr/share/nginx/html: Copia seus arquivos locais para a pasta do site no container.
• RUN mkdir -p /var/www/uploads: Executa um comando durante a criação da imagem (cria uma pasta).
• EXPOSE 80: Informa qual porta o container utiliza.
• CMD ["nginx", "-g", "daemon off;"]: Define o comando principal que inicia o servidor e mantém o container ativo em primeiro plano.

2. Comandos Diretos


Imagens (Receita)

• Criar imagem: docker build -t imagem:tag .
• Listar imagens: docker images
• Apagar imagem: docker rmi imagem:tag

Containers (Em execução)

• Criar e iniciar: docker run -d --name nome -p 9000:80 imagem:tag
• Ver ativos: docker ps
• Ver todos: docker ps -a
• Entrar no terminal: docker exec -it nome sh
• Parar: docker stop nome
• Apagar: docker rm -f nome

Docker Hub (Nuvem)

• Entrar na conta: docker login
• Preparar nome: docker tag imagem:tag usuario/imagem:tag
• Enviar: docker push usuario/imagem:tag
Regra de ouro:
• Comandos de imagem (build, rmi, push) usam imagem:tag.
• Comandos de container (exec, stop, rm) usam o nome (ou ID).
Você gostaria de testar seus conhecimentos com um quiz de múltipla escolha sobre esses comandos e instruções do Dockerfile?

