---
layout: post
title:      "Trading Journal App - Rails Project"
date:       2017-12-21 15:53:21 -0500
permalink:  trading_journal_app_-_rails_project
---


For the Rails project I decided to make an app that would allow traders to keep track of their transactions in the financial markets. 

The objects relational structure is pretty simple. A single trade belongs to a trader and a financial instrument (asset bought or sold). A trader can have many trades and an instrument can have many trades. 

I don't want to go into every detail but instead will focus on challenges and new things I learned while working on the project. 

Routes. Turns out the order in which you specify your routes matters. Yes, I remember that from Sinatra but the difference is that Sinatra doesn't have a separate route file and instead the routes are tied to actions in controller. In Rails you can use any sequence for actions in controller but you have to be careful in the routes. So, as in my case, 

```
  get '/trades/best' => 'trades#best'
  get '/trades/worst' => 'trades#worst'

  resources :trades
```

the routes for the best and the worst trades need to be set before `resources`, otherwise Rails will treat them as `trades/:id `and will send the user to `trades#show` action. 

The second problem I stumbled on was with writing attributes to a linked model. When trader adds a new trade to their journal they have an option of either choosing an existing instrument or creating a new one. Rails allows you to implement both using helpers like `accepts_nested_attributes_for` and `reject_if:` keywords. In my case `reject_if:` would be used to stop Rails from creating an empty Instrument if trader chose an existing from the list. I still don't know why but it didn't work for me, maybe because I used a custom attributes writer. But I fixed the problem quickly by creating a method in TradesController that would erase an empty instrument_attributes hash from params: 

```
    # def remove_empty_symbol
    #   params[:trade].delete(:instrument_attributes) if params[:trade][:instrument_attributes][:symbol] == ""
    # end
```

And it worked nicely until I started filling out the spec.md file which requires the app to have limited logic in controllers. So I commented out my method to move the logic where it belongs - to the model. After some debugging the solution was found. It's pretty simple and looks even better than my custom controller method. Here's my current instrument attributes writer: 

```
  def instrument_attributes=(attr)
    self.instrument = Instrument.find_or_create_by(attr) unless attr[:symbol] == ""
  end
```

Apart from that I didn't have any other issues but I'll mention one interesting thing I learned. You take it for granted how easily numbers are converted to currency in Excel. But what happens when you do it manually in Ruby? Just add a $ sign, right? And what if you have a negative number, for example when you have loss instead of profit? 

```
    $-300.00
```

Not nice! The solution is as simple as this: 

```
  def formatted_profit_loss
    profit_loss >= 0 ? "$#{profit_loss}" : "-$#{-profit_loss}"
  end
```

Who would have tought there is some extra logic involved in converting numbers to currency? Spoiled by Excel! 

Happy coding! 
