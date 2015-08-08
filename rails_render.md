# Rendering
## `respond_to`
* `trips_controller.rb`, html
```
def show
  @trip = Trip.find(params[:id])
end
```

* `trips_controller.rb`, html + json
```
def show
  @trip = Trip.find(params[:id])
  respond_to do |format|
    format.html
    format.json {render json: @trip}
  end
end
```

* `trips_controller.rb`, html with condition + json
```
def show
  @trip = Trip.find(params[:id])
  respond_to do |format|
    format.html do
      if @trip.has_discount?
        render :trip_with_discount
      end
      # if no discount, render usual view
    end
    format.json {render json: @trip}
  end
end
```

* `trips_controller.rb`, html with condition only
```
def show
  @trip = Trip.find(params[:id])
  if @trip.discount?
    render :trip_with_discount
  end
end
```

* `trips_controller.rb`, json only
```
def show
  @trip = Trip.find(params[:id])
  render json: @trip
end
```

* Only need `respond_to` when we need to repond to more then one thing.

* rendering status code list
```
:ok                   #200
:created              #201
:unprocessable_entity #422
:unauthorized         #401
:processing           #102
:not_found            #404
```
examplea
```
render json: @trip.errors, status: :unauthorized
render json: @trip, status: :created, location: @trip
```

## Creating custom route
* creating '/trips/3/discount' to get the discount
  1. method 1(less prefered): inside `config/routes.rb` add `match 'trips/:id/discount' => 'Trip#discount', as: :trip_discount`
  2. method 2(prefered):
```
resources :trips do
  get :discount, on: :member
end
```
* `:member` acts on single resource, while `:collection` acts on collection of resources. examples:
```
routes.rb                     url                 helper
get :discount, on: :member    /trips/:id/discount trip_promote_path(@trip)
get :search, on: :collection  /trips/search       trip_search_path
```
* For this new route, create method in `trips_controller.rb`
```
def discount
  @trip = Trip.find(params[:id])
  if (@trip.discount? == unknown)
    render json: @trip.to_json({only: :discount}), status: :unprocessable_entity
  else
    render json: @trip.to_json({only: :discount}), status: :ok
  end
end
```
* customize JSON reponse
```
@trip.to_json(include: :user, only: [:discoun, :price], except: [:id, :created_at, :updated_at])
```
* default json response, inside `app/models/trip.rb`
```
def as_json(options = nil)
  super(options || {includes: :user, except: [:id]})
end
```

## AJAX
* add `remote: true` the the link
```
<%= link_to 'delete', user, method: :delete, remote: true %>
```
which results in
```
<a href="/users/5" data-method="delete" data-remote="true" rel="nofollow">
  delete
</a>
```
* allow controller to accept javascript in `app/controllers/users_controller.rbz`
```
respond_to do |format|
  format.html {}
  format.json {}
  format.js   {} # <= add this line
end
```
* Responding javascript. Inside `app/views/users/destroy.js.erb`
```
$('#<%= dom_id(@user) %>').fadeOut();
```
* refactor loop content to partial
```
<div id="users">
  <% @users.each do |user| %>
    <%= render user %>
  <% end %>
</div>
```
```
<%= div_for user do %>
  <%= link_to "User #{user.name}", user %>
    <div class="actions">
      <%= link_to 'edit', edit_user_path(user) %>
    <%= link_to 'delete', user, method: :delete, remote: true %>
  </div>
<% end %>
```
* equivalent to
```
<div id="users">
  <%= render @users %>
</div>
```
* ajax form
```
<%= form_for(User.new, remote: true) do |f| %>
  <div class="field">
    <%= f.label :name %><br />
    <%= f.text_field :name %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %
```
* responding javascript: `app/views/create.js.erb`
```
$('div#users').append('<%= escape_javascript(render @user) %>');
$('div#<%= dom_id(@user) %>').effect('highlight');
```

