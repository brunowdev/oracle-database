# Oracle Database

Este arquivo possui dicas com exemplos em SQL

# Como usar o binding

Primeiro você deve criar as variáveis que serão o binding:

```sql
var id number
var name varchar2
```

Após definir as variáveis, você pode setar o valor explicitamente:


begin
  select 1909, 'mil novecentos e nove' into :id, :name from dual;
end;

Você também pode setar o valor pela função exec:

exec :id := 1909
exec :name := 'mil novecentos e nove'

Será criada uma tabela para demonstrar o uso do binding:

create table sqlplus_binding(
  id    number, 
  name  varchar2(15)
);

// Considerando que você setou os bingins com uma das formas demonstradas, o insert abaixo irá funcionar.

begin
  insert into sqlplus_binding values (:id, :name)
end;


[Exemplo original](http://www.adp-gmbh.ch/ora/sqlplus/use_vars.html)