# 3. Avançado
Antes de continuar, é importante que você domine os assuntos abordados nos tópicos anteriores, ou nada fará sentido daqui em diante.

Caso contrário, não hesite em reler os tópicos anteriores e fazer os exercícios. Se algo não estiver claro o suficiente, abra uma discussão na guia [Issues](https://github.com/GugahBrz/postgresql-ptbr/issues), responderei o mais breve possível!

## 3.1 Introdução
Neste capítulo, procuraremos explorar funções avançadas de ```PostgreSQL``` e ```SQL```, a fim de simplificar o gerenciamento dos dados e garantir a execução segura de determinadas operações.

## 3.2 Visualizações
As visualizações são como tabelas virtuais, podemos criar uma visualização selecionando campos de uma ou mais tabelas presentes no banco.

Na seção [INNER JOIN](https://github.com/GugahBrz/postgresql-ptbr/tree/master/parte-2#inner-join) do tópico ligações, associamos as tabelas ```usuarios``` e ```veiculos```, no intuito de encontrar usuarios que possuam veiculos.

Suponha que seja uma consulta recorrente, para evitar reescrever essa consulta o tempo todo, podemos criar uma exibição chamada ```usuarios_com_veiculos```:
```SQL
CREATE VIEW usuarios_com_veiculos AS
  SELECT usuarios.nome, veiculos.tipo
  FROM usuarios
  INNER JOIN veiculos
      ON usuarios.nome = veiculos.dono
```

E então, simplesmente:
```SQL
SELECT * FROM usuarios_com_veiculos;
```
O uso de visualizações é considerado por muitos como um aspecto essencial de uma boa concepção de banco de dados, pois permite encapsular os detalhes e a estrutura das tabelas, bem como evitar erros humanos ao executar certas consultas.

> Nota: Uma visualização pode ser criada com base não apenas em tabelas, mas também em outras visualizações!

## 3.3 Chaves estrangeiras
Imagine agora que existem dois usuários chamados ```John Joe```, como podemos identificar qual veículo pertence a quem?

No estado atual das tabelas, não será possível saber.

Você deve ter notado a presença da coluna ```id``` na tabela ```usuarios```, ela será nossa chave, que posteriormente poderá ser usada como chave estrangeira para associar com segurança registros entre tabelas.

Para continuar, vamos excluir as tabelas usando o comando [DROP](https://github.com/GugahBrz/postgresql-ptbr/tree/master/parte-2#23-cria%C3%A7%C3%A3o-de-uma-tabela) e em seguida, recriar-las da seguinte maneira:

```SQL
CREATE TABLE usuarios (
    id            int primary key,
    nome          varchar(80),
    dt_nasc       date
);

CREATE TABLE veiculos (
    id            int primary key,
    tipo          varchar(80),      -- carro, moto
    dono_id       int references usuarios
);
```

Pontos importantes:

- Uso da sigla ```primary key``` na coluna ```id``` de cada tabela;
- Uso da sigla ```references usuarios``` na coluna ```dono_id```.

Isso quer dizer que, a coluna ```dono_id``` faz agora referencia ao ```id``` de um usuario.

Vamos popular a tabela ```usuarios``` outra vez:

```SQL
INSERT INTO usuarios VALUES (1, 'John Joe', '1997-02-26');
INSERT INTO usuarios VALUES (2, 'Yara Kazman', '1996-02-26');
INSERT INTO usuarios VALUES (3, 'Gilles Riquier', '1995-02-26');
```

E adicionar um veiculo:

```SQL
INSERT INTO veiculos VALUES (1, 'carro', 1);
```

> Nota: De agora em diante, todo tipo de associação deve ser feita utilizando o id, ou seja: usuarios.id = veiculos.dono_id

Se tentarmos adicionar um veículo referenciando um usuário inexistente, SQL retornará um erro:

```SQL
INSERT INTO veiculos VALUES (2, 'carro', 999);
```
```bash
ERROR:  insert or update on table "veiculos" violates foreign key constraint "veiculos_usuario_fkey"
DETAIL : Key (usuario)=(a) is not present in table "usuarios".
```

Ou seja, o usuário/dono com ```id = 999``` não foi encontrado.

> Nota: Chaves estrangeiras é um tópico extremamente abordado no dia a dia de quem trabalha com SQL, este guia fornece uma visão muito superficial do assunto, sugiro que você pesquise mais sobre.

## 3.4 Transações
As transações são um conceito fundamental de banco de dados. Uma transação combina vários estágios em uma única operação de "tudo ou nada". Ou seja, se ocorrer uma falha que impeça o sucesso de um ou mais estágios da transação, nenhuma alteração será aplicada.

Em um contexto real, é comum que dezenas de operações sejam executadas durante a criação/edição/exclusão de um registro, e um erro, por menor que seja, durante a execução de uma dessas operações é suficiente para gerar diversas inconsistências na base de dados, portanto, mediante um erro, nosso banco volta ao estado inicial.

No PostgreSQL, uma transação é declarada envolvendo uma operação SQL com os comandos ```BEGIN``` e ```COMMIT```:

```SQL
BEGIN;
UPDATE tabela SET coluna = valor
    WHERE id = 1;
-- etc etc
COMMIT;
```
> Nota: Isso é feito automaticamente pelo PostgreSQL! Portanto, a menos que você queira personalizar suas transações, não há necessidade de especificá-las.

É possivel quebrar uma transaçoes em varias partes usando os comandos ```SAVEPOINT``` e ```ROLLBACK TO```, neste caso, criamos "checkpoints" e retornamos a eles 

```SQL
BEGIN;
UPDATE tabela SET coluna = valor
    WHERE id = 1;
SAVEPOINT meu_checkpoint;
UPDATE tabela SET coluna = valor
    WHERE id = 2;
-- ops, algo deu errado...
ROLLBACK TO meu_checkpoint;
UPDATE tabela SET coluna = valor
    WHERE id = 3;
COMMIT;
```

> Nota: Mais uma vez, assunto muito importante, sugiro que você pesquise mais sobre.

## 3.5 Funções de janela
## 3.6 Herança
