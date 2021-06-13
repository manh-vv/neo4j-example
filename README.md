# Hello world Neo4j

`docker-compose up -d` and open http://localhost:7474/.

![home](docs/Screen%20Shot%202021-06-13%20at%2015.49.13.png)

- username: `neo4j`
- password: `s3cr3t`
- you can see them in the [.env](.env)

![after-login](docs/Screen%20Shot%202021-06-13%20at%2015.52.10.png)

```bash
# input commands at
# neo4j$
# on the website
```

https://neo4j.com/docs/cypher-manual/current/syntax/

Delete old data

```cypher
// Delete all data
match (n) detach delete n
```

Import data

```cypher
// Import data
// https://neo4j.com/docs/cypher-manual/current/clauses/create/#create-create-node-and-add-labels-and-properties
create (u1:user {name: 'user 1'})
create (u2:user {name: 'user 2'})
create (u3:user {name: 'user 3'})
create (u4:user {name: 'user 4'})
create (p1:phone {name: 'p1'})
create (p2:phone {name: 'p2'})
create (p3:phone {name: 'p3'})
create (p4:phone {name: 'p4'})
create (pa:phone {name: 'pa'})
create (pb:phone {name: 'pb'})
create (pc:phone {name: 'pc'})
create (pd:phone {name: 'pd'})
create (pe:phone {name: 'pe'})
create (pf:phone {name: 'pf'})
create
(p1)-[:belong_to]->(u1),
(p2)-[:belong_to]->(u2),
(pb)-[:belong_to]->(u2),
(p3)-[:belong_to]->(u3),
(p4)-[:belong_to]->(u4),
(pd)-[:belong_to]->(u4)

// so, user 1 has phone of user 2, user 3, and user 4
create
(u1)-[:has]->(p2),
(u1)-[:has]->(p3),
(u1)-[:has]->(pd)

// user 2 has phone of user 4, and unknown phone pa
create
(u2)-[:has]->(p4),
(u2)-[:has]->(pa)

// user 4 has phone of user 3 and unknown phone pc
create
(u4)-[:has]->(p3),
(u4)-[:has]->(pc)

// user 3 has two unknown phone pf and p3
create
(u3)-[:has]->(pf),
(u3)-[:has]->(pe)
```

![import data](./docs/Screen%20Shot%202021-06-13%20at%2016.02.29.png)
![import data success](./docs/Screen%20Shot%202021-06-13%20at%2016.02.42.png)

Query friend

```cypher
// find all friend of user 1
// u1 has phones that belong to user X
// return user X
// show graph
match (cu:user {name: 'user 1'})-->(p:phone)-->(u:user) return cu, p, u

// show only friends name
match (cu:user {name: 'user 1'})-->(p:phone)-->(u:user) return u.name

// if we know id we can use id as bellow
match (cu:user)-->(p:phone)-->(u:user) where id(cu)=36 return cu, p, u
```

![query friends](./docs/Screen%20Shot%202021-06-13%20at%2016.03.01.png)
![query friends success](./docs/Screen%20Shot%202021-06-13%20at%2016.03.16.png)

## Pagination

https://neo4j.com/docs/cypher-manual/current/clauses/skip/#skip-return-middle-rows
