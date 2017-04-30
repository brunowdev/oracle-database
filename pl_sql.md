# PLSQL - Oracle

Este arquivo possui dicas com exemplos em SQL

## Tópicos
1. [Como usar o binding](#htub)
2. [Criando uma função em Java que pode ser utilizada dentro do SQL](#cfj)
3. [Gerar caracteres e números aleatórios](#gcna)
4. [Funções de string e manipulação de caracteres](#fsmc)

<h2 id="htub">Como usar o binding</h2>

Primeiro você deve criar as variáveis que serão o binding:

```sql
var id number
var name varchar2
```

Após definir as variáveis, você pode setar o valor explicitamente:


```sql
begin
  select 1909, 'mil novecentos e nove' into :id, :name from dual;
end;
```

Você também pode setar o valor pela função exec:

```sql
exec :id := 1909
exec :name := 'mil novecentos e nove'
```

Será criada uma tabela para demonstrar o uso do binding:

```sql
create table sqlplus_binding(
  id    number, 
  name  varchar2(15)
);
```

Considerando que você setou os bingins com uma das formas demonstradas, o insert abaixo irá funcionar.

```sql
begin
  insert into sqlplus_binding values (:id, :name) 
end;
```

[Mais sobre o binding](http://www.adp-gmbh.ch/ora/sqlplus/use_vars.html)

<h2 id="cfj">Criando uma função em Java que pode ser utilizada dentro do SQL</h2>

Em alguns casos, é vantajoso ou mesmo a única solução viável utilizar um recurso da linguagem dentro do banco de dados. No caso do Java, pode ser criada uma classe com 
métodos que poderão ser vinculados a funções que serão chamadas dentro de instruções SQL.

Abaixo, veremos um exemplo de uma função em Java que gera um [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier).

```sql
CREATE OR REPLACE AND COMPILE
JAVA SOURCE NAMED "RandomUUID"
AS 
    public class RandomUUID
    {
        public static String create()
        {
        return java.util.UUID.randomUUID().toString();
        }
    }
/
```

Na última linha, a "/", é responsável por finalizar o comando.

Após criar a classe e a função que iremos utilizar, vamos vincular em uma função SQL que poderá ser utilizada sempre que necessário.

```sql
CREATE OR REPLACE FUNCTION RandomUUID
    RETURN VARCHAR2
    AS LANGUAGE JAVA
    NAME 'RandomUUID.create() return java.lang.String';
    /
 ```

 Com a função criada, podemos chamá-la da seguinte forma:

```sql
SELECT randomUUID() FROM DUAL;
```

A saída será parecida com:

```sql
RANDOMUUID()
--------------------------------------------------------------------------------
ef225955-6da1-49f4-8a17-61606ea1abc1
```

[Mais sobre o UUID e desempenho desta função](http://stackoverflow.com/questions/13951576/how-to-generate-a-version-4-random-uuid-on-oraclehttp://stackoverflow.com/questions/13951576/how-to-generate-a-version-4-random-uuid-on-oracle)

<h2 id="gcna">Gerar caracteres e números aleatórios</h2>

O Oracle possui um pacote chamado **DBMS_RANDOM** que serve para geração de dados aleatórios númericos e alfa numéricos.

| Função | Retorno |
| --- | --- |
| RANDOM | gera números aleatórios |
| VALUE | gera números aleatórios com o alcance recbido. Por padrão de 0 à 1 se nenhum parâmetro for passado |
| STRING | gera strings capitalizadas ou não (uppercase ou lowercase)|

Gera um número aleatório positivo ou negativo
```sql
select dbms_random.random from dual;

    RANDOM
----------
2116960498
```

Gera um número aleatório entre 0 e 1
```sql
select dbms_random.value from dual;

     VALUE
----------
         0
```

Gera um número aleatório entre 0 e 500
```sql
select dbms_random.value(1, 500) num from dual;

       NUM
----------
290.010468
```

Gera um número aleatório com 6 dígitos
```sql
select dbms_random.value(100000, 999999) num from dual;

       NUM
----------
515978.975
```

Gera uma string capitaliza de 35 caracteres
```sql
select dbms_random.string('U', 35) str from dual;

STR
--------------------------------------------------------------------------------
TFGVRLKTGKGGLNEWQQUAEAUTTWXYLFTPZRR
```

Gera uma string descapitaliza de 35 caracteres
```sql
select dbms_random.string('L', 35) str from dual;

STR
--------------------------------------------------------------------------------
udjejjzdoevqaygppksi
```

Gera uma string alfanumérica de 20 caracteres com caracteres maíusculos e minúsculos
```sql
select dbms_random.string('A', 20) str from dual;

STR
--------------------------------------------------------------------------------
xewYgvzuYyUYPGybOgiYALInqgPSCaUroWs
```

Gera uma string alfanumérica de 20 caracteres com caracteres maíusculos
```sql
select dbms_random.string('X', 20) str from dual;

STR
--------------------------------------------------------------------------------
YD21I16RVZTW6HD58DAVELN91U3QAK4L6JK

```

Gera uma string alfanumérica de 20 caracteres considerando qualquer caractere que pode ser impresso no console
```sql
select dbms_random.string('P', 20) str from dual;

STR
--------------------------------------------------------------------------------
vV>4;.Jp8Tu[lJSHSgchx1~;M!>$ga]t$"4
```

[Mais sobre funções de caracteres](http://www.databasejournal.com/features/oracle/article.php/3341051/Generating-random-numbers-and-strings-in-Oracle.htm)


<h2 id="fsmc">Funções de string e manipulação de caracteres</h2>

Esta seção contém funções de manipulação de caracteres.


Retorna o código na tabela ASCII do caractere recebido
```sql
SELECT ASCII('b') FROM DUAL;

ASCII('B')
----------
        98

SELECT ASCII('R') FROM DUAL;

ASCII('R')
----------
        82

SELECT ASCII('u') FROM DUAL;

ASCII('U')
----------
       117

SELECT ASCII('N') FROM DUAL;

ASCII('N')
----------
        78

SELECT ASCII('o') FROM DUAL;

ASCII('O')
----------
       111

```

Aplicar uppercase na string

```sql
SELECT UPPER('brunowdev') FROM DUAL;

UPPER('BRUNOWDEV')
---------------------------
BRUNOWDEV
```

Aplicar lowercase na string
```sql
SELECT LOWER('BRUNOWDEV') FROM DUAL;

UPPER('BRUNOWDEV')
---------------------------
brunowdev
```

Aplicar uppercase na primeira letra da string
```sql
SELECT INITCAP('brunowdev') FROM DUAL;

INITCAP('brunowdev')
---------------------------
Brunowdev
```

Abaixo seguem as mesmas funções, porém, agora as funções possuem o prefixo **NLS**, ou seja, elas irão formatar de acordo com as regras do **LOCALE** e **ENCODING** definidos.

```sql
SELECT NLS_UPPER('brunowdev') FROM DUAL;

NLS_UPPER('BRUNOWDEV')
---------------------------
BRUNOWDEV
```

Aplicar lowercase na string
```sql
SELECT NLS_LOWER('BRUNOWDEV') FROM DUAL;

NLS_UPPER('BRUNOWDEV')
---------------------------
brunowdev
```

Aplicar uppercase na primeira letra da string
```sql
SELECT NLS_INITCAP('brunowdev') FROM DUAL;

NLS_INITCAP('brunowdev')
---------------------------
Brunowdev
```

Obter o caractere com base no código da tabela ASCII:

```sql
SELECT(CHR(98)) FROM DUAL;

(CH
---------------------------
b
```

Selecionando mais de um caractere na mesma consulta:
```sql
SELECT(CHR(98) || CHR(114) || CHR(117) || CHR(110) || CHR(111) || CHR(119) || CHR(100) || CHR(101) || CHR(119)) FROM DUAL;

(CHR(98)||CHR(114)||CHR(117
---------------------------
brunowdew

```

Retorna a primeira ocorrência não nula em uma tupla. Para testar, será criada uma tabela de teste.

```sql
CREATE TABLE teste_coalesce (
  coluna1  VARCHAR2(1),
  coluna2  VARCHAR2(1),
  coluna3  VARCHAR2(1)
);
```

Serão inseridos 5 registros para demonstrar a função.

```sql
INSERT INTO teste_coalesce VALUES (NULL, 'B', NULL);
INSERT INTO teste_coalesce VALUES ('R', NULL, 'N');
INSERT INTO teste_coalesce VALUES (NULL, NULL, 'U');
INSERT INTO teste_coalesce VALUES ('N', 'Z', 'J');
INSERT INTO teste_coalesce VALUES (NULL, NULL, 'O');
```

Fazendo o select abaixo, é possível ver que a função retorna sempre a primeira ocorrência não nula do registro.

```sql
SELECT COALESCE(coluna1, coluna2, coluna3) FROM teste_coalesce;

COA
---
B
R
U
N
O

```

Concatenar **duas** strings

```sql
SELECT CONCAT('bruno', 'wdev') FROM DUAL;

CONCAT('BRUNO','WDEV')
---------------------------
brunowdev

```

Concatenar dois **CLOBs**

```sql
set serveroutput on

DECLARE
 clob1 CLOB := TO_CLOB('bruno');
 clob2 CLOB := TO_CLOB('wdev');
 clob3 CLOB;
BEGIN
  SELECT CONCAT(clob1, clob2)
  INTO clob3
  FROM DUAL;

  dbms_output.put_line(clob3);
END;
/
```


Converter um conjunto de caracteres para outro encoding. É normal a quebra de alguns caracteres.

```sql
SELECT CONVERT('Ä Ê Í Õ Ø A B C D E','US7ASCII','WE8ISO8859P1') FROM DUAL;

CONVERT('┬┐┬┐┬┐┬┐ABCDE','US7ASCII','WE8ISO8859P1'
---------------------------------------------
? ??? A B C D E

```

Retorna uma string (varchar2) contendo o código do tipo de dado, o tamanho em bytes e a representação interna do valor (o código dos caracteres, decimal, octal, etc.). Esta função não é muito agradável para o oracle, por isso use com cuidado. Adicionei uma cláusula where para limitar os resultados e facilitar a visualização.
No início, são aplicadas algumas formatações do SQL Plus.
```sql

set linesize 121
col dmp format a50

SELECT table_name, DUMP(table_name) DMP FROM user_tables where rownum < 2;

TABLE_NAME
------------------------------------------------------------------------------------------
DMP
--------------------------------------------------
ICOL$
Typ=1 Len=5: 73,67,79,76,36


```

```sql

SELECT table_name, DUMP(table_name, 16) DMP FROM user_tables where rownum < 2;


TABLE_NAME
------------------------------------------------------------------------------------------
DMP
--------------------------------------------------
ICOL$
Typ=1 Len=5: 49,43,4f,4c,24


```

```sql

SELECT table_name, DUMP(table_name, 16, 7, 4) DMP FROM user_tables where rownum < 2;


TABLE_NAME
------------------------------------------------------------------------------------------
DMP
--------------------------------------------------
ICOL$


```