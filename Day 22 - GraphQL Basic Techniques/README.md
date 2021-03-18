Main Resource used : 
Tryhackme GraphQL Room : https://tryhackme.com/room/graphql
Hacking GraphQL For Beginners from Farah Hawa : https://www.youtube.com/watch?v=OQCgmftU-Og  


GraphQL is a query language and way to interact with APIs just like REST.

The main diffrence between this and REST API's are how you query and how they display those information.

A normal Graphql query can look like this

```javascript
{
  Cereal((name: "Lucky Charms"));
  {
    sugar;
    protein;
  }
}
```

and this will give us output like so :

```javascript
{
  "data" : {
  "Cereal" :{
    "sugar" : "500mg";
    "protein" : "0mg;
        }
    }
}
```

### How it works?

Lets see how we can write these queries :

This is kinda the basic tempalte of how they work

```javascript
{
<type>  {
     field,
     field,
      ...
        }
}
```

Developers will set up schemas to use GraphQL and schema is bascially where we define what types are available to use and in schema Query is the root type , anything here is allowed to be searchable and in this Query type we can set up arguements and stuff we can take to use to search this query like lets say id or name of a product.

Then second field is the type where we define all the fields and that can return data in our response.

Main benefit of graphql is flexibility we can get as much and as little data as we want.

Then the way the functions can return the data is written.
Then we have the root variable which tells which function to use when dealing with which object.

Graphql luckily for us documents itself preety well, it comes with certain objects, types and fields that allow us to get information on all the other types.

So basically we can gain a lot of information without even fuzzing endpoints or anything.

All the types defined in schema method are documented through the `__schema object` . So we can get all info about types from here. And then to know about types we can use the field `types`, then we can search with the fields we want.

Then we can use \_type to build objects and use some param to specify which type we want more information on.


To get schemas and information about a Graphql api we can use : 
```javascript 
query Introspection{
  __schema{
  types{
    name
    description
    }
  } 
}
```

and a great extension to inspect and edit these GraphQL queries in Burp suite is InQL 
