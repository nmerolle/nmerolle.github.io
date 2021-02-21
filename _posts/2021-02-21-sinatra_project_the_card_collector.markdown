---
layout: post
title:      "Sinatra Project: The card collector"
date:       2021-02-21 20:08:21 +0000
permalink:  sinatra_project_the_card_collector
---


For my second project at Flatiron School I developed an app in Sinatra that allows a user to track sports cards in a collection. The individual users should have access to only their own collection and not be able to view, edit or delete cards in another users collection.
I had initially planned to use some simple logic in the index route via the views page like so:

<h3>Your Collection</h3>
<ul>
<% @cards.each do |card| %>
<% if current_user.id == card.user_id %>
<li><a href=”/cards/<%= card.id %>” > <%= card.playername %></a></li>
<%end%>
<%end%>
</ul>

It seemed a perfect solution that a user only saw their own cards on the index page. As it turns out it was far less than perfect as anyone could simply type in a url for a cards show page (such as 127.0.0.1:9393/cards/6) and see it even if they did not own the card. From the show page it was possible for someone to edit or delete the card without being the user who owned the card.
A better solution to the problem was to create a private method in the card controller. Private methods can only be used in the controller where they are set. The method,

def redirect_if_not_owner
     redirect ‘/cards’ unless @card.user == current_user
end

can then be used to check that the current user is the owner of the card and if not redirect them back to the their own cards index. Simply placing this method in the show, edit and delete routes of the controller prevents one user from modifying another users collection.
