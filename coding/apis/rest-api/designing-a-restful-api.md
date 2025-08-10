# Designing A Restful API

Over the past year I've been doing a lot of work on creating RESTful APIs, from the experience of creating APIs I've
learnt a few things that you should try to include on API's you create. In this tutorial we're going to go through some
of the best practices you should try to include in your API.

## Understanding The HTTP Method Types

Before you build a RESTful API it's important to understand the HTTP method types you can use. There are 5 HTTP
methods: - `GET` for fetching data

- `POST` used for storing new data
- `PUT` used for updating a whole entity
- `PATCH` used for updating part of an object
- `DELETE` used to delete data

One advantage of understanding these HTTP methods is that it allows you to make more meaningful routes on your API. You
don't need a `get-product` route and a `add-product` route or a `delete-product` route you can have one route
of `products` and use HTTP methods to decide how to handle this request.

```php
$api->get('products', 'ProductController@getProduct');
$api->post('products', 'ProductController@addProduct');
$api->put('products', 'ProductController@updateProduct');
$api->delete('products', 'ProductController@deleteProduct');
```

## Use Correct Return Types

When using HTTP request types it's important to also use the correct HTTP status codes. The most common HTTP status
codes you will be aware of is the successful 200 code or the not found 404 code, but there are loads
more. [HTTP Codes](https://paulund.co.uk/explaining-the-different-http-status-codes)### Successful Codes

- `200` - successful
- `201` - created
- `204` - no content

On a successful return from the API you can use the HTTP status codes to explain what type of successful action has
happened. For example, if a call to GET data is returned successfully you should return a `200` code along with the
object the API fetched. If you're creating an object you could return a `201` status code to mark the record as created.
If you're deleting an object you simply want to return successful but there's no data to return so you can use a
HTTP `204` to say no content is return but it was successful effectively a boolean true. ### Data Errors

- `400` - Bad request
- `404` - Not found
- `500` - server throw an unexpected error

A `400` status indicates that the server doesn't understand the request due to an invalid syntax. A `404` status shows
that the requested object was not found. A `500` status shows that there was an unexpected error on the server. These
will general come from when your application throws an uncaught exception from a general error in your code. ### Auth
Errors

- `401` - Request has failed authorisation
- `403` - Access token has forbidden access to the API

`401` code indicates that the request could not be performed as it does not pass the authorisation requirements. `403`
indicates that an access token from supplied by the token was not accepted or has expired.

## Versioning

If you're developing an API that's going to be consumed by multiple clients then you need to prepare yourself for
versioning. This is because there will be eventual change to some of the endpoints which might be consumed by one client
but not the other, therefore one client application will work correctly the other client application will break. By
using versions on your API you could have a client using the new `v2` API and the other continuing to use the old `v1`
endpoints. You can do versioning by simply prefixing the endpoints with the current version.

```bash
GET example.com/api/v1/posts
GET example.com/api/v2/posts
```

## How To Name Your Endpoints

When naming your API endpoints you should always think of them as nouns and not verbs. You need to define your endpoints
as resources for example if you have an endpoint where you want to get all the products, it's tempting to use the
URL `/getAllProducts`. In REST APIs we need to use resources and nouns for our endpoints so instead, we will simply use
the endpoint `/products` and the use HTTP request methods to define what we want the endpoint to do. For example with
our endpoint `/products` using a GET method we know we want to get the products. Or we could change the method to a
POST `/products` we use the same endpoint to create a new product.

## Use of Singular or Plural Urls

Sometimes you'll see a mix of singular and plural URLs, I find this can be confusing and like to stick to just plurals.
This will represent a more directory style structure to your URLs. Get all the products by calling `/products`. Get a
product by ID by using `/products/1`

## Filtering With Querystrings

You should use querystrings when you want to filter the results for example on the all posts endpoint you might want to
only the last 10 posts, it doesn't make sense to create an endpoint like `GET example.com/api/v1/posts/all/last/10`, you
should use a querystring like `GET example.com/api/v1/posts?per_page=10`. The reason you should use querystring for this
is that you could pass in more filters such as a category ID you can't
do `GET example.com/api/v1/posts/all/last/10/category/5` as the endpoints start to become confusing and the syntax will
need to be looked up each time. You should use categories in the querystring

```bash
GET example.com/api/v1/posts?per_page=10&category=5
```

## Using Response Envelopes

This is an area that causes some discussion, some people are along the lines of not enveloping the return data which
means the return json will look something like.

```json
{
{
  "foo": "boo"
},
{"foo2": "boo2"},
{"foo3": "boo3" },
{"foo4": "boo4"},
{"foo5": "boo5"
}
}
```

But when developing APIs I like to return responses in a data envelope such as.

```json
{
  data: [
    {
      "foo": "boo"
    },
    {
      "foo2": "boo2"
    },
    {
      "foo3": "boo3"
    },
    {
      "foo4": "boo4"
    },
    {
      "foo5": "boo5"
    }
  ]
}
```

There are a few reasons that I prefer to work this way. One thing is that it allows you to add any additional
information in the return JSON, one example of this is pagination data if you request 10 posts on page 9 you also return
links to the next and previous pages.

```json
{
  data: [
    {
      "foo": "boo"
    },
    {
      "foo2": "boo2"
    },
    {
      "foo3": "boo3"
    },
    {
      "foo4": "boo4"
    },
    {
      "foo5": "boo5"
    }
  ],
  links: {
    "next": "",
    "prev": ""
  }
}
```

Another reason to use data envelopes in your responses is for security reasons with JSON hijacking on older browsers the
following article explains the JSON vulnerability in returning an
array. [Anatomy of a Subtle JSON Vulnerability](http://haacked.com/archive/2008/11/20/anatomy-of-a-subtle-json-vulnerability.aspx/) [JSON Hijacking](http://haacked.com/archive/2009/06/25/json-hijacking.aspx/) [AJAX Security Cheat Sheet](https://www.owasp.org/index.php/AJAX_Security_Cheat_Sheet#Always_return_JSON_with_an_Object_on_the_outside)

## Pagination

It's important to provide a way of pagination the data you return from the API. For example, if you have an endpoint to
get all the products `/products` depending on the size of the company this could return 10 products but it could return
10,000 products. This is why it's important to return the data in paged form typically 10 or 20 items per page. If you
use Laravel then you'll be familiar with the [basic pagination functionality](https://laravel.com/docs/5.4/pagination)
this uses a `page=2` querystring to return a chunk of the data.

```json
{
  "total": 50,
  "per_page": 15,
  "current_page": 1,
  "last_page": 4,
  "next_page_url": "http://laravel.app?page=2",
  "prev_page_url": null,
  "from": 1,
  "to": 15,
  "data": [
    {
      // Result Object
    },
    {
      // Result Object
    }
  ]
}
```

You can then grab the `next_page_url` and `prev_page_url` and return in the API response. Here is a tutorial
on [how to build a VueJS pagination component](https://paulund.co.uk/vuejs-laravel-pagination-component).

## Security - Access Tokens

RESTful services by definition are stateless, therefore you can't handle login authentication with cookies or sessions
like you would in another application. To get around this RESTful services will use access tokens to handle
authentication via an `Authorization` header parameter. There are generally two types tokens, direct access tokens and
refreshed tokens.

### JWT - Access Token

An access token will be returned by the API on successfully authenticating the user, this token is supposed to be short
lived and used to access the application during the "logged" in the duration of the user. The next time the user comes
to log in, a different access token will be used.

### Refreshed Tokens

Refresh tokens will be returned when the user logs in but will also be hashed and stored in the database, you're then
able to attach an expiry timestamp and a user to this token. The benefit of having a timestamp attached to the token is
that you can revoke access to your application by simply expiring the token. Refreshed token are supposed to be longer
life tokens and used for a client to request a short life token from the API. This means that we no longer need to use
the username and password to request a short life token we can use this refresh token. You can also refresh the expiry
of this token on each login which means the user doesn't need to log in each time they use your application. For further
readings on access tokens it's important to be familiar with
the [OAUTH specification](https://oauth.net/articles/authentication/).

## Throttling

Having some sort of throttling protection is very important for your API. You need to make sure people can't just
continually call your API with 100s of requests a second. You should guard against this by making sure the API will fail
if there too many requests per minute. If you're a Laravel user there is an inbuilt middleware to handle throttling by
limiting the requests to 60 a minute, by using the
middleware [\\Illuminate\\Routing\\Middleware\\ThrottleRequests::class](https://github.com/laravel/framework/blob/5.4/src/Illuminate/Routing/Middleware/ThrottleRequests.php)
This will return some handy headers that can be used by your clients to work out how many requests they're allowed to
make. - X-Rate-Limit-Limit - The number of allowed requests

- X-Rate-Limit-Remaining - The number of remaining requests
- X-Rate-Limit-Reset - The number of seconds left in the current period

## How To Display Errors

It's important to define a single envelope used for your error responses, this is important to your clients to have a
consistent node you can use to search for errors. For example, the first code section below will have **code**, *
*message** and **description**. This doesn't give a standard error envelope to search for from any clients using your
API.

```json
{
  "code": 1234,
  "message": "Something bad happened :(",
  "description": "More details about the error here"
}
```

I believe that it's a good idea to use code like below where you return any errors inside an `error` node this way any
clients can do a check for node on the response and will know how to handle the response and if they need to display an
error to the user.

```json
{
  "error": {
    "code": 404,
    "message": "Page not found"
  }
}
```

## Object Responses From Updates & Creation

PUT, POST, PATCH calls will all modify data in the databases. Many APIs will return a boolean status of true to show the
resource has been updated or an error to show the resource hasn't been updated. I prefer to return the updated or
created resource, for example if a PATCH call was made then this has changed some data in your database, instead of
returning a boolean where the client will need to make another API call to get the new resource, why not just return the
new data from the PATCH endpoint. For the POST endpoints they will be used to create a new resource which means they
should return a HTTP status code of a 201. Therefore you should try to return the URL to get the new resource and the
newly created ID in the headers of the response. This will allow clients to be able to query the API to return the newly
created resource.

## Including Related Resources

Including related resources from your API calls can be very helpful to clients trying to pin together related content.
For example, if you had an API for a blog, you'll have an endpoint to get the posts `/posts/100` this will return data
like.

```json
{
  "data": {
    "id": 100,
    "title": "The blog post",
    "featured_image": 1,
    "author_id": 1,
    "category_id": 1
  }
}
```

With a blog post you have links to other resources such as the author information and the category information. This
might be information you need to display on the page to show the blog post correctly. You might want to display the
author bio box at the bottom of the post or a link to the category page so the user can see related articles. In your
endpoint it's useful to allow them to add an `include` parameter which will return the related
resource. `/posts/100?include=author,category` This should return the included resources.

```json
{
  "data": {
    "id": 100,
    "title": "The blog post",
    "featured_image": 1,
    "author_id": 1,
    "author": {
      "id": 1,
      "name": ""
    },
    "category_id": 1,
    "category": {
      "id": 1,
      "name": ""
    }
  }
}
```

## snake_case vs camelCase

It's important to decide on a standard of using snake_case or camelCase in the field names of your API. If you're return
type is JSON then it's important to note that the JavaScript naming convention is to use camelCase for your field names.
Although I find that snake_case is easier to read. If you look at many popular API responses you'll see many of them
will use the snake_case. I suggest you go with snake_case for your field names but whichever one you choose make sure
you stick with it.

## Documentation

When it comes to documentation, it's probably the most important area of your API, especially if your API is externally
accessible. Without good documentation users will not understand how or what data they will be able to access. The
documentation should show examples of complete requests and responses to show real examples of how to use your
endpoints. If you haven't used it already I suggest you take a look at Swagger API
framework. [Swagger Documenation](https://swagger.io/) Swagger is a full API framework, you can use it to define your
API and can even download a starter application will all the endpoints you need straight from a config file. View the
click below to see a demo of a Swagger config and the generated documentation from the YAML
config. [Swagger Editor](https://editor.swagger.io/)
