NodeJS ORM
==========

## About

Node-ORM is a NodeJS module for multiple databases using Object-Relational Mapping.

## Connecting to a Database (MySQL in the example)

    var orm = require("orm");
    var db = orm.connect("mysql://username:password@hostname/database", function (success, db) {
        if (!success) {
            console.log("Could not connect to database!");
            return;
        }
        
        // you can now use db variable to define models
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

## Creating the model on the database

    Person.sync();

## Creating and using a record

    var John = new Person({
    	"name"		: "John",
    	"surname"	: "Doe",
    	"age"		: 20
    });
    console.log("Hello, my name is " + John.fullName() + " and I'm " + John.age + " years old");

## Saving record to database

    John.save(function (success) {
    	if (success) {
    		console.log("Saved! ID=" + John.id);
    	} else {
    		console.log("Something went wrong...");
    	}
    });

## Supported Types

This values are still just supported for .sync() (table creations), not for other instance operations live .save() (yet).

<table>
	<tr>
		<td>Name</td>
		<td>Description</td>
		<td>MySQL Type</td>
	</tr>
	<tr>
		<td>string</td>
		<td>Small text</td>
		<td>VARCHAR(255)</td>
	</tr>
	<tr>
		<td>text</td>
		<td>Big text</td>
		<td>TEXT</td>
	</tr>
	<tr>
		<td>int, integer, num, number</td>
		<td>Signed integer</td>
		<td>INT</td>
	</tr>
	<tr>
		<td>float</td>
		<td>Floating point number</td>
		<td>FLOAT</td>
	</tr>
	<tr>
		<td>bool, boolean</td>
		<td>True or false value</td>
		<td>TINYINT(1) (true=1, false=0)</td>
	</tr>
	<tr>
		<td>data</td>
		<td>Binary data</td>
		<td>BLOB</td>
	</tr>
	<tr>
		<td>enum</td>
		<td>Enumerated value</td>
		<td>ENUM</td>
	</tr>
	<tr>
		<td>struct</td>
		<td>Generic (and simple) object</td>
		<td>TEXT (saved as JSON)</td>
	</tr>
</table>
1. string - Small text