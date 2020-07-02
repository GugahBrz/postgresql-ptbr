# 2. Linguagem SQL
## 2.1 Introdução
O objetivo deste capítulo é fazer uma rápida introdução à linguagem SQL.

Subintende-se que você leu a parte 1 deste guia e conhece a ferramenta ```psql```, bem como criar, acessar e excluir um banco de dados PostgreSQL.

## 2.2 Conceitos básicos
PostgreSQL é um sistema de gerenciamento de banco de dados relacional (SGBDR), ou seja, visa gerenciar dados armazenados em tabelas e relacionados de alguma forma.
> Nota: A noção de dados armazenados em tabelas é tão comum que, às vezes, parece ser o único modelo existente, mas não é verdade. Existem várias arquiteturas diferentes, como por exemplo bancos de dados orientados a documentos e objetos.

Uma tabela é composta de um conjunto de colunas (propriedades) e linhas (valores). Vale lembrar que as linhas de uma tabela não são necessariamente ordenadas (logicamente), mas poderemos ordenar-las como quisermos durante uma consulta.

Essas tabelas são agrupadas em um banco de dados e um conjunto de bancos de dados gerenciados por uma única instância do servidor PostgreSQL, constituindo assim uma instância de banco de dados ou o que chamamos de <i>cluster</i>.

## 2.3 Criação de uma tabela
Você pode criar uma nova tabela especificando o nome dela, seguido pelo nome das colunas e seus respectivos tipos:

```SQL
CREATE TABLE usuarios (
    id            int,
    nome          varchar(80),    -- Um comentario daora
    dt_nasc       date            -- Outro comentario daora
);
```
> Nota: É possível executar este comando no psql, a ferramenta reconhecerá a quebra de linha até encontrar o ponto e vírgula.

O PostgreSQL suporta os tipos padrão de SQL: int, smallint, real, double, char(N), varchar(N), date, time, timestamp e interval, além de outros tipos de utilitários gerais e um rico conjunto de tipos geométricos.

Como visto no exemplo, comentários podem ser feitos usando ```--```, todo o conteúdo escrito após esse sinal será ignorado.

Por fim, mas não menos importante, se não precisar mais de uma tabela, poderá excluí-la usando o seguinte comando:

```SQL
DROP TABLE usuarios;
```

## 2.4 Inserção de dados (INSERT)
O comando ```INSERT``` é usado para preencher uma tabela com linhas:

```SQL
INSERT INTO usuarios VALUES (1, 'John Joe', '1997-02-26');
INSERT INTO usuarios VALUES (2, 'Yara Kazman', '1996-02-26');
INSERT INTO usuarios VALUES (3, 'Gilles Riquier', '1995-02-26');
```

As constantes que não são valores numéricos simples geralmente devem estar entre aspas simples, como no exemplo.
O tipo de data é um dos mais flexíveis e aceita diversos formatos, neste caso, usaremos o formato americano.

Note que, essa sintaxe nos força a lembrar a ordem das colunas, bem como informar um valor para cada uma delas.
Uma sintaxe alternativa permite listar explicitamente as colunas desejadas:

```SQL
INSERT INTO usuarios (id, nome)
    VALUES (1, 'John Joe');
```

Muitos desenvolvedores consideram a segunda alternativa um estilo melhor do que depender de ordem implícita.

## 2.5 Consultas (SELECT)
Os valores de uma tabela podem ser consultados através de uma instrução chamada ```SELECT```.
Por exemplo, para consultar todas as linhas na tabela ```usuarios```:

```SQL
SELECT * FROM usuarios;
```

O sinal ```*``` quer dizer "todas as colunas", trata-se de outra forma de escrever o comando a seguir:

```SQL
SELECT id, nome, dt_nasc FROM usuarios;
```

O resultado obtido deve ser algo como:

```SQL
 id |      nome      |   dt_nasc
----+----------------+-----------
 1  | John Joe       | 1997-02-26
 2  | Yara Kazman    | 1996-02-26
 3  | Gilles Riquier | 1997-02-26

(3 rows)
```

É possivel escrever expressões e/ou usar métodos SQL existentes, como ```count()```, por exemplo:

```SQL
SELECT count(id) AS count FROM usuarios;
```

O resultado esperado é:

```SQL
 count
------
     3

(1 row)
```

Observe como ```AS``` é usado para nomear a coluna gerada a partir do método count.

