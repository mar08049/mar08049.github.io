---
layout: post
title:      "The Purpose for Redux Middleware"
date:       2018-02-20 03:33:07 +0000
permalink:  the_purpose_for_redux_middleware
---


An essential part of the Redux flow is asyncronous actions which dispatches an action to the Reducer. Using Redux middleware with a "dispatch" function is essential if we are collecting data from an external source.

We need a way to 1. Dispatch an action saying we are loading data 2.  Make a request to the api 3.  Wait for the response 4. Dispatch another action with the response data. We can use Redux Thunk for this flow. It allows us to do several things. It allows you to return a function inside an action creator. The function inside of Redux Thunk receives the store's dispatch function as its argument. This makes it easy for us to dispatch multiple actions from inside that returned function. 

Below is an example: 

```
export const getListings = () => {
  return (dispatch) => {
    dispatch({ type: 'FETCH_LISTINGS'});
    return fetch(`${API_URL}/listings`)
      .then(response => response.json())
      .then(listings => dispatch(setListings(listings)))
      .catch(error => console.log(error));
  }
}

```

Above we return a function and not an action. We can now dispatch actions from inside of the returned function.  First, we dispatch an action to state that we are about to make a request to our API. We then make the request. We do not hit our then() function until the response is received, this means that we are not dispatching our action of type 'FETCH_LISTINGS' until we receive our data. That makes it possible for us to send that data.

Redux Middleware like Redux Thunk gives us the power to dispatch multiple actions giving the function inside of the action creater an argument of dispatch. With this pattern we can dispatch multiple actions inside the returned function.


