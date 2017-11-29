# AJAX and APIs

## Learning Objectives
- Explain what an API is and how to use one
- Describe AJAX and how it lets us build rich client-side applications
- Use `fetch()` to make an AJAX call
- Use promises to handle AJAX responses asynchronously
- Render new HTML content using data loaded from an AJAX request.
- Perform GET, POST, PUT, and DELETE requests to an API to modify data.

## Framing

Over the past few weeks, we've learned to build full-stack applications using html, css and ruby on rails. Our applications  persist data to a database, implement some business logic around our data and our users' requests and then display all of that to our user.

Today, we're going to learn how to make our applications more dynamic by making it so our application doesn't need to refresh the page to make a request to our server!

## Turn & Talk
> 10 minutes: 5 min T&T, 5 min review

[Open up Google Maps in your browser](https://www.google.com/maps). As you interact with the web app, consider the following questions:

<details>
  <summary>1. How many times do you see the page refresh?</summary>

</details>

<details>
  <summary>2. When you request new data (i.e. by searching for a location), does the page refresh? Does the Google Maps app get data from a server?</summary>

</details>


## What is an API
API stands for "Application Programming Interface" and is a way of describing certain software design. At the highest level, an API is any application with a set of instructions for how programmers can interact with it. The DOM and Active Record are actually examples of APIs.

When we're talking about the web (and web APIs), we're generally talking about an application that we interact with through it's routes. However, requests to an API's routes won't respond with views, they'll respond with data.

### API Data
An API will receive a scripted request and send a response, just like the full-stack applications we've built so far. The difference is that the API won't render views, it'll just send back data. That data will generally be in one of two forms: XML or JSON.

#### XML  
**XML** stands for "eXtensible Markup Language" and is the granddaddy of serialized data formats (itself based on HTML). XML is fat, ugly and cumbersome to parse. It is increasingly the less common of the two formats you'll encounter. It is still a major format due to its legacy usage across the web. You'll probably always favor using a JSON API, if available.

XML looks like this:

```
<users>
  <user id="23">
    <name><![CDATA[Bob]]></name>
  </user>
  <user id="72">
    <name><![CDATA[Tim]]></name>
  </user>
</users>
```

#### JSON
**JSON** stands for "JavaScript Object Notation" and has become a universal standard for sending and receiving data across the web. It is light-weight, easy-to-read and quick to parse.

JSON looks like this:

```json
{
  "users": [
    {"name": "Bob", "id": 23},
    {"name": "Tim", "id": 72}
  ]
}
```

#### What is Serialization?
All data sent via HTTP is sent as strings. Unfortunately, what we really want to pass between web applications is **structured data** (i.e., arrays and hashes). In order to do so, native data structures are **serialized**: converted from a javascript object into a string representation of the data. This string can be transmitted and then parsed back into data (de-serialized).

### Working with an API
APIs can be either public or private. If an API is public, anyone can access the data behind it. If an API is private, then  you'll need to get a password (called an API Key) or go through some other form of authorization before you can access data through that API.

We'll start by exploring a public API: [PokéApi](https://pokeapi.co/)

For the next 10 minutes, explore the PokéApi. Remember that web APIs take requests for data through the URL.

1. How do you get data for all Pokemon?
2. What about for a specific Pokemon?
3. How do you get data about a location?
4. What is similar and different about these URLs and the ones we've used for our routes in Rails?

## What is AJAX
**AJAX** stands for "Asynchronous JavaScript and XML".

Back in the early- and mid-1990s, the only way for a user to request new data was through the server-based request-response cycle that we've learned and covered with Rails. The user would click on a link or change some data in the UI and the whole page would reload. This was inefficient for the user and the User Experience; it was also and inefficient use of bandwidth, as an entire rendered page had to be transmitted rather than just the new or updated data.

This is where AJAX came in to play. AJAX is a way for us to make HTTP requests in JavaScript. So we can make requests to our server (asynchronously) without having to reload the page!

AJAX was first implemented in Internet Explorer as the `XMLHttpRequest` object and later adopted by Mozilla and Safari. In 2005, Gmail and Google Maps were rebuilt using `XMLHttpRequest and a developer named Jesse James Garret wrote an article title *"Ajax: A New Approach to Web Applications"*, where he coined the term *AJAX*.

Building apps with `XMLHttpRequest` lead to a better user experience and faster applications but it was extremely verbose and cumbersome to work with. To make it easier, jQuery implemented the `.ajax()` api, abstracting away `XMLHttpRequest` into a jQuery-like set of function calls.

More recently, WHATWG (the standards body for HTML) proposed the `fetch()` api as a browser-native implementation of AJAX similar to the jQuery api.

### Lets see it in Action

We will use jQuery's `.ajax()` method to make AJAX calls to an API. The standard requests we will be making are GET, POST, PUT, PATCH and DELETE.

Let's try that out ourselves. Clone down [this repo](https://git.generalassemb.ly/ga-wdi-exercises/js-pokeapi-ajax).

In `script.js`, we will use AJAX to send a `GET` request to the Pokemon API API...

```js
// Make sure to add your API key to the URL!
const url = 'https://pokeapi.co/api/v2/pokemon/1/';

// $.ajax takes an object as an argument with at least three key-value pairs...
// (1) The URL endpoint for the JSON object.
// (2) Type of HTTP request.
// (3) Datatype. Usually JSON.
$.ajax({
  url: url, // (1)
  type: 'GET', // (2)
  dataType: 'json', // (3)
}).done(() => {
  console.log('Ajax request success!');
}).fail(() => {
	console.log('Ajax request fails!');
}).always(() => {
	console.log('This always happens regardless of successful ajax request or not.');
});
```

Every AJAX request needs a URL (where we're making our request), the type (or method) of our request and the dataType that we want back (JSON or XML). jQuery's `.ajax()` method uses Promises to handle the asynchronicity of making AJAX requests. 

### Aside: Promises & Asynchronous JS
An AJAX request is asynchronous: we'll make our request to the server and some time later will get our response. In the meantime, the rest of our code will keep running. We need some way to handle it when it's finished. We've previously handled asynchronicity like this using callbacks. jQuery's `.ajax()` method uses Promises.

You'll notice there are 3 functions chained onto the AJAX call. These are our **promise methods**. Promises are callbacks that may or may not happen. A promise represents the future result of an asynchronous operation. It's how we handle the return value of our `$.ajax` request.

* `.done()` - this code is run if the AJAX call is successful
* `.fail()` - this code is run if the AJAX call is not successful
* `.always()` - this code is always run, regardless of whether the AJAX call is successful or not

The `$.ajax()` method makes our asynchronous call to the server using `XMLHttpRequest` and returns a Promise: an object that represents some future value. If our HTTP Request is successful, it'll call `.done()`, if it isn't then it'll call `.fail()`.

### The API Response
But how do we access the JSON object we saw in the browser before? By passing in an argument to the anonymous function callback. The `$.ajax` call returns a response that you can then pass in as an argument to the promise.

```js
.done((response) => {
  console.log(response)
})
```

> `response` contains the information received from the AJAX call.

We can drill through this response just like any other JS object...

```js
.done((response) => {
  console.log(response.forms.name)
})
```

## You Do: DOM Manipulation Using Response Data (10 minutes)

Delete the HTML inside of `index.html` (though not the two `<script>` tags!) and replace it with a form and a `<h1>`. Your form should include a single input for an ID and a submit button.

Add an event listener to your form so that when the user submit it, you make an AJAX call to the PokéAPI to find the Pokemon with the ID from the ID form field.

Inside your `.done()` promise callback, set the text of your `<h1>` to be the name of the Pokemon the user searched for.

> Hint: What does `.preventDefault()` do?

## Break (10 minutes)

## AJAX and CRUD
So we've used AJAX to do an asynchronous `GET` request to an API. More often than not, 3rd party APIs are read-only. They don't want anyone updating the Pokemon! There are some APIs that you can do full CRUD on, just not for the PokeApi.

It just so happens that we've built a Tunr Rails API where we can do full CRUD with AJAX. Go ahead and fork and clone [this repo](https://git.generalassemb.ly/ga-wdi-exercises/tunr_rails_ajax)!

Once you've cloned the repo, `cd` into it and run the usual commands.

```bash
$ bundle update
$ rails db:drop db:create db:migrate db:seed
```

We can now use `$.ajax()` to make asynchronous HTTP requests to our Tunr API! Run `rails s` and visit `localhost:3000` in your browser. Open up `app/assets/javascripts/application.js`, you should see this at the bottom of the file:

```
$(document).ready(() => {
  $('.js-get').on('click', e => {
    e.preventDefault();
    console.log('clicked!');
  });

  $('.js-post').on('click', e => {
    e.preventDefault();
    console.log('clicked!');
  });

  $('.js-put').on('click', e => {
    e.preventDefault();
    console.log('clicked!');
  });

  $('.js-delete').on('click', e => {
    e.preventDefault();
    console.log('clicked!');
  });
});
```

We're going to replace the code inside our four event handlers with 4 AJAX calls to our Rails API.

### Get
Lets start with our `GET` request. This won't look too different from the one we created for the PokeApi before:

<details>
	<summary>AJAX `GET` request</summary>
	
```
$('.js-get').on('click', () => {
	$.ajax({
	  type: 'GET',
	  dataType: 'json',
	  url: '/artists'
	}).done((response) => {
	  console.log(response)
	}).fail((response) => {
	  console.log('Ajax get request failed.')
	})
})
```

</details>

### Post

<details>
	<summary>AJAX `POST` request</summary>
	
```
$('.js-post').on('click', () => {
    $.ajax({
      type: 'POST',
      data: {
        artist: {
          name: 'Limp Bizkit',
          nationality: 'USA',
          photo_url: 'http://nerdist.com/wp-content/uploads/2014/12/limp_bizkit-970x545.jpg'
        }
      },
      dataType: 'json',
      url: '/artists'
    }).done((response) => {
      console.log(response)
    }).fail((response) => {
      console.log('AJAX POST failed')
    })
})
```

</details>

### Put

<details>
	<summary>AJAX `PUT` request</summary>
	
```
$('.js-put').on('click', () => {
    $.ajax({
      type: 'PUT',
      data: {
        artist: {
          name: 'Robert Goulet',
          nationality: 'British',
          photo_url: 'http://media.giphy.com/media/u5yMOKjUpASwU/giphy.gif'
        }
      },
      dataType: 'json',
      url: '/artists/6'
    }).done((response) => {
      console.log(response)
    }).fail(() => {
      console.log('Failed to update.')
    })
})
```

</details>

### Delete

<details>
	<summary>AJAX `DELETE` request</summary>
	
```
$('.js-delete').on('click', () => {
    $.ajax({
      type: 'DELETE',
      dataType: 'json',
      url: '/artists/4'
    }).done((response) => {
      console.log('DELETED')
      console.log(response)
    }).fail(() => {
      console.log('Failed to delete.')
    })
})
```

</details>

## Closing
JavaScript developers often need to perform CRUD functionality to/from an API. Today we've used jQuery to do this, but there are other smaller libraries that exclusively handle API endpoint interactions. Right now, an AJAX library called axios is very popular, which we'll use later on in the course. There is also the JS language-standard `fetch`, which will replace `XMLHttpRequest` eventually, but its implementation has not stabilized.

## Sample Quiz Questions

* Write an AJAX GET request to a known end point.  
* How does a promise differ from a callback?  
* Write an AJAX POST to create an object in a rails application.  

-------
## Practice

* Do everything we've done in class today for Songs.
* Create an AJAX request in another app you've created (e.g., projects, Scribble). Be sure to make sure your controller actions respond to JSON.

## Bonus

### API Keys
While the majority of APIs are free to use, many of them require an API "key" that identifies the developer requesting access. This is done to regulate usage and prevent abuse. Some APIs also rate-limit developers, meaning they have caps on the free data allowed during a given time period.

Let's try making a GET request to the [Giphy API](https://api.giphy.com/)...

* First with no key: [http://api.giphy.com/v1/gifs/search?q=funny+cat](http://api.giphy.com/v1/gifs/search?q=funny+cat)
* Then with a key: [http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC](http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC)

> In order to format JSON in the browser nicely, you might need a plugin like Chrome's [JSON Formatter](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?hl=en).

**It is very important that you not push your API keys to a public Github repo.** This is especially true when working with [Amazon Web Services (AWS)](https://aws.amazon.com/). Here's an example of a [stolen key horror story](https://wptavern.com/ryan-hellyers-aws-nightmare-leaked-access-keys-result-in-a-6000-bill-overnight).

## The Weather Underground API (10 minutes)

For the first part of this lesson we'll be using the [Weather Underground API](http://www.wunderground.com/weather/api/d/docs). Follow the link and [sign up](https://www.wunderground.com/member/registration?mode=api_signup) for a key. You'll need to take the following steps...
* Sign up for a Weather Underground account
* From the main API page, select "Explore My Options"
* Click on "Purchase Key" - don't worry, it's free
* Fill out the resulting form

<details>
  <summary><strong>If you get stuck try this visual guide to get back on track</strong></summary>

  [Sign-Up Guide](wunderground.md)

</details>

Once you've done that, visit the below link but make sure to replace `your_key` with your API key...

`http://api.wunderground.com/api/your_key/conditions/q/CA/San_Francisco.json`

You should see a big JSON object. Lucky for us, we'll be able to navigate through it using Javascript.

<details>
  <summary><strong>Now that you have your API key, where should you publish the API key? With whom do we share it?</strong></summary>

  > No where! Keep it out of your git repo! Share it with no one! Treat it like a password and keep it secret.

</details>

### Fetch, jQuery, XMLHttpRequest
There are three ways of making AJAX calls in JavaScript. The oldest is `XMLHttpRequest`. `fetch()` is the emerging standard.

Here we have three `GET` requests to the same endpoint using each of the three methods:

#### `fetch()`
```js
// fetch defaults to a `GET` request
fetch('https://pokeapi.co/api/v2/pokemon/1/')
  .then((response) => {
    console.log(response);
  })
  .catch(err => {
	  console.log(err)
  });
```

**Read more:**
* [https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
* [https://davidwalsh.name/fetch](https://davidwalsh.name/fetch)

#### jQuery `.ajax()`
```js
$.ajax({
  url: 'https://pokeapi.co/api/v2/pokemon/1/',
  type: 'GET',
  dataType: 'json',
}).done((response) => {
  console.log('Ajax request success!');
  console.log(response);
}).fail(() => {
	console.log('Ajax request fails!');
})
```

**Read more:**
* [http://api.jquery.com/jquery.ajax/](http://api.jquery.com/jquery.ajax/)

#### `XMLHttpRequest`

```js
const request = new XMLHttpRequest();

request.addEventListener("load", function() {
  console.log(this.responseText);
});
request.open("GET", 'https://pokeapi.co/api/v2/pokemon/1/');
request.send();
```

**Read more:**
* [https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)

### CRUD with Fetch

Here is how you would perform the same CRUD actions on Artists but using `fetch()`.

#### Get

```js
fetch('/artists.json')
	.then(response => {
		console.log(response);
	})
	.catch(error => {
		console.log(error);
	})
```

#### Post

```js
fetch('/artists.json', {
	method: 'POST',
	body: JSON.stringify({
		artist: {
		  name: 'Limp Bizkit',
		  nationality: 'USA',
		  photo_url: 'http://nerdist.com/wp-content/uploads/2014/12/limp_bizkit-970x545.jpg'
		}
	})
}).then(response => {
	console.log(response);
}).catch(error => {
	console.log(error);
})
```

#### Put

```js
fetch('/artists/6.json', {
	method: 'PUT',
	body: JSON.stringify({
		artist: {
			name: 'Robert Goulet',
			nationality: 'British',
			photo_url: 'http://media.giphy.com/media/u5yMOKjUpASwU/giphy.gif'
		}
	})
}).then(response => {
	console.log(response);
}).catch(error => {
	console.log(error);
})
```

#### Delete

```js
fetch('/artists/4.json', {
	method: 'DELETE'
}).then(response => {
	console.log(response);
}).catch(error => {
	console.log(error);
})
```












