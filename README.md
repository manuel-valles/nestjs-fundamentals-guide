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

- Run NestJS in development mode: `yarn start:dev`.

  ### 2.1. Creating a Basic Controller ([Full Controllers documentation](https://docs.nestjs.com/controllers)):

  - Controllers are one of the most important building blocks in NestJS application as they handle requests.
  - Create a controller with the CLI:
    - `nest generate controller` or `nest g co`: provide the name of it, e.g. 'coffees'.
    - Some flags:
      - `nest g co --no-spec`: without the test file;
      - `nest g co modules/abc --dry-run`: to try a command, e.g. where it would be allocated.
  - In order to create a basic controller, we use classes and decorators. Decorators associate classes with required metadata and enable Nest to create a routing map (tie requests to the corresponding controllers).
  - Nest provides decorators for all of the standard HTTP methods: `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, and `@Head()`. In addition, `@All()` defines an endpoint that handles all of them.
  - The route path for a handler is determined by concatenating the (optional) _prefix_ declared for the controller, and any _path_ specified in the method's decorator. Using a path prefix in a @Controller() decorator allows us to easily group a set of related routes, and minimize repetitive code. For example, a path prefix of 'coffees' combined with the decorator @Get(`flavours`) would produce a route mapping for requests like GET `/coffees/flavours`.

    #### 2.1.1. Using Route Parameters:

    - In order to define routes with parameters, we can add route parameter tokens in the path of the route to capture the dynamic value at that position in the request URL. Route parameters declared in this way can be accessed using the @Param() decorator, which should be added to the method signature.
    - @Param() is used to decorate a method parameter (e.g. `params`), and makes the route parameters available as properties of that decorated method parameter inside the body of the method. So, we can access the id parameter by referencing `params.id`. You can also pass in a particular parameter token to the decorator, and then reference the route parameter directly by name in the method body: `@Param('id') id: string`.

    #### 2.1.2. Response Status Codes:

    - The response **status code** is always **200** by default, except for POST requests which are **201**. We can easily change this behavior by adding the `@HttpCode(...)` decorator at a handler level.

    #### 2.1.3. Query Params:

    - Nest has a helpful decorator for getting all or a specific portion of the query parameters called `@Query()`, which works similar to `@Param()` and `@Body()`.

  ### 2.2. Creating a Basic Service ([Full Providers documentation](https://docs.nestjs.com/providers)):

  - Many of the basic Nest classes may be treated as a **provider** – _services, repositories, factories, helpers_, and so on. The main idea of a provider is that it can be injected as dependency; this means objects can create various relationships with each other.
  - Controllers should handle HTTP requests and delegate more complex tasks to providers. Providers are plain JavaScript classes that are declared as providers in a module.
  - To create a service using the CLI, simply execute the command `$ nest g service coffees` or `nest g s`.
  - The `@Injectable()` decorator attaches metadata, which declares that CatsService is a class that can be managed by the Nest IoC container.
  - The CoffeesService is injected through the **class constructor**. Notice the use of the private syntax. This shorthand allows us to both declare and initialize the coffeesService member immediately in the same location.
  - Nest is built around the strong design pattern commonly known as **Dependency injection**. In Nest, thanks to TypeScript capabilities, it's extremely easy to manage dependencies because they are resolved just by type. Nest will resolve the _coffeesService_ by creating and returning an instance of _CoffeesService_ (or, in the normal case of a singleton, returning the existing instance if it has already been requested elsewhere). This dependency is resolved and passed to your controller's constructor (or assigned to the indicated property).

    #### 2.2.1. Handling errors:

    - Nest provides a built-in **HttpException** class and a set of standard exceptions that inherit from the base HttpException, like **NotFoundException**:
      ```ts
      throw new HttpException(`Coffee #${id} not found`, HttpStatus.NOT_FOUND);
      throw new NotFoundException(`Coffee #${id} not found`);
      ```
    - [Further information about Exception filters](https://docs.nestjs.com/exception-filters)

  ### 2.3. Creating a Basic Module ([Full Modules documentation](https://docs.nestjs.com/modules)):

  - A module is a class annotated with a `@Module()` decorator. The @Module() decorator provides metadata that Nest makes use of to organize the application structure. It takes a single object whose properties describe the module:
    - The **providers** that will be instantiated by the Nest injector and that may be shared at least across this module;
    - The set of **controllers** defined in this module which have to be instantiated;
    - The list of imported modules or **imports** that export the providers which are required in this module;
    - Thw subset of providers or **exports** that are provided by this module and should be available in other modules which import this module.
  - Each application has at least one module, a **root module**. The root module is the starting point Nest uses to build the application graph (the internal data structure Nest uses to resolve module and provider relationships and dependencies).
  - Modules are strongly recommended as an effective way to organize your components. Thus, for most applications, the resulting architecture will employ multiple modules, each encapsulating a closely related set of capabilities.
  - To create a module using the CLI, simply execute the command `$ nest g module coffees`.
