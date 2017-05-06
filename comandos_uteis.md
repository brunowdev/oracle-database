# Comandos úteis

Este arquivo possui comandos úteis no dia a dia de um DBA Oracle


Exibe todos os parâmetros do banco
```sql
select * from v$parameter;
```

Exibe todos os processos rodando em segundo plano
```sql
select * from v$session;
```

Exibe todos os usuários do banco
```sql
select * from dba_users OU select username from dba_users
```

Lista todos os usuários/schemas, ids e data de criação
```sql
select * from all_users
```

Retorna o status do banco
```sql
select status from v$instance;
```
