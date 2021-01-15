---
layout: post
title:      "this keyword in JS"
date:       2021-01-15 02:32:14 +0000
permalink:  this_keyword_in_js
---


If you've been developing in JS for a while you've probably come across the keyword *this*. 
In this article, I introduce one of the first concepts you need to understand: the execution context. 

For a more in depth dive into all the intricacies of this, I would recommend reading K[yle Simpson's : You Don't Know JS Yet ](https://github.com/getify/You-Dont-Know-JS). He does an incredible job at walking you through how JS works and I have improved immensely by taking my time working through his books.

If I had to summarize: The keyword *this* points at the object that a method is called on. We can think of it as the execution context.

In other words, the function is being invoked in the context of the object, and that object's properties are being made available to the function. The execution context could be different every time the function is called, and depends on the object the method is being called on. 
This actually gives us alot of flexibility, since the execution context is dynamic it is therefore re-usable.

Let's work through an example:

```
let dog = {
    speak() {
        console.log(`Woof! My name is ${this.name}.`)
    }
}
// this is an object with a method. A method is simply a property that points to a function.

let rover = Object.create(dog);
// here we create a new object setting its prototype link to dog.
rover.name = "Rover";
// here we set a property, name, on the rover object with a value of "Rover"

// the final rover object:

rover = {
    name: "Rover"
}

let snoopy = Object.create(dog);
// here we create a new object setting its prototype link to dog.
snoopy.name = "Snoopy";
// here we set a property, name, on the snoopy object with a value of "Snoopy"

// the final snoopy object:

snoopy = {
    name: "Snoopy"
}
```

Now that we've set up our objects, it's time to inspect how they work. We are focusing on how this, in the dog object is interpreted, depending on it's execution context.

```
rover.speak;
//Woof! My name is Rover.

snoopy.speak;
//Woof! My name is Snoopy.
```

There are several things going on here:
1. We call rover.speak.
2. The rover object doesn't have an 'own property' that's called speak.
3. We travel up the prototype chain to the dog object and find the method we are looking for.
4. We delegate the function call to the dog object, which is aware of the execution context through the keyword *this*.
5. In this case *this* refers to the rover object, which has a name property and is plugged in: #{this.name} translates 
     to rover.name in this execution context.
		 
The pattern for snoopy is almost identical, with the difference lying in what this translates to: ${this.name} will translate to snoopy.name.

We see that the keyword *this* is dynamic, the execution context is different for rover and snoopy and we've re-used the same speak method in the dog object to get different results.

Call(...)

If we invoked dog.speak we would get the following return value:

// Woof! My name is undefined.

Following the pattern we outlined above, here is what's going on:

1. We call dog.speak.
2. In this case *this* refers to the dog object, which doesn't have a name property defined. 
    That's the equivelant of calling dog.name which returns undefined.

JS gives us a way to specify the value of *this*. You are essentially manually providing the execution context for the function.

we could do the following:

```
dog.speak.call(snoopy);
// Woof! My name is Snoopy.
```

This works, because we are telling the function: "Hey, *this* should refer to the object snoopy. Whatever properties are defined on the object snoopy is available to you"

Conclusion:

This is just an introduction to the keyword *this*. As you go along your JS journey you'll see how essential *this* is to how classes work, you'll learn some nuances of how it is interpreted in arrow functions, and how *this* differs in strict mode vs non-strict mode. You'll also come across *this* when providing callback functions for event handlers in React.

For now, this is a good starting point. We've seen how we can write a function that uses *this* to extend it with the capability of changing certain values depending on the execution context. This is one of the many ways we can re-use functions. Rather than write a slightly different method for each object, we can take advantage of the dynamic nature of *this*.
