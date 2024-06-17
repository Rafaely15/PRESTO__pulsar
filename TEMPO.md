## 4. Instalação do TEMPO

```Ruby
cd presto
git clone git://git.code.sf.net/p/tempo/tempo
```
Você notará que aparecerá uma nova pasta chamada tempo no repositório do `presto`, para verificar use o comando


```Ruby
cd tempo
```

```Ruby
./prepare

```
 Instalar C shell (csh)
Primeiro, instale o csh no seu sistema:

```Ruby
sudo apt-get update
sudo apt-get install csh

```
Por fim,

Instalar Ferramentas de Desenvolvimento
Primeiro, instale as ferramentas de desenvolvimento, incluindo autoconf, automake e libtool.

```Ruby
sudo apt-get update
sudo apt-get install autoconf automake libtool
```
 Executar o Script `prepare`
Depois de instalar as ferramentas, execute novamente o script prepare:


```Ruby
cd ~/presto/tempo
./prepare

```
Seguir os Passos de Configuração e Compilação
Continue com os passos de configuração e compilação:

```Ruby
./configure
make
sudo make install

```
Adicionar ao PATH (se necessário)
Se necessário, adicione o diretório de instalação ao PATH:



```Ruby
export PATH=/usr/local/bin:$PATH
echo 'export PATH=/usr/local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc


```
Verificar a Instalação
Finalmente, verifique a instalação executando o comando `tempo`:

```Ruby
tempo

```
Assim, retornaremos a pasta do presto ,   com o comando 

```Ruby
cd ..

```
Posteriormente, teste com o comando 
```Ruby
python tests/test_presto_python.py

```
```Ruby
tempo
```

Instalação concluida com sucesso!


