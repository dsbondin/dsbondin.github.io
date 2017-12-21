---
layout: post
title:      "Using {session} Hash In Sinatra"
date:       2017-12-21 18:58:06 +0000
permalink:  using_session_hash_in_sinatra
---

This post could be helpful to those who have problems with implementing Flash gem in Sinatra. I've been there and I found a way around. 

When I was working on Sinatra Portfolio Project, one of the features I wanted to implement in my app was using custom error messages. For example, when user hasn't filled a certain field, the program should redirect them back to the input page and display an error message explaining why it happened. 

There's a gem for Sinatra that does exactly that, called Flash. But for some reason it didn't work for me, and after doing some research I realized I wasn't the only one. So instead of trying to fix the gem I decided to experiment a bit with other options. I knew that session hash can be accessed throughout the app, in any view or controller. So instead of writing the message to the flash I decided to create a key-value pair in {session}, like so: 

```
    if params[:content].empty? or params[:rating] == nil
      session[:notice] = "Please fill in the review field and rate the product"
      redirect "/reviews/#{@product.id}/new"
    else
```

And then displaying it in the view: 

```
    <%= session[:notice] %>
```

There's no need to put it into `if` statement because if there was no notice it would simply not be displayed in the view.

The problem is, however, that when the user visits this page again, the notice will still be there. The gem I'm trying to emulate here was called Flash for a reason - it displays message only once. So let's quickly fix this: 

```
    <%= session[:notice] %>
    <% session[:notice] = "" %> 
```
 
 Here we're clearing the notice after it was displayed to the user. Let's add some color to it and we're done! 
 
```
    <font color="red">
    <%= session[:notice] %>
    <% session[:notice] = "" %> <!-- clears error message for future requests -->
    </font>
```

