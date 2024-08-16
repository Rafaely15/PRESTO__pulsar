# Istalação do docker

Para fazer a istalação do docker no site [Docker](https://www.nerdlivre.com.br/como-instalar-o-docker-no-ubuntu/)

Após a instalação faça a instalação da imagem do presto.

# Comandos para usar o Docker-Presto

## Download do presto 

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
caso você tenha perdido o conteiner, basta saber o ID do conteiner e executar o seguinte comando
```Ruby
docker start 9747ec45499a 
```
ou 
```Ruby
docker start <ID>
```

Após isso, você faz para ver os conteiner usados
```Ruby
docker ps -la 
```
para usar novamente o conteiner você usa
```Ruby
docker exec -it 9747ec45499a /bin/bash
```
ou 
```Ruby
docker exec -it <ID> /bin/bash
```
### 3. Uso do presto

Verificar quais dcoker estão em execução 
```Ruby
docker ps
```
Reiniciar o docker que parou 
```Ruby
docker start -it <ID_docker> bash
```
Executar o docker 
```Ruby
docker exec -it <ID_docker> bash
```
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
