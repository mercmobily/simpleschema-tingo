simpleschema-tingo
==================

Tingo component of the [SimpleSchema](https://github.com/mercmobily/simpleschema) module.

This module provides a "mixin" to the main SimpleSchema classes. If you "mix" this constructor class with the main SimpleSchema class, you will end up with a class able to handle Tingodb-specific fields (that is, `ObjectId` fields ).

# Driver-specific Mixins

Basic schemas work really well for any database. However, it's handy to have driver-specific schemas which take into consideration driver-specific features.

Driver-specific schemas come in the form of `mixins`: they are basic classes that should be "mixed in" with the main one. In order to mixin the constructor's prototypes, you will need to use  [SimpleDeclare - Github](https://github.com/mercmobily/simpleDeclare). See the example below on how to use it.

# Functions provided

The TingoSchemaMixin overloads objects created with the following functions:

  * `idTypeCast()` It will cast a field to Tingo's own `ObjectId` type. If the field is not valid, casting will fail
  * `makeId()` Rather than using the default id creator, which by default just returns a random number, the overloaded `makeId()` will use Tingo's own `ObjectId()` function. NOTE: `makeId()` is available as an object method, _and_ as a class method

# A practical example

Here is a practical example on how to use TingoSchemaMixin

    var Schema = require( 'simpleschema' );
    var declare = require( 'simpledeclare' );
    var TingoSchemaMixin = require('simpleschema-tingo')


    // Mixing in Schema (the basic class) with TingoSchemaMixin
    MyTingoSchema = declare( [ Schema, TingoSchemaMixin ] );

    person = new MyTingoSchema({
      personId:  { type: 'id',     required: true },
      anotherId: { type: 'id',    required: true },
      name:      { type: 'string', required: true },
    });

    var p = { name: 'Tony', anotherId: '1234' } ;
    MyTingoSchema.makeId( p, function( err, id ){
      if( err ){
        console.log("Error making the id:");
        console.log( err );
      } else {
        p.personId = id;
        console.log( "MADE ID IS: ", id );
        person.validate( p, function( err, newP, errors){
          if( err ){
            console.log("Oh no!");
            console.log( err );
          } else {
            console.log("New P:");
            console.log( newP );
            console.log("Errors");
            console.log( errors);
          }
        });
      }
    });




