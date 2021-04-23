# Master/Slave Postgres Replication através do método de logical
Com as configurações dessa máquinas é possível criar uma replicação lógica de uma base para outro. Criei esse exemplo com objetivo de simular algumas situações de ***CAOS*** e adicionar um guia
de boas práticas ou formas de restauração de ambiente.

**Quickstart**: 
```
docker-compose up
```

## Master: 
* User: postgres
* Password: admin
* Port: 5432

## Replica: 
* User: postgres
* Password: admin
* Port: 5433
