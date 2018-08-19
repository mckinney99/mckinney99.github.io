---
layout: post
title:      "Feeling (slug)ish?"
date:       2018-01-09 00:31:15 +0000
permalink:  feeling_slug_ish
---


As I finish up my final project for Sinatra, I am reminded of how difficult this topic was for me. Too many files got me too overwhelmed in the beginning, and I was feeling pretty sluggish. This final project has answered most of my remaining questions on this topic, and I'm now feeling pretty confident in my Sinatra/Routes knowledge. So to commerorate my sluggish feelings, here is a simple "how to" on getting slugs to work with your Sinatra/MVC style app.

```
#App > Concerns
#In order to properly use slugs, we need to ensure the names we elist to create slugs, are slug friendly.
#In our app folder we great a new folder, "Concerns" where we put this code:


module Slugifiable
  module InstanceMethods
    def slug
      name = self.name.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
      name
    end
  end

  module ClassMethods
    def find_by_slug(name)
      self.all.find do |o|
        o.slug == name
      end
    end
  end
end
```

Once our Slugifiable class is created, we can now use slug in our Controller methods. A simple way to think of a slug, is replace any word with ":id" to ":slug". Unlike the ID however, the slug is the actual name of the obect being created. We would not want to use slugs on personable, or irrelevent information.

Below we see how to find an object in a database using slug.

```
get "/recipes/:slug" do
    if logged_in?
      @recipe = Recipe.find_by_slug(params[:slug])
      erb :'recipes/show_recipes'
    else
      redirect '/users/login'
    end
  end
```

The power of slug can really be displayed when we want to show an array of links that our user has created for us. We simply iterate over the objects our user has created, and display our parameter in an "a" tag.

```
<% @user.recipes.each do |r|%>
<a href="/recipes/<%=r.slug%>"><%= r.name %></a><br><br>
<%end%>
```
