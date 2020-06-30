# 1. Início
PostgreSQL é um banco de dados relacional open-source, todas as distribuições encontram-se disponiveis no [site oficial](https://www.postgresql.org/ftp/source/).

## 1.1. Instalação
A instalação do PostgreSQL é simples e na maior parte dos casos, basta baixar e executar o instalador interativo disponivel no [site oficial](https://www.postgresql.org/download/).

Algumas distribuições Unix podem instala-lo por meio do repositorio APT:

```bash
sudo apt-get install postgresql
```

Para instalar uma versao especifica, basta:

```bash
apt-get install postgresql-VERSAO
```

Este guia foi feito com base na versao 11 de PostgreSQL.

## 1.2. Arquitetura
Antes de mais nada, é preciso conhecer um pouco da arquitetura de PostgreSQL.

PostgreSQL é baseado no modelo "cliente/servidor", resultado da cooperaçao dos seguintes processos:

- CLIENTE: Geralmente é um aplicativo desenvolvido pelo usuário, que pretende executar operações no banco de dados. Mas pode também ser um simples console, uma aplicaçao grafica, etc.
- SERVIDOR: Responsável por gerar os arquivos do banco de dados, os clientes se conectam ao servidor, que por sua vez deve executar as ações solicitadas. O servidor é chamado postgres.

Como na maioria dos aplicativos baseados no modelo "cliente/servidor", eles podem ser hospedados em hosts diferentes, nesse caso, eles se comunicam através de uma conexão TCP/IP.

O servidor PostgreSQL pode lidar com vários clientes simultâneamente, para isso, ele inicia um novo processo para cada conexão. O cliente e o novo processo do servidor se comunicam sem intervenção do processo original do postgres, que segue aguardando por novas conexões.

## 1.3. Criação de um banco de dados


## 1.4. Acesso a um banco de dados

