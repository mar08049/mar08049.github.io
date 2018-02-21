---
layout: post
title:      "Returning JS Promises using Fetch()"
date:       2018-02-21 01:40:43 +0000
permalink:  returning_js_promises_using_fetch
---


An important part of React is having the ability to connect to an API to return data. This can be done using a fetch request. A fetch() request returns a Promise which is an object that represents some value that will be available later. We have to wait to access this data until the promise "resolves" and becomes available by connecting a then() function onto our fetch() call. The then() function will activated when the promise that fetch() returns is resolved which allows us to access the response data. 

```
  callApi = () => {
    console.log('a')
    fetch(`http://localhost:3000/api/listings`)
      .then(function(response) {
        console.log('b')
          return response.json()
      })
      .then(function(listings) {
        //dispatch(setListings(listings))
        console.log('c',listings)
      })
      .catch(error => console.log('d',error))
      console.log('e')

      //a, c + listings 
  }
```

The above code has a fetch() function that is making a request from a url. As you can see we have two then functions that return data. These do not return data until the promise is completely resolved. So to simplify the above function, we have a fetch function with two then functions inside. then outside of fetch we have a catch function that will initiate when an error occurs; if there are no errors it will not initiate. By the above logic we can conclude that the letters that will be logged will be logged in the following order("a, e, b, c").



