---
layout: post
title:      "Partials, partials, partials!"
date:       2019-07-22 04:42:21 +0000
permalink:  partials_partials_partials
---


In case the title didn't properly describe my enthusiasm, I'm a big fan of partials! They're a great way to simplify views and can be used in ways you might not expect, given the fact that you can pass local variables to them. So, I'd like to share a couple of ways I've used them to my advantage in my recent project, FoodView.

If you know anything about partials, you know they're great for reducing redundant code. We can kill two views with one partial using the render keyword:

```
<h1>Dish Edit</h1>

<%= render "form" %>

<br><%= link_to "Delete This Dish", dish_path(@dish), method: :delete %>
```

The "form" can be used for both the edit and new views. Easy enough.

If that's not already the coolest thing, though, you can *also* nest partials, use locals to pass in a variety of objects to the same partial, and even have partials render on certain conditions. Incredibly useful.

As a nesting example, I decided to encapsulate the fields_for :restaurants used in my nested Dish creation form.

```
<%= form_for @dish do |d| %>
  <%= render partial: "application/errors", locals: { object: @dish } %>

   <<<form fields>>>

  <% if @dish.restaurant_id.nil? %>
    <%= render partial: "restaurants/nested_form", locals: { d: d, restaurant: @restaurant } %>
  <% else %>
    <%= d.hidden_field :restaurant_id, value: @dish.restaurant_id %>
  <% end %>
  <br><%= d.submit %><br>
<% end %>
```

This basically says that, if a dish already has a restaurant assigned, don't bother rendering the nested form since you won't need to create a restaurant for the dish. And, when the form is rendered, I just extend the form_for variable d into the partial, and then also set the restaurant variable defined in the controller. Then, the restaurant_id is submitted via a hidden field to be added to the dish after the fact.

Another fun way to use (abuse?) partials is for error messages! Notice the second line of the previous code snippet. Assuming you want your error messages to be consistent across controllers and routes (why wouldn't they?), rendering your error display as a partial makes it easy to keep your code DRY while tailoring it to a specific view, or views. For example:

```
<% if object.errors.any? %>
    <div id="error_explanation" class="field_with_errors">
        <h2>There were some errors:</h2>
        <ul>
            <% object.errors.full_messages.each do |message| %>
                <li><%= message %></li>
            <% end %>
        </ul>
    </div>
<% end %>
```

is a pretty standard error display. But, when you can pass in any object you want - whether @dish, @restaurant, or @user depending on the view, the same error process is followed each time. Although it's best to keep views as devoid of logic as possible, it doesn't hurt to throw in a conditional now and again to keep things interesting.

Gotta love Rails!

Thanks for reading.
