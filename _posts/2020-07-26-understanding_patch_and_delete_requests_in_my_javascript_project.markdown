---
layout: post
title:      "Understanding Patch and Delete Requests in my Javascript Project"
date:       2020-07-26 14:08:04 +0000
permalink:  understanding_patch_and_delete_requests_in_my_javascript_project
---


For my Javascript project, I created a sneaker catalog web application called Sneaker Search. Within my project's application I wanted to gve the user the option to edit and delete their sneaker that they have added to their Sneaker list. I had no issues with my post request but when it came to implementing my patch request and delete request I hit a few roadblocks and I'm happy to share my solutions to the issues that I faced. First issue was how I would display the edit button action to see if the when the user clicks the edit button I would get a response in my console stating 'you pressed edit'. Below is a snippet of my code within my frontend's index.js file.

```
document.addEventListener('DOMContentLoaded', () => {
  // fetch and load sneakers
  console.log("DOM is Loaded");
  getSneakers()

  const createSneakerForm = document.querySelector("#create-sneaker-form")

  createSneakerForm.addEventListener("submit", (e) => createFormHandler(e))

  const sneakerContainer = document.querySelector('#sneaker-container')

  sneakerContainer.addEventListener("click", e => {
    
    const id = parseInt(e.target.dataset.id);
    const sneaker = Sneaker.findById(id);

   if (e.target.dataset.action === 'edit') {
     console.log('you pressed edit')
     document.querySelector('#sneaker-container').innerHTML = sneaker.renderUpdateForm();

   } else if (e.target.dataset.action === 'delete') {
     console.log('you pressed delete')

    document.querySelector(`#sneaker-${e.target.dataset.id}`).remove()
     deleteSneaker(sneaker)

   }  
  });
  document.querySelector('#sneaker-container').addEventListener("submit", (e) => updateFormHandler(e))
});

```

The conditional statement is where I detailed my edit and delete functions. The document.querySelector is ponting to #sneaker-container because that is where the sneaker data is being stored and I'm pulling the innerHTML from there and passing it along to my renderUpdateForm() with the help of the const variable "sneaker" which holds the id of the user's sneaker. Below is what my renderUpdateForm() will show the user with all values that the sneaker has currently before the user decides to edit the sneaker.

```
    renderUpdateForm() {
      return `
      <form data-id=${this.id} id="update-${this.id}">
          <h3>Edit a Sneaker!</h3>
          
          <label>Sneaker Name</label>
          <input id='input-name' type="text" name="name" value="${this.name}" class="input-text">
          <br><br>

          <label>Sneaker Description</label>
          <textarea id='input-description' name="description" rows="8"  cols="80" value="${this.description}"></textarea>
          <br><br>

          <label>Image URL</label>
          <input id='input-url' type="text" name="image" value="${this.image_url}" class="input-text">
          <br><br>

          <label>Sneaker Category</label>
          <select id='input-categories' type="text" name="categories" value="${this.category.name}">
              <option value="1">Basketball</option>
              <option value="2">Lifestyle</option>
              <option value="3">Running</option>
          </select>
          <br><br>

          <input id='edit button' type="submit" name="submit" value="Edit Sneaker" class="submit">
      </form>

      ` 
  }
	
```

Once the user clicks to edit their sneaker, the event within the updateFormHandler is then fired off, below is thedetailed function:

```
function updateFormHandler(e) {
  e.preventDefault();
  const id = parseInt(e.target.dataset.id)
  const sneaker = Sneaker.findById(id)
  const name = e.target.querySelector('#input-name').value
  const description = e.target.querySelector('#input-description').value
  const image_url = e.target.querySelector('#input-url').value
  const category_id = parseInt(e.target.querySelector('#input-categories').value)
  
  patchSneaker(sneaker, name, description, image_url, category_id)
}
```

The constant variables are then passed down to my patchSneaker function:

```
function patchSneaker(sneaker, name, description, image_url, category_id) {
  const bodyJSON = { name, description, image_url, category_id }
  fetch(`http://localhost:3000/api/v1/sneakers/${sneaker.id}`, {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json",
    },
    body: JSON.stringify(bodyJSON),
  })
    .then(response => response.json())
    // our backend responds with the updated sneaker instance represented as JSON

    .then(sneaker => {
      console.log(sneaker)

      const sneakerData = sneaker.data  
      const sneakerAttributes = sneaker.data.attributes
   
      let updatedSneaker = new Sneaker(sneakerData, sneakerAttributes)

      document.querySelector('#sneaker-container').innerHTML += updatedSneaker.renderSneakerCard();
    })
}
```

With my patch request, I had an issue correctly displaying the data I wanted to show in my browser. First I had to create a constant variable sneakerData that will equal to the particular sneaker I want to edit. I also created another constant variable to equal to the sneaker's attributes which holds the name, description, image_url, and category. Both are then passed down to another variable called updatedSneaker which will be creating the new Sneaker and rendering the newly edited sneaker. I utilize my debigger alot in order for me to know that I'm accessing the correct information that is being passed down to my functions.

Similar to my edit response wthin my conditional statement, when the user clicks 'delete', the console.log reponse stating  'you pressed delete' is showed in my console to validate that this particular buttin has been pressed. Then it finds the particular sneaker I want to delete.

```
    document.querySelector(`#sneaker-${e.target.dataset.id}`).remove()
     deleteSneaker(sneaker)
```

We then find the sneaker data id we like to remove from array and then we fire off the deleteSneaker with that particular sneaker we are deleting. 

```
function deleteSneaker(sneaker) {

  fetch(`http://localhost:3000/api/v1/sneakers/${sneaker.id}`, {
    method: "DELETE",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json",
    },
    
  })
  .then(response => response.json());
}
```

In my deleteSneaker functon we are similarly fetching the path for the sneaker.id as we did in the updateSneaker functuon but just simply spitting out .then(response => response.json()) since the action will just be deleting the sneaker.

Hope this was a visually helpful read that helps you understand edit and delete for Javascript.

