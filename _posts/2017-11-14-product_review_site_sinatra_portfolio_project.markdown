---
layout: post
title:      "Product Review Site Sinatra Portfolio Project"
date:       2017-11-14 22:25:06 -0500
permalink:  product_review_site_sinatra_portfolio_project
---


The idea is pretty straightforward - users can sign up and add reviews to the existing products or add their own. I used 3 models for the app: User, Product and Review. Both User and Product have many Reviews, Reviews belong to a User and a Product. User has many Products he/she left a review for (`through: :reviews`) and vice versa. 

The signup/login code is similar to the one used in Fwitter project except that I added error messages in case user leaves a blank field for username or password. 

```
    session[:notice] = "Username or password can't be empty"
    ...
    session[:notice] = "Incorrect username or password, please try again"
```

Here I used the session hash instead of sinatra-flash gem, I'll probably write about it in another post.

To make sure all usernames are unique, I used a helper #`username_taken?` method: 

```
    def username_taken?
      !!User.find_by(username: params[:username])
    end
```

Thanks to the `!!` this method can return either `true` if a particular username is found in the database, or `false` otherwise.

The app gives a user an option to visit site without signing in. You can still see all products and reviews but cannot add them.

Once logged in, your username will be shown in the top right corner on every page. Here I used a layout.html file which gives the site some basic styling and allows other custom pages to be loaded into the body of the layout file.

```
    <% if logged_in? %>
      Logged in as: <b><%= current_user.username %></b>&emsp;&emsp;&emsp;
      <a href="/logout">logout</a>
    <% end %>

    <%= yield %>

    <center><a href="/">Home</a></center>
```

The `/products` route loads a page with the list of all products that have been reviewed on the site. If you're logged in you'll be able to see a link to add a new product for review: 

```
<% if logged_in? %>
  <p><a href="/products/new">Add a new product</a></p>
<% end %>
```

I must admit the routes here are not completely RESTful, there is some deviation from conventional use. For example, the GET request to create a new review, instead of the standard `/reviews/new` looks like this: 

```
  get '/reviews/:product_id/new' do
    if logged_in?
      @product = Product.find(params[:product_id])
      erb :'reviews/new'
    else
      session[:notice]  = "You must be logged in to add a product review"
      redirect '/login'
    end
  end
```

The reason is that each review belongs to a particular product and the link to create a new review is located on the product page. So when sending a GET request you need to somehow pass the information about this particular product, hense `params[:product_id]` is used.

Along with writing a new review for a product the user is given an option (actually, a duty) to rate the product. This is achieved using a radio button input: 

```
    <p>Rate this product:
      <% i = 1 %>
      <% 5.times do %>
        <input type="radio" name="rating" value="<%= i %>"><%= i %></input>
        <% i += 1 %>
      <% end %>
    </p>
```

A user can edit and delete only their reviews and ratings. First, the links to edit and delete are only shown to the users who left the review, here the `current_user` helper method comes in handy.

```
      <% if logged_in? and review.user == current_user %>
        <a href="/reviews/<%=review.id%>/edit">Edit review</a>
        &emsp;&emsp;&emsp;
        <a href="/reviews/<%=review.id%>/delete">Delete review</a>
      <% end %>
```

Of course, anyone can visit these routes through a browser address bar. To prevent this, the same validation is used in ReviewsController: 

```
  get '/reviews/:review_id/edit' do
    @review = Review.find(params[:review_id])
    if logged_in? and @review.user == current_user
      erb :'reviews/edit'
    else
      session[:notice] = "You cannot edit this review"
      redirect "/products/#{@review.product.id}"
    end
  end
```

Another feature that differentiates this app from our previous Sinatra labs is that I used a GET request instead of POST to delete a review. This is done purely for aesthetic reasons - Edit and Delete links look better than an Edit link and a Delete button, in my opinion: 

```
  get '/reviews/:review_id/delete' do
    @review = Review.find(params[:review_id])
    @review.delete if logged_in? and @review.user == current_user
    redirect "/products/#{@review.product.id}"
  end
```

Hope this is clear and could be helpful to other students. The repository is here: 

[https://github.com/dsbondin/review-app-sinatra-project](https://github.com/dsbondin/review-app-sinatra-project)

-dnsbnd
