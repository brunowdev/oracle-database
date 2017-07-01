
# SQL & PLSQL - Oracle

Este arquivo possui dicas e comandos do utilitário [SQL Plus](https://pt.wikipedia.org/wiki/SQL*Plus)

## Tópicos
1. [Conexão remota](#remotecon)

<h2 id="remotecon">Conexão remota</h2>

Para conectar via linha de comando à uma outra máquina:
sqlplus "usuario/senha@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(Host=enderecoHost)(Port=porta))(CONNECT_DATA=(SID=sidDaMaquina)))"
