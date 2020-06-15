---


---

<h1 id="caching-in-homluv">Caching in HomLuv</h1>
<p>The skywalker api uses caching in its responses for faster timing since response fetching is requires fetching data from another API and requires further processing on it.</p>
<p>Skywalker uses <a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.caching.memory.imemorycache?view=dotnet-plat-ext-3.1">IMemoryCache</a>  and a concurrent dictionary to maintain cache as key value pairs.</p>
<p><strong>Concurrent Dictionary</strong></p>
<blockquote>
<p>Represents a thread-safe collection of key/value pairs that can be accessed by multiple threads concurrently</p>
<p><em>Source</em> : <a href="https://docs.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentdictionary-2?view=netcore-3.1">MSDN</a></p>
</blockquote>
<p>Uses thread safe double locking checking mechanism to initialize the dictionary.</p>
<pre class=" language-csharp"><code class="prism  language-csharp"><span class="token keyword">public</span> ConcurrentDictionary<span class="token operator">&lt;</span><span class="token keyword">string</span><span class="token punctuation">,</span> <span class="token keyword">string</span><span class="token operator">&gt;</span> SharedDictionary
<span class="token punctuation">{</span>
    <span class="token keyword">get</span>
    <span class="token punctuation">{</span>
        <span class="token comment">//Double-checked_locking</span>
        <span class="token keyword">if</span> <span class="token punctuation">(</span>_sharedDic <span class="token operator">==</span> <span class="token keyword">null</span><span class="token punctuation">)</span>
        <span class="token punctuation">{</span>
            <span class="token keyword">lock</span> <span class="token punctuation">(</span>_lockObject<span class="token punctuation">)</span>
            <span class="token punctuation">{</span>
                <span class="token keyword">if</span> <span class="token punctuation">(</span>_sharedDic <span class="token operator">==</span> <span class="token keyword">null</span><span class="token punctuation">)</span>
                <span class="token punctuation">{</span>
                    _sharedDic <span class="token operator">=</span> _cache<span class="token punctuation">.</span>Get<span class="token operator">&lt;</span>ConcurrentDictionary<span class="token operator">&lt;</span><span class="token keyword">string</span><span class="token punctuation">,</span> <span class="token keyword">string</span><span class="token operator">&gt;</span><span class="token operator">&gt;</span><span class="token punctuation">(</span>CACHE_SHARED_DIC_NAME<span class="token punctuation">)</span><span class="token punctuation">;</span>
                    <span class="token keyword">if</span> <span class="token punctuation">(</span>_sharedDic <span class="token operator">==</span> <span class="token keyword">null</span><span class="token punctuation">)</span>
                    <span class="token punctuation">{</span>
                        _sharedDic <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">ConcurrentDictionary</span><span class="token operator">&lt;</span><span class="token keyword">string</span><span class="token punctuation">,</span> <span class="token keyword">string</span><span class="token operator">&gt;</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
                    <span class="token punctuation">}</span>
                <span class="token punctuation">}</span>
            <span class="token punctuation">}</span>
        <span class="token punctuation">}</span>

        <span class="token keyword">return</span> _sharedDic<span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p>As a very basic level, to enable caching for your endpoint, you only need to add the following just above the endpoint method</p>
<pre class=" language-cs"><code class="prism  language-cs"> [TypeFilter(typeof(CustomCacheFilterAttribute), Arguments = new object[] { "&lt;YOUR_CACHE-KEY&gt;" })]
public async IActionResult MyEndpoint()
{}
</code></pre>
<p><strong>Explanation</strong></p>
<ol>
<li>
<p><a href="https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.typefilterattribute?view=aspnetcore-3.1">TypeFilterAttribute</a></p>
<p>TypeFilterAttribute is a special predefined attribute that can be used to insert custom attributes in between the request processing.</p>
<p>TypeFilterAttribute requires a type in its constructor. The type of the custom attribute needed.</p>
<blockquote>
<p>Types that are referenced using the <code>TypeFilterAttribute</code> don’t need to be registered with the DI container. They do have their dependencies fulfilled by the DI container.</p>
<p><code>TypeFilterAttribute</code> can optionally accept constructor arguments for the type.</p>
</blockquote>
<p>Depending on the type of the custom attribute, it acts on that level of request processing.</p>
<p><img src="https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters/_static/filter-pipeline-2.png?view=aspnetcore-3.1" alt="The request is processed through Authorization Filters, Resource Filters, Model Binding, Action Filters, Action Execution and Action Result Conversion, Exception Filters, Result Filters, and Result Execution. On the way out, the request is only processed by Result Filters and Resource Filters before becoming a response sent to the client."></p>
</li>
<li>
<p>CustomCacheFilterAttribute</p>
<p>CustomCacheFilterAttribute is a custom made attribute deriving from <a href="https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-3.1#resource-filters">IResourceFilter</a></p>
</li>
</ol>
<h2 id="customcachefilterattribute">CustomCacheFilterAttribute</h2>
<p>Considering the figure, CustomCacheFilterAttribute is a IResourceFilter. Which means it executes just after authorisation, which makes it useful to short-circuit most of the pipeline. For example, a caching filter can avoid the rest of the pipeline on a cache hit.</p>
<p>Resource filter is hit twice and thus CustomCacheFilterAttribute is a two step process.</p>
<ol>
<li>
<p><code>void IResourceFilter.OnResourceExecuting(ResourceExecutingContext context)</code></p>
<blockquote>
<p>Check if request have any cached response or not,</p>
<p>if yes then it short circuit the filter pipeline and cached response would be sent back to client</p>
</blockquote>
<p>This step is executed just after authorization, i.e before Model binding or any other filters.</p>
<p>Steps performed in this method</p>
<ol>
<li>
<p>Formulate a key using request path, query parameters and request body.</p>
</li>
<li>
<p>Check if the key is not null or it doesn’t contain userid</p>
</li>
<li>
<p>Try getting the value for this key from the cache</p>
</li>
<li>
<p>If present, continue</p>
</li>
<li>
<p>Set the cachedValue to the response</p>
</li>
</ol>
</li>
<li>
<p><code>void IResourceFilter.OnResourceExecuted(ResourceExecutedContext context)</code></p>
<blockquote>
<p>If new request then it has been set by caching for timespan of 60 minutes</p>
</blockquote>
<p>This step is executed after result execution, i.e the trip through the business logic.</p>
<p>Steps performed in this method</p>
<ol>
<li>Check if the key is not null or it doesn’t contain userid</li>
<li>Check if the cache already has a value stored for its key</li>
<li>If it is not already cached, continue. Otherwise, return the response</li>
<li>If the response to sent was successful (200 OK), then cache it with a TTL of 1 hour.</li>
</ol>
</li>
</ol>
<p><img src="https://user-images.githubusercontent.com/60777148/84380388-e16bb380-ac04-11ea-974c-86ca438e6527.jpeg" alt="Caching flow diagram"></p>

