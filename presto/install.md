# PRESTO__pulsar
Este github é direcionado para a instalação do programa PRESTO pulsar,  e suas configurações.

A instalação será feita no sistema operacacional linux, logo o processo de instalação será feito como o arquivo original,  de referência [PRESTO](https://github.com/scottransom/presto/blob/master/INSTALL.md)

O PRESTO tem alguns pré-requisitos, 
deve funcionar em um sistema semelhante ao Debian/Ubuntu, assim, instale no seu computador 

## 1- Primeiros passos

Instale os  pacotes indicados a abaixo no seu computador, se você for linux, cole no terminal, ou ainda, se prefere pode instalar em algum ambiente conda.

```ruby
sudo apt-get install git build-essential libfftw3-bin libfftw3-dev pgplot5 libglib2.0-dev libcfitsio-bin libcfitsio-dev libpng-dev latex2html gfortran tcsh autoconf libx11-dev python3-dev python3-numpy python3-pip
```

Certifique-se de que sua variável de ambiente `PRESTO` aponte para o `PRESTO` git checkout de nível superior. E certifique-se de que `$PRESTO/lib` e `$PRESTO/bin` não estejam em suas variáveis ​​de ambiente `PATH` ou `LD_LIBRARY_PATH` ou `PYTHONPATH` conforme solicitamos no passado.  Que nesse caso,você precisará utilizar o comando " `nano ~/.bashrc ` para verificação das  modificações. (Importante)

No meu caso apareceu alguns erros relacionadas a essas variaveis destacadas no texto, logo, foi preciso inserir na ultima linha do código `nano ~/.bashrc ` o seguinte código abaixo, então deixo como sugestão como foi feito

```ruby
nano ~/.bashrc

```

Algumas alterações que foi preciso ser feito com o ` nano ~/.bashrc`.

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

Posteriormente a isso utilize `Ctrl + 0` depois `Ctrl + X` e por  fim `Y`, posteriormente coloque no terminal o comando 

```ruby
source ~/.bashrc

```

Provavelmente também é uma boa ideia limpar suas compilações anteriores. Basta entrar no diretório `src` e fazer um `make clean` e depois voltar aqui.

No documento original ele da duas opções de para compilação, o Python virtual ou Conda ativado, no meu caso utilizei o um ambiente conda, por já ter mais dominio.

## 2- Criando e configurando o ambiente no Conda

Com isso criei um novo ambiente (env)

```ruby
conda create --name presto_env python=3.10
conda activate presto_env
conda install -c matplotlib numpy pandas scipy ipykernel 
python -m ipykernel install --user --name presto_env --display-name "presto_env"

```

É importante  verificar no arquivo se você está instalando os pacotes no ambiente virtual criado, para não ter conflitos.


Agora edite o arquivo `~/.bashrc` com o comando `nano ~/.bashrc` e coloque na última linha o conteúdo abaixo:

```Ruby
alias jupyter-notebook="~/.local/bin/jupyter-notebook --no-browser"
```

Para sair e salvar suas alterações digite `CTRL+X` e depois `Y` + `ENTER`.

Feche e abra o seu terminal novamente. Agora você pode abrir o `jupyter` com o comando `jupyter-notebook`. Observe que um token aparece na tela. Copie este token com o mouse, abra um navegador, aponte seu navegador para `localhost:8888` e cole o token quando solicitado.


Ou simplesmente use, no bash "jupyter-lab"

## 3 - Instalação dos primeiros pacotes para compilar

Agora você precisa instalar os 3 pacotes

```ruby
conda install meson meson-python ninja
```
Agora configure as compilações de código C/Fortran:

```ruby
cd presto

meson setup build --prefix=$CONDA_PREFIX
```
 No meu caso foi preciso configurar o `meson setup build --prefix=$CONDA_PREFIX` pois estava usando o ambiente do conda, caso sua escolha seja o Python virtual, basta seguir as orientações do link no item1.

 Ao fazer esse procedimento, surgiu alguns erros:

 Deixarei como sugestão: 

 ```ruby
(presto_env) rafaely :/presto$ meson setup build --prefix=$CONDA_PREFIX
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

### Desafios que enfrentei

##### Erro 1:Testando o `tests/ python test_presto_python.py` nesse caso

```Ruby
"(presto_env) rafaely@rafaely-IdeaPad-3-15ALC6:~/presto$ python tests/test_presto_python.py
Traceback (most recent call last):
  File "/home/rafaely/presto/tests/test_presto_python.py", line 3, in <module>
    from presto import presto
ModuleNotFoundError: No module named 'presto'
(presto_env) rafaely@rafaely-IdeaPad-3-15ALC6:~/presto$ "
```
Nesse caso é preciso você instalar o `presto`


1. Certifique-se de que o diretório de instalação está no PYTHONPATH:


```Ruby
export PYTHONPATH=/home/rafaely/miniconda3/envs/presto_env/lib/python3.9/site-packages:$PYTHONPATH

```

2. Reinstale o módulo python do presto

```Ruby 
python setup.py install
```
3. Verifique o diretório site-packages:

```Ruby 
ls /home/rafaely/miniconda3/envs/presto_env/lib/python3.9/site-packages

```
4. por fim instale o `presto`

```Ruby
pip install /home/rafaely/presto

```

5. Instale o `pyproject.toml`

```Ruby
cd /home/rafaely/presto/python
pip install .
```

#### Erro 2: MAKEWISDOM

1. Instalação do `makewisdom`, o makewisdom está localizado no diretório de build do PRESTO, especificamente em $PRESTO/build/src. Navegue até esse diretório:

```Ruby
cd $PRESTO/build/src
```
2. Execute o makewisdom a partir do diretório src:


```Ruby 
./makewisdom
```

3. Verifique o conteúdo do diretório $PRESTO/lib para garantir que fftw_wisdom.txt está lá:


```Ruby 
ls -l $PRESTO/lib/fftw_wisdom.txt
```
4. Abra o nano para a função meson.build.

```Ruby
nano $PRESTO/meson.build
```
5. Encontre a linha que define _global_c_args. E apague apenas essa linhas, salve com o ctrl+x + s/y + enter


6. Compile novamente 

```Ruby
meson compile -C build
meson install -C build

```
7. algumas alternativas se aparecer outros erros 

```Ruby
ls -l $PRESTO/lib/fftw_wisdom.txt

cp $PRESTO/build/src/fftw_wisdom.txt $PRESTO/lib/

nano $PRESTO/meson.build

```
Encontre a linha que define _global_c_args e remova -DUSE_FFTW_MALLOC.

```Ruby
meson compile -C build
meson install -C build

```

depuração 

```Ruby
gdb --args python tests/test_presto_python.py

run

bt

```
Verifique o repositório do github

```Ruby
cd $PRESTO
git pull origin main


cd $PRESTO
meson setup --wipe build


meson setup build --prefix=$HOME
meson compile -C build
meson install -C build


# verificação da variavel de momento 

env | grep PRESTO
env | grep LD_LIBRARY_PATH

#Analisar o Core Dump

ulimit -c unlimited

python tests/test_presto_python.py
gdb python core

#verifique o repositorio do presto
git branch -a

git checkout master
git pull origin master

#reinstalando dependencias

sudo apt-get install mpi-default-bin mpi-default-dev

meson setup --wipe build
meson compile -C build
meson install -C build

#ativar o core

ulimit -c unlimited

python tests/test_presto_python.py
gdb python core


git pull origin master


```
Por fim, recompile e reinstale 

```Ruby
meson setup --wipe build
meson setup build
meson compile -C build
meson install -C build
python tests/test_presto_python.py


```

CASO APAREÇA ESSE ERRO 

```Ruby
presto_env) rafaely@rafaely-IdeaPad-3-15ALC6:~/presto$ python tests/test_presto_python.py
Warning:  '/home/rafaely/presto/lib/fftw_wisdom.txt' is not up-to-date.
          You should run 'makewisdom'.  See $PRESTO/INSTALL.
