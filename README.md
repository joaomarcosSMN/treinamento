# Anotações VideoAulas YouTube T-SQL

> Anotações da playlist: [Curso de SQL Server (T-SQL)](https://www.youtube.com/playlist?list=PLucm8g_ezqNqI5cW3alteV5olcMCcHYRK)

> Fiquem a vontade para revisar, conferir informações e edita-las se necessário

**Grupos:**
- DDL : create, alter, drop 
- DML : insert, update, delete
- DCL : grant, revoke
- DQL : select

campos = coluna
registros = linhas (ou tuplas)

**Tipos de dados:**
- char : string tamanho fixo, max 8000 caracteres
- varchar : string tamanho variavel, max 8000 caracteres
- nchar : unicode, string tamanho fixo, max 4000 caracteres
- nvarchar : unicode,  string tamanho variavel, max 4000 caracteres
- bit : 0, 1, nulo
- tinyint : num inteiro de 0 a 255 (1 byte)
- smallint : num inteiro -32768 a 32767 (2 bytes)
- int : num inteiro -2147483648 a 2147483648 (4 bytes)
- bigint (8 bytes)
- real (4 bytes)
- datetime (8 bytes)
- smalldatetime (4 bytes)
- date (3 bytes)
- time (3-5 bytes)
- text
- money (8 bytes)

```
CREATE DATABASE db_Biblioteca
ON PRIMARY (
NAME = db_Biblioteca,
FILENAME='C:\SQL\db_Biblioteca.MDF',
SIZE=6MB,
MAXSIZE=15MB,
FILEGROWTH=10%
)

USE db_Biblioteca
```

CTRL + K + CTRL + C => comentar linhas
CTRL + K + CTRL + U => descomentar linhas

sp_helpdb db_Biblioteca
=> mostra configurações da database (path do filename por exemplo)


**Constraints (Restrições)**
- regras nas colunas
- podem ser especificadas no momento de criação da table ou apos ter sido criada

- NOT NULL
- UNIQUE (PK ja tem caracteristica unique, mas chaves nao primarias podem ser unique)
- PK (nao pode ser null)
- FK (aponta para uma PK de outra table)

```
CONSTRAINT fk_ID_Autor FOREIGN KEY (ID_Autor) REFERENCES tbl_autores(ID_Autor)
```

- CHECK (limita uma faixa de valores que podem ser colocados numa coluna (quando esta definido em um unica coluna), tmb pode ser definida para uma tabela, limitando valores em mais de uma coluna)
- DEFAULT (caso nenhum outro valor seja especificado, assume este valor default)

```
CREATE TABLE tbl_Livro 
(ID_Livro SMALLINT PRIMARY KEY IDENTITY(100,1),
Nome_Livro VARCHAR(50) NOT NULL,
ISBN VARCHAR(30) NOT NULL UNIQUE,
ID_Autor SMALLINT NOT NULL,
Data_Pub DATETIME NOT NULL,
Preco_Livro MONEY NOT NULL)
```

IDENTITY(100,1) => começa em 100 e vai subindo de 1 em 1

```
CREATE TABLE tbl_autores(
ID_Autor SMALLINT PRIMARY KEY,
Nome_Autor VARCHAR(50),
Sobrenome_Autor VARCHAR(60)
)

CREATE TABLE tbl_editoras (
ID_Editora SMALLINT PRIMARY KEY IDENTITY, 
Nome_Editora VARCHAR(50) NOT NULL
)

sp_help tbl_autores
```

**AUTO INCREMENT**
- no SQL SERVER trata-se da palavra chave IDENTITY
- pode usar uma coluna de identidade por tabela

```
CREATE TABLE tbl_teste_identidade
(ID_Teste SMALLINT PRIMARY KEY IDENTITY,
valor SMALLINT NOT NULL)
```

**ALTER TABLE - DROP**

```
ALTER TABLE tbl_Livro
DROP COLUMN ID_Autor

ALTER TABLE tabela
DROP CONSTRAINT nome_constraint

ALTER TABLE tbl_Livro
ADD ID_Autor SMALLINT NOT NULL
CONSTRAINT fk_ID_Autor FOREIGN KEY (ID_Autor) REFERENCES tbl_autores

ALTER TABLE tbl_Livro
ADD ID_editora SMALLINT NOT NULL
CONSTRAINT fk_id_editora FOREIGN KEY (ID_editora) REFERENCES tbl_editoras
```

```
ALTER TABLE tbl_Livro
ALTER COLUMN ID_Autor SMALLINT 
-- se tiver info inseridas é mais complicado

ALTER TABLE Clientes
ADD PRIMARY KEY (ID_Cliente)
-- ID_Cliente deve existir antes de ser transformada em PK

DROP TABLE <nome_tabela>
-- caso tenha relacionamentos e constraints, deve-se excluir estes primeiro antes de dropar a table
```

```
INSERTO INTO tablea (col1, col2) VALUES (valor1, valor2)

INSERT INTO tbl_Autores (ID_Autor, Nome_Autor, SobreNome_Autor) VALUES (1, 'Daniel', 'Barret')

INSERT INTO tbl_Autores (ID_Autor, Nome_Autor, SobreNome_Autor) VALUES (2, 'Gerald', 'Carter')

INSERT INTO tbl_Autores (ID_Autor, Nome_Autor, SobreNome_Autor) VALUES (3, 'Mark', 'Sobell')

INSERT INTO tbl_Autores (ID_Autor, Nome_Autor, SobreNome_Autor) VALUES (4, 'William', 'Stanek')

INSERT INTO tbl_Autores (ID_Autor, Nome_Autor, SobreNome_Autor) VALUES (5, 'Richard', 'Blum')
```

```
INSERT INTO tbl_editoras (Nome_Editora) VALUES ('Prentice Hall')

INSERT INTO tbl_editoras (Nome_Editora) VALUES ('O'Reilly')

INSERT INTO tbl_editoras (Nome_Editora) VALUES ('Microsoft Press')

INSERT INTO tbl_editoras (Nome_Editora) VALUES ('Wiley')
```

```
INSERT INTO tbl_Livro (Nome_Livro, ISBN, Data_Pub, Preco_Livro, ID_Autor, ID_Editora) VALUES ('Linux Command Line and Shell Scripting', 143894894, '20091221', 68.35, 5, 4)

INSERT INTO tbl_Livro (Nome_Livro, ISBN, Data_Pub, Preco_Livro, ID_Autor, ID_Editora) VALUES ('SSH, the Secure Shell', 175877444, '20091221', 58.30, 1, 2)

INSERT INTO tbl_Livro (Nome_Livro, ISBN, Data_Pub, Preco_Livro, ID_Autor, ID_Editora) VALUES ('Using Samba', 178554614, '20001221', 61.45, 2, 2)
```

```
INSERT INTO tbl_Livro (Nome_Livro, ISBN, Data_Pub, Preco_Livro, ID_Autor, ID_Editora) VALUES ('Fedora and Red Hat Linux', 125787881, '20101101', 62.24, 3, 1)

INSERT INTO tbl_Livro (Nome_Livro, ISBN, Data_Pub, Preco_Livro, ID_Autor, ID_Editora) VALUES ('Windows Server 2012 Inside Out', 123356789, '20040517', 66.80, 4, 3)

INSERT INTO tbl_Livro (Nome_Livro, ISBN, Data_Pub, Preco_Livro, ID_Autor, ID_Editora) VALUES ('Microsoft Exchange Server 2010', 123366789, '20001221', 45.30, 4, 3)
```

**TRUNCATE TABLE**
- permite excluir todos os dados da tabela
- como DELETE sem usar a clausula WHERE

```
TRUNCATE TABLE Clientes 
-- apenas os dados são excluidos, a tabela permanece

```

**CONSULTA**
```
SELECT coluna FROM tabela

SELECT Nome_Autor FROM tbl_autores
SELECT * FROM tbl_autores
SELECT Nome_Livro FROM tbl_Livro
```

```
SELECT Nome_Livro, ISBN
FROM tbl_Livro
ORDER BY Nome_Livro

SELECT Nome_autor, Sobrenome_autor
FROM tbl_autores
ORDER BY Sobrenome_autor
```

**ORDER BY**
- ASC (por padrão)
- DESC

```
SELECT * FROM tbl_Livro 
ORDER BY Nome_Livro DESC

-- consulta de valores diferentes
SELECT DISTINCT ID_Autor
FROM tbl_Livro
```

**WHERE**
```
SELECT * FROM tbl_Livro WHERE ID_Autor='1'
```

**AND e OR**
- filtra registros baseados em mais de uma condição
```
SELECT * FROM tbl_Livro
WHERE ID_Livro > 2 OR ID_Autor < 3
```

**UPDATE**
```
UPDATE tabela 
SET coluna = valor
WHERE 'filtro'
-- sem where vai atualizar todos os dados da tabela
```

```
UPDATE tbl_Livros
SET Preco_Livro = 65.45
WHERE ID_Livro = 102

UPDATE tbl_autores
SET Sobrenome_Autor = 'Cartman'
WHERE ID_Autor = 2

UPDATE tbl_Livro
SET Preco_Livro = 80.00, ISBN = '20202002'
WHERE ID_Livro = 102
```

**SELECT TOP**
- especifica o numero de registros a retornar
- SELECT TOP numero|percentual colunas FROM tabela

```
SELECT TOP 3 Nome_Livro FROM tbl_Livro

-- com percent = 10% 
SELECT TOP 10 PERCENT Nome_Livro FROM tbl_Livro
```

**ALIAS**
```
SELECT colunas AS nome_alias FROM tabela
SELECT Nome_Livro AS Livro, ID_autor AS Autor FROM tbl_Livro
```

**UNION**
- combina duas ou + declarações SELECT
- cada declaracao deve ter o msm numero de colunas, tipos de dados e ordem das colunas
```
SELECT ID_Autor FROM tbl_autores UNION
SELECT ID_Autor FROM tbl_Livro
```
- UNION ALL => traz todos e com repetições

```
SELECT Nome_Autor FROM tbl_autores
UNION
SELECT Nome_Livro FROM tbl_Livro
```

**SELECT INTO**
- seleciona dados de uma ou mais tabelas e os insere em uma tabela diferente
- usada para backups

```
SELECT ID_Livro, Nome_Livro, ISBN 
INTO Livro_ISBN
FROM tbl_Livro
WHERE ID_Livro > 2
```
```
-- Backup:
SELECT *
INTO tbl_Livro_backup
FROM tbl_Livro
```


**Funções Agregadas**
- MIN
- MAX
- AVG
- SUM
- COUNT

```
SELECT COUNT(*) FROM tbl_Livro
SELECT MAX/MIN/AVG/SUM(Preco_Livro) FROM tbl_Livro
SELECT MAX (Preco_Livro)  AS PrecoMaximo FROM tbl_Livro
```

**BETWEEN**

```
SELECT * FROM tbl_Livro WHERE Data_Pub BETWEEN '20040517' AND '20100517'

SELECT Nome_Livro AS Livro, Preco_Livro AS Preço 
FROM tbl_Livro
WHERE Preco_Livro BETWEEN 40.00 AND 60.00
```

**LIKE e NOT LIKE**
- determina se uma cadeia de caracteres especifica corresponde a um padrao especificado. Um padrao pode incluir caracters normais e curingas
- not like : inverte a comparação verificando a NÃO correspondencia
- usado com WHERE
- WHERE coluna LIKE padrao
- uso:
 - '%' - qualquer cadeia de 0 ou mais caracteres
 - '_' - qlqr caracter unico
 - '[ ]' - qlqr caracter unico no intervalo ou conjunto especificado (`[a-h]`; `[aeiou]`)
 - '[^]' - qlqr caracter unico  que nao esteja no intervalo ou conj especificado (`[a-h]`; `[aeiou]`)

```
SELECT Nome_livro
FROM tbl_livro
WHERE Nome_Livro LIKE 'S%'

SELECT Nome_livro
FROM tbl_livro
WHERE Nome_Livro LIKE '_i%'
```
```
-- nomes que começam com s ou l
SELECT Nome_livro
FROM tbl_livro
WHERE Nome_Livro LIKE '[sl]%'

SELECT Nome_livro
FROM tbl_livro
WHERE Nome_Livro LIKE '_i__o%'

SELECT Nome_livro
FROM tbl_livro
WHERE Nome_Livro NOT LIKE 'm%'
```

**JOINS**
- para obter dados provenientes de duas ou mais tabelas, baseado em um relacionamento entre colunas nestas tabelas
- INNER JOIN: retorna linhas quando houver pelo menos uma correspondencia em ambas as tabelas (PK em uma, FK em outra tabela)
- OUTER JOIN: retorna linhas mesmo quando nao houver pelo menos uma correspondencia em uma das tabelas (ou ambas). Divide-se em:
  - LEFT JOIN
  - RIGHT JOIN
  - FULL JOIN

```
SELECT colunas
FROM tabela 1
INNER JOIN tabela 2
ON tabela1.coluna = tabela2.coluna

SELECT * FROM tbl_Livro 
INNER JOIN tbl_autores
ON tbl_Livro.ID_Autor = tbl_autores.ID_Autor
--  traz os valores das duas tabelas, e organiza a linha baseada no ID_Autor
```

```
SELECT tbl_Livro.Nome_Livro, tbl_Livro.ISBN, tbl_autores.Nome_Autor
FROM tbl_Livro
INNER JOIN tbl_autores
ON tbl_Livro.ID_Autor = tbl_autores.ID_Autor

SELECT L.Nome_Livro, E.Nome_editora
FROM tbl_Livro AS L
INNER JOIN tbl_editoras AS E
ON L.ID_editora = E.ID_editora

-- o codigo é compilado, e é checado se há erro em alguma linha, por isso é possivel usar L.Nome_Livro antes mesmo de declarar a tabela Livro como L
```

**INNER JOIN**
- como se fosse uma interseção entre conjuntos

**OUTER JOINS**
- LEFT JOIN: retorna todas as linhas da tabela à esquerda, mesmo se não houver nenhuma correspondência na tabela à direita
- RIGHT JOIN: retorna todas as linhas da tabela à direita, mesmo se não houver nenhuma correspondencia na tabela à esquerda
- FULL JOIN: retorna linhas quando houver uma correspondencia em qualquer uma das tabelas. É uma combinação de LEFT e RIGHT JOINS
- obs: tabela que usou primeiro para o relacionamento = tabela da esquerda

```
SELECT coluna
FROM tab_esq
LEFT (OUTER) JOIN tab_dir
ON tab_esq.coluna = tab_dir.coluna

SELECT * FROM tbl_Livro
LEFT JOIN tbl_autores
ON tbl_Livro.ID_Autor = tbl_autores.ID_Autor
```

- a vantagem de usar o LEFT JOIN é que além de trazer os dados da tbl_esquerda, tmb traz os relacionamentos com a tbl_direita (no caso, select * from tbl_esquerda, traria apenas os dados)
- RIGHT JOIN => mesma lógica, só inverte o lado das tabelas
- para trazer os dados excluindo as correspondencias:

```
SELECT coluna
FROM tab_esq
LEFT (OUTER) JOIN tab_dir
ON tab_esq.coluna = tab_dir
WHERE tab_dir.coluna IS NULL
```

**FULL JOIN**
- combina RIGHT e LEFT JOIN; traz todos os dados
- indica quais dados tem correspondencia entre si
```
SELECT coluna
FROM tab_esq
FULL (OUTER) JOIN tab_dir
ON tab_esq.coluna = tab_dir.coluna
```

- FULL JOIN excluindo correspondencias:
```
SELECT coluna
FROM tab_esq
FULL (OUTER) JOIN tab_dir
ON tab_esq.coluna = tab_dir.coluna
WHERE tab_esq.coluna IS NULL
OR tab_dir IS NULL
```

**IN e NOT IN**
- determina se um valor corresponde a qualquer valor uma subsconsulta ou lista
- retorna sempre TRUE ou FALSE
- pode substituir o OR em queries com mais de uma condição

```
SELECT * FROM tbl_Livro
WHERE ID_Autor IN (1,2)
SELECT * FROM tbl_Livro
WHERE ID_Autor NOT IN (1,2)
```

**CAMPO CALCULADO**
- realiza calculo de forma automatica e guarda o resultado em alguma coluna

```
CREATE TABLE Produtos (
codProduto smallint, 
NomeProduto varchar(20),
Preco money,
Quant smallint,
Total As (Preco * Quant)
)

INSERT INTO Produtos VALUES (1, 'Mouse', 15.00, 2)
INSERT INTO Produtos VALUES  (2, 'Teclado', 18.00, 1)
INSERT INTO Produtos VALUES  (3, 'Fones', 25.00, 1)
INSERT INTO Produtos VALUES  (4, 'Pendrive', 25.00, 3)
INSERT INTO Produtos VALUES  (5, 'SD card', 29.00, 2)
INSERT INTO Produtos VALUES  (6, 'DVD-R', 1.30, 12)

SELECT SUM(Total) FROM Produtos
```


**INDICES**
- permite que as aplicacoes de BD encontrem os dados mais rapidamente, sem ter que ler a tabela toda.
- os users nao vem os indices (?)
- apenas criar em tabelas que recebam muitas consultas; tabelas indexadas levam mais tempo para serem atualizadas
```
CREATE INDEX nome_index 
ON nome_tbl (nome_coluna)
```

**REGRAS**
- sao configuracoes que permitem especificar como determinados parametros do banco de dados devem se comportar, como por exemplo, limitar faixas de valores em colunas, ou especificar valores invalidos para registros

```
CREATE RULE nome_regra AS parametros_da_regra

CREATE RULE rl_preco AS @VALOR > 10.00
-- @ para criar uma variavel

-- EXECUTE SP_BINDRULE rl_preco, 'tbl_Livro.Preco_Livro'
```
- stored procedure (codigo pre-armazenado que executa algum tipo de ação)
- bindrule : vincula alguma regra, apos há a indicação da regra e da tabela.coluna para fazer isso


**BACKUP DE BD E RESTAURAÇÃO**
```
BACKUP DATABASE test
TO DISK = 'C:\projetos\test.bak'
GO
```

**CONCATENÇÃO DE STRINGS**
- string1 | coleuna + string2 | coluna
```
SELECT 'Fábio ' + 'da Boson Treinamentos' AS Boson
SELECT Nome_autor, Sobrenome_autor FROM tbl_autores

SELECT Nome_autor + ' ' + Sobrenome_autor AS 'Nome Completo'
FROM tbl_autores 

SELECT 'Eu gosto do livro ' + Nome_Livro AS 'Meu Livro' FROM tbl_Livro WHERE ID_autor = 2
```

**COLLATION (Colação ou Agrupamento)**
- trata-se da codificação dos caracteres em uma ordem padrão
- muitos sistemas de colação são baseados em ordem numerica ou alfabetica
- a colação usada pelos bancos de dados é configurada durante a instação do sistema
- collation-charts.org (site com cartas de agrupamento em varios alfabetos e idiomas)

```
-- opções de agrupamento
SELECT * FROM fn_helpcollations()

-- esquema de colação usada atualmente
SELECT SERVERPROPERTY('Collation') AS 'Colação'

-- alterar o esquema de agrupamento
ALTER DATABASE db_Biblioteca
COLLATE Greek_CI_AI

-- verifica o esquema de uma tabela
SELECT DATABASEPROPERTYEX('db_Biblioteca') 

-- alterar em nivel de coluna
SELECT * FROM tbl_Livro
ORDER BY Nome_Livro
COLLATE Icelandic_CI_AI
```

**CLAUSULA WITH TIES**
```
SELECT TOP(3) NOME_TIME, Pontos
FROM tbl_times
ORDER BY Pontos DESC
```
- acima retornou 3 times, porém, no exemplo, o 3o e o 4o possuem a mesma pontuação;
- o 4o time não apareceu, pois, pelo contexto do banco, um foi cadastrado antes que o outro e por isso o 3o teve prioridade para aparecer

```
SELECT TOP(3) WITH TIES NOME_TIME, Pontos
FROM tbl_times
ORDER BY Pontos DESC
```
- mostra 4 times, pois 3o e 4o possuem o mesmo valor
- WITH TIES sempre é utilizada com ORDER BY


**VIEWS - Criar, Alterar e Excluir**
- tabela virtual baseada no conjunto de resultados de uma consulta SQL

```
CREATE VIEW [nome_exibição]
AS SELECT colunas
FROM tabela
WHERE condições
```
```
CREATE VIEW vw_LivrosAutores
AS SELECT tbl_Livro.Nome_Livro AS Livro,
tbl_autores.Nome_Autor AS Autor
FROM tbl_Livro
INNER JOIN tbl_autores
ON tbl_Livro.ID_Autor = tbl_autores.ID_Autor

SELECT Livro, Autor
FROM vw_LivrosAutores
-- WHERE Livro LIKE 'S%'
```

- alterando e dropando VIEW
```
ALTER VIEW vw_LivrosAutores
AS SELECT tbl_Livro.Nome_Livro AS Livro,
tbl_autores.Nome_Autor AS Autor, Preco_Livro AS Valor
FROM tbl_Livro
INNER JOIN tbl_autores
ON tbl_Livro.ID_Autor = tbl_autores.ID_Autor

DROP VIEW vw_LivrosAutores
```

**SubQueries (Subconsultas) com Tabelas Derivadas**
- eh uma declaração SQL embutida em uma consulta externa
- fornece uma resposta a consulta externa na forma de um valor escalar, lista de valores, ou conjunto de dados, equivalentes a uma expressao, lista ou tabela para a consulta externa

```
SELECT (SELECT 'Fábio') AS Subconsulta;
```
- Fora do () é a consulta externa, o que esta dentro é a consulta interna (subconsulta)
- o resultado da consulta interna passa para a consulta externa
- Exemplo: 3 tabelas: cliente, produto e compra; associa preco, produto e quantidade para retornar o total do preço, e a lista de compras com clientes. Cada cliente comprou mais de um vez, entao, a subconsulta fará com que saiba o total gasto por cada cliente
- Em outros exemplos da aula, pegou toda a consulta passada e colocou dentro de uma subconsulta

```
SELECT Resultado.Cliente, SUM(Resultado.Total) AS Total
FROM
(SELECT CL.Nome_Cliente AS Cliente, PR.Preco_Produto * CO.Quantidade AS TOTAL
FROM Cliente AS CL
INNER JOIN Compras AS CO
ON CL.ID_Cliente = CO.ID_Cliente
INNER JOIN Produtos AS PR
ON CO.ID_Produto = PR.ID_Produto) AS Resultado
GROUP BY Resultado.Cliente
ORDER BY Total
```

**CTE - COMMON TABLE EXPRESSION (subconsultas)**
- variação sintática de uma subsconsulta, similar a uma exibição (view)
- pode ser acessada multiplas vezes dentro da consulta principal, como se fosse uma exibição ou tabela
- pq GROUP BY nem sempre funciona? nem todos os valores envolvidos na consulta sao agrupaveis, ou seja, nao estão na lista de seleção

```
WITH ConsultaCTE (Cliente, Total)
AS (SELECT CL.Nome_Cliente AS Cliente, PR.Preco_Produto * CO.Quantidade AS Total
FROM Clientes AS CL
INNER JOIN Compras AS CO
ON CL.ID_CLIENTE = CO.ID_Cliente
INNER JOIN Produtos AS PR
ON CO.ID_Produto = PR.ID_Produto)

SELECT Cliente, SUM(TOTAL) AS ValorTotal
FROM ConsultaCTE
GROUP BY Cliente
ORDER BY ValorTotal
```

**VARIAVEIS NO SQL SERVER**
- podem ser declaradas no corpo de um batch ou procedimento
- pode-se atribuir-lhes valores usando-se declarações SET ou SELECT
- variaveis sao inicializadas por padrao com NULL

```
DECLARE @nome_var tipo
DECLARE @texto VARCHAR(40), @valor INT
```

```
-- Atribuir valores com SET:
SET @valor = 50
SET @texto = 'Boson'
SET @data_nac = GETDATE()

-- Usando Aliases:
SELECT @valor AS Valor, @texto AS Texto
```

- Atribuir valor com SELECT (caso o select retorne multiplos dados, vai ser setado o ultimo valor):
```
SELECT nome_var = coluna FROM tabela WHERE condição SELECT nome_var AS alias

DECLARE @livro VARCHAR(30)

SELECT @livro = nome_livro
FROM tbl_livro
WHERE ID_Livro = 101
SELECT @livro AS 'Nome do Livro'
```
```
DECLARE @preco MONEY, @quantidade INT, @nome VARCHAR(30)
SET @quantidade = 5

SELECT @preco = Preco_Livro, @nome = Nome_Livro
FROM tbl_livro
WHERE ID_Livro = 101

SELECT @nome AS 'Nome do Livro',
@preco * @quantidade AS 'Preço dos Livros'
```

**CONVERSÃO DE TIPOS DE DADOS NO SQL SERVER COM CAST E CONVERT**
- CAST:
```
CAST(expressao AS novo_tipo_dados)

SELECT 'O preço do livro' + Nome_Livro + ' é de R$ ' + 
CAST(Preco_Livro AS VARCHAR(6)) AS Item FROM tbl_livro 
WHERE ID_autor = 2
```

- CONVERT: 
```
CONVERT(novo_tipos_dados, expressao, estilo)
-- estilo é usado normalmente para converter datas ou trabalhar com float/real (mm/dd/aaaa, aaaammdd, etc)

SELECT 'O preço do livro ' + Nome_Livro + ' é de R$' + CONVERT(VARCHAR(6), Preco_Livro) FROM tbl_Livro
```

```
SELECT 'A data de publicacao ' +
CONVERT(VARCHAR(18), Data_Pub)
FROM tbl_Livro
WHERE ID_Livro = 102

SELECT 'A data de publicacao ' +
CONVERT(VARCHAR(18), Data_Pub, 103)
FROM tbl_Livro
WHERE ID_Livro = 102

-- 103 é o codigo de um estilo de date/datetime
```

**IF / ELSE NO SQL SERVER**
```
IF condição
 BEGIN
   Bloco de códigos
 END;
-- for apenas uma linha de codigo, nao eh necessario usar BEGIN e END
```

```
SELECT 
  @NOME = nome_aluno,
  @MEDIA = (tbl_alunos.nota1 + tbl_alunos.nota2 + tbl_alunos.nota3)

FROM tbl_alunos
WHERE nome_aluno = 'Fábio'
  IF @MEDIA >= 7.00
  BEGIN
    SELECT @RESULTADO = 'Aprovado';
  END;
  ELSE
  BEGIN
    SELECT @RESULTADO = 'Reprovado';
  END;

  SELECT 'O aluno ' + @NOME + ' está ' + @RESULTADO + ' com média ' + CAST(@MEDIA AS VARCHAR);
```

**LOOP WHILE**

- BEGIN / END => com bloco de comandos
```
WHILE condicao
  BEGIN 
    Bloco de codigos
  END
```
```
DECLARE @valor INT
SET @valor = 0

WHILE @valor < 10
  BEGIN
    PRINT 'Numero:' + CAST(@valor AS VARCHAR(2))
    SET @valor = @valor + 1
  END;
```

```
DECLARE @codigo INT
SET @codigo = 0

WHILE @codigo < 106
  BEGIN
    SELECT ID_Livro AS ID, Nome_Livro AS Livro, Preco_Livro AS Preço
    FROM tbl_Livro
    WHERE ID_Livro = @codigo 
    SET @codigo = @codigo + 1
  END;
```

**STORED PROCEDURES - PARTE 01**
- lotes (batches) de declarações SQL que podem ser executadas como uma subrotina
- permitem centralizar a logica de acesso aos dados em um unico local, facilitando a manutencao e otimizacao de codigo
- tmb é possivel ajustar permissoes de acesso aos usuarios, definindo quem pode ou nao executa-las

```
CREATE PROCEDURE nome_procedimento 
  (@Parametro Tipo_dados)
AS
  Bloco de códigos
```
```
CREATE PROCEDURE teste
AS
SELECT 'Jottaemee' AS Nome

EXEC teste
```
```
CREATE PROCEDURE p_LivroValor
AS
SELECT Nome_Livro, Preco_livro
FROM tbl_Livro
```
```
-- sp_helptext => é uma stored procedure para extrair o conteudo de texto de uma stored procedure
EXEC sp_helptext p_LivroValor
```
```
CREATE PROCEDURE p_LivroISBN
WITH ENCRYPTON
AS
SELECT Nome_Livro, ISBN
FROM tbl_Livro
-- nesse caso o sp_helptext nao consegue ler o conteudo, devido o ENCRYPTON
```


**STORED PROCEDURES - PARTE 02**

- Para modificar SP:
```
ALTER PROCEDURE nome_procedimento
bloco de código da sp

-- vai preencher quase o mesmo codigo preenchido no moemnto de criação, porém, é mais viável fazer uma alteração do que excluir e criar outro, já que no ALTER as permissões sao mantidas
```

```
ALTER PROCEDURE teste (@parl AS int)
AS
SELECT @parl

EXEC teste 22
```

```
ALTER PROCEDURE p_LivroValor 
(@ID SMALLINT)
AS
SELECT Nome_Livro AS Livro, Preco_Livro AS Preco
FROM tbl_Livro
WHERE ID_Livro = @ID

EXEC p_LivroValor 104
```

```
ALTER PROCEDURE teste (@par1 AS INT, @par2 AS varchar(20))
AS
BEGIN
  SELECT @par1
  SELECT @par2
END
-- o 'AS' nos parametros (1a linha) é opcional

EXEC teste 22, 'Laranja'
EXEC teste @par1=25, @par2='Abacate'
```


**STORED PROCEDURES - PARTE 03**
```
ALTER PROCEDURE p_LivroValor
(@ID SMALLINT, @Preco MONEY)
AS
SELECT Nome_Livro AS Livro, Preco_Livro AS Preco
FROM tbl_Livro
WHERE ID_Livro > @ID AND Preco_Livro > @Preco

EXEC p_LivroValor @ID=103, @Preco=60

-- o 'AS' antes do SELECT é obrigatorio, indica que o inicio do bloco do SP
```

```
ALTER PROCEDURE p_LivroValor(
@Quantidade SMALLINT,
@ID SMALLINT)
AS
SELECT Nome_Livro AS Livro, Preco_Livro * @Quantidade AS Preco
FROM tbl_Livro
WHERE ID_Livro = @ID

EXEC p_LivroValor @ID=103, @Quantidade=10
```

```
CREATE PROCEDURE p_insere_editora(
@nome VARCHAR(50))
AS
INSERT INTO tbl_editoras (Nome_Editora)
VALUES(@nome)

EXEC p_insere_editora @nome = 'Editoral'

SELECT * FROM tbl_editoras
```
 
(ATUALIZAR) PARTE 4 ESTÁ NO MEU PC PESSOAL - JM

**FUNÇÕES DEFINIDAS PELO USUARIO - PARTE 01**   
```
SELECT dbo.nota_media('Fábio')
```

**FUNÇÕES DEFINIDAS PELO USUARIO - PARTE 02**

- Função com Valor de Tabela Embutida (Inline): 
  - são similares a uma exibição, porem permitem utilizar parametros. Retornam um conjunto completo de dados

```
CREATE FUNCTION nome_f (params)
RETURN Table
AS
RETURN (declaracao_select)


CREATE FUNCTION retorna_itens (@valor REAL)
RETURNS Table
AS
RETURN (
  SELECT L.Nome_Livro, A.Nome_Autor, E.Nome_Editora
  FROM tbl_Livro AS L
  INNER JOIN tbl_autores AS A
  ON L.ID_Autor = A.ID_Autor
  INNER JOIN tbl_editoras AS E
  ON L.ID_editora = E.ID_Editora
  WHERE L.Preco_Livro > @valor
)
```

- usando (no caso, só vai retornar as colunas definidas no select):
```
SELECT Nome_livro, Nome_Autor
FROM retorna_itens(62.00)
```

**FUNÇÕES DEFINIDAS PELO USUARIO - PARTE 03**

- Função com Valor de Tabela com Várias Instruções
  - combina a habilidade da função escalar de conter codigos complexos com a habilidade da função com valor de tabela de retornar um resulset
  - este tipo de função cria uma variável do tipo table e a popula a partir do codigo. Essa tabela é então passada de volta à função, de modo que possa ser usada em declarações SELECT

```
CREATE FUNCTION multi_tabela()
RETURNS @valores Table
  (Nome_Livro VARCHAR(50), Data_Pub DATETIME, Nome_Editora VARCHAR(50), Preco_Livro MONEY)
AS
BEGIN
INSERT @valores (Nome_Livro, Data_Pub, Nome_Editora, Preco_Livro)
  SELECT L.Nome_Livro, L.Data_Pub, E.Nome_Editora, L.Preco_Livro
  FROM tbl_Livro AS L
  INNER JOIN tbl_editoras AS E
  ON L.ID_editora = E.ID_Editora
RETURN
END

-- usando
SELECT * FROM multi_tabela()
```
- a função acima esta sem parametros, mas poderia ser criada sim com parametros
- funciona como um INSERT INTO, porem, em vez de colocar VALUES, coloca o SELECT para selecionar as colunas en que os valores serao adicionados


**TRIGGERS PARTE 01**
- um trigger é um tipo especial de Stored Procedure que é executada automaticamente quando um user realiza uma operação de modificação de dados em uma tabela especificada
- as operações que podem disparar um trigger são:
  - INSERT
  - UPDATE
  - DELETE
- triggers nao sao executados diretamente, mas disparam apenas em resposta a eventos
- triggers disparam uma vez para cada operação de modificação (e nao uma vez por linha afetada, no Oracle há as duas opções)
- no SQL Server pode ser disparado de dois modos:
  - After: o codigo presente no trigger é executado apos todas as ações terem sido completadas na tabela especificada
  - Instead Of: o codigo presente no trigger é executado no lugar da operação que causou seu disparo



Característica | INSTEAD OF | AFTER
---------------|------------|------
Declaração DML | Simulada, mas não executada | Executada, mas pode ser revertida no trigger (Roll back)
Timing         | Antes das constraints PK e FK | Apos a transacao completa, mas antes do commit
No de eventos por tabela | Um | Multiplos
Aplicavel em Views | Sim | Não
Permite Aninhamento | Depende das opcoes do servidor | Depende das opcoes do servidor
É recursivo | Não | Depende das opcoes do Banco de dados

**Fluxo de transação**
- para desenvolver triggers, eh necessario conhecimento do fluxo geral da transacao, para evitar conflito entre os triggers e constraints
- transacoes se movem atraves de verificacoes e codigos na ordem:

1. verificao de IDENTITY INSERT
2. restrição (Constraint) de Nulos (NULL)
3. checagem de tipos de dados
4. execucao de trigger INSTEAD OF (a execucao do DML para aqui, esse trigger nao eh recursivo)
5. restrição de PK
6. restrição "check"
7. restrição de FK
8. execucao do DML e att do log de transacoes
9. execucao do trigger AFTER
10. commit da transacao (confirmacao)


**TRIGGERS PARTE 02**
```
CREATE TRIGGER nome_trigger
ON tabela | view
[WITH ENCRYPTON]
  AFTER
    [INSERT, UPDATE, DELETE]
AS
(código do trigger)


CREATE TRIGGER nome_trigger
ON tabela | view (onde sera executado)
[WITH ENCRYPTION (opcional, criptografa o codigo do trigger)]
  AFTER (disparado qdo todas as operacoes especificadas nas declaracoes SQL tiverem sido executadas com sucesso)| INSTEAD OF (disparado no lugar das declaracoes SQL)
    [INSERT, UPDATE, DELETE (trigger disparado quando acontecer modificacao a partir de qlqr combinacao desses eventos]
AS
(codigo do trigger)
```

```
CREATE TRIGGER teste_after
ON tbl_editoras
AFTER INSERT
AS 
PRINT 'Olá Mundo'

-- 'invocando'
INSERTO INTO tbl_editoras VALUES ('Editora10')
```

```
CREATE TRIGGER teste_after
ON tbl_editoras
AFTER INSERT
AS 
INSERT INTO tbl_autores VALUES (25, 'Jose', 'da Silva')

DROP TRIGGER teste_after
```

```
CREATE TRIGGER teste_insteadof
ON tbl_autores
INSTEAD OF INSERT
AS 
PRINT 'Ola, nao inseri o registro desta vez'
```

**TRIGGERS PARTE 03**
- o administrador do sistema pode desabilitar temporariamente um trigger se houver necessidade. Para isso, usar o comando DDL ALTER TABLE:
```
ALTER TABLE nome_tabela
ENABLE | DISABLE TRIGGER nome_trigger
```

- verificar existencia do trigger:
```
-- tabela especifica:
EXEC sp_helptrigger @tabname=Nome_Tabela

-- banco de dados todo:
USE nome_banco_dados
SELECT *
FROM sys.triggers
WHERE is_disabled = 0 OR is_disabled = 1

-- 0 => traz os triggers desabilitados
-- 1 => habilitados
```

**TRIGGERS PARTE 04**
- Funcao UPDATE() retorna True se uma coluna especificada for afetada por uma transação DML
- Podemos criar um gatilho que executa um codigo caso a coluna especificada seja alterada por um comando DML usando essa função
```
CREATE TRIGGER trigger_after_autores
ON tbl_autores
AFTER INSERT, UPDATE
AS
IF UPDATE(nome_autor)
  BEGIN
    PRINT 'O nome do autor foi alterado'
  END
ELSE
  BEGIN
    PRINT 'Nome nao foi modificado'
  END
```

**TRIGGERS PARTE 05**

- Aninhamento e Triggers Recursivos
  - Um trigger ao ser disparado, pode executar uma declaracao DML que leva ao disparo de outro trigger (ou o disparo de 'ele proprio'S )
  - Para isso, a opção de servidor "Permitir que Gatilhos disparem outros gatilhos", em prorpeidades do Servidor -> Avançado, deve estar configurado com TRUE (é o padrao)

```
-- para desabilitar / habilitar
EXEC sp_configure 'Nested Triggers', 0 | 1;
RECONFIGURE;
```

- Trigger recursivo:
  - tipo especial de trigger after aninhado
  - ocorre quando um trigger executa uma declaracao DML que o dispara novamente
  - pode-se habilitar ou desabilitar com ALTER DATABASE
```
ALTER DATABASE nome_banco_dados
SET RECURSIVE_TRIGGERS ON | OFF

ALTER DATABASE db_Biblioteca
SET RECURSIVE_TRIGGERS ON

CREATE TABLE tbl_trigger_recursivo (
Codigo INT PRIMARY KEY)
```
```
CREATE TRIGGER trigger_rec ON tbl_trigger_recursivo
AFTER INSERT
AS
DECLARE @cod INT
SELECT
@cod = MAX(codigo)
FROM teste_trigger
IF @cod < 10
  BEGIN
    INSERT INTO tbl_trigger_recursivo SELECT MAX(codigo) + 1 FROM tbl_trigger_recursivo
  END
ELSE
  BEGIN
    PRINT 'Trigger recursivo finalizado'
  END
```

**RENOMEAR COLUNAS E TABELAS**
```
sp_rename 

SELECT * FROM sys.tables

-- mudar nome de coluna
sp_name 'NomeTabela.NomeAtualColuna', 'NovoNomeColuna', 'COLUMN';

-- mudar nome de tabela
sp_rename 'NomeTabelaAntigo', 'NovoNomeTabela'
```
