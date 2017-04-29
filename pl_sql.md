# PLSQL - Oracle

Este arquivo possui dicas com exemplos em SQL

[Como usar o binding](#Comousarobinding)
[Criando uma função em Java que pode ser utilizada dentro do SQL](#Criando-uma-função-em-Java-que-pode-ser-utilizada-dentro-do-SQL)

## Como usar o binding [Comousarobinding] ##

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


##Criando uma função em Java que pode ser utilizada dentro do SQL [Criando-uma-função-em-Java-que-pode-ser-utilizada-dentro-do-SQL]

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

# Gerar caracteres e números aleatórios

O Oracle possui um pacote chamado **DBMS_RANDOM** que serve para geração de dados aleatórios númericos e alfa numéricos.

| Função | Retorno |
| --- | --- |
| RANDOM | gera números aleatórios |
| VALUE | gera números aleatórios com o alcance recbido. Por padrão de 0 à 1 se nenhum parâmetro for passado |
| STRING | gera strings capitalizadas ou não (uppercase ou lowercase)|

Gera um número aleatório positivo ou negativo
```sql
select dbms_random.random from dual;
```

Gera um número aleatório entre 0 e 1
```sql
select dbms_random.value from dual;
```

Gera um número aleatório entre 0 e 500
```sql
select dbms_random.value(1, 500) num from dual;
```

Gera um número aleatório com 6 dígitos
```sql
select dbms_random.value(100000, 999999) num from dual;
```

Gera uma string capitaliza de 35 caracteres
```sql
select dbms_random.string('U', 35) str from dual;
```

Gera uma string descapitaliza de 35 caracteres
```sql
select dbms_random.string('L', 35) str from dual;
```

Gera uma string alfanumérica de 20 caracteres com caracteres maíusculos e minúsculos
```sql
select dbms_random.string('A', 20) str from dual;
```

Gera uma string alfanumérica de 20 caracteres com caracteres maíusculos
```sql
select dbms_random.string('X', 20) str from dual;
```

Gera uma string alfanumérica de 20 caracteres considerando qualquer caractere que pode ser impresso no console
```sql
select dbms_random.string('P', 20) str from dual;
```

[Mais sobre funções de caracteres](http://www.databasejournal.com/features/oracle/article.php/3341051/Generating-random-numbers-and-strings-in-Oracle.htm)
