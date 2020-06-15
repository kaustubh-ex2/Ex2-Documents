Task

Map a list of labels and count to new labels from a json.
Eg. if the case is like this

Image 1 : Label1, Label2, Label3

Image 2 : Label1, Label3,

Image 3 : Label1, Labele3, Label4

Image 4 : Label1, Label2

Then the response we'll get from API will be something like

    "labels": [
    	{
    		"name": "Label1",
    		"count": 4
    	},
    	{
    		"name": "Label3",
    		"count": 3
    	},
    	{
    		"name": "Label2",
    		"count": 2
    	},
    	{
    		"name": "Label4",
    		"count": 1
    	}
    ]

The json needs to be structured for size and readability both.

TL;DR

Attempt #5 is chosen.

Attempt #1

Json looks like (i.e JSON1.json)

    {
    	"child1": "parent1",
    	"child2": "parent1",
    	"child3": "parent2",
    	"child4": "parent3"
    }

And translation code looks like

    public static List<Label> TranslatedLabelNames(List<Label> oldList)
    {
    	//----- Iterate on old list and enter translated key into the dictionary
    	Dictionary<string, int> map = new Dictionary<string, int>();
    	List<Label> newList = new List<Label>();
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
    	return map.Select(dictRecord => new Label
    	{
    		LabelName = dictRecord.Key,
    		Count = dictRecord.Value
    	}).ToList();
    	return newList;
    }

Complexity: foreach( O(N) ), ContainsKey( O(~1) ), Add( O(1) )

Effectively: O(N)

Time taken:

           	10 Labels	100 Labels	400 Labels	1000 Labels
  Time (ms)	6        	6         	7         	9          

Attempt #2

json looks like (i.e. JSON2.json)

    {
    	"parent1": ["child1", "child2"],
    	"parent2": "child3",
    	"parent3": "child4"
    }

And translation code looks like

    public static List<Label> TranslatedLabelNames2(List<Label> oldList)
    {
    	Dictionary<string, int> map = new Dictionary<string, int>();
    	List<Label> newList = new List<Label>();
    	foreach (Label l in oldList)
    	{
    		string TranslatedKey = JSON.json.FirstOrDefault(x => x.Value.Contains(l.LabelName)).Key;
    		if (map.ContainsKey(TranslatedKey))
    			 map[TranslatedKey] += l.Count;
    		 else
    			 map.Add(TranslatedKey, l.Count);
    	}
    
    	//----- Construct a new list from the dictionary to send to the frontend
    	return map.Select(dictRecord => new Label
    	{
    		LabelName = dictRecord.Key,
    		Count = dictRecord.Value
    	}).ToList();
    	return newList;
    }

Complexity: foreach( O(N) ), FirstOrDefault( O(N) ), ContainsKey( O(~1) ), Add( O(1) )

Effectively: O(N (2))

Time taken:

           	10 Labels	100 Labels	400 Labels	1000 Labels
  Time (ms)	6        	7         	8         	9          

After Thoughts

- The JSON is considerably shorter
- The code on the other hand takes more time

Attempt #3

There is an extra step invloved in the translation i.e. the converting of map into the newList and returning the new list.
We can skip that step and directly make changes in the new list.

JSON is JSON2.json, same as in Attempt #2

Translation code is

    public static List<Label> TranslatedLabelNames2(List<Label> oldList)
    {
    	List<Label> newList = new List<Label>();
    	foreach (Label l in oldList)
    	{
    		string TranslatedKey = JSON.json.FirstOrDefault(x => x.Value.Contains(l.LabelName)).Key;
    		if (newList.Any(x => x.LabelName == TranslatedKey))
            	newList.First(x => x.LabelName == TranslatedKey).Count += l.Count;
    		else
    			newList.Add(new Label
    			{
    				LabelName = TranslatedKey,
    				Count = l.Count
    			});
    	}
    	return newList;
    }

Complexity: foreach( O(N) ), FirstOrDefault( O(N) ), First( O(N) ), ContainsKey( O(~1) ), Add( O(1) )

Effectively: O(N (2))

Time taken:

           	10 Labels	100 Labels	400 Labels	1000 Labels
  Time (ms)	4.2      	4.6       	5         	6.6        

After Thoughts

- This made the execution way faster.
- Complexity isn't the only thing affecting execution time.
- What if we do the same in the first approach as well ?

Attempt #4

JSON is same as JSON1.json

We are employing the removal of that step in the first approach as well.

Translation code looks like

    public static List<Label> TranslatedLabelNames(List<Label> oldList)
    {
    	List<Label> newList = new List<Label>();
    	foreach (Label l in oldList)
    	{
    		string TranslatedKey = JSON.json[l.LabelName];
    		if (newList.Any(x => x.LabelName == TranslatedKey))
            	newList.First(x => x.LabelName == TranslatedKey).Count += l.Count;
    		else
    			newList.Add(new Label
    			{
    				LabelName = TranslatedKey,
    				Count = l.Count
    			});
    	}
    	return newList;
    }

Complexity: foreach( O(N) ), Retrieval from JSON.json( O(1) ), First( O(N) ), ContainsKey( O(~1) ), Add( O(1) )

Effectively: O(N (2))

Time taken:

           	10 Labels	100 Labels	400 Labels	1000 Labels
  Time (ms)	2.4      	2.7       	3         	4.1        

Attempt #5

JSON is in a new format i.e. JSON3.json
It is more uniform with the current style of the code.
Offers more readability along with reasonable size.

Translation code

    public static List<Label> TranslatedLabelNames3(List<Label> oldList)
    {
        List<Label> newList = new List<Label>();
        foreach (Label l in oldList)
        {
    		string TranslatedKey = parsedJson.FirstOrDefault(x => x.OD.Contains(l.LabelName)).Translated;
    		if (newList.Any(x => x.LabelName == TranslatedKey))
    			newList.First(x => x.LabelName == TranslatedKey).Count += l.Count;
    		else
    			newList.Add(new Label
    			{
    			LabelName = TranslatedKey,
    			Count = l.Count
    			});
    
        }
        return newList;
    }

Complexity: foreach( O(N) ), FirstOrDefault( O(N) ), First( O(N) ), ContainsKey( O(~1) ), Add( O(1) )

Effectively: O(N (2))

Time taken:

           	10 Labels	100 Labels	400 Labels	1000 Labels
  Time (ms)	1.9      	2.2       	2.8       	5.7        

After Thoughts

- Need to consider overhead along with time complexity to be more accurate
- LINQ has a lesser overhead and thus performs better even with O(N)
- This approach seems to perform better than any other approach
- This approach reaches a bottleneck at 1000 labels. Further testing hasn't been done to concretely support the claim
