# PRESTO__pulsar
Nesse github será direcionado para a instalação do programa PRESTO__pulsar,  e sua configurações utilizadas

Estou realizando a instalação inicialmente no meu computador pessoal, nele tenho o terminal ubuntu, logo o processo de instalação está próximo ao original,  a referencia utilizada é o git  [PRESTO](https://github.com/scottransom/presto/blob/master/INSTALL.md)

Como sempre, há um conjunto de pacotes essenciais necessários para construir o PRESTO. Este comando deve funcionar em um sistema semelhante ao Debian/Ubuntu, assim, instale no seu computador 

## 1- Primeiros passos

```ruby
Sudo apt-get install git build-essential libfftw3-bin libfftw3-dev pgplot5 libglib2.0-dev libcfitsio-bin libcfitsio-dev libpng-dev latex2html gfortran tcsh autoconf libx11-dev python3-dev python3-numpy python3-pip
```

Certifique-se de que sua variável de ambiente `PRESTO` aponte para o `PRESTO` git checkout de nível superior. E certifique-se de que `$PRESTO/lib` e `$PRESTO/bin` não estejam em suas variáveis ​​de ambiente `PATH` ou `LD_LIBRARY_PATH` ou `PYTHONPATH` conforme solicitamos no passado.  Que nessecaso,você precisará utilizar o comando " `nano ~/.bashrc ` para verificação das  modificações.

No meu caso apareceu alguns erros relacionadas a essas variaveis destacadas no texto, logo, foi preciso inserir na ultima linha do código `nano ~/.bashrc ` o seguinte código abaixo, então deixo como sugestão como foi feito

```ruby
nano ~/.bashrc

```

```ruby
export PRESTO=/caminho/para/presto
export PATH=$(echo $PATH | sed -e "s|$PRESTO/lib:||" -e "s|:$PRESTO/lib||" -e "s|$PRESTO/bin:||" -e "s|:$PRESTO/bin||")
export LD_LIBRARY_PATH=$(echo $LD_LIBRARY_PATH | sed -e "s|$PRESTO/lib:||" -e "s|:$PRESTO/lib||")
export LD_LIBRARY_PATH=$(echo $LD_LIBRARY_PATH | sed -e "s|$PRESTO/lib:||" -e "s|:$PRESTO/lib||")

export PRESTO=/home/rafaely/presto
export PATH=/home/rafaely/miniforge3/envs/presto_env/bin:$PATH
export LIBRARY_PATH=/home/rafaely/miniforge3/envs/presto_env/lib/x86_64-linux-gnu:$LIBRARY_PATH
export LD_LIBRARY_PATH=/home/rafaely/miniforge3/envs/presto_env/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH

export PATH=$HOME/bin:$PATH
```

No seu caso, será preciso modificar o caminho dos ambientes como no caso da linha `export LD_LIBRARY_PATH=/home/seu/caminho/do/arquivo/utilizado/x86_64-linux-gnu:$LD_LIBRARY_PATH`.

Posteriormente a isso utilize o comando 

```ruby
source ~/.bashrc

```

Provavelmente também é uma boa ideia limpar suas compilações anteriores. Basta entrar no diretório `src` e fazer um `make clean` e depois voltar aqui.

No documento original ele da duas opções de para compilação, o Python virtual ou Conda ativado, no meu caso utilizei o um ambiente conda, por já ter mais dominio.

## 2- Criando e configurando o ambiente no Conda

Com isso criei um novo ambiente (env)

```ruby
conda create --name fetch_env python=3.10
conda activate fetch_env
conda install -c matplotlib numpy pandas scipy ipykernel 
python -m ipykernel install --user --name fetch_env --display-name "fetch_env"

```
Agora edite o arquivo `~/.bashrc` com o comando `nano ~/.bashrc` e coloque na última linha o conteúdo abaixo:

alias jupyter-notebook="~/.local/bin/jupyter-notebook --no-browser"

Para sair e salvar suas alterações digite `CTRL+X` e depois `Y` + `ENTER`.

Feche e abra o seu terminal novamente. Agora você pode abrir o `jupyter` com o comando `jupyter-notebook`. Observe que um token aparece na tela. Copie este token com o mouse, abra um navegador, aponte seu navegador para `localhost:8888` e cole o token quando solicitado.

## 3 - Instalação dos primeiros pacotes para compilar

Agora você precisa instalar os 3 pacotes

```ruby
conda install meson meson-python ninja
```
Agora configure as compilações de código C/Fortran:

```ruby
cd $PRESTO

meson setup build --prefix=$CONDA_PREFIX
```
 No meu caso foi preciso configurar o `meson setup build --prefix=$CONDA_PREFIX` pois estava usando o ambiente do conda, caso sua escolha seja o Python virtual, basta seguir as orientações do link no item1.

 Ao fazer esse procedimento, surgiu alguns erros:

 Deixarei como sugestão: 

 ```ruby
(presto_env) rafaely@Windows11:/presto$ meson setup build --prefix=$CONDA_PREFIX
Did not find pkg-config by name 'pkg-config'
Found pkg-config: NO
Did not find CMake 'cmake'
Found CMake: NO
Run-time dependency glib-2.0 found: NO

meson.build:16:7: ERROR: Dependency lookup for glib-2.0 with method 'pkgconfig' failed: Pkg-config for machine host machine not found. Giving up.

A full log can be found at /home/rafaely/presto/build/meson-logs/meson-log.txt

============SOLUÇÃO===========

sudo apt-get update
sudo apt-get install pkg-config
sudo apt-get install libglib2.0-dev

sudo apt-get install libx11-dev
sudo apt-get install cmake

sudo apt-get install libpng-dev
conda activate presto_env

```

Por fim, para testar use

```Ruby
meson setup build --prefix=$CONDA_PREFIX

```

Para evitar possíveis problemas de vinculação e execução, recomendo fazer:

```Ruby
python check_meson_build.py

```
Se tudo parecer bem, você será informado. Caso contrário, recomendo tentar corrigir os problemas detectados e começar novamente.

Agora faça a construção e a instalação reais por meio de:

```Ruby
meson compile -C build
meson install -C build

```
O autor informa: Você verá muitos avisos do compilador devido à minha péssima codificação C (eu deveria consertar isso...), mas tudo bem. Deve haver logs caso algo dê errado em `$PRESTO/build/meson-logs`. Você deve ser capaz de executar o prepfold (por exemplo), neste momento e ver as informações de uso. Se isso não funcionar, consulte as informações de solução de problemas abaixo.

Finalmente, instale os códigos e ligações Python via pip:

```Ruby
cd presto
tests/ python test_presto_python.py
```
Ao executar o primeiro comando, apareceu um erro relacionado a um pacote que está faltando chamado `TEMPO`. Para resolver o problema, na própria pagina de instalação tem o repositório indicado de link [TEMPO](https://tempo.sourceforge.net/), ou simplesmente faça

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
