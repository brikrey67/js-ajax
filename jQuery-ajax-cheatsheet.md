# jQuery AJAX Cheat Sheet

In order to do an AJAX `get` request to a 3rd party API...

```javascript
$.ajax({
  // $.ajax takes an object as an argument with at least three key-value pairs...
  // (1) The URL endpoint for the JSON object.
  // (2) Type of HTTP request.
  // (3) Datatype. Usually JSON.
  url: url, // (1)
  type: 'get', // (2)
  dataType: 'json' // (3)
}).done((response) => {
  console.log(response)
  // Here is where you place code for DOM manipulation or anything else you'd like to do with the response
}).fail(() => {
  console.log('Ajax request fails!')
}).always(() => {
  console.log('This always happens regardless of successful ajax request or not.')
})
```

In order to do an AJAX `get` request to your own rails API...

```javascript
$.ajax({
  type: 'GET',
  dataType: 'json',
  url: '/artists'
}).done((response) =>  {
  console.log(response)
}).fail((response) => {
  console.log('Ajax get request failed.')
})
```

In order to do an AJAX `post` request to your own rails API...
```javascript
$.ajax({
  type: 'POST',
  data: {artist: {photo_url: 'http://www.fillmurray.com/300/300', name: 'Fill Murray', nationality: 'Fillish'}},
  dataType: 'json',
  url: '/artists'
}).done((response) =>  {
  console.log(response)
}).fail((response) => {
  console.log('Ajax get request failed')
})
```

In order to do an AJAX `put` request to your own rails API...

```javascript
$.ajax({
  type: 'PUT',
  data: {
    artist: {
      name: 'Robert Goulet',
      nationality: 'American',
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
```

In order to do an AJAX `delete` request to your own rails API:

```javascript
$.ajax({
  type: 'DELETE',
  dataType: 'json',
  url: '/artists/9'
}).done((response) => {
  console.log('DELETED')
  console.log(response)
}).fail(() => {
  console.log('Failed to delete.')
})
```
