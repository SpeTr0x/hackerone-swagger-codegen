# Swagger Codegen for the H1 codegen libraries

## Overview
This is a project to generate our own clients library with Swagger.  We created this to support several features that were not (yet) present in the current generators. We can maintain it here, and see how the original
generators progress. All changes we can upstream to the `swagger-codegen-generators` project.

## What's Swagger?
The goal of Swagger™ is to define a standard, language-agnostic interface to REST APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined via Swagger, a consumer can understand and interact with the remote service with a minimal amount of implementation logic. Similar to what interfaces have done for lower-level programming, Swagger removes the guesswork in calling the service.


Check out [OpenAPI-Spec](https://github.com/OAI/OpenAPI-Specification) for additional information about the Swagger project, including additional libraries with support for other languages and more. 

## How do I use this?
At this point, you've likely generated a client setup.  It will include something along these lines:

```
.
|- README.md    // this file
|- pom.xml      // build script
|-- src
|--- main
|---- java
|----- com.hackerone.codegen.v3.generators.python.H1PythonClientCodegen.java // generator file
|---- resources
|----- handlebars
|------ python // template files
|----- META-INF
|------ services
|------- io.swagger.codegen.v3.CodegenConfig
```

You _will_ need to make changes in at least the following:

`H1PythonClientCodegen.java`

Templates in this folder:

`src/main/resources/handlebars/python`

Once modified, you can run this:

```
mvn package
```

In your generator project.  A single jar file will be produced in `target`.  You can now use that with codegen:

```
java -cp /path/to/swagger-codegen-cli.jar:path/to/hackerone-swagger-codegen-1.0.0.jar io.swagger.codegen.Codegen -l H1SwaggerPyClientCodegen -i /path/to/swagger.yaml -o ./test
```

Now your templates are available to the client generator and you can write output values

## But how do I modify this?
The `H1PythonClientCodegen.java` has comments in it--lots of comments.  There is no good substitute
for reading the code more, though.  See how the `H1PythonClientCodegen` implements `CodegenConfig`.
That class has the signature of all values that can be overridden.

For the templates themselves, you have a number of values available to you for generation.
You can execute the `java` command from above while passing different debug flags to show
the object you have available during client generation:

```
# The following additional debug options are available for all codegen targets:
# -DdebugSwagger prints the OpenAPI Specification as interpreted by the codegen
# -DdebugModels prints models passed to the template engine
# -DdebugOperations prints operations passed to the template engine
# -DdebugSupportingFiles prints additional data passed to the template engine

java -DdebugOperations -cp /path/to/swagger-codegen-cli.jar:/path/to/your.jar io.swagger.codegen.Codegen -l H1SwaggerPyClientCodegen -i /path/to/swagger.yaml -o ./test
```

Will, for example, output the debug info for operations.  You can use this info
in the `api.mustache` file.
