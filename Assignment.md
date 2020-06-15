# .Net onboarding assignment

## Problem statement

Use all of the concepts previously learnt and develop an API application.

Use the below URL and find the API's list : 
https://sprint-api.newhomesource.com/DataExplorer/    : PartnerId : 9383

Now you need to consume these APIs and display the data. Design and document your APIs using swagger.

1. Typeahead Locations
2. home detail
3. User Create
4. Profile
5. BookMarkdata
6. Create, Update & Delete BookMark

## Initial thoughts

Need to explore the NHS(new home source) APIs and understand how or what models are set up, how responses are returned, just the general functioning of the website.

Need to set up the basic assignment, folder structure, connect all wires for it to perform even as a blank project. For that we need some researching. I was given a quick glance at the official API folder structure, although at the time it went over my head :P.

Need to use restsharp for the request and response at NHS side of the app.

Need to show it in swagger too.

So at the very basic level, the app should be like

<img src="https://user-images.githubusercontent.com/60777148/84576396-18db8b00-add2-11ea-8bca-ebd83386d3ab.png" alt="Basic idea" style="zoom:60%;" />

## Folder setup

The basic layer setup is going to be a simple N-tier architecture setup. Somewhat like this

![Basic layer setup](https://user-images.githubusercontent.com/60777148/84576462-8c7d9800-add2-11ea-88c9-302ee9d4ac52.png)

The NHS apis are going to be our database layer. And instead of *presentation layer*, we are going to have an *API layer*.

1. Make a new solution. For Simplicity, let say the project name is **Assignment**
   1. `dotnet new sln --name Assignment`
2. Make the Data Access layer (DA)
   1. `dotnet new classlib --name Assignment.Data -f netcoreapp3.0`
   2. Add Data layer to solution. `dotnet sln add Assignment.Data/Assignment.Data.csproj`
3. Make the Business Logic layer (BL)
   1. `dotnet new classlib --name Assignment.Business -f netcoreapp3.0`
   2. Add Business layer to solution. `dotnet sln add Assignment.Business/Assignment.Business.csproj`
   3. Navigate inside BL.
   4. Add a reference to Data layer. `dotnet add reference ../Assignment.Data/Assignment.Data.csproj`

4. Make the API layer.
   1. `dotnet new webapi -n <PROJECT-NAME>.API`
   2. Add API layer to solution. `dotnet sln add Assignment.API/Assignment.API.csproj`
   3. Navigate inside API layer
   4. Add a reference to Business layer. `dotnet add reference ../Assignment.Business/Assignment.Business.csproj`

Its pretty straightforward:

Data layer will contain models and repositories.

Business layer will contain, that's right, business logic. Due to the reference, BL can also access the models in DA.

API layer contains the endpoints and other related things (if any).

**The Problem**

Due to one way reference, our API layer cannot access the models. If you haven't implemented any API till this point, i suggest you try with a dummy API to understand this issue.

So, should we add a reference of DA to API layer ?
NO

**The Solution**

We can add another layer called "core layer" which contains out common entities like models.

All of our layers can access this common layer.

1. `dotnet new classlib --name Assignment.Core -f netcoreapp3.0`
2. Add Core layer to solution. `dotnet sln add Assignment.Core/Assignment.Core.csproj`
3. Navigate inside each of other layers and add a reference of the core layer.
4. Add a reference to Core layer. `dotnet add reference ../Assignment.Core/Assignment.Core.csproj`

**The Problem**

Now, you should know that the dependencies and repository resolution is done in startup.cs in API layer. To do that, we need a reference from API layer to DA layer and we can't do that.

We can define a method that does dependency resolution in our newly created core layer and use it in startup.

NO

To do that, we would need to add a reference `Core -> DA` to access the repositories.

This will result in a circular dependency.

**The Solution**

We create another layer where all our dependencies are registered and is referenced by the API layer.

1. `dotnet new classlib --name Assignment.Root -f netcoreapp3.0`
2. Add Root layer to solution. `dotnet sln add Assignment.Root/Assignment.Root.csproj`
3. Install the necessary packages to do dependency injection.
4. Navigate inside Base layer.
5. Add a reference to DA. `dotnet add reference ../Assignment.Data/Assignment.Data.csproj`
6. Add a reference to BL. `dotnet add reference ../Assignment.Business/Assignment.Business.csproj`
7. Navigate to API and add a reference to Root layer.

So finally our project structure looks like

![Layers structure](https://user-images.githubusercontent.com/60777148/84577258-6e1a9b00-add8-11ea-86b6-99bd8360382e.png)

## Anchor

1. After making two or three APIs, there starts to emerge a pattern in the whole process, the development, the arrangement. So you'll start making things generic.
2. The models you require in the request are different from the ones sent in response. There, a concept called DTO might help.

## Some other problems that I came across

1. Model creation

   The NHS api has a well defined response structure. Which means that it uses many models. Now I hand typed all models (almost all), before finding a better way. If you look at the BDX explorer, after getting the response, you will see a section **Generate Code**. This will help you a lot.  

2. Custom things that are not available traditionally. Eg. Request that requires any one of two parameters

   Now its easy to put a description saying that, but to actually validate if the user actually sent at least one of the parameters is difficult. After searching a lot, 

   <details>
     <summary>I came across a solution. Make your own validator.</summary>
     <a href="https://github.com/kaustubh-ex2/Assignment/blob/ec1193a4225703cc1d6145c8558fe87597daccbd/Assignment.Core/Validators/AtLeastGroupValidator.cs">The validator I made</a>
   </details>

3. Displaying Enum as text and getting it as integer

   I faced this problem because of a lack of understanding of enums. What i will say is, figure out how to display it as text only.

## Final Project

https://github.com/kaustubh-ex2/Assignment
