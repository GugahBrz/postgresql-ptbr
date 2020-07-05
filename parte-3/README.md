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
## 3.4 Transações
## 3.5 Funções de janela
## 3.6 Herança
