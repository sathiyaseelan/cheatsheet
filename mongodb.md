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

* Finding a document

		db.movies.findOne();
		db.movies.find();

* Findig document using conditions

		db.movies.findOne({rated: "PG-13"});
		db.movies.find({rated: "PG-13"});

		db.movies.find({rated: "PG-13", year: 2008});
		
		//For specifing nested property - Don't forget quotes
		db.movies.find({"tomato.meter": 80});
		
		/*For finding documents based on array elements*/

		//To match the documents having exact same array
		db.movies.find({"authors": ["Ethan Coen", "Joel Coen"]});

		//To match the documents having the element in any position in array
		db.movies.find({"actors": "Tom Hanks"});

		//Based on specific position in the array - starts with index 0
		db.movieDetails.find({"actors.0": "Claudia Cardinale"})

* Filter document properties in output using projection

		/*Projection - filter the output properties of document*/
		// Second argument - denotes projection
		//1 - include; 2 - don't include
		
		// Just remove title from the output
		db.movies.find({"actors": "Tom Hanks"}, {title: 0});
		// Show title, remove all other properties from output - but _id will be included
		db.movies.find({"actors": "Tom Hanks"}, {title: 1});
		// Show title, remove all other properties(including _id) from output 
		db.movies.find({"actors": "Tom Hanks"}, {title: 1,_id: 0});
##### Comparision operators
		/** General syntax of comaprision operators  **/
		/** field : {$operator : value} **/
		/** field : {$operator : [value1, value2]} //For $in, $nin **/
	
		db.movieDetails.find({runtime: {$gt : 50}});
		db.movieDetails.find({runtime: {$gt : 50, $lte : 80}});
		db.movieDetails.find({"tomato.meter" : {$gt :50},runtime: {$gt : 50, $lte : 80}});
		db.movieDetails.find({rated: { $ne : "UNRATED"}});
		db.movieDetails.find({rated: { $in : ["PG", "PG-13", "R"]}});
##### Element operators
		//To match based on element's existence or non-existence
		db.movieDetails.find({"tomato.meter" : {$exists : false}});
		db.movieDetails.find({"tomato.meter" : {$exists : false}});
		//To match based on type of elements
		db.movieDetails.find({_id : {$type : "objectId"}});
		db.movieDetails.find({_id : {$type : "string"}});
##### Logical operators
		// or operator
		db.movieDetails.find({ $or : [{"tomato.meter" : {$gt :50}},{runtime: {$gt : 50, $lte : 80}}]});
		// and operator for different fields are implicit by , 
		// ie, below two queries fetch same result.
		db.movieDetails.find({"tomato.meter" : {$gt :50},runtime: {$gt : 50, $lte : 80}});
		db.movieDetails.find({ $and: [{"tomato.meter" : {$gt :50}},{runtime: {$gt : 50, $lte : 80}}]});
		db.movieDetails.find({"tomato.meter" : {$exists : true, $ne : null}});
##### Regex operator
		db.movieDetails.find({ "awards.text" : { $regex: /^Won.*/}});
##### Array operators
		//To check the array contains all the mentioned values.
		//ie, the array can contain these values in any order along with other values too
		db.movieDetails.find({genres :{$all :["Comedy","Crime","Drama"]}});

		//To match the sizeof array
		db.movieDetails.find({genres: {$size :2}});

		//To match the complex element in the array against multiple conditions.

		//lets say documents contains field boxOffice like below,
		//boxOffice: [{country: "UK",value: 50},{country: "US",value: 20}]
		//To match documents having boxoffice in UK and value >30
		db.movieDetails.find({boxOffice: {$elemMatch: {country: "UK", value : {$gt: 30}}}});
##### Helper methods.

* count()  - To count the resultset
* pretty() - To pretty print the resultset in shell

##Advanced Topics

######Object Id
ObjectId format = 12 byte HEX string

|DATE|MAC_ADDR|PID|COUNTER|
|---|---|---|---|
|4|3|2|3|


######Cursor

find() method returns the cursor.

cursor has below methods.

* hasNext()
* next()