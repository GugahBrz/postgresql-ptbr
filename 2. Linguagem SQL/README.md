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
## 2.6 Ligações (JOIN)
## 2.7 Funções de agregação
## 2.8 Edição (UPDATE)
## 2.9 Exclusão (DELETE)
