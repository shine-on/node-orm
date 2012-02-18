NodeJS ORM
==========

## Call for testers

* please test-report configurable constraints on MySQL;

## About

Node-ORM is a multi-database Object-Relational Mapping for NodeJS.

## Features

* validators;
* beforeSave/afterSave hooks;

## Installing

    npm install orm

## Connecting to a database

    var orm = require("orm");
    var dsn = "mysql://username:password@hostname/database";

    orm.connect(dsn, function (success, db) {
        if (!success) {
            console.log("Could not connect to database!");
            return;
        }
        
        // you can now use db variable to define models
    });

## Connecting to a database via raw API

    var orm = require("orm");
    var mysql = require("mysql");
    var client = mysql.createClient({
        user: 'root',
        password: 'root'
    });

    orm.connect("mysql", client, function (success, db) {
        // same as above...
    });

## Defining a model

    var Person = db.define("person", {
        "name"   : { "type": "string" },
        "surname": { "type": "string", "default": "" },
        "age"    : { "type": "int" }
	}, {
        "methods" : {
            "fullName" :function () {
                return this.name + " " + this.surname;
            }
        }
    });

## Creating a model

    Person.sync();

## Using a model

    var John = new Person({
    	"name"		: "John",
    	"surname"	: "Doe",
    	"age"		: 20
    });
    console.log("Hello, my name is " + John.fullName() + " and I'm " + John.age + " years old");

## Saving a model

    John.save(function (err, JohnCopy) {
    	if (!err) {
    		console.log("Saved! ID=" + John.id); // you can use John or JohnCopy
    	} else {
    		console.log("Something went wrong...");
    		console.dir(err);
    	}
    });

## Adding associations

    Person.hasOne("father", Person);
    // or just
    Person.hasOne("mother"); // defaults to same model
    
    // this will create "person_friends" table with "person_id" and "friend_id"
    Person.hasMany("friends", Person, "friend");

## Changing associations

    John.setFather(Jeremy, function () {
    	John.setMother(Jane, function () {
    		John.addFriends(Jeremy, Jane, function () {
    			console.log("Jeremy and Jane (John's parents) are now his friends too");
    		});
    	});
    });
    
    John.getFather(function (JohnFather) {
    	console.log("John's father is " + JohnFather.name);
    });
    John.unsetMother(function () {
    	console.log("John has no mother now!");
    });
    John.removeFriends(Jeremy, Jane, function () {
    	console.log("John has no friends now!");
    });
    // or just don't send any, all will be removed
    John.removeFriends(function () {
    	console.log("John has no friends now!");
    });

## API

Check out [API](https://github.com/dresende/node-orm/wiki) for more.

## Supported database types

Currently supported database types are:

* MySQL (via https://github.com/felixge/node-mysql)
* PostgreSQL (via https://github.com/brianc/node-postgres)
* MongoDB (alpha quality, via https://github.com/christkv/node-mongodb-native)

## Supported data types

Currently, following data types affect only .sync() method (i.e. CREATE TABLE).
Others, like .save() method (i.e. UPDATE), does not perform any data type checks.

<table>
	<tr>
		<th>Name</th>
		<th>Description</th>
		<th>MySQL Type</th>
        <th>PostgreSQL Type</th>
        <th>MongoDB Type</th>
	</tr>
	<tr>
		<td>string</td>
		<td>Small text</td>
		<td>VARCHAR(255)</td>
        <td>VARCHAR(255)</td>
        <td>String</td>
	</tr>
	<tr>
		<td>text</td>
		<td>Big text</td>
		<td>TEXT</td>
        <td>TEXT</td>
        <td>String</td>
	</tr>
	<tr>
		<td>int, integer, num, number</td>
		<td>Signed integer</td>
		<td>INT</td>
        <td>INTEGER</td>
        <td>Int</td>
	</tr>
	<tr>
		<td>float</td>
		<td>Floating point number</td>
		<td>FLOAT</td>
        <td>REAL</td>
        <td>Float</td>
	</tr>
	<tr>
		<td>bool, boolean</td>
		<td>True or false value</td>
		<td>TINYINT(1) (true=1, false=0)</td>
        <td>BOOLEAN</td>
        <td>Boolean</td>
	</tr>
	<tr>
		<td>date</td>
		<td>Date/time value (seconds precision)</td>
		<td>DATETIME</td>
        <td>TIMESTAMP</td>
        <td>?</td>
	</tr>
	<tr>
		<td>data</td>
		<td>Binary data</td>
		<td>BLOB</td>
        <td>BYTEA</td>
        <td>String</td>
	</tr>
	<tr>
		<td>enum</td>
		<td>Enumerated value</td>
		<td>ENUM</td>
        <td>ENUM</td>
        <td>String</td>
	</tr>
	<tr>
		<td>struct, object</td>
		<td>Generic (and simple) object</td>
		<td>TEXT (saved as JSON)</td>
        <td>TEXT (saved as JSON)</td>
        <td>Object</td>
	</tr>
</table>
