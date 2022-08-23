# MongoDB

No MongoDB temos os `Bancos de Dados/Databases`, e ao invés de tabela/table como é chamado no mundo relacional temos as `coleções/collections` e ao invés de registros/records chamamos de `documentos/documents`.

## Visão Geral

- Termos: 
`NoSQL` - Mais vinculado ao marketing, para gerar impácto;
`Não relacional` - Termo mais correto.

- Tipos:
`Chave/Valor` - Muito usado para cache. Ex.: Redis;
`Documento` - Banco de Dados baseados em documento, como é o caso do MongoDB;
`Grafo` - Banco de Dados baseados no conceito matemático de vertices e arestas, muito indicado para trabalharmos com redes sociais já que a rede é organizada sob relações e isso forma uma teia de relacionamentos;
`Coluna` - Banco de Dados baseados em coluna, indicado para grande volume de dados. Ex.: BigTable do Google.

- Banco de Dados orientado a Documento - MongoDB:
`Banco sem esquema` - O que quer dizer? Quando estamos em um banco relacional e vamos definir nossos dados usamos a DDL(Data Definition Language - Linguagem de definição de dados) para criar as tabelas, colunas, definir o tipo de dado em cada coluna, quais as restrições de tamanho... criamos todo o esquema dos dados e quando formos inserir os dados no BD todo o conjunto de restrições e validações vão ser aplicadas para que o dado sempre esteja consistente. Já em um banco sem esquema, criamos uma collection/coleção e de forma muito simples podemos adicionar novos dados, deixar de informar outros dados sem problemas, porque não tem esquema, não tem essa camada de formalidade para definir exatamente como os dados serão persistidos dentro do Banco de Dados, nesse caso MongoDB;
`Orientado a Documento(JSON)` - O MongoDB armazena os seus dados em formato JSON, então teremos chave/valor, array de objetos. Ex.:

``` JSON
{ 
  "_id" : 1,
  "name": "Janeiro/2022",
  "month": 1,
  "year": 2022,
  "debts": [{
    "_id": 1,
    "name": "Luz",
    "value": 150,
    "status": "PAGO"
  }],
  "credits": [{
    "_id": 1,
    "name": "Salário",
    "value": 1000
  }]
}
```

- Vantagens MongoDB:
`Escalonamento` - Horizontal, conseguimos distribuir o Banco de Dados em vários servidores de forma horizontal.
Em bancos relacionais só conseguimos escalar de forma vertical, aumentando a mémoria e processador do servidor, não conseguindo distribuir o banco de dados em multiplos servidores, pois fica dificil manter a persistência dos dados.

## Comandos Básicos

### Startando o servidor e acessando o console do MongoDB

- Para startar servidor do mongodb vamos rodar o comando seguinte no terminal:

```
mongod
```

**Obs.:** Mas no meu caso quando instalei o mongodb na máquina virtual ele já starta o servidor automáticamente. Podemos ver o servidor rodando com o comando seguinte:

```
sudo service mongod status
```

E caso o servidor caia podemos rodar o comando seguinte:

```
sudo service mongod start
```

- Para abrirmos o console do mongodb vamos rodar o comando seguinte:

```
mongo
```

### Criando Banco de Dados no mongo DB

- Dentro do MongoDB Shell(console), podemos ver todos os bancos de dados em um cluster que temos acesso usando o comando seguinte:

```
> show databases
       dbs 
```

- Podemos notar que `admin` e `local` são bancos de dados que fazem parte de cada MongoDB cluster. A partir daqui, existem duas coisas a serem lembradas. A primeira é que não há um comando "criar" no MongoDB Shell. Para criar um banco de dados usamos o comando `use`. Se o banco de dados não existir, então o cluster do MongoDB irá criá-lo:

```
> use testdb
      [name_database]
```

- Isso é tudo o que temos que fazer para que o banco de dados seja criado. Mas isso nos leva ao segundo ponto. Mesmo que o banco de dados exista, se inserirmos o comando `show dbs`, o banco de dados que acabamos de criar não aparece.
O segundo ponto é que o banco de dados não é totalmente criado até que você coloque algo nele. Para isso iremos criar uma coleção usando comando seguinte:

```
> db.createCollection("testCollection") 
                      [name_collection]
```

**Obs.:** Como o comando `createCollection` identificou que deveria colocar os dados no `test`? Quando utilizamos o comando `use`, o `testdb` tornou-se o banco de dados sobre o qual os comandos operam. Para descobrir qual banco de dados é o atual, digitamos o comando seguinte:

```
> db
```

O comando `db` exibe o nome do banco de dados atual. Para mudar para um banco de dados diferente, digitamos o comando `use` com o nome do banco de dados.

- E para visualizarmos as coleções/collections desse banco de dados, usamos o comando seguinte:

```
show collections
```

### Excluindo Banco de Dados no mongo DB

- Primeiramente, vamos verificar a lista de banco de dados disponíveis usando o comando:

