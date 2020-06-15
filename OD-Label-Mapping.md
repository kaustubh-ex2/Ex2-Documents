---


---

<h1 id="task">Task</h1>
<p>Map a list of labels and count to new labels from a json.<br>
Eg. if the case is like this</p>
<p>Image 1 : Label1, Label2, Label3</p>
<p>Image 2 : Label1, Label3,</p>
<p>Image 3 : Label1, Labele3, Label4</p>
<p>Image 4 : Label1, Label2</p>
<p>Then the response we’ll get from API will be something like</p>
<pre class=" language-json"><code class="prism  language-json">
<span class="token string">"labels"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span>
	<span class="token punctuation">{</span>
		<span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"Label1"</span><span class="token punctuation">,</span>
		<span class="token string">"count"</span><span class="token punctuation">:</span> <span class="token number">4</span>
	<span class="token punctuation">}</span><span class="token punctuation">,</span>
	<span class="token punctuation">{</span>
		<span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"Label3"</span><span class="token punctuation">,</span>
		<span class="token string">"count"</span><span class="token punctuation">:</span> <span class="token number">3</span>
	<span class="token punctuation">}</span><span class="token punctuation">,</span>
	<span class="token punctuation">{</span>
		<span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"Label2"</span><span class="token punctuation">,</span>
		<span class="token string">"count"</span><span class="token punctuation">:</span> <span class="token number">2</span>
	<span class="token punctuation">}</span><span class="token punctuation">,</span>
	<span class="token punctuation">{</span>
		<span class="token string">"name"</span><span class="token punctuation">:</span> <span class="token string">"Label4"</span><span class="token punctuation">,</span>
		<span class="token string">"count"</span><span class="token punctuation">:</span> <span class="token number">1</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">]</span>
</code></pre>
<p>The json needs to be structured for size and readability both.</p>
<h2 id="tldr">TL;DR</h2>
<p><a href="https://gist.github.com/kaustubh-ex2/9ba0f2907c4ef8c9ae320e7ad7be3f39#attempt-5">Attempt #5</a> is chosen.</p>
<h2 id="attempt-1">Attempt #1</h2>
<p>Json looks like (i.e JSON1.json)</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
	<span class="token string">"child1"</span><span class="token punctuation">:</span> <span class="token string">"parent1"</span><span class="token punctuation">,</span>
	<span class="token string">"child2"</span><span class="token punctuation">:</span> <span class="token string">"parent1"</span><span class="token punctuation">,</span>
	<span class="token string">"child3"</span><span class="token punctuation">:</span> <span class="token string">"parent2"</span><span class="token punctuation">,</span>
	<span class="token string">"child4"</span><span class="token punctuation">:</span> <span class="token string">"parent3"</span>
<span class="token punctuation">}</span>
</code></pre>
<p>And translation code looks like</p>
<pre class=" language-cs"><code class="prism  language-cs">public static List&lt;Label&gt; TranslatedLabelNames(List&lt;Label&gt; oldList)
{
	//----- Iterate on old list and enter translated key into the dictionary
	Dictionary&lt;string, int&gt; map = new Dictionary&lt;string, int&gt;();
	List&lt;Label&gt; newList = new List&lt;Label&gt;();
	foreach (Label l in oldList)
	{
		string TranslatedKey = JSON.json[l.LabelName];
		if (map.ContainsKey(TranslatedKey))
			map[TranslatedKey].Count += l.Count;
		else
			map.Add(new Label
			{
				LabelName = TranslatedKey,
				Count = l.Count
			});
	}

	//----- Construct a new list from the dictionary to send to the frontend
	return map.Select(dictRecord =&gt; new Label
	{
		LabelName = dictRecord.Key,
		Count = dictRecord.Value
	}).ToList();
	return newList;
}
</code></pre>
<p>Complexity: foreach( O(N) ), ContainsKey( O(~1) ), Add( O(1) )</p>
<p>Effectively: O(N)</p>
<p>Time taken:</p>

