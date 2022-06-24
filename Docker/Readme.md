
# Observações:
## Comandos genericos:
- $(pwd)  - pega o diretório atual
## Grupo utilizado para descrever palavras chaves comentadas abaixo.
- "imagens utilizadas nos containers":
	- São imagens do docker Registry ou seja, quando desenvolvemos uma aplicação, geramos a imagem dela e depois geramos o container baseado na imagem.

# Extruturação do Docker:
## Client:
- Serve para execução dos comandos, ou seja, sempre que executa um comando "docker build, docker pull, etc" está sendo rodado no Client.
## Docker Host/Docker Daemon:
- Este cara é o resposável por gerenciar os meus containers e as minhas imagens utilizadas nos containers.
## Docker Registry:
- É onde vamos upar as imagens dos nossos containers, para podermos baixa-las em nosso servidor e depois criarmos o container e rodarmos o código da imagem. O Registry, é como se fosse o github, mas de imagens de containers. 

![Captura de Tela 2022-04-22 às 20 42 13](https://user-images.githubusercontent.com/36082343/164868048-023584ba-84a2-48c2-87b4-a44b915a86e8.png)

# Executando um container:
- Ao rodar o comando "docker container run hello-world" ele fez o passo a passo da imagem abaixo:

![Captura de Tela 2022-04-22 às 20 52 04](https://user-images.githubusercontent.com/36082343/164868770-d7a41d30-0582-498b-89a5-a99943009c55.png)

# Comandos docker:
## Legenda:
- SF - Quer dizer Sem Flag ou seja só o comando limpo, ex: docker container ls
		
## Rodar um container:
- docker container run IMAGE-NAME
	- Exemplo:
		- docker container run --name meucontainer IMAGE-NAME
	- Flags:
		- SF 		- Vai rodar a imagem em um container sem nenhuma especificação.
		- --name 	- Vai definir um name para o container se nao definir a mesma ele coloca um aleatório.
		- --rm	- Define que após a execução do container ele deve ser removido
		- -it		- Define que o container deve ser executado em modo interativo e habilitando o tty, em resumo é para podermos acessar o container e executar comandos dentro dele, ex "docker container run -it ubuntu /bin/bash", o /bin/bash definido e para dizer que irei usar o bash para acessar e rodar os comandos, pode ser usado o sh, quando sair do container ele morre.
		- -d 		- Executar um container no modo deamon, ou seja, em segundo plano.
		- -p		- Define uma porta para fazer o Port-Bind do container para a maquina, para definir as portas deve ser passado a seguinte "-p 8080:80" sendo a "8080" a porta da maquina que está executando o container e "80" a porta do container ex "docker container run -d -p 8080:80 nginx"
		- --network - Ao criar o container podemos definir a qual rede network este container será conectado, para isso devemos passar a seguinte flag "--network NAME_NETWORK" sendo a "NAME_NETWORK" o nome da rede que o container será conectado.
		- -v		- Define um volume da maquina local para compartilhar com o container, caso quiser compartilhar uma pasta pode passar o caminho, caso quiser compartilhar um "volume" passar o nome do volume, para isso devemos passar a seguinte flag "-v /compartilhado:/compartilhado_container" sendo "/compartilhado" o caminho do volume na maquina local e "/compartilhado_container" o caminho da pasta dentro do container.
		- -e		- Definir variavel de ambiente para o container, para isso devemos passar a seguinte flag "-e VARIAVEL=VALOR" sendo "VARIAVEL" o nome da variavel e "VALOR" o valor da variavel.

## Remover um container:
- docker container rm NAME/CONTAINER_ID
- Flags:
	- -f - Remoção forçada utilizada para forçar a remoção de um container que está em execução.

## Listar os containers:
- docker container ls
- Flags:
	- SF - Sem Flag lista os containers em execução/rodando
	- -a - Lista os containers que estão parados também

## Executar um comando para o conainer:
- docker container exec

## Comandos de troubleshooting(Ver os logs do container):
- Inspect do status, argumentos de criação, imagem utilizada, disco, network:
	- docker container inspect CONTAINER_ID
- Entrando no container e ativando o -it:
	- docker container exec -it CONTAINER_ID /bin/bash
- Verificar os logs do container:
	- docker container logs CONTAINER_ID
		- Flags:
			- -n - Numero de linhas que deseja ver do log, ex "docker container logs -n 5 CONTAINER_ID".
			- -f - Verificar logs continuamente, ou seja, conforme for surgindo logs ele vai printando.
			- -t - Data e Hora dos logs.

## Stop container:
- docker container stop CONTAINER_ID

## Start container:
- docker container start CONTAINER_ID

## Criar uma Imagem de container(DockerFile):
- docker image build -t DockerfileBasic CONTEXT
	- obs:
		- O "CONTEXT" no final é o caminho do meu Dockerfile onde está configurado tudo o que vai ter na imagem.
		- Dockerfile:
			- No exemplo do Dockerfile LINK_ARQUIVO estou cocatenando todas as instalações junto com o "&&" pois isso me garante que sempre que eu rodar a construção do container o mesmo não irá pegar nada de cache isso previne possiveis problemas.
			- Mas podemos usar varios comandos "RUN" um em baixo do outro o ponto é que se encontrar cache ele vai puxar do cache.
	- Flags:
		- -t - Nome da imagem(Verificar o padrão que é usado!)
- Comandos possiveis com DockerFile:

![Captura de Tela 2022-04-23 às 15 02 44](https://user-images.githubusercontent.com/36082343/164934293-90c1b8ae-f5e2-4df2-a9ce-9761b5bb594d.png)

## Listar as imagens criadas:
- docker image ls

## Remover imagens:
- Remover imagens sem referencia REPOSITORY está igual a <none>:
	- docker image prune
- Remover uma imagem em especifica:
	- docker image rm IMAGE_ID/REPOSITORY
	
## Obter dados da imagem:
- Obter informações sobre a imagem
	- docker image inspect IMAGE_ID/REPOSITORY
- Histórico de criação das camadas da imagem junto com os comandos para criação:
	- docker image history IMAGE_ID/REPOSITORY

## Boas praticas criação de imagem: 
- Nome da imagem: 
	- eliezer/api-teste:v1
		- eliezer 	- É o namespace, a quem pertence a imagem.
		- api-teste 	- Repositório, é igual ao repositório git que precisamos colocar o nome, aqui é a mesma coisa.
		- v1			- É a versao da imagem, cada alteração incrementa a versao, pode usar a mesma versão da aplicação.
	- Imagens sem namespaces são imagens oficiais do docker, ex "ubuntu:20.10", "ubuntu" é o repositório e o "20.10" é a versão.
	- Latest:
		- Sempre é bom subir para o DockerHub a sua versão da aplicação e a latest, para que quando voce baixar a imagem sem especificar a versão será puxado automaticamente a latest, para fazer isto deve rodar o comando:
				docker tag NAMESPACE/REPOSITORY:IMAGE_TAG NAMESPACE/REPOSITORY:latest						
- .dockerignore:
	- Ignorar arquivos na hora de copiar as pastas da sua API/Aplicação para a imagem, mesma funcionalidade do .gitignore
	
## Subindo imagem para DockerHub:
- Fazer login:
	- docker login
- Fazer logount:
	- docker logout
- Subir imagem para DockerHub:
	- docker push NAME_IMAGE

## Network:
- Listar os networks:
	- docker network ls
- Criar uma network:
	- docker network create NAME_NETWORK
- Conectar containers ao network:
	- docker network connect NAME_NETWORK CONTAINER_ID
		- obs:
			- Após conectar se der um "docker inspect" vai mostrar que está conectado a NAME_NETWORK e ao bridge, com isso temos os containers conectados ao mesmo network.
- Desconectar o container da rede:
	- docker network disconnect NAME_NETWORK CONTAINER_ID
- Inspecionar dados da network:
	- docker network inspect NAME_NETWORK

## Diretórios compartilhados com os containers: 
- Comando abaixo compartilha um diretório /root/vol especificado com o container no /app, utilizar tanto para pasta quanto para volumes nas flags do comando run acima descrevo melhor este comando:
	- Pasta:
		- docker container run -v /root/vol:/app ...
	- Volume:
		- docker container run -v NOME_VOLUME:/app ...
	- Observações:
		- - Ao mapear diretório utilizando o Volume o path do container sempre deve seguir o mesmo, pois senão os arquivos nao serão achados dentro do container, exemplo:
			- Mapeei o volume "mongo_vol" para "/data/db" do container, criei um "Database" e tabelas, ao recriar o container se eu não mapear o mesmo "/data/db" o "Database" e tabelas criadas no container anteriormente nao serão achadas.

- Volume compartilhado com os containers mas usando docker volume, e quem gerencia ele é o docker deamon:
	- Utilidades do docker volume:
		- - Util para compartilhar arquivos entre containers ou para um container criar um arquivo e nós conseguirmos pegar ele mesmo estando forra da maquina.
		- - Util para base de dados que rodam em containers, ou seja, ao criarmos um container "mongo" podemos definir um volume para ele, e se caso o container ser removido os dados não serão perdidos, pois estão no volume, e quando subir novamente o container ele vai pegar os dados do volume.
	- Criar docker volume:
		- docker volume create NOME_VOLUME
	- Listar os docker volumes:
		- docker volume ls
	- Inspecionar o volume: 
		- docker volume inspect NOME_VOLUME
			- Dará as informações do volume inclusive o MoutPoint que é o path onde grava os arquivos.

## Docker compose:
- Criar os containers e subir eles a partir de um docker-compose.yaml, caso alterar o .yaml pode ser rodado o up e ele vai recriar o container com as novas configs:
	- docker-compose up
		- Flags:
			- -d 			- Executar no modo deamon, ou seja, em segundo plano.
			- --env-file 	- Carregar arquivo de configuração, para o docker-compose.yaml no mesmo deve ser definido as variaveis "${TAG}" e no .env (Que seria o arquivo de configuração) temos "TAG=latest" com isso definimo variaveis, exemplo "docker-compose --env-file ./.env up -d", ou na api-produto:
				- LINK DO REPOSITÓRIO API-PRODUTO
			- --build		- Força recriar a imagem a partir do Dockerfile especificado no docker-compose.yaml.
		
- Derruba os containers que foi criado com o docker-compose.yaml:
	- docker-compose down 

## Comandos prontos:
- Criar db Mongo:
	- docker container run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongouser -e MONGO_INITDB_ROOT_PASSWORD=mongopwd mongo
	- Mongo com mapeamento de volume:
		- docker container run -d -p 27017:27017 --name mongodb -e MONGO_INITDB_ROOT_USERNAME=mongouser -e MONGO_INITDB_ROOT_PASSWORD=mongopwd --network produto_net -v mongo_vol:/data/db  mongo:4.4.3

- Criar volumes com a data atual:
	- data=$(date +%Y-%m-%d)
	- docker volume create $data

## Links:
- Estrutura do Kubernetes:
	- https://kubernetes.io/pt-br/docs/tutorials/kubernetes-basics/explore/explore-intro/#:~:text=Um%20N%C3%B3%20%C3%A9%20uma%20m%C3%A1quina,Pods%20nos%20n%C3%B3s%20do%20cluster.
