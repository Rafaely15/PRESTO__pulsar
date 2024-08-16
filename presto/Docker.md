# Istalação do docker

Para fazer a istalação do docker no site [Docker](https://www.nerdlivre.com.br/como-instalar-o-docker-no-ubuntu/)

Após a instalação faça a instalação da imagem do presto.

# Comandos para usar o Docker-Presto

## Download da imagem do presto e singulary 

### 1. Docker images:
```Ruby
docker pull alex88ridolfi/presto5:latest
docker pull alex88ridolfi/presto5:png
```
### 2.Singularity images:
```Ruby
singularity pull docker://alex88ridolfi/presto5:latest
singularity pull docker://alex88ridolfi/presto5:png
```
Para mais informações verifique [Docker_presto](https://github.com/scottransom/presto/blob/master/INSTALL.md)

Para entrar no bash do presto 

```Ruby
docker run -it alex88ridolfi/presto5:png /bin/bash
```
caso você tenha perdido o conteiner, execute 
```Ruby
Docker ps 
```
Com o ID em mão, coloque o ID do  conteiner e executar o seguinte comando, essa função vai startar o seu docker para que assim possa ser executado
```Ruby
docker start 9747ec45499a 
```
ou 
```Ruby
docker start <ID>
```

Após isso, você digite no terminal, e verifique se de fato foi startado o docker que você solicitou
```Ruby
docker ps 
```
Assim, execute o comando para poder abrir novamente o docker e o seu bash, devo destacar que o "-it" representa você utilizar o docker de maneira interativa.
```Ruby
docker exec -it 9747ec45499a /bin/bash
```
ou 
```Ruby
docker exec -it <ID> /bin/bash
```
Nesse passo você já estará dentro da imagem do presto do docker, assim, digite o comando "ls" e verá que terá as pastas "Tempo, presto5"



### 3. Dentro da imagem do presto

Para ir usando o docker siga o passo [manuseio-presto](https://github.com/Rafaely15/PRESTO__pulsar/blob/master/presto/Manuseio-do-presto.md)

------------------------ Algumas informações uteis -------------------------

Se você estiver usando um computador com acesso via chave ssh, e deseja vincular os arquivos que estão no ssh para que o docker tenha acesso, faça

k



Configurar o presto para colocar a saida png  (dentro do docker e no bash da imagem)
```Ruby
nano /software/presto5/examplescripts/ffdot_example.py
 ```
modifique 'device=/XWIN' para 'device='ffdot_combined.png/PNG'

Trazendo um arquivo do docker para o meu computador 
```Ruby
docker cp <ID_docker>:/software/presto5/examplescripts/ffdot_combined.png /home/rafaely 
```
Docker sendo usado atualmente: ID 9747ec45499a.

Para abrir o arquivo ps faça a instalação dele na sua maquina 
```Ruby
sudo apt-get update
sudo apt-get install gv
```

Abrir o arquivo
```Ruby
gv nome_do_arquivo.ps
```
Para gerar e analisar os arquivos utilize o manual do presto normalmente.

# Instruções adicionais 
