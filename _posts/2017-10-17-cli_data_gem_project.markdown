---
layout: post
title:      "CLI Data gem project"
date:       2017-10-17 21:21:49 -0400
permalink:  cli_data_gem_project
---


Finally the project that I could do from scratch! Well, sort of. I could just copy one of the provided examples and refactor it to my data source, but then it would be a lot like a test-driven lab, even easier. 

You see, the problem I had with labs based on passing multiple small tests was that sometimes I didn't really understand what role a certain method or class was supposed to be playing in the program. I read the test file, saw the requirements and programmed the method to fit the spec. Only when I was finishing the program I began to really understand how the flow was distributed between different classes and methods and how they collaborated.

The latest project was a step up in my programming experience. I must admit I still peeked at the examples but in the end a large part of architecture was built by myself. 

Initially I wanted to scrape tradingeconomics.com where you can find all sorts of macroeconomic data on any country. But that would be too easy because the homepage already contains all important indicators and there's no need to go a level deeper. So I decided to turn to the switchup.org, the site I used for my research on coding bootcamps before I chose Flatiron!

My CLI gem provides information on 10 best coding bootcamps in NYC. I didn't make a separate Scraper class, all scraping happens inside `BestCodingBootcamps::Bootcamp` class. The program scrapes the NYC bootcamps page to get the list of bootcamps, initializes `Bootcamp` instances and provides each with the `:name` and `:url` attributes scraped from that page.

```
  def self.create_bootcamps
    main = Nokogiri::HTML(open("https://www.switchup.org/locations/nyc-coding-bootcamp"))
    main.search("div h3 a")[0..9].each do |b|
      bootcamp = self.new
      bootcamp.name = b.text.split(". ")[1]
      bootcamp.url = "https://www.switchup.org#{b.attribute("href")}"
    end
  end
	
  def initialize
    @@all << self
  end
```

`#ranking`,  `#about` and  `#courses` instance methods then scrape each bootcamp's page for information that their names suggest. `#courses` returns an array of courses. Some were duplicates so I had to use `.uniq` method to eliminate them.

```
  def courses
    list = doc.search("a.course-listing").collect do |c|
      c.text.strip
    end
    list.uniq
  end
```

Initially I had `attr_accessor` for all bootcamp attributes but then I realized that I was already using `#ranking`,  `#about` and  `#courses` in my code so I only left those that needed getters and setters: `:name` and `:url`.

There is a one-way relationship between `BestCodingBootcamps::CLI` class `BestCodingBootcamps::Bootcamp` classes. CLI knows about Bootcamp and calls it with `.create_bootcamps` class method. Bootcamp doesn't know about CLI. The `#menu` and other user interface interaction methods are pretty straightforward, the only peculiarity is that the `#menu` method uses recursion. I didn't like the use of `input = nil` and `if-else` inside the `while` loop, so I inserted reference to the method itself inside some if-else statements. It doesn't look very pretty with three `#menu` calls but I still like it more than 'while'... 'if-else'.

In general, I'm satisfied with my program, but still looking forward to refactor it if needed.

P.S. I just realized my gem could be a lot nicer if I added an option to visit the school's website. I'll probably do that in the next few days.

[https://github.com/dsbondin/best-coding-bootcamps-cli-gem](https://github.com/dsbondin/best-coding-bootcamps-cli-gem)
