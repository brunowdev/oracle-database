
# SQL & PLSQL - Oracle

Este arquivo possui dicas e comandos do utilitário [SQL Plus](https://pt.wikipedia.org/wiki/SQL*Plus)

## Tópicos
1. [Conexão remota](#remotecon)

<h2 id="remotecon">Conexão remota</h2>

Para conectar via linha de comando à uma outra máquina:

```sql
sqlplus "usuario/senha@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(Host=enderecoHost)(Port=porta))(CONNECT_DATA=(SID=sidDaMaquina)))"
```

Ou, caso o TNS tenha sido configurado previamente no tsnames.ora da máquina:

```sql
sqlplus usuario@tnsName
```

Em seguida, será solicitada a senha.

Para trocar o usuário ou executar uma conexão remota dentro do sqlplus:

```sql
conn usuario@tnsName
```
