## REST

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
