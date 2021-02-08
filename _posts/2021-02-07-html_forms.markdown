---
layout: post
title:      "HTML forms"
date:       2021-02-08 02:13:49 +0000
permalink:  html_forms
---


As web developers we often need to ask people who visit our apps for information. That includes things we regularly interact with on the internet like: sign-up forms to create accounts, checkout forms to provide payment and address information,  or contact forms. There are so many other uses as well because at their core, forms are simply an interface to gather user input and send it somewhere. 

The typical pattern followed for a form is to gather user input and send it to a backend server where the request and data get processed. A response is then formulated on the back-end and sent back to the browser to be displayed to the user.

For a simple signup form this would play out like this:

* 	You create the form and specify what data to collect, where to send that data to, and what sort of request to use (GET, POST, PATCH, DELETE, etc.)
* 	A user fills out the form you created, providing information like name, email and password.
*  The user hits submit.
*  The form data gets sent to the endpoint you specified when designing the form. This is often a URL that matches to a certain endpoint on your backend app. 
*  The data then gets processed on the backend. Processing in this case could mean verifying that the user input is correct, that there are no existing users with the same credentials and finally creating the new user.
*  A response is then sent back to the browser. This could be a response that indicates the user was successfully created or that there was an error.

In this article we’ll start with the most basic form: the HTML form element and some of the most used form elements which we will explore below.

In subsequent articles I will be delving into more detail on how to connect simple forms with JavaScript to make asynchronous requests to the backend and how to create forms using frameworks like React and Ruby on Rails.


To start building a form we need to use the HTML <form> element:

```
<form>
</form>
```

Here, you can specify a few attributes on the form element to tell the form what to do with the data when the submit button is clicked.
Commonly, you would specify a method to use when sending your request (GET, POST, DELETE, etc), and what your endpoint will be. For example: the endpoint could be “mysite.com/users” which would indicate that I am trying to create a new user.


```
<form method=”POST” action=”#”>
</form>
```

Here, I’ve told the form to use a POST request to send data to the server. The action attribute is where you would specify your endpoint. In this case, it’s just a #.

Next up, we’re going to populate our form with elements to capture user data. 
The most commonly used form element is the input element and is used to capture text data.

```
<label for="first-name">First Name:</label>
        <input type="text" name="first-name" id="first-name">
        <br>
        <label for="last-name">Last Name:</label>
        <input type="text" name="last-name" id="last-name">
        <br>
```

Here I tell the form to have two entries to collect the user’s first and last name. I indicate in the input’s type attribute that I am collecting text and I give it an ID which is what the label element will use to associate with. 

To ask for a user’s email, the process is quite similar, the only difference being that the type attribute will now be “email”. This tells browsers that this input cell is for email, and so devices can autocomplete when appropriate. 

```
<label for="email">Email:</label>
 <input type="email" name="email" id="email">
```

To complete our signup form, we need a password and a submit button.

To collect the password, we will once again use the <input> form element, and specify its type attribute to “password”. This will inform browsers that this is a password field and anything typed in it will appear as dots or asterisks for privacy.

```
<label for="password">Password:</label>
<input type="password" name="password" id="password">
```

For our submit button, we use a button element and specify its type to “submit”. This way when the button is clicked, the form will submit the data using the method and action attributes we specified in our wrapping form element.

```
<button type="submit">Signup</button>
```

The code for our final form looks like this:

```
<form method="POST" action="#">
        <label for="first-name">First Name:</label>
        <input type="text" name="first-name" id="first-name">
        <br>
        <label for="last-name">Last Name:</label>
        <input type="text" name="last-name" id="last-name">
        <br>
        <label for="email">Email:</label>
        <input type="email" name="email" id="email">
        <br>
        <label for="password">Password:</label>
        <input type="password" name="password" id="password">
        <br>
        <button type="submit">Signup</button>
    </form>
```

The main points to consider when creating a basic HTML form :
* You will have a main form element that will wrap all the other form elements.
* In the form element, you specify the method to use for the request and the enpoint that the form will send the data to.
* A label will match it's for attribute to an id of a form element.
* For each input you should specify the type of data you are collecting.
* In your button element, you should specify the type of button that it is so that the form can correctly submit data to the endpoint you specified.



