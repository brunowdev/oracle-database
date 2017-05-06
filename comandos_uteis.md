# Comandos úteis

Este arquivo possui comandos úteis no dia a dia de um DBA Oracle


Comandos de **startup**

Executa os três estágios na sequência (OPEN, NOMOUNT, MOUNT)
```sql
startup 
```

Sobe o banco como **nomount**
```sql
startup nomount
```

Altera o banco para o estado **mount**
```sql
alter session mount;
```

Altera o banco para o estado **open**
```sql
alter session open;
```

Retorna o status do banco (OPEN, NOMOUNT, MOUNT)
```sql
select status from v$instance;
```

Comandos de **shutdown**

Executa o shutdown padrão (aguarda até o fim das sessões e o commit/rollback das transações)
```sql
shutdown 
```

Executa o shutdown **transactional** (semelhante ao normal, mas não espera as seções finalizarem)
```sql
shutdown transactional
```

Executa o shutdown **immediate** (não aguarda o fim das seções). O mais recomendado para ambiente de produção.
```sql
shutdown immediate
```

Executa o shutdown **abort** (não aguarda o fim das seções e nem fecha os arquivos). O mais extremo, que só deve ser utilizado caso o banco esteja sendo corrompido. Exemplo: Controladora riscando o disco e corrompendo os dados.
```sql
shutdown abort
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
