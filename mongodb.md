#MongoDB commands

###General commands

* To start the server process

		mongod

* To start the mongo shell

		mongo

* To import collections

		mongoimport

***
###CRUD
####Creating Documents

* Insert a single document

		 db.movies.insertOne({title: 'Rocky', rating: 7.8, imdb: 't1235'});

 _id field will be automatically generated, we can override by passing _id field 

* Insert many documents

		db.movies.insertMany([{title: 'Rocky', rating: 7.8, imdb: 't1235'}, {title: 'Rocky2', rating: 7.5, imdb: 't1233'}], {ordered: false});

* array of documents should be passed to insertMany

* second argument ordered attribute

* If ordered true, then insert operation will stop with the first error. 
####Reading Documents


##### Advanced Topics

_id format = 12 byte HEX string

|DATE|MAC_ADDR|PID|COUNTER|
|---|---|---|---|
|4|3|2|3|
