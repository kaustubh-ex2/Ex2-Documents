# Caching in HomLuv

The skywalker api uses caching in its responses for faster timing since response fetching is requires fetching data from another API and requires further processing on it.

Skywalker uses [IMemoryCache](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.caching.memory.imemorycache?view=dotnet-plat-ext-3.1)  and a concurrent dictionary to maintain cache as key value pairs.

**Concurrent Dictionary**

> Represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently
>
> *Source* : [MSDN](https://docs.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentdictionary-2?view=netcore-3.1)

Uses thread safe double locking checking mechanism to initialize the dictionary.

```csharp
public ConcurrentDictionary<string, string> SharedDictionary
{
    get
    {
        //Double-checked_locking
        if (_sharedDic == null)
        {
            lock (_lockObject)
            {
                if (_sharedDic == null)
                {
                    _sharedDic = _cache.Get<ConcurrentDictionary<string, string>>(CACHE_SHARED_DIC_NAME);
                    if (_sharedDic == null)
                    {
                        _sharedDic = new ConcurrentDictionary<string, string>();
                    }
                }
            }
        }

        return _sharedDic;
    }
}
```



As a very basic level, to enable caching for your endpoint, you only need to add the following just above the endpoint method

```cs
 [TypeFilter(typeof(CustomCacheFilterAttribute), Arguments = new object[] { "<YOUR_CACHE-KEY>" })]
public async IActionResult MyEndpoint()
{}
```

**Explanation**

1. [TypeFilterAttribute](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.typefilterattribute?view=aspnetcore-3.1)

   TypeFilterAttribute is a special predefined attribute that can be used to insert custom attributes in between the request processing.

   TypeFilterAttribute requires a type in its constructor. The type of the custom attribute needed.

   > Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container. They do have their dependencies fulfilled by the DI container.
   >
   > `TypeFilterAttribute` can optionally accept constructor arguments for the type.

   Depending on the type of the custom attribute, it acts on that level of request processing.

   ![The request is processed through Authorization Filters, Resource Filters, Model Binding, Action Filters, Action Execution and Action Result Conversion, Exception Filters, Result Filters, and Result Execution. On the way out, the request is only processed by Result Filters and Resource Filters before becoming a response sent to the client.](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters/_static/filter-pipeline-2.png?view=aspnetcore-3.1)

2. CustomCacheFilterAttribute

   CustomCacheFilterAttribute is a custom made attribute deriving from [IResourceFilter](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-3.1#resource-filters)

## CustomCacheFilterAttribute

Considering the figure, CustomCacheFilterAttribute is a IResourceFilter. Which means it executes just after authorisation, which makes it useful to short-circuit most of the pipeline. For example, a caching filter can avoid the rest of the pipeline on a cache hit.

Resource filter is hit twice and thus CustomCacheFilterAttribute is a two step process.

1. `void IResourceFilter.OnResourceExecuting(ResourceExecutingContext context)`

   > Check if request have any cached response or not,
   >
   > if yes then it short circuit the filter pipeline and cached response would be sent back to client

   This step is executed just after authorization, i.e before Model binding or any other filters.

   Steps performed in this method

   1. Formulate a key using request path, query parameters and request body.

   2. Check if the key is not null or it doesn't contain userid
   3. Try getting the value for this key from the cache
   4. If present, continue
   5. Set the cachedValue to the response

2. `void IResourceFilter.OnResourceExecuted(ResourceExecutedContext context)`

   > If new request then it has been set by caching for timespan of 60 minutes

   This step is executed after result execution, i.e the trip through the business logic.

   Steps performed in this method

   1. Check if the key is not null or it doesn't contain userid
   2. Check if the cache already has a value stored for its key
   3. If it is not already cached, continue. Otherwise, return the response
   4. If the response to sent was successful (200 OK), then cache it with a TTL of 1 hour.

![Caching flow diagram](https://user-images.githubusercontent.com/60777148/84380388-e16bb380-ac04-11ea-974c-86ca438e6527.jpeg)
