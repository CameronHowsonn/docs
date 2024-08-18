# Stream API

The stream API utilizes the Java 11's stream functionality which has been modernized and optimized so that in turn it is more cost-effective when it comes to processing power.

There are many different streams which will be covered in the docs, but for argument sake, we're going to use the inventory stream for our example.

```java
ItemStream items = Inventory.stream();
Item firstItem = items.first();
```

This then streams all of the items in your inventory and will allow you to filter on those to return the desired item.

### Caching

Caching refers to the act of storing something in it's state at present to refer to later. This is used a lot in scripts to save processing the calls again.

However, the moment you cache something it's out of date and no longer live. If you've cached an npc and the npc has moved, referring to your cachedNpc.tile() won't be the same as recalling the npc.

```java
//non cached call
if (Npcs.stream().name("Bob").first().valid() && Npcs.stream().name("Bob").first().inViewport()) {
	//execute
}

//cached call
Npc bob = Npcs.stream().name("Bob").first();
if (bob.valid() && bob.inViewport()) {
	//execute
}
```

In the non cached call, I'm iterating the npc stream twice for the same npc versus the cached call which iterates the npc stream once and you access the cached npc twice.

### Filtering

The act of filtering will remove items from our stream which don't meet the criteria provided. Removing said items are the way we can filter down to a group of items which share the same criteria, e.g. all iron ore to drop.
It's also the way we can filter down to a single item e.g. a knife ready to cut some logs.

#### Optimized Filtering

Filtering on the desktop client is much quicker as there is more processing power available. However, on Mobile it's a little more restricted. In turn, we need to consider how we want to filter our streams.
With streams generally it's recommended to .filtered() on the stream. This is not optimal for mobile and should only be used as a last resort.

```java
Inventory.stream().filtered(i -> i.id() == 995).first();
```
In this snippet I'm filtering my inventory stream to return the item which ID is 995 (coins) and then using .first() to return the first item from the stream. While this is a valid way to filter the stream, it's not optimal.

```java
Inventory.stream().id(995).first();
```
The above snippet is the optimal way. id(), name(), at(), within(), and nearest() have all been optimized to help reduce the processing power required to do the filtering, and for mobile you need optimization where possible.