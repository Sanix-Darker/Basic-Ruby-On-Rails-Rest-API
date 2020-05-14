# Basic-Ruby-On-Rails Rest API

## Project Setup

**Install all gems**:

```shell
$ bundle install
```

**Update the database with new data model**:

```shell
$ rake db:migrate
```

**Feed the database with default seeds**:

```shell
$ rake db:seed
```

**Start the web server on `http://localhost:3000` by default**:

```shell
$ rails server
```

**Run all RSpec tests and Rubocop**:

```shell
$ rake test
```

## Usage

| HTTP verbs | Paths  | Used for |
| ---------- | ------ | --------:|
| POST | /register| Create a user|
| POST | /login   | Authenticate a user |
| GET | /posts    | List all posts|
| GET | /posts/:post_id | Show a single post |
| POST | /posts | Create a post |
| PUT | /posts/:post_id | Update a post |
| DELETE | /posts/:post_id | Delete a post |
| GET | /posts/:post_id/comments | List all comments of a post |
| GET | /posts/:post_id/comments/:comment_id | Show a single comment |
| POST | /posts/:post_id/comments | Create a comment |
| PUT | /posts/:post_id/comments/:comment_id | Update a comment |
| DELETE | /posts/:post_id/comments/:comment_id | Delete a comment |

## Use Case Examples

### Authentication

**Create a new user**:

```shell
$ curl -X POST -H 'Content-type: application/json' -d '{"email": "testuser@email.com", "password": "testuser123"}' localhost:3000/register
```

**Authenticate a user**:

```shell
$ curl -X POST -H 'Content-type: application/json' -d '{"email": "testuser@email.com", "password": "testuser123"}' localhost:3000/login
```

On successful login, `{"auth_token": <token>}` will be returned. This token will be expired after 24 hours.

### CRUD

**In order to access the posts and comments, add `-H 'Authorization: <token>'` to the header of every request for CRUD operations.**

The `create`, `update` and `delete` actions can only be executed by users authorized on admin. A default admin user is definded in `db/seeds.rb`. After seeding the database, `{"email": "admin@email.com", "password": "admin123"}` can be used to login as an admin.

**Create a new post**:

```shell
$ curl -X POST -H 'Content-type: application/json' -d '{"title": "My title", "content": "My content"}' localhost:3000/posts
```

**Get all posts**:
``shell
curl -H "Authorization: Bearer <auth_token>" http://localhost:3000/posts
``

**Create a new comment**:

```shell
$ curl -X POST -H 'Content-type: application/json' -d '{"name": "Sanix", "message": "My message"}' localhost:3000/posts/1/comments
```

The `name` field is optional with default value `anonym`.

**Update an existing post by id**:

```shell
$ curl -X PUT -H 'Content-type: application/json' -d '{"title": "My new title", "content": "My new content"}' localhost:3000/posts/1
```

**Update an existing comment by id**:

```shell
$ curl -X PUT -H 'Content-type: application/json' -d '{"name": "Sanix", "message": "My new message"}' localhost:3000/posts/2/comments/1
```

**Delete an existing post by id**:

```shell
$ curl -X DELETE localhost:3000/posts/1
```

All the comments of this post will be deleted as well.

**Delete an existing comment by id**:

```shell
$ curl -X DELETE localhost:3000/posts/2/comments/1
```
