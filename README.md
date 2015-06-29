# tuto-mcfly-server-access

## The project 

In this tutorial we are going to create a server in which some objects will be secured (you must login to access them)
 and others will be unsecured.

The server will be built with loopback. 

## Server side

### Begining

We first need to create a folder :
```Bash
mkdir tuto-mcfly-server-access
```


We then need to create a server :
```
slc loopback 
```

You can check it works :
```
slc run
```

### Creating some objects

Now we will create the BaseUser (secured) object in the server.  
Download **https://github.com/cosmojs/universal** and copy the common folder into your folder.


Then we install mcfly-loopback :
```
npm install --save mcfly-io/mcfly-loopback
```

In order to add BaseUser to our server go to model-config.json and add :

```JSON
"BaseUser": {
        "dataSource": "db",
        "public": true
    }
```



Don't forget to hide user 
```JSON
"User": {
        "dataSource": "db",
        "public": false
    },
```
We need to attach our objects to a datasource :
```
slc loopback:datasource
```

You can name it mongo and choose MongoDb.
This is the way we store data.  
(You can see below in config.js that we choosed a local storage. However, you can use mongolab).

If you used mongo, don't forget to install it :
```
npm install loopback-connector-mongodb
```


### Security : From AccessToken to JWT

To pass to JWT : Go to server/boot/authentication.js
```Javascript
var mcflyLoopback = require('mcfly-loopback');
var config = require('../config');

module.exports = function enableAuthentication(server) {
 // enable authentication
 server.enableAuth();
 mcflyLoopback.oauth(server, config);
};
```

Let's create a new file server/config.js  
```javascript
module.exports = {
 mongoURI: process.env.MONGO_URI || 'localhost',
 userModel: process.env.USER_MODEL || 'BaseUser',
 authHeader: process.env.AUTH_HEADER || 'Satellizer',
 tokenSecret: process.env.TOKEN_SECRET || 'A hard to guess string',
 oauth: {
   facebook: {
     secret: process.env.FACEBOOK_SECRET || 'yourcode'
   },
   google: {
     secret: process.env.GOOGLE_SECRET || 'yourcode'
   }
}
}
```


If we want the app to handle google, facebook etc.... Let's add :
```javascript
module.exports = {
 mongoURI: process.env.MONGO_URI || 'localhost',
 userModel: process.env.USER_MODEL || 'BaseUser',
 authHeader: process.env.AUTH_HEADER || 'Satellizer',
 tokenSecret: process.env.TOKEN_SECRET || 'A hard to guess string',
 oauth: {
   facebook: {
     secret: process.env.FACEBOOK_SECRET || 'yourcode'
   },
   google: {
     secret: process.env.GOOGLE_SECRET || 'yourcode'
   }
}
}
```

### Adding a secured data
Create **Car** with loopback 
```
slc loopback:model Car
```
This will generate a file common/models/Car.json  
In order to handle **access** add 

```JSON
"acls": [{
       "principalType": "ROLE",
       "principalId": "$everyone",
       "permission": "DENY"
   }, {
       "accessType": "READ",
       "principalType": "ROLE",
       "principalId": "$authenticated",
       "permission": "ALLOW"
   }],
```

More details about ACLs here  
[Here Loopback](http://docs.strongloop.com/display/public/LB/Model+definition+JSON+file;jsessionid=96C4FBE779BE4B9E9E79EE47F3C19411#ModeldefinitionJSONfile-ACLs)


Hence, the common/models/Car.json file should look like to this :
```JSON
{
   "name": "Car",
   "base": "PersistedModel",
   "idInjection": true,
   "options": {
       "validateUpsert": true
   },
   "properties": {
       "name": {
           "type": "string"
       }
   },
   "validations": [],
   "relations": {},
   "acls": [{
       "principalType": "ROLE",
       "principalId": "$everyone",
       "permission": "DENY"
   }, {
       "accessType": "READ",
       "principalType": "ROLE",
       "principalId": "$authenticated",
       "permission": "ALLOW"
   }],
   "methods": []
}
```


### Some necessary tools

You may find some errors if running the server.  
To prevent it run :  

```
npm install mcfly-io/mcfly-loopback
```


## Client side

We first need to create a folder :
```Bash
mkdir tuto-mcfly-client-access
```

Then we use the generator:
```
yo mcfly tuto-mcfly-client-access
```

You will have to set an array of choices :
![capture d ecran 2015-06-29 a 11 15 24](https://cloud.githubusercontent.com/assets/8570784/8403789/587f31e2-1e50-11e5-9ab4-de5d66989c1c.png)

If you have this error:  
![capture d ecran 2015-06-29 a 11 33 28](https://cloud.githubusercontent.com/assets/8570784/8404072/b538e782-1e52-11e5-9c85-f01a8c30f927.png)

Try this workaround:

In the project where the install failed you'll find a file named .gulps-package.json  
This file is generated before the generator tries to do an npm install in case it fails

So you can do the following:

  * copy the content of the devDependencies section of this file in the devDependencies of package.json  
  * modify the version of gulp-sass to be 2.0.2  
  * save the resulting package.json  
  * run npm install this should put you in the same state as if the generated succeeded  
  * let me know how it went  



On crée un module avec le generator
yo mcfly:module common
yo mcfly:controller common login
gulp browsersyncl


cf wiki

bower install satellizer
In package.json add satellizer in browser  

Dans login.js passer satellizer en parametre de la fonction

ajouter scope en param de la fonction

lb-ng to build a file

lbServices à ajouter à scrips c'est le fichier qui permet de communiquer avec le server depuis le client

AJouter bodyparser poour loopback c'est un middlewear















