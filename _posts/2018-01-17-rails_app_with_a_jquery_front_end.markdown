---
layout: post
title:      "Rails App with a jQuery Front End"
date:       2018-01-17 21:17:53 -0500
permalink:  rails_app_with_a_jquery_front_end
---


As in my previous blog post, I will focus on the problems and challenges I encountered while adding JS features to my Trading Journal application. To remind, my app allows a user to keep a record of their transactions in the financial markets. 

The first problem came up when I was working on the trades show page. The user is given an option of sifting through trades by clicking on the `Next Trade` link. When the link is clicked the trade attributes are changed dynamically using jQuery. The problem is that my app allows users to delete their trades and when they do so the trades and their ids are gone from the database permanently. So if you look at the trades table in the database the ids column would look something like this: 

`1, 2, 3, 5, 6, 9, 10...`

When you get to the 4th trade you get a bug: the page still shows the third trade and the console shows an error as the program cannot find the needed attributes (keys) in the received JSON as the trade is non-existent. Appending a `.fail` method to the request chain will not solve the issue  - the AJAX went fine, the problem is with the object that was received. I solved the issue by making the TradesController handle the situation when the `@trade` was not found in the database: 

```
  def show
    if !!@trade
      @comment = @trade.comments.build
      respond_to do |format|
        format.html {render :show}
        format.json {render json: @trade}
      end
    else
      render json: {error: "This trade has been deleted or hasn't been created yet.", id: params[:id]}
    end
  end
```

Here i'm creating a special JSON object that has two keys: `:error` that will display a message on the page, and `:id` of the trade that wasn't found in the database. The `:id` is used to update the Next Trade link, otherwise the user will be stuck with the previous trade id and wouldn't be able to move forward. 

```
const renderEmptyTrade = function(trade) {
  $("h3#trade-info").text(trade.error);
  $(".trade_id").attr("trade-id", trade.id);
  ...
}
```

By the way, this line of code - `$(".trade_id").attr("trade-id", trade.id);` is used in multiple functions (and writing this just made me think that I should probably extract it into a separate function, and I'll probably do that soon). The show page also has a button that renders comments for the trade and a button that adds a form to create a new comment. 

`<a href="#" class="trade_id" trade-id="<%= @trade.id %>">Next Trade</a>`


`<button href="/comments" class="trade_id hide-when-no-trade" trade-id="<%= @trade.id %>" id="show-comments">Show Comments</button>`


`<button style="display: none" class="trade_id hide-when-no-trade" trade-id="<%= @trade.id %>" id="add-comment">Add Comment</button>`

The comments need to know which trade they belong to, right? The buttons and the link belong to one class: `.trade_id`, so updating their attributes only takes one line of code.  

Another thing that got me stuck for a while was hijacking the Submit button of the `new_comment` form. The thing is that the form is rendered with a click of a button, so `$(document).ready` isn't going to help here. The solution was pretty simple though, I just needed to call the `postComment() `function at the end of the `renderForm()`: 

```
const renderForm = function(tradeId) {
  $.get("/trades/" + tradeId + "/comments/new").success(function(response) {
    $("#comment-form").html(response);
    $("#add-comment").hide();
    postComment();
  })
}
```

Hope this was helpful. Happy coding! 

[https://github.com/dsbondin/trading-journal-rails-portfolio-project](http://)



