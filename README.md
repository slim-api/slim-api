#slim-api
Basic slim api project and generator

![travis-ci status](https://travis-ci.org/gabriel403/slim-api.svg)

#Status

pre-alpha, init and create models/controllers/scaffolds is mostly complete, migrations and develop relations in model migrations.

#What?

A simple generator for producing simplified controllers/models and migrations, using Slim, Phinx and eloquent. It should be relatively simple to replace the controller|model template, and use something other than Phinx for migration or eloquent for models.

#Why?

I wanted to be able to create API end points as easily as possible, and I love the simplicity of Slim, and after a sordid time with RoR this seemed like a fun thing to do!

#How?


###Init

Basic useage is simple, we first have to initiate the project, this creates a default skeleton for the project and initiates the phinx configuration.

```
slimapi init <project name> [location]
```

Location defaults to the cwd if not specified.

###Models

We can then generate a model, this creates a migration, a simple model class and DI configuration.

```
slimapi generate model <model name> <model definitions>
```

Model definitions are a space seperated list of column definitions, of the form `name:type:limit:null:unique`, so

```
slimapi generate model Foo bar:integer baz:string:128:false bazbar:string:128::true
```

Would create a migration of 3 columns, baz would have a character limit and can't be null, bazbar would have a character limit and must be unique.

###Controllers

We can create a controller, this creates a simple controller, route and DI configuration.

```
slimapi generate controller <controller name> [methods]
```

Methods defaults to index, get, post, put and delete and are empty by default.
The controller name influences how the route is designed.

```
slimapi generate controller Foo index post
```

Would generate a controller named Foo with empty methods index and post. It would also create the GET/POST `/foo` route.

```
slimapi generate controller Foo
```

Would generate a controller named Foo with empty methods index, get, post, put, delete.
It would also create the GET/POST `/foo` routes and the GET/PUT/DELETE `/foo/{id}` routes.

###Scaffold

Scaffolding combines controller and model generation but with added jazz. It configures the controller to receive the model as a constructor param, configures the DI to inject the model to the controller and finally populates the normally empty controller methods with basic CRUD functionality. You can't provide arguments to specify controller methods (it creates them all), but you can supply your methods definition.

```
slimapi generate scaffold foo field1:integer field2:string
```

This would generate the Foo controller and appropriate routes, the Foo model/migration with field1/field2 as fillables and any required DI configuration.
