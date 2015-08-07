# REST

* REST: REpresentitave State Transfer by Roy Fielding
* REST in Rails 2+:
```
  1. show   : GET    /users/3
  2. create : POST   /users/3
  3. update : PUT    /users/3
  4. destroy: DELETE /users/3
```
* Browser does not support `put` and `delete`, so
``` 
<%= link_to 'update', user, method: :put %>
```
becomes html
```
<a href="/users/r" data-method="put">update</a>
```
and is further transformed by Rails unobstructive Javascript to
```
<form method="post" action="/users/4">
  <input name="_method" value="delete" />
</form>
```
Rails server look for `_method` and set the value to `method`
* `rake routes` shows all routes
* `new_user_path` gets the relative path (/users/new)
* `new_user_url` gets the url (full path)
* Form For: The following code
```
<%= form_for(@user) do |f| %>
  <%= f.submit %>
<% end %>
```
will become
  1. if `@user` isn't saved to database
```
  <from action="users" method="post">
    <input name="commit" type="submit" value="Create Zombie" />
  </form>
```
  2. if `@user` is saved to database
```
  <from action="users/4" method="post">
    <input name="_method" type="hidden" value="put">
    <input name="commit" type="submit" value="Update Zombie" />
  </form>
```

## Routes
* Routes to parameters:
  1. `/trips?user_id=4` => parameters: `{:user_id => 4}`
  2. `/users/4/trips  ` => parameters: `{:user_id => 4}`
  3. `/users/4/trips/2` => parameters: `{:user_id => 4, :id => 2}`
* If User `has_many :trips`, the bad way to find trips owned by user 4 is
```
/trips?user_id=4
```
The good way is:
```
/users/4/trips
```
  * `config/routes.rb`:
```
resources :users do
  resources :trips
end
```
  * `app/controller/users_controller.rb`
```
before_filter :get_user

def get_user
  @user = User.find(params[:user_id])
end

def show
  @trip = @user.trips.find(params[:id])
end

def index
  @trips = @user.trips
end
```
