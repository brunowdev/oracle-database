# Comandos úteis

Este arquivo possui comandos úteis no dia a dia de um DBA Oracle


Comandos de **startup**

Executa os três estágios na sequência (OPEN, NOMOUNT, MOUNT)
```sql
startup 
```

Retorna o status do banco (OPEN, NOMOUNT, MOUNT)
```sql
select status from v$instance;
```

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

Retorna a versão do banco, arquitetura, versão de patch, etc.
```sql
select banner from v$version;
```
