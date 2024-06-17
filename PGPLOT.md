# Essa seção está destinada para instalação do Pgplot

## Índice

- [Instalação](#instalação)
- [Erros](#Erros)

# instalação 
Para instalar o `PGPLOT`, você precisa realizar as seguintes etapas. Essas etapas são descritas mais detalhadamente abaixo para cada sistema operacional.

Copie o arquivo de distribuição por FTP anônimo da Caltech. Este é um arquivo tar compactado:[download PGPLOT](ftp://ftp.astro.caltech.edu/pub/pgplot/pgplot5.2.tar.gz.) 

```Ruby
sudo apt-get update
sudo apt-get install build-essential gfortran libpng-dev
``` 
 Baixar o `PGPLOT`
Baixe o PGPLOT do site oficial. Você pode fazer isso usando wget:


```Ruby
wget ftp://ftp.astro.caltech.edu/pub/pgplot/pgplot522.tar.gz

```



Extraia o arquivo abaixo 

```Ruby
tar zxvf pgplot522.tar.gz
cd pgplot
```

Para Configurar o `PGPLOT`

Edite o arquivo `drivers.list` para especificar os drivers de dispositivos que deseja incluir. A configuração padrão geralmente é suficiente, mas você pode personalizá-la conforme necessário.


```Ruby
nano drivers.list

```

Compilar o `PGPLOT`



```Ruby
cp makefile.std makefile
make

```






Ao  chegar nessa parte você pode enfrentar problemas na configuração do `Makefile`.

Pensando nisso, vou deixar as configurações utilizadas para melhorar a compilação o `Makefile`

#Erros

### Erros 1: Erro na compilaçãodo `makefile`

```Ruby
(presto_env) rafaely@Windows11:~/presto/pgplot$ cp makefile.std makefile
make
cp: cannot stat 'makefile.std': No such file or directory
f77 -c -u ./src/pgarro.f
f77: fatal error: no input files
compilation terminated.
make: *** [makefile:109: pgarro.o] Error 1

```
##### Soluções:

Passo 1: Verificar a Existência dos Arquivos
Certifique-se de que o arquivo `makefile.std` está presente no diretório pgplot. Se não estiver, pode haver um problema com o download ou a extração dos arquivos.

Passo 2: Baixar e Extrair PGPLOT
Vamos garantir que você tenha o arquivo correto. Execute os seguintes comandos novamente para baixar e extrair o PGPLOT:

```bash
wget ftp://ftp.astro.caltech.edu/pub/pgplot/pgplot522.tar.gz
tar zxvf pgplot522.tar.gz
cd pgplot

```
Passo 3: Verificar os Arquivos
Verifique se o arquivo makefile.std está presente no diretório pgplot:

```bash
drivers.list  grfont.dat  makefile  makefile.aps  makefile.gf  makefile.imake  makefile.iris  makefile.std  rgb.txt  src
```
Passo 4: Compilar o PGPLOT
Se o arquivo makefile.std estiver presente, siga os passos abaixo para compilar e instalar:

Copie `makefile.std` para makefile:

```bash
cp makefile.std makefile

```
Edite o makefile para garantir que os compiladores corretos sejam usados. Se você estiver usando gfortran, altere o makefile para usar gfortran em vez de f77:
```bash
nano makefile

```
Procure por `f77` e substitua por `gfortran`.

Salve e feche o arquivo.

Execute o comando make para compilar:

```bash
make

```
Passo 6: Configurar Variáveis de Ambiente
Adicione as variáveis de ambiente ao seu arquivo `.bashrc`:



```Ruby
echo 'export PGPLOT_DIR=~/pgplot' >> ~/.bashrc
echo 'export PGPLOT_DEV=/xw' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=~/pgplot:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

Passo 7: Testar a Instalação
Execute o programa de teste para verificar se a instalação foi bem-sucedida:

```Ruby
cd ~/pgplot
./pgdemo1

```

O erro permaneceu, e com isso foi preciso adicionar manualmente alguns parametros no makefile

```Ruby
nano makefil
```
Editar o makefile:

Certifique-se de que as seguintes linhas estão presentes e corretamente configuradas:

```Ruby
# Define o compilador Fortran
F77 = gfortran

# Define o diretório contendo os arquivos de fonte
SRCDIR = ./src

# Define as flags de compilação
FFLAGS = -I$(SRCDIR) -u

# Lista de arquivos objeto a serem criados
OBJS = $(patsubst $(SRCDIR)/%.f,%.o,$(wildcard $(SRCDIR)/*.f))

# Regra padrão para compilar todos os arquivos objeto
all: $(OBJS)

# Regra para compilar arquivos .o a partir dos arquivos .f
%.o: $(SRCDIR)/%.f
    $(F77) $(FFLAGS) -c $< -o $@

# Limpeza dos arquivos compilados
clean:
    rm -f $(OBJS) pgplot

```

Após isso, 
