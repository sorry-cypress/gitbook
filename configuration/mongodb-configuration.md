---
description: Configuration entries are similar for API and for Director services
---

# MongoDB Configuration



`MONGODB_URI="mongodb://mongo:27017"`

MongoDB connection URL, required if using "mongo" execution driver.



`MONGODB_DATABASE="sorry-cypress"`

MongoDB database name, required if using "mongo" execution driver.

`MONGODB_TLS`

TLS connection settings, settings to `true` will enable TLS for MongoDB connection



`MONGODB_AUTH_MECHANISM`

MongoDB authentication mechanism. See [MongoDB Authentication](https://mongodb.github.io/node-mongodb-native/3.0/tutorials/connect/authenticating/).



`MONGODB_AUTH_USER`

MongoDB authentication user, when `MONGODB_AUTH_MECHANISM` is non-empty



`MONGODB_AUTH_PASSWORD`

MongoDB authentication `password`, when `MONGODB_AUTH_MECHANISM` is non-empty



\`\`

