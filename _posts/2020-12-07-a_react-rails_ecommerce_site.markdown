---
layout: post
title:      "A React-Rails ecommerce site."
date:       2020-12-07 22:35:37 +0000
permalink:  a_react-rails_ecommerce_site
---


This article is meant to accompany an app that I built for my graduation project for the Flatiron School. The code for the front-end can be found here:
And the repo for the backend is here.

This is a basic ecommerce site, which is built using React, Redux, Thunk and React-Router on the frontend, and Rails on the backend. This will be useful to you if you’re currently building your first full-stack-app and would like to take a look at how all the different parts come together and communicate. 

This app also features authentication, cookies, authorization, and persistence of data in the browser’s local storage. I chose to persist some data in the browser, so that a user’s cart would survive page refreshes.

User Story:

The user of the site is coming here to shop. They are presented with a list of products, which they can add to a cart. They can check that cart out as a guest, or if they would like to create an account, their cart will be remembered between sessions. 

A note on authentication: this way of implementing authentication is not intended for production purposes, but more so to better understand how authentication and authorization works. If you’re looking for a solution to secure your app, a common one for Rails applications is Devise. 


Front-end:

Our react frontend handles several things:

* Has components that display a user’s cart and a list of products.
* Takes in user input for logging in and signing up.
* Maintains state by dispatching actions to the Redux store, in response to certain events that we specify.
* It also persists some data to the browser’s local storage.
* Communicates asynchronously with the Rails API endpoints, by using Redux-Thunk.


Backend:

The backend is a Rails API. It’s main job is to send data to the frontend in response to requests that are made. With some of the responses, our Rails backend will send cookies that contain data on: the session, user, or cart contents.

A note on Rails as an API and using cookies: When you setup a Rails API, it will not have the cookies feature on by default and you will have to specifically include it. 



