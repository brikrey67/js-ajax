# Fetch, jQuery, XMLHttpRequest

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

- [https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [https://davidwalsh.name/fetch](https://davidwalsh.name/fetch)


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

- [http://api.jquery.com/jquery.ajax/](http://api.jquery.com/jquery.ajax/)

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
- [https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)
