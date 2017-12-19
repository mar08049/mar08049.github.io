---
layout: post
title:      "Using Ruby Class Methods to connect Rails and JS through HTML"
date:       2017-12-19 13:58:37 +0000
permalink:  using_ruby_class_methods_to_connect_rails_and_js_through_html
---


Instructors at Flatiron school have made it very clear that when we are working with two stacks, it is important for us to stop, think, and understand what stacks we are in. I have found this challenging as I go from Ruby to JS which is a completely new topic for me. Ruby variables and methods do not work with Javascript and the same can be said about Javascipt working with Ruby: Technically it doesn't. But how do we add Javascript and jQuery to a Rails project if they are two different stacks?

In my project I created Ruby Class methods to assist me in making the fine tuned code machine work, in the same way that electricity works together with plumbing even though they aren't directly connected.

One of the requirments of the project was as follows: 

"Must render at least one show page (show resource - 'one specific thing') via jQuery and an Active Model Serialization JSON Backend. For example, in the blog domain, you might allow a user to sift through the posts by clicking a 'Next' button on the posts show page, with the next post being fetched and rendered via JQuery/AJAX."

This requires that you create rails link_to's for a previous and next button that allows you to sift through different show pages(without rerendering the page). To do this I created two class methods that I could call on an instance of Trip.

```
def next
    trips = trip_id_by_name
    trip_index = trips.find_index(id)
    next_trip = trip_index + 1
    if next_trip < trips.length
      trips[next_trip]
    else
      id
    end
  end

```

The "previous" method is very similar. I also created a private helper method "trip_id_by_name" for these methods to use. I then did this in my views:

```
  <%= link_to "Previous", trip_path(@trip.previous), :class => "previous_link", :"data-attribute" => @trip.id %>
  <%= link_to "Next", trip_path(@trip.next), :class => "next_link", :"data-attribute" => @trip.id  %>
```

Here I call the two methods on an instance of Trip and give it a class of "previous_link" and "next_link."

This view is what holds connects my JS and Rails. 

Then i use the class that I gave those links to perform my ajax requests:

```
function nextTrip(){
  $(".next_link").on("click"), function(e){
    e.preventDefault();
    var nextId = parseInt($(".next_link").attr("data-attribute")) + 1;
    $.get("/trips/" + nextId + ".json", function(data){
      $(".tripInfo").text(data[""]);
      $(".placeInfo").text(data[""]);
      $("previous_link").attr("data-attribute", data["id"])
    })
  }
}
```

Although it is important to remember that these two stacks are different, using logic and some creativity, we can use these two tools together flawlessly.
