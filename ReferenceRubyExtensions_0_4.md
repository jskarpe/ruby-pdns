﻿#summary Extensions to the Ruby Language
#labels Phase-Historical,Deprecated
# Extensions to the Ruby Language #



Ruby has a rich set of objects and methods, in this kind of application some additional functions might be useful, I've chosen to extend some parts of the core Ruby language in ways that would make sense.

## Arrays ##
Responses, lists of IPs and other bits are all [Arrays](http://www.ruby-doc.org/core/classes/Array.html), the following new methods have been created:

### shuffle! ###
This simply shuffles the order of entries inside the array, it's destructive in that it does not return a new array but simply replace the array it is being run on with a shuffled copy of itself.

The shuffling is done using the standard Ruby [rand()](http://ruby-doc.org/core/classes/Kernel.html) function and comes with the same caveats of true randomness.

**Code:**
```
ar = [1,2,3,4,5,6,7,8,9,10]
ar.shuffle!
pp ar
```

**Output:**
```
[10, 3, 7, 1, 5, 2, 6, 4, 9, 8]
```

### random ###
Extracting random items in an array is generally easy, doing so with weights and biases are much harder, yet to deal with [:Ruby\_PDNS/Recipe/WeightedRoundRobin:certain kinds of problem] it's very useful.

The _random_ method to Arrays lets you access a single element in an array via various methods, either random or weighted.

#### Simple Random Extraction ####

**Note:** you have no guarantee that the same item will not be returned more than once.

```
[1,2,3].random          #=> 2
[1,2,3].random          #=> 1
[1,2,3].random          #=> 3
```

#### Weighted Random Extraction ####

```
[1,2,3].random([1,4,1]) #=> 2
[1,2,3].random([1,4,1]) #=> 1
[1,2,3].random([1,4,1]) #=> 2
[1,2,3].random([1,4,1]) #=> 2
[1,2,3].random([1,4,1]) #=> 3
```

In this example the array passed as a property is an array of weights that matches 1:1 the item in the array, the higher the number the higher the probability that it will be returned.

#### Content based Weighted Random Extraction ####

When called in this way the length of each array element will be used as the weight, it would literally call the _length_ method on each array member and the integer response as the weight.

This method will not be massively useful in a DNS scenario today but was implemented for completeness sake as I do anticipate future cases where it might be useful,

```
['dog', 'cat', 'hippopotamus'].random(:length) #=> "hippopotamus"
['dog', 'cat', 'hippopotamus'].random(:length) #=> "dog"
['dog', 'cat', 'hippopotamus'].random(:length) #=> "hippopotamus"
['dog', 'cat', 'hippopotamus'].random(:length) #=> "hippopotamus"
['dog', 'cat', 'hippopotamus'].random(:length) #=> "cat"
```


### randomize ###

Since the _random_ method in its various use cases above return just 1 array item it has the risk of returning the same item more than once as shown in the examples above, a method exists to return a weighted shuffled copy of the array, u can then access the resulting items one by one.

```
[1,2,3].randomize           #=> [2,1,3]
[1,2,3].randomize           #=> [1,3,2]
[1,2,3].randomize([1,4,1])  #=> [2,1,3]
[1,2,3].randomize([1,4,1])  #=> [2,3,1]
[1,2,3].randomize([1,4,1])  #=> [1,2,3]
[1,2,3].randomize([1,4,1])  #=> [2,3,1]
[1,2,3].randomize([1,4,1])  #=> [3,2,1]
[1,2,3].randomize([1,4,1])  #=> [2,1,3]
```

See the [Weighted Round Robin](RecipeWeightedRoundRobin.md) example for a good example of using this method.