Testing FFT stuff... success
Testing FFTW call... success
Warning:  '/home/rafaely/presto/lib/fftw_wisdom.txt' is not up-to-date.
          You should run 'makewisdom'.  See $PRESTO/INSTALL.
Testing tablesixstepfft call... success
Testing reading infiles... success
Testing writing infiles... success
Note: If the following fails, edit $PRESTO/meson.build and remove
        '-DUSE_FFTW_MALLOC' from the _global_c_args definition, and re-build.
Testing allocation and freeing of memory... success
Testing psrparams and orbitparams stuff... success
Testing spectralpower and spectralphase... success
Testing vector shifting / rotation... success
Testing orbit integration stuff... success
Testing Fourier response generation... success
Testing angle functions... success

Error in chkfopen(): No such file or directory
   path = 'resid2.tmp'
(presto_env) rafaely@rafaely-IdeaPad-3-15ALC6:~/presto$
``` 

a exportação do arquivo tempo não está dentro do presto, assim não vai ser possivel pegar os arquivos, funções e dados, logo, abra o `nano ~/.bashrc ` e altere o caminho da exportação da saida do tempo ajuste para 

```Ruby
export TEMPO=~/presto/tempo
```





Ao executar o primeiro comando, apareceu um erro relacionado a um pacote que está faltando chamado `TEMPO`. Para resolver o problema, na própria pagina de instalação tem o repositório indicado de link [TEMPO](https://tempo.sourceforge.net/), ou simplesmente faça

Nessa parte você precisará  instalar o `Tempo`

## 4. Instalação do [TEMPO](https://github.com/Rafaely15/PRESTO__pulsar/blob/main/TEMPO.md)

## 5. Retorno da instalação do PRESTO

Após isso, vamos para o proximo passo

```Ruby
1- python examplescripts/ffdot_example.py
2- python python/fftfit_src/test_fftfit.py
```

Na segunda linha do código, será solicitado a isntalação do Pgplot.

## 6. Instalação do [Instal_pgplot](https://github.com/Rafaely15/PRESTO__pulsar/blob/main/PGPLOT.md)

Manual do PGPLOT: [PGPLOT](https://sites.astro.caltech.edu/~tjp/pgplot/install.html) 

## 7. Retorno da instalação do Presto



