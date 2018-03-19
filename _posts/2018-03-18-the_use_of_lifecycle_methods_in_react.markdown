---
layout: post
title:      "The use of lifecycle methods in React"
date:       2018-03-19 01:56:17 +0000
permalink:  the_use_of_lifecycle_methods_in_react
---


I remember my first reaction to Learn.co lessons on React lifecycle methods. At that time I thought to myself: "There's no way I'm going to remember all of this stuff! And why seven and not eight?". Honestly I'm not even sure if there are 7, 8 or 11, and the truth is that I don't have to. Working on an actual React project made me realize that you only use methods that you need. And if you don't know how to implement some feature, there is a good chance someone had a similar problem before and already came up with a solution. That, I believe, is how these methods were created by React engineers. 

Here's how I came to use these methods in my React application. For example, on the login page I wanted to redirect user only upon successful authentication, otherwise, if there were errors, the user should remain on that page. So here I `usedcomponentWillReceiveProps` method:

```
  componentWillReceiveProps(nextProps) {
    // redirect only if user logs in successfully
    if (!!nextProps.token) {
      this.props.history.push('/quotes/random')
    };
    // does not clear username field if username is found but password is wrong
    if (nextProps.errors && nextProps.errors.password) {
      this.setState({
        password: ''
      });
    } else {
      this.setState({
        username: '',
        password: ''
      });
    }
  }
```

If `nextProps` receives a token (which means user is logged in successfully) then we redirect. Otherwise, if there's an error with password, the app will leave the username in the field and will clear the password. Or `else`, which means there's no token and the problem is not with the password (it's probably wrong username), then we clear both username and password fields. 

Please note that from security standpoint it's not the best practice to tell a user that the username is correct but the password is wrong. I just thought it would be a nice feature, I don't think hackers will try to break my Advaita quotes app :)

Happy coding! 

https://advaita-says.herokuapp.com


