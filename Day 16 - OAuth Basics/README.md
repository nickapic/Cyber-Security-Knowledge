Main Resource Used : 
https://portswigger.net/web-security/oauth : Web Security Academy : OAuth Authentication
https://www.youtube.com/watch?v=j-bHvqQ378s :Understanding How OAuth2 Works : Engineer Man

OAuth is basically an algorithm that helps us access the users data from a website with a cool. Lets so for example we have our website A and we want people to login without having them to make an account we can use OAuth and ask data from an Oauth provider like Discord lets say.  We can specify yhe specific data we want from them and then the user would just have authorize if they allow access for our website A to access that specific bit of data. So in simpler terms OAuth allows the user to grant this access without exposing their login credentials to the requesting application. 

Many times sites use this along with Social media websites to let you log in using them. This is super popular now , but it does have a lot of implementation mistakes.

OAuth allows web developers to also request access to certain data to integrate 3rd Party functionality. It works via defining a series of interactions b/w 3 distinct parties ,namely client, resource owner and OAuth service Provider. There are a lot of diffrent ways this can be implmented as well. The most popular types of it are "authorization code" and "implicit" : 
The main steps for it are : 
1. Client app requests access to the specified data , after telling which grant type they want to use.
2. User will then be prompted to give their consent 
3. The client recieves a unique access token that proves they have permission to view the users data.
4. The client application can then just use this token to make API calls and fetch the needed data from resource server.

OAuth Authentication : 

This was not intended before but now its very popular for websites to use OAuth to help authenticate users.

The basic OAuth flows remain alrgely the same for this , the main diffrence would be how the cleint applications  uses the data it gets. From the user side, the user OAuth auth is something similar to SSO.

Generally used like : 
1. User chooses to option login with the OAuth Provider, the client application then uses the Oauth provider to access to some data that it can use to idenitfy the user. Could be emails, username,etc.
2. The application can then use access token to access the information from OAuth Service Provider API endpoints.
3. After having the Token, the applicaiton uses it in place of username , email to log the user in and the access token that it gets is often used instead of traditional password.

Vulnerabilites in this arise due to : 

1. Due to the Data Specification in OAuth being vague and flexible.
2. Plenty of opportunties for Bad practices to creep in
3. Lack of Built-in Security features. It relies mostly on developers and also extra security features needing to be implemented.
4. Depending on the type of data being transfered it could be higly sensitive at times, and could be intercepted at times. 