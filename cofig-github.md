# Instalação e tutorial de instalação da chave SSH

### Passo 1:  Instale o Git:
Primeiro, certifique-se de que o Git está instalado no seu sistema. Você pode instalar o Git usando o gerenciador de pacotes da sua distribuição. Para o Ubuntu, você pode usar o seguinte comando:

```Ruby

sudo apt update
sudo apt install git
```

### Passo 2: Configurar suas informações de usuário:
Configure seu nome de usuário e e-mail para o Git. 
Estas informações serão usadas em todos os commits que você fizer.

```Ruby

git config --global user.name "Seu Nome"
git config --global user.email "seu-email@example.com"
```

### Passo 3: Gerar uma chave SSH:

Gerar uma chave SSH permite que você se conecte ao GitHub sem precisar digitar seu nome de usuário e senha todas as vezes. Para gerar uma chave SSH, use o seguinte comando:

```Ruby
ssh-keygen -t ed25519 -C "seu-email@example.com"
```


Siga as instruções e pressione Enter para aceitar o local padrão dos arquivos de chave. Quando solicitado, digite uma senha forte (ou pressione Enter para não usar uma senha).

### Passo 4: Adicionar a chave SSH ao ssh-agent:
Inicie o ssh-agent no seu terminal e adicione a sua chave SSH.

```Ruby
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Passo 5: Copiar a chave SSH para a área de transferência:

Copie o conteúdo da sua chave pública para que você possa adicioná-la à sua conta do GitHub. Use o seguinte comando para exibir a chave pública:

```Ruby
cat ~/.ssh/id_ed25519.pub
```

Copie o conteúdo exibido no terminal. Deve ser algo como 
```Ruby
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA....
```

### Passo 6: Adicionar a chave SSH à sua conta do GitHub:

1. Vá até GitHub e faça login na sua conta.

2. No canto superior direito da tela, clique na sua foto de perfil e depois em "Settings".
No menu à esquerda, clique em "SSH and GPG keys".
Clique no botão "New SSH key".

3. No campo "Title", dê um nome à sua chave (por exemplo, "My Laptop Key").

4. No campo "Key", cole o conteúdo da sua chave pública que você copiou anteriormente.

5. Clique em "Add SSH key".

Testar a conexão SSH com o GitHub:
Teste se a configuração está funcionando corretamente executando o seguinte comando:

```Ruby
ssh -T git@github.com
```

A primeira vez que você fizer isso, pode ser solicitado a confirmar a autenticidade do host GitHub. Digite yes para continuar. Se tudo estiver configurado corretamente, você deve ver uma mensagem de sucesso como esta:

```Ruby
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```


# Tutorial: Envio de Commits

## 1. Configurando o Repositório Git Local

### Passo 1: Inicializar o Repositório Git

1. Navegue até o diretório do seu projeto:

```Ruby 
cd /caminho/para/seu/projeto
```

2. Inicialize um novo repositório Git:


```Ruby 
git init
```

### Passo 2: Configurar a URL Remota com SSH
1. Verifique a URL remota configurada (se houver):


```Ruby 
git remote -v
```

2. Se necessário, remova a URL remota existente (se estiver configurada para HTTPS):


```Ruby 
git remote remove origin
```

3. Adicione a URL do repositório remoto usando SSH:

```Ruby 
git remote add origin git@github.com:seu-usuario/nome-do-repositorio.git
```

Substitua seu-usuario e nome-do-repositorio pelos valores apropriados.

## Passo 3: Adicionar e Enviar Arquivos

1. Adicione os arquivos que você deseja versionar:

```Ruby 
git add nome-do-arquivo
```
2. Faça um commit com uma mensagem descritiva:

```Ruby 
git commit -m "Sua mensagem de commit"
```
3. Envie suas alterações para o repositório remoto:

```Ruby 
git push -u origin master
```
O -u configura o rastreamento do branch local com o branch remoto.

## Verificação e Manutenção

### Passo 1: Verificar a URL Remota

Verifique se a URL remota está correta:

```Ruby
git remote -v
```

A saída deve ser algo como:

```Ruby
origin  git@github.com:seu-usuario/nome-do-repositorio.git (fetch)
origin  git@github.com:seu-usuario/nome-do-repositorio.git (push)
```

### Passo 2: Testar a Conexão SSH
Teste a conexão com o GitHub:



```Ruby 
ssh -T git@github.com
```
Você deve ver uma mensagem como:

```Ruby 
Hi seu-usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

## Modificar o repostório para enviar os arquivos.


### Passo 1: Navegue para o diretório do repositório local que você deseja
```Ruby
cd /caminho/para/seu/repositorio
```

### Passo 2: Remova a URL remota existente
```Ruby
git remote remove origin
```

### Passo 3: Adicione a nova URL remota
```Ruby
git remote add origin git@github.com:novo-usuario/novo-repositorio.git
```

### Passo 4: Verifique a nova URL remota
```Ruby
git remote -v
```
### Passo 5: Adicione, commit e envie as mudanças para o novo repositório
```Ruby
git add .
git commit -m "Sua mensagem de commit"
git push -u origin master
```