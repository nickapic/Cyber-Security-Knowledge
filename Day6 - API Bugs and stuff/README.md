### API Ennumeration and stuff

Resources used : https://www.youtube.com/watch?v=yCUQBc2rY9Y, 


API : Application Programming Interface 

Used in Web and Mobile apps, and a lot of developers are lazy

APIs kinda like this : 
```

GET /apiendpoint/1 # Example of REST	
GET /apiendpoint?param=somevaluetoquery # This example of GraphQL

```

APIs often use JSON or XML , JSON is way more common types of APIs are SOAP,RESTful, GraphQL, etc.
JSON : Is just a way to represent data in a text format, they start with curly braces and arrays in json start with [] and the json item starts with {}.

Apps which might have API's : 
1. Webapp which has mobile App
2. Apps with a lot of frontend complexity
3. Almost all mobile apps
4. Webpage taking long time to load
5. Webapp with dev documentation

The two main ones are GraphQL and Restful 

RestFul : Are super easy to spot they use CRUD operations as their basis and are the most popular ones.
**Try playing around with the Type of Request and possible endpoints all the time as if get exists maybe a delete or update exists as well**

GRAPHQL : Difficult to spot , some examples could be like gql?q= , graphql?q= or g?q= , all the things are on one endpoint as its super easy to ennumerate and uses mutation, query and so on. 



Our main goal with most of the API ennumeration is to fuzz these endpoints on REST or Graphql apis which can lead to sensitive data disclosure also playing around with them and what type of request it is like POST,CREATE,UPDATE,DELETE and so on.



And to do these Fuzzing and so on we can use tools like FFUF,WFUZZ and so on a quick cheat sheet on FFUF is here :
```bash
ffuf -w /path/to/wordlist -u https://target/api/FUZZ  # Fuzzing an API endpoint on website target

# FFuf to ignore pages which give 401 error 

ffuf -w /path/to/values.txt -u https://target/script.php?valid_name=FUZZ -fc 401

# Fuzz Post Request Data 
ffuf -w /path/to/postdata.txt -X POST -d "username=admin\&password=FUZZ" -u https://target/login.php -fc 401

```

Link to FFUF : https://github.com/ffuf/ffuf and its documentation.

Here is the Link to WFUZZ : https://github.com/xmendez/wfuzz