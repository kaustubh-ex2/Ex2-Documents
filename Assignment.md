---


---

<h1 id="net-onboarding-assignment">.Net onboarding assignment</h1>
<h2 id="problem-statement">Problem statement</h2>
<p>Use all of the concepts previously learnt and develop an API application.</p>
<p>Use the below URL and find the API’s list :<br>
<a href="https://sprint-api.newhomesource.com/DataExplorer/">https://sprint-api.newhomesource.com/DataExplorer/</a>    : PartnerId : 9383</p>
<p>Now you need to consume these APIs and display the data. Design and document your APIs using swagger.</p>
<ol>
<li>Typeahead Locations</li>
<li>home detail</li>
<li>User Create</li>
<li>Profile</li>
<li>BookMarkdata</li>
<li>Create, Update &amp; Delete BookMark</li>
</ol>
<h2 id="initial-thoughts">Initial thoughts</h2>
<p>Need to explore the NHS(new home source) APIs and understand how or what models are set up, how responses are returned, just the general functioning of the website.</p>
<p>Need to set up the basic assignment, folder structure, connect all wires for it to perform even as a blank project. For that we need some researching. I was given a quick glance at the official API folder structure, although at the time it went over my head :P.</p>
<p>Need to use restsharp for the request and response at NHS side of the app.</p>
<p>Need to show it in swagger too.</p>
<p>So at the very basic level, the app should be like</p>
<img src="https://user-images.githubusercontent.com/60777148/84576396-18db8b00-add2-11ea-8bca-ebd83386d3ab.png" alt="Basic idea">
<h2 id="folder-setup">Folder setup</h2>
<p>The basic layer setup is going to be a simple N-tier architecture setup. Somewhat like this</p>
<p><img src="https://user-images.githubusercontent.com/60777148/84576462-8c7d9800-add2-11ea-88c9-302ee9d4ac52.png" alt="Basic layer setup"></p>
<p>The NHS apis are going to be our database layer. And instead of <em>presentation layer</em>, we are going to have an <em>API layer</em>.</p>
<ol>
<li>
<p>Make a new solution. For Simplicity, let say the project name is <strong>Assignment</strong></p>
<ol>
<li><code>dotnet new sln --name Assignment</code></li>
</ol>
</li>
<li>
<p>Make the Data Access layer (DA)</p>
<ol>
<li><code>dotnet new classlib --name Assignment.Data -f netcoreapp3.0</code></li>
<li>Add Data layer to solution. <code>dotnet sln add Assignment.Data/Assignment.Data.csproj</code></li>
</ol>
</li>
<li>
<p>Make the Business Logic layer (BL)</p>
<ol>
<li><code>dotnet new classlib --name Assignment.Business -f netcoreapp3.0</code></li>
<li>Add Business layer to solution. <code>dotnet sln add Assignment.Business/Assignment.Business.csproj</code></li>
<li>Navigate inside BL.</li>
<li>Add a reference to Data layer. <code>dotnet add reference ../Assignment.Data/Assignment.Data.csproj</code></li>
</ol>
</li>
<li>
<p>Make the API layer.</p>
<ol>
<li><code>dotnet new webapi -n &lt;PROJECT-NAME&gt;.API</code></li>
<li>Add API layer to solution. <code>dotnet sln add Assignment.API/Assignment.API.csproj</code></li>
<li>Navigate inside API layer</li>
<li>Add a reference to Business layer. <code>dotnet add reference ../Assignment.Business/Assignment.Business.csproj</code></li>
</ol>
</li>
</ol>
<p>Its pretty straightforward:</p>
<p>Data layer will contain models and repositories.</p>
<p>Business layer will contain, that’s right, business logic. Due to the reference, BL can also access the models in DA.</p>
<p>API layer contains the endpoints and other related things (if any).</p>
<p><strong>The Problem</strong></p>
<p>Due to one way reference, our API layer cannot access the models. If you haven’t implemented any API till this point, i suggest you try with a dummy API to understand this issue.</p>
<p>So, should we add a reference of DA to API layer ?<br>
NO</p>
<p><strong>The Solution</strong></p>
<p>We can add another layer called “core layer” which contains out common entities like models.</p>
<p>All of our layers can access this common layer.</p>
<ol>
<li><code>dotnet new classlib --name Assignment.Core -f netcoreapp3.0</code></li>
<li>Add Core layer to solution. <code>dotnet sln add Assignment.Core/Assignment.Core.csproj</code></li>
<li>Navigate inside each of other layers and add a reference of the core layer.</li>
<li>Add a reference to Core layer. <code>dotnet add reference ../Assignment.Core/Assignment.Core.csproj</code></li>
</ol>
<p><strong>The Problem</strong></p>
<p>Now, you should know that the dependencies and repository resolution is done in startup.cs in API layer. To do that, we need a reference from API layer to DA layer and we can’t do that.</p>
<p>We can define a method that does dependency resolution in our newly created core layer and use it in startup.</p>
<p>NO</p>
<p>To do that, we would need to add a reference <code>Core -&gt; DA</code> to access the repositories.</p>
<p>This will result in a circular dependency.</p>
<p><strong>The Solution</strong></p>
<p>We create another layer where all our dependencies are registered and is referenced by the API layer.</p>
<ol>
<li><code>dotnet new classlib --name Assignment.Root -f netcoreapp3.0</code></li>
<li>Add Root layer to solution. <code>dotnet sln add Assignment.Root/Assignment.Root.csproj</code></li>
<li>Install the necessary packages to do dependency injection.</li>
<li>Navigate inside Base layer.</li>
<li>Add a reference to DA. <code>dotnet add reference ../Assignment.Data/Assignment.Data.csproj</code></li>
<li>Add a reference to BL. <code>dotnet add reference ../Assignment.Business/Assignment.Business.csproj</code></li>
<li>Navigate to API and add a reference to Root layer.</li>
</ol>
<p>So finally our project structure looks like</p>
<p><img src="https://user-images.githubusercontent.com/60777148/84577258-6e1a9b00-add8-11ea-86b6-99bd8360382e.png" alt="Layers structure"></p>
<h2 id="anchor">Anchor</h2>
<ol>
<li>After making two or three APIs, there starts to emerge a pattern in the whole process, the development, the arrangement. So you’ll start making things generic.</li>
<li>The models you require in the request are different from the ones sent in response. There, a concept called DTO might help.</li>
</ol>
<h2 id="some-other-problems-that-i-came-across">Some other problems that I came across</h2>
<ol>
<li>
<p>Model creation</p>
<p>The NHS api has a well defined response structure. Which means that it uses many models. Now I hand typed all models (almost all), before finding a better way. If you look at the BDX explorer, after getting the response, you will see a section <strong>Generate Code</strong>. This will help you a lot.</p>
</li>
<li>
<p>Custom things that are not available traditionally. Eg. Request that requires any one of two parameters</p>
<p>Now its easy to put a description saying that, but to actually validate if the user actually sent at least one of the parameters is difficult. After searching a lot,</p>

  I came across a solution. Make your own validator.
  <a href="https://github.com/kaustubh-ex2/Assignment/blob/ec1193a4225703cc1d6145c8558fe87597daccbd/Assignment.Core/Validators/AtLeastGroupValidator.cs">The validator I made</a>

</li>
<li>
<p>Displaying Enum as text and getting it as integer</p>
<p>I faced this problem because of a lack of understanding of enums. What i will say is, figure out how to display it as text only.</p>
</li>
</ol>
<h2 id="final-project">Final Project</h2>
<p><a href="https://github.com/kaustubh-ex2/Assignment">https://github.com/kaustubh-ex2/Assignment</a></p>

