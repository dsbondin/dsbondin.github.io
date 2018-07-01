---
layout: post
title:      "Single Page?"
date:       2018-07-01 00:03:29 +0000
permalink:  single_page
---


All of us Flatiron students have learned some type of framework for creating single page applications or SPA. Usually it's either AngularJS or ReactJS. The advantages of SPA over MPA (multiple page applications) seem obvious - speed, convenience, easy cross-platform integration. But is SPA the single best choice? Should everyone just start using SPA and transform their old MPA? Lets look at the benefits and downsides of both ways. 

Multiple Page Application used to be the standard way of building websites. When user clicks on a link, the browser sends an HTTP request to the server and gets back a responce in the form of a new HTML page. During that time the page is frozen - users can't do anything until the request is fulfilled. Also the server needs to update all the data, like headers, footers and images even if they are the same as on the first page. JavaScript and jQuery help solve this issue but they become hard to work with in large projects. That's the main reason React and similar frameworks were created. SPA, on the other hand, behaves just like a desktop app without a need to install one. 

So the main benefit of SPA is that it's quick, highly reactive (the name React comes for a reason) and consumes less internet traffic. Another one is that it's easy to create a mobile app based on the desktop browser version. In SPA the front end is decoupled from back end, which is usually just an API that provides the data. This data is client-agnostic and can be fed to both the desktop and mobile clients in the same format. 

However with benefits come some downsides. SPA is harder to optimize for search engines. If your website's primary goal is to bring traffic from Google that becomes a problem. There are tools and ways to address that issue but it's still a lot more challenging than with MPA. Another disadvantage is that SPA can't run without JavaScript. If a user disables JS on their browser they simply won't be able to see your website. 

So the choice whether to use SPA or MPA for your website should depend on the project type and available resources. Lets say you are building a content-heavy website like thebalance.com providing expert articles on personal finance.  Traffic mainly comes from search engines where users type in their questions. After reading the article most people will close the page and some might go to another article. In this case MPA is a better solution. Twitter, on the other hand, is a different case. People usually go directly to Twitter to read their feed and reactive SPA will make their experience much more convenient. 

Happy coding!





