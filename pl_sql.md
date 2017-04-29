# Oracle Database

Este arquivo possui dicas com exemplos em SQL

# Como usar o binding

Primeiro você deve criar as variáveis que serão o binding:

var id number
var name varchar2

Após definir as variáveis, você pode setar o valor explicitamente:

begin
  select 1909, 'mil novecentos e nove' into :id, :name from dual;
end;