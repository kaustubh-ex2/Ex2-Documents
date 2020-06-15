# Initiation

The project begins with an introduction to Homluv, its functioning and getting it to operate. The introduction is given by Ruchika Verma and Kritika Gakher.

### Understanding

Homluv website has two levels of pages with difference in specificity in search (or the depth in which the user searches primarily)

* First level page

  ![First level triggers](https://user-images.githubusercontent.com/60777148/83666154-1f0b8380-a5ea-11ea-88ba-b67c3e9cd089.png)

  The first level page is seen when the user clicks on any of these images and comes to a catalog of these categories.

* Second level page

  ![Second level triggers](https://user-images.githubusercontent.com/60777148/83666320-64c84c00-a5ea-11ea-87ca-0fb0d41fdf01.png)

  The second level page is seen when the user searches a location. 

* Images are lazy loaded

* Inspiration page shows the popular homes to the user for -

  - The last location the user searched (if the user searched)
  - The current location of the user (if granted access)
  - For the case other than above two,  {{ TODO }}

* User can luv (like) a home as a logged in or an anonymous user. In both cases the luvs will be preserved in their profile (should they ever make it) or in their browser cache if they don't.

* A home can be viewed in two states

  - Quick view
    - As a separate section on the right side of the page
  - Home Detail Page
    - As a dedicated page for home detail. It shows all the home images present along with the map to its location and community images

* As a new member of the Homluv team, you are required to access/test the *Contact builder* feature with specific credentials provided by the team because any other creds send an actual message to the builder in the States.

# Learning .Net core the right way

We are given some topics to study as a precursor to the practical assignment.

The topics are:

### 1. N-Tier Architecture (3-tier)

Its difficult to wrap your head around so much content. It might prove difficult to mould the concept into your own scenario. Try these resources to first get a basic understanding of the concept.
	
**References**
	

- Explains as a concept for understanding https://www.geeksforgeeks.org/what-is-net-3-tier-architecture/
- Explains as a concept as well as the code for implementation purpose https://www.c-sharpcorner.com/UploadFile/4d9083/create-and-implement-3-tier-architecture-in-Asp-Net/

### 2. GraphQL

We were required to study graphQL as a concept and not its application in .net . After searching various tutorials on how to implement it, there were few prominent resources for graphQL, both as a concept and implementation (if needed).
	
**References**

As a concept
	

- Official Docs https://graphql.org/learn/
- A comparison in the widely used method (REST) and GraphQL https://www.howtographql.com/basics/1-graphql-is-the-better-rest/
- Level Up Tuts video https://www.youtube.com/watch?v=VjXb3PRL9WI

Playground

- {{ TODO }}

Implementation

- Hot Chocolate Docs https://hotchocolate.io/docs/introduction.html
- A youtube video https://www.youtube.com/watch?v=JiiELR_UiDo

### 3. Repository pattern

This is a new and interesting topic. It can be a great introduction to design patterns.

> A Repository mediates between the domain and data mapping layers, acting like an in-memory domain object collection. Client objects construct query specifications declaratively and submit them to Repository for satisfaction. Objects can be added to and removed from the Repository, as they can from a simple collection of objects, and the mapping code encapsulated by the Repository will carry out the appropriate operations behind the scenes. Conceptually, a Repository encapsulates the set of objects persisted in a data store and the operations performed over them, providing a more object-oriented view of the persistence layer. Repository also supports the objective of achieving a clean separation and one-way dependency between the domain and data mapping layers.

[Article link](https://medium.com/net-core/repository-pattern-implementation-in-asp-net-core-21e01c6664d7)

*TL;DR*
A repository is a collection of objects that performs operations on the persistance layer (Database).

We combine repository pattern with interfaces as a method of abstraction. Each model/entity generally requires its own repository to handle its logic. But considering the logic, we can also make a generic repository with the basic functionalities and derive newer repositories from this. This contributes to a cleaner and maintainable codebase.

**References**

[An Awesome medium article on generic repository pattern in .net](https://medium.com/@chathuranga94/generic-repository-pattern-for-asp-net-core-9e3e230e20cb)
[Codemaze article](https://code-maze.com/net-core-web-development-part4/) Although it can be somewhat difficult to grasp.

https://www.c-sharpcorner.com/article/repository-pattern-in-asp-net-core/
[Programming with Mosh's video](https://www.youtube.com/watch?v=rtXpYpZdOzM)

https://www.exceptionnotfound.net/the-repository-service-pattern-with-dependency-injection-and-asp-net-core/

### 4. Project Architecture (.NET Core)

Another good topic to know.

It was a topic difficult to understand at first, having no experience on it and some bad resources on google giving virtually no idea on how to proceed. So i just read the topics for a theoretical knowledge about the layers in a project and learned more about it *while* working on the assignment.

What the gist is:
Every part of the code shouldn't know the world outside it more than it needs to. In other words: Abstraction.
More will be written about it later in the [Assignment Doc](https://github.com/kaustubh-ex2/Ex2-Documents/blob/master/Assignment.md).

**References**

[A good story on medium (Premium story)](https://medium.com/@chathuranga94/n-tier-architecture-in-asp-net-core-d1f1b14f2010)

https://stackoverflow.com/a/55567348

## C# Programming

1. ### Data types and Type Conversion

2. ### Operators, Arrays and String

3. ### Enums and Classes

4. ### Inheritance and Polymorphism

5. ### Newtonsoft (Serialize and Deserialize)

6. ### Namespaces

7. ### Regular Expressions

8. ### Interfaces and Dependency Injection

9. ### Exception Handling

10. ### Collections and Generics

11. ### File IO

12. ### Synchronous and Asynchronous Programming

13. ### Linq

    All of the above topics are covered in the [official MSDN docs](https://docs.microsoft.com/en-us/dotnet/csharp/) and can easily be found on various websites like [TutorialsPoint](https://www.tutorialspoint.com/csharp/csharp_overview.htm), [c-sharpcorner](https://www.c-sharpcorner.com/UploadFile/e9fdcd/basics-of-C-Sharp/) etc.

    Linq being a new concept may require further reading. There are some good videos on Youtube as well.

    https://www.youtube.com/watch?v=yClSNQdVD7g

    https://www.youtube.com/watch?v=z3PowDJKOSA&list=PL4iLdIOlUy5IuGnp7NRdKHlB0PHZdS9EE

## Tools and Packages

1. ### Restsharp

   > RestSharp is probably the most popular HTTP client library for .NET. Featuring automatic serialization and deserialization, request and response type detection, variety of authentications and other useful features, it is being used by hundreds of thousands of projects.
   >
   > [restsharp.org](restsharp.org)

   Again, the docs are enough.

   It helps if you've already studied [Point #5](#Newtonsoft (Serialize and Deserialize)) since you will need to deserialise the response from restsharp to your desired model.

   Restsharp is essentially fetch() with extra powers.

2. ### Swagger

   > Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs. The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as OpenAPI. Both names are used interchangeably; however, OpenAPI is preferred. It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation). One goal is to minimize the amount of work needed to connect disassociated services. Another goal is to reduce the amount of time needed to accurately document a service.

   Sounds pretty accurate!

   While starting .net API, there was a time of 404s.

   After making a new API, I used to get 404 because of incorrect URL. Another thing was that I had to use Postman to test the API. (Yet another application on my fragile system).

   Swagger (Squashbuckle .net package) provides a nice little UI with all your endpoints and a way to test each and every one in-browser.

   **Thing(s) that you might run into**

   1. Describing your parameters

      Read about xml comments in swagger for it.

   

   **References**

   https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.1&tabs=visual-studio

   https://www.c-sharpcorner.com/article/implement-swagger-ui-with-web-api/

   https://www.youtube.com/watch?v=zQRgB6nasUc

3. ### Design Patterns

   To be honest i didn't read about any patterns other than repository pattern.

   Here is a list of many other patterns

   https://refactoring.guru/design-patterns/csharp