É possivel adicionar condiçoes a consulta por meio da cláusula ```WHERE```, ela é baseada em uma expressao booleana e somente as linhas que atenderem essa condiçao serao retornadas. Podemos ainda utilizar os operadores ```AND```, ```OR``` e ```NOT```para complementar, por exemplo:

```SQL
SELECT * FROM usuarios
    WHERE id > 1 AND id <> 2;
```
    
O resultado esperado é:

```SQL
 id |      nome      |   dt_nasc
----+----------------+-----------
 3  | Gilles Riquier | 1997-02-26

(1 row)
```

Você pode solicitar que os resultados de uma consulta sejam retornados em uma ordem específica:

```SQL
SELECT * FROM usuarios
ORDER BY nome;
```

```SQL
 id |      nome      |   dt_nasc
----+----------------+-----------
 3  | Gilles Riquier | 1997-02-26
 1  | John Joe       | 1997-02-26
 2  | Yara Kazman    | 1996-02-26

(3 rows)
```

E que as linhas duplicadas sejam removidas do resultado de uma consulta:

```SQL
SELECT DISTINCT dt_nasc
    FROM usuarios;
```

```SQL
    dt_nasc
-----------
 1997-02-26
 1996-02-26

(2 rows)
```

## 2.6 Ligações (JOIN)
Neste tópico, criaremos uma segunda tabela chamada ```veículos```, na qual atribuiremos veículos de um determinado tipo aos usuários.

```SQL
CREATE TABLE veiculos (
    id            int,
    tipo          varchar(80),    -- carro, moto
    dono          varchar(80)
);
```
> Nota: Leia um pouco sobre [Enumerated Types](https://www.postgresql.org/docs/9.1/datatype-enum.html), poderia ser util no campo tipo.

Em seguida, vamos inserir alguns registros:

```SQL
INSERT INTO veiculos VALUES (1, 'carro', 'John Joe');
INSERT INTO veiculos VALUES (2, 'moto', 'Camille Rivard');
INSERT INTO veiculos VALUES (3, 'carro', 'Delphine Chartier');
```
Agora, podemos descobrir quais usuários do nosso sistema possuem (ou não) um veículo!

### INNER JOIN
Provavelmente o método mais conhecido e utilizado, retorna somente os registros comuns às duas (ou mais) tabelas.

```SQL
SELECT usuarios.nome, veiculos.tipo
FROM usuarios
INNER JOIN veiculos
    ON usuarios.nome = veiculos.dono
```

```SQL
     nome      |    tipo
---------------+-----------
   John Joe    |   carro
 
 (1 row)
```

### LEFT JOIN
Retorna todos os registros da tabela base (LEFT) e os registros comuns às duas (ou mais) tabelas.

```SQL
SELECT usuarios.nome, veiculos.tipo
FROM usuarios
LEFT JOIN veiculos
    ON usuarios.nome = veiculos.dono
```

```SQL
      nome       |    tipo
-----------------+----------
    John Joe     |   carro
   Yara Kazman   |    NULL
  Gilles Riquier |    NULL
 
 (3 rows)
```

### RIGHT JOIN
Retorna todos os registros das demais tabelas (RIGHT) e os registros comuns às duas (ou mais) tabelas.

```SQL
SELECT usuarios.nome, veiculos.tipo
FROM usuarios
RIGHT JOIN veiculos
    ON usuarios.nome = veiculos.dono
```

```SQL
      nome       |    tipo
-----------------+----------
    John Joe     |   carro
      NULL       |    moto
      NULL       |   carro
 
 (3 rows)
```

### FULL OUTER JOIN
Retorna todos os registros nas tabelas relacionadas, independentemente de registros comuns ou não.

```SQL
SELECT usuarios.nome, veiculos.tipo
FROM usuarios
FULL OUTER JOIN veiculos
    ON usuarios.nome = veiculos.dono
```

```SQL
      nome       |    tipo
-----------------+----------
    John Joe     |   carro
   Yara Kazman   |    NULL
  Gilles Riquier |    NULL
      NULL       |    moto
      NULL       |   carro
 
 (5 rows)
```

Vale ressaltar que ainda existem três outros tipos de ligação:
- LEFT EXCLUDING JOIN
- RIGHT EXCLUDING JOIN
- OUTER EXCLUDING JOIN

Apesar de serem utilizados com menor frequencia, vale a pena pesquisar um pouco a respeito.

## 2.7 Edição (UPDATE)
## 2.8 Exclusão (DELETE)
