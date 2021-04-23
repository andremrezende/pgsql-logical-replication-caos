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

Primeiro crieo schema test na Master e na Réplica.

```sql
--Apaga tabela
drop table test.customers;

--Criar na master e replica
CREATE TABLE test.customers (
  id int PRIMARY KEY
);

-- criar na master
CREATE PUBLICATION PUBLICATION_TO_ALL FOR TABLE test.customers; 

-- criar na replica
CREATE SUBSCRIPTION SUBSCRIBE_TO_ALL_2 CONNECTION 'host=db-origin dbname=postgres user=postgres password=admin' PUBLICATION PUBLICATION_TO_ALL;

-- executar na master
insert into test.customers select * from generate_series(1, 100)

-- verificar novos valores
select * from test.customers;

-- apaga todos valores de uma unica vez da tabela
truncate table test.customers;

-- Consultar publications na master
select * from pg_publication;

-- Consultar tabelas nas publications na master
select * from pg_publication_tables;

-- Consultar subscriptions na replica
select * from pg_subscription;

-- Adicionar nova tabela na publication
alter publication publication_to_all add table test.customers;

-- Após adicionar nova tabela na publication, realizar refresh na replica
alter subscription subscribe_to_all_2 refresh publication;
```