---
layout: post
title:      "My first full stack app "
date:       2020-03-16 17:32:10 +0000
permalink:  my_first_full_stack_app
---

For my JS project I built an app that would help me  better organize my tasks. 


The app is simple: you can create any number of workspaces that you like, and you can populate each workspace with events and the estimated time it would take to complete. 

While I do have many ideas to extend the features of this app, I wanted to keep it as simple as possible as this is my first time building out both the front and back-end.  For a preliminary iteration, I wanted to make sure that I was able to get everything up and running on both the front and back- end. More specifically, I wanted to get better acquainted with capturing user input, communicating it to the server, doing something with that data, returning JSON to the front end, and finally manipulating the DOM. 

To do so, the functionality would first only be focused on creating a workspace card and populating that card with events. The app will also allow you to delete both workspaces and events

In this app you can: 
-	Create a workspace
-	Create an event
-	Delete a workspace
-	Delete an event
-	Generate a quote

Seeing that communicating with the backend is an essential piece, I thought I’d run you through all the steps related to creating a workspace. 

I started off by defining my Workspace object in the frontend. For now, it would have:
-	a title, an 
-	id that corresponds to the table id in the database, 
-	An array of events, 
-	and down the line would have some behavior in the form of a method. 

```
class Workspace {
    constructor(workspaceJSON) {
        this.id = workspaceJSON.id;
        this.name = workspaceJSON.name;
        this.events = workspaceJSON.events;
    };
};
```




On the backend I created a corresponding Workspace model 

```
class Workspace < ApplicationRecord
    has_many :events, :dependent => :destroy
end
```

note: adding the option of :dependent => :destroy ensures that when you destroy a workspace, all its associated events will be destroyed. This eliminates the need for us to go fishing for associated events whenever we want to delete a workspace. 

The next step is to capture user input using a form. A user should be able to name a workspace.

```
<form id="new-workspace-form">
 New Workspace: <br>
  <input type="text" name="name" id="workspace-name" placeholder="name your workspace">

   <button id="create-workspace-btn" type="submit">Create Workspace</button>
</form>
```


Great, so now a user can type in a workspace name, but when they hit submit, nothing happens yet. We now want our app to “sense” when the submit button has been clicked, save it to the database, and display the newly created workspace.


We now want to:
-	“sense” when a user completes and submits a form. 
-	We also want to package the user’s input, send it off to the Rails backend, and handle it’s response. 

We now have a clear idea of what the next two immediate steps are:

-	Set up an event listener on the form being submitted and 
-	start writing a FETCH request. 

First, we identify the form using its id:

const workspaceForm = document.querySelector("#new-workspace-form");

next we define the event listener that’ll fire when the form is submitted:

```
workspaceForm.addEventListener("submit", event) => {
   	event.preventDefault();
    	postWorkspace(event.target);
}); 
```


The code above does two things:

-	prevents the default behavior of the form which typically performs a request to a given URL. If we don’t do this, the page would actually refresh and our function wouldn’t work as we’d like it to. 
-	Calls postWorkspace(event.target) which is the function we will build next to handle the FETCH request. Event.target in this case is the form.

If you console.log(event.target) you get: 

```
<form id="new-workspace-form">
 	New Workspace: <br>
                    <input type="text" name="name" id="workspace-name" placeholder="name your workspace">

                    <button id="create-workspace-btn" type="submit">Create Workspace</button>
                </form>
```

To access the Workspace name, we make use of the input element’s name attribute value.

In this case the workspace name can be found in event.target.name.value


The postWorkspace function is what handles the FETCH request cycle, which we can break down into three main parts:
1.	Sending the user data
2.	Receiving the JSON
3.	Passing the JSON to a function that’ll display the newly created workspace

```
function postWorkspace(formData) {
        // console.log(formData)
    fetch("http://localhost:3000/workspaces", {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Accept': "application/json"
        },
        body: JSON.stringify({
            // we are converting the json below into a string so it can be transported.
            // "workspace": formData.name.value
            // the format below is exactly what will be received in params. 
            workspace: { 
                name: formData.name.value
            }
        })
        })
    .then(response => response.json())
    .then(json => handleNewWorkspaceJSON(json))
};
```

We are “POST”-ing the workspace name to http://localhost:3000/workspaces (which we will set up on the back-end shortly)

And when we receive back the promise response: convert it to JSON, then pass that JSON to a function that’ll handle creating a new JS object and display it.
 
function handleNewWorkspaceJSON(workspaceJSON) {
    
```
const newWorkspaceObject = createWorkspaceObject(workspaceJSON);
displayWorkspace(newWorkspaceObject);

workspaceForm.reset();
};
```
 
handleNewWorkspaceJSON(workspaceJSON) acts as an operator function that calls other functions to get the task at hand done. Its primary purpose in this case is to display the JSON. 

The sequence of events:
-	Create a new workspace Object.
-	Pass the object into the function that handles displaying it.

Displaying the workspace object can be further broken down into:
-	Creating a new workspace card (generating HTML).
-	Appending that card to where we want in the DOM.


The function definitions are provided below:

```
function createWorkspaceObject(workspaceJSON) {
    let newWorkspaceObject = new Workspace(workspaceJSON);
    workspaceObjects.push(newWorkspaceObject);
    return newWorkspaceObject;
}; 
```


```
function displayWorkspace(workspaceObject) {
    let card = createWorkspaceCard(workspaceObject);
    workspacesDeck.append(card);
};
```

createWorkspaceCard() is a function that will accept the workspace Object as an argument and generate the HTML we need for a workspace card to be displayed.



Rails Back-end:

The Rails backend was set up to run as an API, we know that we need a few things to be able to accept the request and respond to it:

A route:

resources :workspaces, only: [:create]

A workspaces controller with a create action:

```
def create
        new_workspace = Workspace.create(name: params[:workspace][:name])
        render json: new_workspace
    end
```


 and the workspace model that we set up earlier. 

The POST request will be directed to the create action in the Workspaces controller. A new workspace is created and persisted to the database, and then is rendered as a JSON response which gets sent back to the front-end.







