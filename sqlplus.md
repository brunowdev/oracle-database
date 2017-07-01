
# SQL & PLSQL - Oracle

Este arquivo possui dicas e comandos do utilitário [SQL Plus](https://pt.wikipedia.org/wiki/SQL*Plus)

## Tópicos
1. [Conexão remota](#remotecon)
2. [Formatação do console](#fc)

<h2 id="remotecon">Conexão remota</h2>

Para conectar via linha de comando à uma outra máquina:

```sql
sqlplus "usuario/senha@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(Host=enderecoHost)(Port=porta))(CONNECT_DATA=(SID=sidDaMaquina)))"
```

Ou, caso o TNS tenha sido configurado previamente no **tsnames.ora** da máquina:

```sql
sqlplus usuario@tnsName
```

Em seguida, será solicitada a senha.

Para trocar o usuário ou executar uma conexão remota dentro do sqlplus:

```sql
conn usuario@tnsName


<h2 id="fc">Formatação do console</h2>

```sql
-- Executar estes comandos dentro do console do SQL Plus, eles serão aplicados na sessão
SET NEWPAGE NONE
SET PAGESIZE 0
SET SPACE 0
SET LINESIZE 16000
SET ECHO OFF
SET FEEDBACK OFF
SET VERIFY OFF
SET HEADING OFF
SET TERMOUT OFF
SET TRIMOUT ON
SET TRIMSPOOL ON
SET COLSEP |

```
