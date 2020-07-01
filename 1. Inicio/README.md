# 1. Início
PostgreSQL é um banco de dados relacional open-source, todas as distribuições encontram-se disponiveis no [site oficial](https://www.postgresql.org/ftp/source/).

## 1.1. Instalação
A instalação do PostgreSQL é simples e na maior parte dos casos, basta baixar e executar o instalador interativo disponivel no [site oficial](https://www.postgresql.org/download/).

Algumas distribuições Unix podem instala-lo por meio do repositorio APT:

```bash
$ sudo apt-get install postgresql
```

Para instalar uma versao especifica, basta:

```bash
$ apt-get install postgresql-VERSAO
```

Este guia foi feito com base na versao 11.8 do PostgreSQL.

## 1.2. Arquitetura
Antes de mais nada, é preciso conhecer um pouco da arquitetura de PostgreSQL.

PostgreSQL é baseado no modelo "cliente/servidor", resultado da cooperaçao dos seguintes processos:

- <b>CLIENTE</b>: Geralmente é um aplicativo desenvolvido pelo usuário, que pretende executar operações no banco de dados. Mas pode também ser um simples console, uma aplicaçao grafica, etc.
- <b>SERVIDOR</b>: Responsável por gerar os arquivos do banco de dados, os clientes se conectam ao servidor, que por sua vez deve executar as ações solicitadas. O servidor é chamado postgres.

Como na maioria dos aplicativos baseados no modelo "cliente/servidor", eles podem ser hospedados em hosts diferentes, nesse caso, eles se comunicam através de uma conexão TCP/IP.

O servidor PostgreSQL pode lidar com vários clientes simultâneamente, para isso, ele inicia um novo processo para cada conexão. O cliente e o novo processo do servidor se comunicam sem intervenção do processo original do postgres, que segue aguardando por novas conexões.

## 1.3. Criação de um banco de dados
Um servidor PostgreSQL pode gerenciar vários bancos de dados.

Geralmente, é usado um banco de dados diferente para cada projeto.
> Exemplo: Um aplicativo (cliente) de delivery tem seu proprio banco de dados.

Para criar um novo banco de dados, chamado ```meu_banco```, use o seguinte comando:

```bash
$ createdb meu_banco
```
> Nota: É comum chamar um banco de dados de NOME-AMBIENTE, por exemplo; delivery-production

Se este comando não retornar nada, perfeito! Você poderá pular o restante desta seção.

Se você obteve algo semelhante a:

```bash
$ createdb: command not found
```

Então o PostgreSQL não foi instalado corretamente. Tente chamar o comando com o caminho absoluto:

```bash
$ /usr/local/pgsql/bin/createdb meu_banco
```
> Nota: O caminho para o servidor pode ser diferente. Entre em contato o administrador da sua rede ou verifique as instruções de instalação para corrigir o problema.

Se você não quiser mais usar seu banco de dados, poderá excluí-lo. O processo é tão simples quanto criá-lo, basta:

```bash
$ dropdb meu_banco
```
> Nota: Essa ação remove fisicamente todos os arquivos associados ao banco de dados e não pode ser desfeita; portanto, deve ser feito com muito cuidado.

## 1.4. Acesso a um banco de dados
Agora que o banco de dados foi criado, podemos acessa-lo!

Para isso, usaremos o comando ```psql``` que atuará como nosso cliente e permitirá a execução de comandos SQL em nosso servidor.
> Nota: É possível usar ferramentas gráficas como [pgAdmin](https://www.pgadmin.org/) ou [Postico](https://eggerapps.at/postico/) para tornar a experiência mais intuitiva, neste guia, seguiremos com o terminal.

Digite o seguinte comando:

```bash
$ psql meu_banco
```
> Nota: Se você não especificar o nome do banco de dados, o psql usará o nome da sua conta de usuário por padrão.

Você deve obter o seguinte resultado:

```bash
psql (12.3)
Type "help" for help.

meu_banco=#
```

A última linha indica que o psql está "na escuta" e que você pode fazer consultas SQL no espaço de trabalho mantido pelo psql.

Experimente estes comandos:

```bash
meu_banco=# SELECT version();
```

```bash
meu_banco=# SELECT current_date;
```

Eles devem retornar a versão do postgres e a data do servidor, respectivamente.

Para sair do psql, digite:

```bash
meu_banco=# \q
```
> Nota: O psql possui seus próprios comandos, para obter mais informações, digite \h