<table>
<thead>
<tr>
<th></th>
<th>10 Labels</th>
<th>100 Labels</th>
<th>400 Labels</th>
<th>1000 Labels</th>
</tr>
</thead>
<tbody>
<tr>
<td>Time (ms)</td>
<td>6</td>
<td>6</td>
<td>7</td>
<td>9</td>
</tr>
</tbody>
</table><h2 id="attempt-2">Attempt #2</h2>
<p>json looks like (i.e. JSON2.json)</p>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span>
	<span class="token string">"parent1"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"child1"</span><span class="token punctuation">,</span> <span class="token string">"child2"</span><span class="token punctuation">]</span><span class="token punctuation">,</span>
	<span class="token string">"parent2"</span><span class="token punctuation">:</span> <span class="token string">"child3"</span><span class="token punctuation">,</span>
	<span class="token string">"parent3"</span><span class="token punctuation">:</span> <span class="token string">"child4"</span>
<span class="token punctuation">}</span>
</code></pre>
<p>And translation code looks like</p>
<pre class=" language-cs"><code class="prism  language-cs">public static List&lt;Label&gt; TranslatedLabelNames2(List&lt;Label&gt; oldList)
{
	Dictionary&lt;string, int&gt; map = new Dictionary&lt;string, int&gt;();
	List&lt;Label&gt; newList = new List&lt;Label&gt;();
	foreach (Label l in oldList)
	{
		string TranslatedKey = JSON.json.FirstOrDefault(x =&gt; x.Value.Contains(l.LabelName)).Key;
		if (map.ContainsKey(TranslatedKey))
			 map[TranslatedKey] += l.Count;
		 else
			 map.Add(TranslatedKey, l.Count);
	}

	//----- Construct a new list from the dictionary to send to the frontend
	return map.Select(dictRecord =&gt; new Label
	{
		LabelName = dictRecord.Key,
		Count = dictRecord.Value
	}).ToList();
	return newList;
}
</code></pre>
<p>Complexity: foreach( O(N) ), FirstOrDefault( O(N) ), ContainsKey( O(~1) ), Add( O(1) )</p>
<p>Effectively: O(N<sup>2</sup>)</p>
<p>Time taken:</p>

<table>
<thead>
<tr>
<th></th>
<th>10 Labels</th>
<th>100 Labels</th>
<th>400 Labels</th>
<th>1000 Labels</th>
</tr>
</thead>
<tbody>
<tr>
<td>Time (ms)</td>
<td>6</td>
<td>7</td>
<td>8</td>
<td>9</td>
</tr>
</tbody>
</table><p><strong>After Thoughts</strong></p>
<ul>
<li>The JSON is considerably shorter</li>
<li>The code on the other hand takes more time</li>
</ul>
<h2 id="attempt-3">Attempt #3</h2>
<p>There is an extra step invloved in the translation i.e. the converting of map into the newList and returning the new list.<br>
We can skip that step and directly make changes in the new list.</p>
<p>JSON is JSON2.json, same as in Attempt #2</p>
<p>Translation code is</p>
<pre class=" language-cs"><code class="prism  language-cs">public static List&lt;Label&gt; TranslatedLabelNames2(List&lt;Label&gt; oldList)
{
	List&lt;Label&gt; newList = new List&lt;Label&gt;();
	foreach (Label l in oldList)
	{
		string TranslatedKey = JSON.json.FirstOrDefault(x =&gt; x.Value.Contains(l.LabelName)).Key;
		if (newList.Any(x =&gt; x.LabelName == TranslatedKey))
        	newList.First(x =&gt; x.LabelName == TranslatedKey).Count += l.Count;
		else
			newList.Add(new Label
			{
				LabelName = TranslatedKey,
				Count = l.Count
			});
	}
	return newList;
}
</code></pre>
<p>Complexity: foreach( O(N) ), FirstOrDefault( O(N) ), First( O(N) ), ContainsKey( O(~1) ), Add( O(1) )</p>
<p>Effectively: O(N<sup>2</sup>)</p>
<p>Time taken:</p>

