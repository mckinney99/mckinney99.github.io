---
layout: post
title:      "CLI Gem and Tonic!"
date:       2017-11-30 05:23:38 +0000
permalink:  cli_gem_and_tonic
---


For my CLI gem project I chose an application that would be able to scrape a website for details of the most popular cocktails from around the world. This project taught me a lot. Let's start with nodes in Nokogiri.

```
doc = Nokogiri::HTML(open('https://vinepair.com/articles/50-most-popular-cocktails-world-2017/'))
    names = doc.css("body.category-wine-blog article h3").each do |node|
      hash = {}
      hash[:name] = node.text
      hash[:description] = node.next.next.text
      if hash[:description] == ""
        hash[:description] = node.next.next.next.next.text
      end
```

The way Nokogiri scrubs through a webpage, is node by node. I had troubled with finding the different p tags within the page since they were not seperated by any divs. I ended up using the .next method to skip ahead to the next node. I also had to include an if state in case that .next.next was in fact a picture. If you visit the website located in the Nokogiri::HTML, you will see that under some of the cockail names lies a picture. So in my code I made it jump ahead to the next set of nodes if a picture was in fact present.

Another fun thing I learned:

```
def find_info(input)
    drink = @@all[input.to_i - 1]
    puts drink[:name]
    puts drink[:description]
  end
```

Indexing through arrays filled with hashes. It took me a lot longer than I expected to figure this out. At first I was trying to manipulate the array, but later I realized it would be easier to simply assign the array a variable, aka drink, and puts the hash keys to that individual index hash of the array.

All in all it was a fun project. I feel like I need a little more experience in Ruby, but am confident that I have the fundamentals. Keep coding on!
