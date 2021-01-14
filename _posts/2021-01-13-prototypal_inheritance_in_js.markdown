---
layout: post
title:      "Prototypal Inheritance in JS"
date:       2021-01-14 02:59:20 +0000
permalink:  prototypal_inheritance_in_js
---


Imagine you're in a classroom and each student has a card with a word and a definition on it. The teacher asks a student for the definition of a word. The student looks to their card but the word they have doesn't match. The student is free to ask other students to reveal the words written on their cards until they arrive at the answer. That is quite similar to how prototypal inheritance works. 

Every object has a link to another object. When you try to access a property in the object, JS will first look into the object to see if the property is there. If it's there, then great it'll return it and end its search. If it doesnt exist, it will then follow that link to the object's prototype, then the prototype of the prototype and so on, until it reaches Object. The prototype of Object is null.

In the following code example, we have two cards represented by two objects. 
Let's pretend for now that object b's prototype is object a. (we will see how we can specify an object's prototype later on in this article.)

```
let a = {
    minute: "Infinitely or immeasurably small"
}

let b = {
    obtain: "Come into posession of"
}
```


Now, let's try to retrieve some properties from these objects.

```
a.minute // "Infinitely or immeasurably small"
```

That's straightforward, we first look at object a and see that in this object's 'own properties' minute exists, and so its value is returned.

```
a.obtain // undefined
```

Here, we try to access a property called 'obtain', which doesnt exist in this object's 'own properties'. We then travel up the prototype chain to this object's prototype, which is Object. The property still isn't found and so we get a return value of undefined. 

```
b.obtain // "Come into posession of"
b.minute // "Infinitely or immeasurably small"
```

This is where we see prototypal inheritance in action. The property "minute" isn't one of object b's 'own properties'. We had to travel up the prototype chain to object a to get that value.


**Setting an object's prototype: Object.create()**

So how do we specify object b's prototype to be object a?

Rather than write out a literal object like this:

```
let b = {
    obtain: "Come into posession of"
}
```

we would first create an object using Object.create(). THis method accepts an argument which is the prototype you want to specify. In this case, to tell object b that object a is it's prototype we would write:

```
let b = Object.create(a)
```

then to add properties to object b we would do the following:

```
b.obtain = "Come into posession of"
```

To recap:
* we set up object b to point to object a as its prototype.
* we then set up 'obtain' as object b's own property and point it to the value "Come into posession of".

**Inspecting An Object's Own Properties**

Let's go back to the student example. The word they had on their card is still the same word, it hasn't changed. But by cooperating with their classmates, the word was retrieved. This is similar to what we observe when retrieving a value from an object's prototype. The object's own properties haven't changed, the value was simply retrieved from the object's prototype. Let's take a look at the following code example to see this:

We are still working with the two objects, a and b, with a being the prototype of b.
let's inspect the own properties of b.

```
b.hasOwnProperty("obtain") // true
b.hasOwnProperty("minute") // false
b.minute // ""Infinitely or immeasurably small"
```

We can access the property minute through the prototype chain. But if we ask object b if it has "minute" as one of its properties, we get a return value of false. 

I hope you found this article to be helpful, I know I found this topic to be quite confusing when I first started. The concept itself is quite simple, but where I got tripped up is seeing the words __proto__ and prototype in different contexts. I've since learnt that what you see in something like [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter),  has to do with the new keyword and how constructors work (but more on that in another post).
 
I would encourage you to play around with creating objects, assigning an object a prototype using Object.create(), and to inspect your object's own properties using hasOwnProperty.


