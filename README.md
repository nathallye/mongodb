# MongoDB

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