```
> show dbs
```

- Em seguida vamos "usar" o banco de dados que queremos excluir:

```
> use testdb
      [name_database]
```

- Feito isso, podemos excluí-lo com o comando seguinte:

```
> db.dropDatabase()
```

#### Excluindo uma coleção do Banco de Dados no mongo DB

- Primeiramente, vamos verificar a lista de coleções disponíveis no banco de dados em questão usando o comando:

```
> show collections
```

- Feito isso, podemos excluir a coleção desejada com o comando seguinte:

```
> db.testCollection.drop()
     [name_collection]
```

**Obs.:** Uma vez que excluimos todas as coleções/collections de uma base de dados, este é excluído automáticamente pelo mongo.

## Inserções

- Primeiramente, vamos verificar a lista de banco de dados disponíveis usando o comando:

```
> show dbs
```

- Em seguida vamos "usar" o banco de dados que queremos inserir dados/documents:

```
> use testdb
      [name_database]
```


- Para inserir dados/document em uma coleção/collection desse banco de dados vamos usar o método `insert` com a notação seguinte:

```
> db.testCollection.insert({"name": "Janeiro/2022", "month": 1, "year": 2022})
   [name_collection]      [object json]
```

**Obs.:** Mesmo que a coleção/collecion informada na inserção dos dados não exista ela será criada automáticamente, pois o mongodb é um banco de dados sem esquema.

- Podemos inserir dados utilizando o método `save`, esse método pode tanto inserir atualizar dados(contado que seja passada a chave correta para que o elemento seja localizado):

```
> db.testCollection.insert({"name": "Fevereiro/2022", "month": 2, "year": 2022})
   [name_collection]      [object json]
```

## Consultas

- Primeiramente, vamos verificar a lista de banco de dados disponíveis usando o comando:

```
> show dbs
```

- Em seguida vamos "usar" o banco de dados que queremos consultar dados/documents:

```
> use testdb
      [name_database]
```

- E podemos verificar a lista de coleções disponíveis no banco de dados em questão usando o comando:

```
> show collections
```

- Feito isso, para localizar *TODOS* os dados/documents de determinada collection de um BD, vamos usar o comando seguinte:

```
> db.testCollection.find()
    [name_collection]
```

- E para que a saída dos dados/documents de determinada collection de um BD saia formatada vamos chamar o método `pretty`:

```
> db.testCollection.find().pretty()
```

- Para localizar o *PRIMEIRO* registro/document de determinada collection de um BD, vamos usar o comando seguinte:

```
> db.testCollection.findOne()
```

- E para localizar o *PRIMEIRO* registro/document de determinada collection de um BD passando um filtro, vamos usar o comando seguinte:

```
> db.testCollection.findOne({month: 2})
```

- Para localizar *MAIS DE UM* registro/document de determinada collection de um BD passando um ou/or de critérios:

```
> db.testCollection.find({$or: [{month: 2}, {month: 3}]}).pretty()
```

- Para pular um elemento podemos usar o método `skip`, como no exemplo a seguir:

```
> db.testCollection.find({year: 2022}).skip(1).pretty()
```

**Obs.:** Nesse caso o `skip(1)` irá pular o primeiro elemento.

- Para limitar a resposta em determinado número de elementos/documents podemos usar o método `limit`, como no exemplo a seguir:

```
> db.testCollection.find({year: 2022}).limit(2).pretty()
```

## Atualização

- Primeiramente, vamos verificar a lista de banco de dados disponíveis usando o comando:

```
> show dbs
```

- Em seguida vamos "usar" o banco de dados que queremos atualizar dados/documents:

```
> use testdb
      [name_database]
```

- E podemos verificar a lista de coleções disponíveis no banco de dados em questão usando o comando:

```
> show collections
```

- Feito isso, para atualizar dados/documents de determinada collection de um BD, vamos usar critérios para selecionar um determinado elemento/document ou conjunto/documents de elemenentos da collection para que seja aplicada a atualização. 
Para selecionar um único elemento podemos usar o próprio `id`, mas nesse caso iremos usar o `and`(ele vai atualizar o primeiro elemento que encontrar com o filtro aplicado no end/e).
E para para informarmos quais atributos dos documents encontrados através do filtro aplicado no end serão atualizados vamos usar o `set`, como no exemplo abaixo:

``` 
> db.testCollection.update(
... {$and: [{month: 1}, {year: 2022}]},
... {$set: {credits: [{name: "Salary", value: 5000}]}}
... )
```

**Obs.:** Mesmo o atributo *credits* não exista nos documents encontrados ele será criado.

## Contador

- Para saber quantos documents existe em determinada collection, vamos usar o comando seguinte:

```
> db.testCollection.count()
```

## Removendo elemento/document

- Para remover um document de uma determinada collection podemos usar o método `remove` passando um critério, como no exemplo a seguir:

```
> db.testCollection.remove({"name" : "Agosto/2022"})
```