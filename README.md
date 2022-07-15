# MixFrame Assessment Project

The project’s main goal is to display the list of users with the following information:
name, email, formatted address, phone, link to a website and a company name.

We can add and remove users from a list and also search users by a name.


## Tech Stack

**Client:** VueJs, Vuetify, axios, HTML, CSS


## Run Locally

Clone the project

```bash
  git clone https://github.com/Priya2994/Mixfame-Interview-Task.git
```

Go to the project directory

```bash
  cd Mixfame-Interview-Task
```

Install dependencies

```bash
  npm install
```

Start the server

```bash
  npm run serve
```

Compiles and minifies for production
```
npm run build
```

Lints and fixes files
```
npm run lint
```


## API Reference

We are using https://jsonplaceholder.typicode.com for api data.

***Note: Users Post and Delete API are not working, we are mocking it in frontend. Still api call is configured.***

#### Get all users

```http
  GET https://jsonplaceholder.typicode.com/users
```

The code will fetch a list of users from the API.

#### Add new user

```http
  POST https://jsonplaceholder.typicode.com/users
```

| Payload | Type     | Required                       |
| :-------- | :------- | :-------------------------------- |
| `name`      | `string` | &check; |
| `suite`      | `string` | &check; |
| `email`      | `string` | &check;|
| `phone`      | `string` |&check;|
| `website`      | `string` |&check;|
| `suite`      | `string` | &check;|
| `street`      | `string` | &check;|
| `city`      | `string` | &check;|
| `phone`      | `string` | &check;|
| `website`      | `string` | &cross;|
| `zipcode`      | `string` | &check;|
| `companyName`      | `string` | &check;|

The code will add a new user.

#### Delete User

```http
  DELETE https://jsonplaceholder.typicode.com/users/${item}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `id`      | `string` | **Required**. Id of user to delete |

The code will delete the user matched the passed id.
## Screenshots

![App Screenshot](URL)


## Demo


## Authors

- [@priyanka](https://github.com/Priya2994)

