# API Keys

While the majority of APIs are free to use, many of them require an API "key" that identifies the developer requesting access. This is done to regulate usage and prevent abuse. Some APIs also rate-limit the number of requests you send over a period of time. This is to manage load on the API.

Let's try making a GET request to the [Giphy API](https://api.giphy.com/)

- First with no key: [http://api.giphy.com/v1/gifs/search?q=funny+cat](http://api.giphy.com/v1/gifs/search?q=funny+cat)
- Then with a key: [http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC](http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC)

**It is very important that you do not push your API keys to a public Github repo.** This is especially true when working with [Amazon Web Services (AWS)](https://aws.amazon.com/). Here's an example of a [stolen key horror story](https://wptavern.com/ryan-hellyers-aws-nightmare-leaked-access-keys-result-in-a-6000-bill-overnight).

## Exercise: The Weather Underground API

For the first part of this exercise we'll be using the [Weather Underground API](http://www.wunderground.com/weather/api/d/docs). Follow the link and [sign up](https://www.wunderground.com/member/registration?mode=api_signup) for a key. You'll need to take the following steps...

- Sign up for a Weather Underground account
- From the main API page, select "Explore My Options"
- Click on "Purchase Key" - don't worry, it's free
- Fill out the resulting form

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
