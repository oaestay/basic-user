# Basic User Application

- [Introduction](#introduction)
- [Setup](#setup)
- [Test APIs](#test-apis)

## Introduction

This is a basic user application that's composed of 4 components:

- Sign In Client
- Admin Client
- Proxy API
- Private API

Both clients are written in ReactJS and both APIs are written in Ruby on Rails.

### Sign In Client

Basic app, that has a form containing a text field to write an email address, and an image field. The email and image are sent using Axios to the Proxy API.

If the image matches the user, there's a 90% chance that an 'OK' message appears, in any other case, the error is displayed.

### Admin Client

A simple app that has a few routes, that allow the user to read, create, edit and delete users in the database.

### Proxy API

Rails API, that forwards the requests of the previous clients to the Private API. It has a private key, that is checked by the private API to allow requests. 

### Private API

API connected to the user's database, and has the method to check if an image matches an email address. Also, it's in charge of sending an email to the account's email address when it's accessed.

## Setup

### Sign In Client

Install the dependencies
```
yarn install
```

Change the URL of the Proxy API in App.js

Run the application on port 3001 (to change the port, modify package.json)
```
yarn start
```

### Admin Client

Install the dependencies
```
yarn install
```

Run the application on port 3002 (to change the port, modify package.json)
```
yarn start
```

### Proxy API

Install the dependencies
```
bundle install
```

Set environment variables:
- `API_SECRET_KEY` (Secret API key, must match the variable in the private API)
- `SECOND_SERVICE_URL` (URL of the private API server, ex: https://private-user-api.herokuapp.com)


Run the server on port 3010
```
rails s -p 3010
```

### Private API

Install the dependencies
```
bundle install
```

Create the database

```
rails db:development:prepare
```

Run the migrations
```
rails db:migrate
```

Set environment variables:
- `API_SECRET_KEY` (Secret API key, must match the variable in the private API)
- `SECOND_SERVICE_URL` (URL of the private API server, ex: https://private-user-api.herokuapp.com)

Configure the mailer in `*.rb` files in `config/environment/`, and the email address in `user_notifier_mailer.rb`

Run the server on port 3020
```
rails s -p 3020
```

## Test APIs

If you have followed the previous steps, you should have both APIs working together. To test the APIs, you could use tools as [Postman](https://www.getpostman.com/), [HTTPie](https://httpie.org/), or simply cURL.

Create an user using the admin client, or simply post a JSON to the API:

```
# POST $PROXY_URL/users
{
    email: "someemail@dom.com",
    image: $IMAGE_IN_BASE64,
}
```

Then, you should be able to verify the user:

```
# POST $PROXY_URL/rest/login
{
    email: "someemail@dom.com",
    image: $IMAGE_IN_BASE64,
}
```

There's also automated test in the private API, using RSpect and FactoryGirl.
