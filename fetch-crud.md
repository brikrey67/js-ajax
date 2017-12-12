# CRUD with Fetch

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
