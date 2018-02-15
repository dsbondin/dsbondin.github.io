---
layout: post
title:      "Final Project"
date:       2018-02-12 09:56:01 -0500
permalink:  final_project
---


It's been 5 months since I started at Learn.co and it was an amazing experience. It still is actually. I must admit the final project has been the most challenging so far. Even though I had solved all the labs on React-Redux, it turned out I still lacked comprehensive understanding of these technologies. Up until now I thought that back-end is my favorite part of programming, now it's backend AND React-Redux. Not sure why, probably because only now I start getting it :)

Initially I wanted my project to be simple so I could finish it quickly and start job hunting. On the other hand I understand that it's going to be a major part of my portfolio. So I came up with a compromise: it will be a simple app that shows a user random quotes on Advaita Vedanta with a possibility to rate and add new quotes to the database. The bonus part is that it implements user authentication. 

As it turns out, user authentication on React/Redux is a little more complicated than on Rails. With Rails you can simply set up Devise and configure it to your app requirements. Here you need to do everything manually. There are no session hashes, so if you want to save the current user's token on their browser you use localStorage.setItem(): 

```
function setCurrentUser(state, json) {
  if (json.token) {
    localStorage.setItem('token', json.token);
    localStorage.setItem('username', json.user.username);
  }
  return {...state,
    currentUser: json.user,
    token: json.token,
    signup_errors: json.signup_errors,
    errors: json.errors,
    isLoading: false};
}
```

Since my app is making lots of asynchronous requests, I used Redux thunk middleware a lot. The chain of function calls may seem a little complicated, but everything has it's meaning. For example, if a user wants to delete a quote from their collection, they use this button rendered by `<Quote>` component: 

```
<button className="close" onClick={() => props.removeQuote(props.quote.id)}>&times;</button>
```

which in turns calls the `removeQuote` function passed to `<Quote>` from `<QuotesList>` as a prop:

```
  removeQuote = (quoteId) => {
    const token = localStorage.getItem('token')
    this.props.removeQuoteFromCollectionAPI(quoteId, token)
  }
```

The removeQuote() function takes the user's token from localStorage (we need to make sure the user is authenticated to remove a quote) and calls `removeQuoteFromCollectionAPI(quoteId, token)` action: 

```
export function removeQuoteFromCollectionAPI(quoteId, token) {
  return dispatch => {
    dispatch({type: "SEND_ASYNCHRONOUS_REQUEST"});
    return fetch("http://localhost:3001/quotes/remove", {
      method: "delete",
      body: JSON.stringify({quote_id: quoteId}),
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Token": token
      }
    })
    .then(response => response.json())
    .then(JSON => { dispatch({
      type: "REMOVE_QUOTE_FROM_COLLECTION",
      JSON: JSON}
    )})
  }
}
```

The function dispatches an action to reducer saying it's making a request to server, sends that request with the user's token in headers and quoteId in the body, and once the server has verified the user and found the quote in their collection, it sends back the request with the updated collection. The action function then dispatches another action to reducer to update the state with the new collection: 

```
export function quotesReducer(state = initialState, action) {
  switch (action.type) {
    case 'SEND_ASYNCHRONOUS_REQUEST':
      return {...state, isLoading: true, message: null}
   ...
    case 'REMOVE_QUOTE_FROM_COLLECTION':
      return {...state, list: action.JSON.quotes, errors: action.JSON.errors, message: action.JSON.message, isLoading: false}
    default:
      return state;
  }
};
```

Another interesting thing is about how I solved the issue with redirecting after successful login/signup. Initially I simply pushed the redirect path to the browser history at the bottom of the handleSubmit method. But the problem was that it was redirecting even on unsuccessful logins/signups. Lifecycle hooks came handy here: 

```
  componentWillReceiveProps(props) {
    if (!!props.token) {
      this.props.history.push('/quotes/random')
    }
  }
```

There's a lot more to it. If you're intrested the repo is here: https://github.com/dsbondin/advaita-says

Happy coding!


