---


---

<h1 id="initiation">Initiation</h1>
<p>The project begins with an introduction to Homluv, its functioning and getting it to operate. The introduction is given by Ruchika Verma and Kritika Gakher.</p>
<h3 id="understanding">Understanding</h3>
<p>Homluv website has two levels of pages with difference in specificity in search (or the depth in which the user searches primarily)</p>
<ul>
<li>
<p>First level page</p>
<p><img src="https://user-images.githubusercontent.com/60777148/83666154-1f0b8380-a5ea-11ea-88ba-b67c3e9cd089.png" alt="First level triggers"></p>
<p>The first level page is seen when the user clicks on any of these images and comes to a catalog of these categories.</p>
</li>
<li>
<p>Second level page</p>
<p><img src="https://user-images.githubusercontent.com/60777148/83666320-64c84c00-a5ea-11ea-87ca-0fb0d41fdf01.png" alt="Second level triggers"></p>
<p>The second level page is seen when the user searches a location.</p>
</li>
<li>
<p>Images are lazy loaded</p>
</li>
<li>
<p>Inspiration page shows the popular homes to the user for -</p>
<ul>
<li>The last location the user searched (if the user searched)</li>
<li>The current location of the user (if granted access)</li>
<li>For the case other than above two,  {{ TODO }}</li>
</ul>
</li>
<li>
<p>User can luv (like) a home as a logged in or an anonymous user. In both cases the luvs will be preserved in their profile (should they ever make it) or in their browser cache if they don’t.</p>
</li>
<li>
<p>A home can be viewed in two states</p>
<ul>
<li>Quick view
<ul>
<li>As a separate section on the right side of the page</li>
</ul>
</li>
<li>Home Detail Page
<ul>
<li>As a dedicated page for home detail. It shows all the home images present along with the map to its location and community images</li>
</ul>
</li>
</ul>
</li>
<li>
<p>As a new member of the Homluv team, you are required to access/test the <em>Contact builder</em> feature with specific credentials provided by the team because any other creds send an actual message to the builder in the States.</p>
</li>
</ul>
<h1 id="learning-.net-core-the-right-way">Learning .Net core the right way</h1>
<p>We are given some topics to study as a precursor to the practical assignment.</p>
<p>The topics are:</p>
<h3 id="n-tier-architecture-3-tier">1. N-Tier Architecture (3-tier)</h3>
<p>Its difficult to wrap your head around so much content. It might prove difficult to mould the concept into your own scenario. Try these resources to first get a basic understanding of the concept.</p>
<p><strong>References</strong></p>
<ul>
<li>Explains as a concept for understanding <a href="https://www.geeksforgeeks.org/what-is-net-3-tier-architecture/">https://www.geeksforgeeks.org/what-is-net-3-tier-architecture/</a></li>
<li>Explains as a concept as well as the code for implementation purpose <a href="https://www.c-sharpcorner.com/UploadFile/4d9083/create-and-implement-3-tier-architecture-in-Asp-Net/">https://www.c-sharpcorner.com/UploadFile/4d9083/create-and-implement-3-tier-architecture-in-Asp-Net/</a></li>
</ul>
<h3 id="graphql">2. GraphQL</h3>
<p>We were required to study graphQL as a concept and not its application in .net . After searching various tutorials on how to implement it, there were few prominent resources for graphQL, both as a concept and implementation (if needed).</p>
<p><strong>References</strong></p>
<p>As a concept</p>
<ul>
<li>Official Docs <a href="https://graphql.org/learn/">https://graphql.org/learn/</a></li>
<li>A comparison in the widely used method (REST) and GraphQL <a href="https://www.howtographql.com/basics/1-graphql-is-the-better-rest/">https://www.howtographql.com/basics/1-graphql-is-the-better-rest/</a></li>
<li>Level Up Tuts video <a href="https://www.youtube.com/watch?v=VjXb3PRL9WI">https://www.youtube.com/watch?v=VjXb3PRL9WI</a></li>
</ul>
<p>Playground</p>
<ul>
<li>{{ TODO }}</li>
</ul>
<p>Implementation</p>
<ul>
<li>Hot Chocolate Docs <a href="https://hotchocolate.io/docs/introduction.html">https://hotchocolate.io/docs/introduction.html</a></li>
<li>A youtube video <a href="https://www.youtube.com/watch?v=JiiELR_UiDo">https://www.youtube.com/watch?v=JiiELR_UiDo</a></li>
</ul>
<h3 id="repository-pattern">3. Repository pattern</h3>
<p>This is a new and interesting topic. It can be a great introduction to design patterns.</p>
<blockquote>
<p>A Repository mediates between the domain and data mapping layers, acting like an in-memory domain object collection. Client objects construct query specifications declaratively and submit them to Repository for satisfaction. Objects can be added to and removed from the Repository, as they can from a simple collection of objects, and the mapping code encapsulated by the Repository will carry out the appropriate operations behind the scenes. Conceptually, a Repository encapsulates the set of objects persisted in a data store and the operations performed over them, providing a more object-oriented view of the persistence layer. Repository also supports the objective of achieving a clean separation and one-way dependency between the domain and data mapping layers.</p>
</blockquote>
<p><a href="https://medium.com/net-core/repository-pattern-implementation-in-asp-net-core-21e01c6664d7">Article link</a></p>
<p><em>TL;DR</em><br>
A repository is a collection of objects that performs operations on the persistance layer (Database).</p>
<p>We combine repository pattern with interfaces as a method of abstraction. Each model/entity generally requires its own repository to handle its logic. But considering the logic, we can also make a generic repository with the basic functionalities and derive newer repositories from this. This contributes to a cleaner and maintainable codebase.</p>
<p><strong>References</strong></p>
<p><a href="https://medium.com/@chathuranga94/generic-repository-pattern-for-asp-net-core-9e3e230e20cb">An Awesome medium article on generic repository pattern in .net</a><br>
<a href="https://code-maze.com/net-core-web-development-part4/">Codemaze article</a> Although it can be somewhat difficult to grasp.</p>
<p><a href="https://www.c-sharpcorner.com/article/repository-pattern-in-asp-net-core/">https://www.c-sharpcorner.com/article/repository-pattern-in-asp-net-core/</a><br>
<a href="https://www.youtube.com/watch?v=rtXpYpZdOzM">Programming with Mosh’s video</a></p>
<p><a href="https://www.exceptionnotfound.net/the-repository-service-pattern-with-dependency-injection-and-asp-net-core/">https://www.exceptionnotfound.net/the-repository-service-pattern-with-dependency-injection-and-asp-net-core/</a></p>
<h3 id="project-architecture-.net-core">4. Project Architecture (.NET Core)</h3>
<p>Another good topic to know.</p>
<p>It was a topic difficult to understand at first, having no experience on it and some bad resources on google giving virtually no idea on how to proceed. So i just read the topics for a theoretical knowledge about the layers in a project and learned more about it <em>while</em> working on the assignment.</p>
<p>What the gist is:<br>
Every part of the code shouldn’t know the world outside it more than it needs to. In other words: Abstraction.<br>
More will be written about it later in the [assignment Gist](Assignment gist).</p>
<p><strong>References</strong></p>
<p><a href="https://medium.com/@chathuranga94/n-tier-architecture-in-asp-net-core-d1f1b14f2010">A good story on medium (Premium story)</a></p>
<p><a href="https://stackoverflow.com/a/55567348">https://stackoverflow.com/a/55567348</a></p>
<h2 id="c-programming">C# Programming</h2>
<ol>
<li>
<h3 id="data-types-and-type-conversion">Data types and Type Conversion</h3>
</li>
<li>
<h3 id="operators-arrays-and-string">Operators, Arrays and String</h3>
</li>
<li>
<h3 id="enums-and-classes">Enums and Classes</h3>
</li>
<li>
<h3 id="inheritance-and-polymorphism">Inheritance and Polymorphism</h3>
</li>
<li>
<h3 id="newtonsoft-serialize-and-deserialize">Newtonsoft (Serialize and Deserialize)</h3>
</li>
<li>
<h3 id="namespaces">Namespaces</h3>
</li>
<li>
<h3 id="regular-expressions">Regular Expressions</h3>
</li>
<li>
<h3 id="interfaces-and-dependency-injection">Interfaces and Dependency Injection</h3>
</li>
<li>
<h3 id="exception-handling">Exception Handling</h3>
</li>
<li>
<h3 id="collections-and-generics">Collections and Generics</h3>
</li>
<li>
<h3 id="file-io">File IO</h3>
</li>
<li>
<h3 id="synchronous-and-asynchronous-programming">Synchronous and Asynchronous Programming</h3>
</li>
<li>
<h3 id="linq">Linq</h3>
<p>All of the above topics are covered in the <a href="https://docs.microsoft.com/en-us/dotnet/csharp/">official MSDN docs</a> and can easily be found on various websites like <a href="https://www.tutorialspoint.com/csharp/csharp_overview.htm">TutorialsPoint</a>, <a href="https://www.c-sharpcorner.com/UploadFile/e9fdcd/basics-of-C-Sharp/">c-sharpcorner</a> etc.</p>
<p>Linq being a new concept may require further reading. There are some good videos on Youtube as well.</p>
<p><a href="https://www.youtube.com/watch?v=yClSNQdVD7g">https://www.youtube.com/watch?v=yClSNQdVD7g</a></p>
<p><a href="https://www.youtube.com/watch?v=z3PowDJKOSA&amp;list=PL4iLdIOlUy5IuGnp7NRdKHlB0PHZdS9EE">https://www.youtube.com/watch?v=z3PowDJKOSA&amp;list=PL4iLdIOlUy5IuGnp7NRdKHlB0PHZdS9EE</a></p>
</li>
</ol>
<h2 id="tools-and-packages">Tools and Packages</h2>
<ol>
<li>
<h3 id="restsharp">Restsharp</h3>
<blockquote>
<p>RestSharp is probably the most popular HTTP client library for .NET. Featuring automatic serialization and deserialization, request and response type detection, variety of authentications and other useful features, it is being used by hundreds of thousands of projects.</p>
<p><a href="restsharp.org">restsharp.org</a></p>
</blockquote>
<p>Again, the docs are enough.</p>
<p>It helps if you’ve already studied <a href="#Newtonsoft" title="Serialize and Deserialize">Point #5</a> since you will need to deserialise the response from restsharp to your desired model.</p>
<p>Restsharp is essentially fetch() with extra powers.</p>
</li>
<li>
<h3 id="swagger">Swagger</h3>
<blockquote>
<p>Swagger is a language-agnostic specification for describing <a href="https://en.wikipedia.org/wiki/Representational_state_transfer">REST</a> APIs. The Swagger project was donated to the <a href="https://www.openapis.org/">OpenAPI Initiative</a>, where it’s now referred to as OpenAPI. Both names are used interchangeably; however, OpenAPI is preferred. It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation). One goal is to minimize the amount of work needed to connect disassociated services. Another goal is to reduce the amount of time needed to accurately document a service.</p>
</blockquote>
<p>Sounds pretty accurate!</p>
<p>While starting .net API, there was a time of 404s.</p>
<p>After making a new API, I used to get 404 because of incorrect URL. Another thing was that I had to use Postman to test the API. (Yet another application on my fragile system).</p>
<p>Swagger (Squashbuckle .net package) provides a nice little UI with all your endpoints and a way to test each and every one in-browser.</p>
<p><strong>Thing(s) that you might run into</strong></p>
<ol>
<li>
<p>Describing your parameters</p>
<p>Read about xml comments in swagger for it.</p>
</li>
</ol>
<p><strong>References</strong></p>
<p><a href="https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.1&amp;tabs=visual-studio">https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.1&amp;tabs=visual-studio</a></p>
<p><a href="https://www.c-sharpcorner.com/article/implement-swagger-ui-with-web-api/">https://www.c-sharpcorner.com/article/implement-swagger-ui-with-web-api/</a></p>
<p><a href="https://www.youtube.com/watch?v=zQRgB6nasUc">https://www.youtube.com/watch?v=zQRgB6nasUc</a></p>
</li>
<li>
<h3 id="design-patterns">Design Patterns</h3>
<p>To be honest i didn’t read about any patterns other than repository pattern.</p>
<p>Here is a list of many other patterns</p>
<p><a href="https://refactoring.guru/design-patterns/csharp">https://refactoring.guru/design-patterns/csharp</a></p>
</li>
</ol>