<table>
<thead>
<tr>
<th></th>
<th>10 Labels</th>
<th>100 Labels</th>
<th>400 Labels</th>
<th>1000 Labels</th>
</tr>
</thead>
<tbody>
<tr>
<td>Time (ms)</td>
<td>4.2</td>
<td>4.6</td>
<td>5</td>
<td>6.6</td>
</tr>
</tbody>
</table><p><strong>After Thoughts</strong></p>
<ul>
<li>This made the execution way faster.</li>
<li>Complexity isn’t the only thing affecting execution time.</li>
<li>What if we do the same in the first approach as well ?</li>
</ul>
<h2 id="attempt-4">Attempt #4</h2>
<p>JSON is same as JSON1.json</p>
<p>We are employing the removal of that step in the first approach as well.</p>
<p>Translation code looks like</p>
<pre class=" language-cs"><code class="prism  language-cs">public static List&lt;Label&gt; TranslatedLabelNames(List&lt;Label&gt; oldList)
{
	List&lt;Label&gt; newList = new List&lt;Label&gt;();
	foreach (Label l in oldList)
	{
		string TranslatedKey = JSON.json[l.LabelName];
		if (newList.Any(x =&gt; x.LabelName == TranslatedKey))
        	newList.First(x =&gt; x.LabelName == TranslatedKey).Count += l.Count;
		else
			newList.Add(new Label
			{
				LabelName = TranslatedKey,
				Count = l.Count
			});
	}
	return newList;
}
</code></pre>
<p>Complexity: foreach( O(N) ), Retrieval from JSON.json( O(1) ), First( O(N) ), ContainsKey( O(~1) ), Add( O(1) )</p>
<p>Effectively: O(N<sup>2</sup>)</p>
<p>Time taken:</p>

<table>
<thead>
<tr>
<th></th>
<th>10 Labels</th>
<th>100 Labels</th>
<th>400 Labels</th>
<th>1000 Labels</th>
</tr>
</thead>
<tbody>
<tr>
<td>Time (ms)</td>
<td>2.4</td>
<td>2.7</td>
<td>3</td>
<td>4.1</td>
</tr>
</tbody>
</table><h2 id="attempt-5">Attempt #5</h2>
<p>JSON is in a new format i.e. JSON3.json<br>
It is more uniform with the current style of the code.<br>
Offers more readability along with reasonable size.</p>
<p>Translation code</p>
<pre class=" language-cs"><code class="prism  language-cs">public static List&lt;Label&gt; TranslatedLabelNames3(List&lt;Label&gt; oldList)
{
    List&lt;Label&gt; newList = new List&lt;Label&gt;();
    foreach (Label l in oldList)
    {
		string TranslatedKey = parsedJson.FirstOrDefault(x =&gt; x.OD.Contains(l.LabelName)).Translated;
		if (newList.Any(x =&gt; x.LabelName == TranslatedKey))
			newList.First(x =&gt; x.LabelName == TranslatedKey).Count += l.Count;
		else
			newList.Add(new Label
			{
			LabelName = TranslatedKey,
			Count = l.Count
			});

    }
    return newList;
}
</code></pre>
<p>Complexity: foreach( O(N) ), FirstOrDefault( O(N) ), First( O(N) ), ContainsKey( O(~1) ), Add( O(1) )</p>
<p>Effectively: O(N<sup>2</sup>)</p>
<p>Time taken:</p>

<table>
<thead>
<tr>
<th></th>
<th>10 Labels</th>
<th>100 Labels</th>
<th>400 Labels</th>
<th>1000 Labels</th>
</tr>
</thead>
<tbody>
<tr>
<td>Time (ms)</td>
<td>1.9</td>
<td>2.2</td>
<td>2.8</td>
<td>5.7</td>
</tr>
</tbody>
</table><p><strong>After Thoughts</strong></p>
<ul>
<li>Need to consider overhead along with time complexity to be more accurate</li>
<li>LINQ has a lesser overhead and thus performs better even with O(N)</li>
<li>This approach seems to perform better than any other approach</li>
<li>This approach reaches a bottleneck at 1000 labels. Further testing hasn’t been done to concretely support the claim</li>
</ul>

