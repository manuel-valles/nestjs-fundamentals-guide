# NestJS Fundamentals

A guide from the [Official NestJS Fundamentals](https://courses.nestjs.com/).

## 1. Installation

- `npm i -g @nestjs/cli`
- `nest --version` (current version **7.5.1**)
- `nest new`: Provide a name for your app and the package manager of your choice, e.g. `coffee app` and `yarn`
- `cd coffee-app`
- `yarn start`

_NOTE_: Decorators (@Module, @Controller,...) are functions that apply logic. They can be applied to classes, methods, properties and parameters.

## 2. Creating a REST API application

- [Full Controllers documentation](https://docs.nestjs.com/controllers)
- Run NestJS in development mode: `yarn start:dev`.

### 2.1. Creating a Basic Controller:

- Controllers are one of the most important building blocks in NestJS application as they handle requests.
- Create a controller with the CLI:
  - `nest generate controller` or `nest g co`: provide the name of it, e.g. 'coffees'.
  - Some flags:
    - `nest g co --no-spec`: without the test file;
    - `nest g co modules/abc --dry-run`: to try a command, e.g. where it would be allocated.
- In order to create a basic controller, we use classes and decorators. Decorators associate classes with required metadata and enable Nest to create a routing map (tie requests to the corresponding controllers).
- Nest provides decorators for all of the standard HTTP methods: `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, and `@Head()`. In addition, `@All()` defines an endpoint that handles all of them.
- The route path for a handler is determined by concatenating the (optional) _prefix_ declared for the controller, and any _path_ specified in the method's decorator. Using a path prefix in a @Controller() decorator allows us to easily group a set of related routes, and minimize repetitive code. For example, a path prefix of 'coffees' combined with the decorator @Get(`flavours`) would produce a route mapping for requests like GET `/coffees/flavours`.

### 2.2. Using Route Parameters:

- In order to define routes with parameters, we can add route parameter tokens in the path of the route to capture the dynamic value at that position in the request URL. Route parameters declared in this way can be accessed using the @Param() decorator, which should be added to the method signature.
- @Param() is used to decorate a method parameter (e.g. `params`), and makes the route parameters available as properties of that decorated method parameter inside the body of the method. So, we can access the id parameter by referencing `params.id`. You can also pass in a particular parameter token to the decorator, and then reference the route parameter directly by name in the method body: `@Param('id') id: string`.

### 2.3. Response Status Codes:

- The response **status code** is always **200** by default, except for POST requests which are **201**. We can easily change this behavior by adding the `@HttpCode(...)` decorator at a handler level.

### 2.4. Query Params:

- Nest has a helpful decorator for getting all or a specific portion of the query parameters called `@Query()`, which works similar to `@Param()` and `@Body()`.
