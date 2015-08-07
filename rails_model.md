## Models
* ``rails g scaffold table_name attr1:type1 attr2:type2 ...``
create a migrate file that create the table with corresponding attributes, and views, controllers.
* ``rails g migration AddAttr1AndAttr2ToTable Attr1:type1 Attr2:type2``
create a migration that adds two columns. The following are options you can put in `migration/date_time_add_attr1_and_attr2_to_table.rb`

```
deafult: <value>
limit: 30
null: false
first: true
after: email
unique: true
```
* ``rake db:migrate``
To run all missing migrations
* ``rake db:rollback``
To rollback the previous migration
* ``rake db:schema:dump``
Dump current schema for future setup
* ``rake db:setup``
Creat database from schema dumped by ``rake db:schema:dump``
* ``rails g migration RemoveAttr1AndAttr2FromTable Attr1 Attr2``
create a migration that removes columns
* Some other commands you can put in migration
```
rename_column :users, :name, :username
rename_table  :users, :accounts
drop_table    :users
change_column :usres, :age, :integer, limit: 4
```

## Models - more
* Scope usage: inside `controller`, is a `query_clause` is very long,  change `table.query_clause` to `table.scope_name`, and put the `clause` in model as `scope :scope_name, query_clause`. Some example `query_clause` are:
```
where(logged_in: true)
where("age < 20")
order("created_at desc").limit(3)
```
* Callbacks: syntx: 
```
event :func_name
def func_name
  do_something
end
```
Full list of event: {before|after}_{validation|save|create|update|destroy}
* N+1 problem. Assume we have table User, Trip, and Trip.user_id is a foreign key that points to User._id, indicating the trip is owned by which user. Thefollowing code:
```
users = User.all
users.each do |user|
  puts user.trip.price
end
```
should be re-written as
```
users = User.includes(:trip).all
users.each do |user|
  puts user.trip.price
end
```
* If we have table A and B, we `add_index :bs, :a` when
  1. B `belongs_to :a`
  2. A `has_many :bs`
  3. A `has_one :b`
* Suppose we have table A, B, and C. And suppose A `has_many :bs`, and B `belongs_to :c` (so B has two foreign keys, B.a and B.c), we can associate A and C by A `has_many :c, through: :bs`. And the following queries:
  1. `A.cs << C.find_by_c("someString")` will generate
  ```
  SELECT * FROM "cs" where c = "someString"
  INSERT INTO "bs" (a, c) values (A.a, C.c)
  ```
  2. `A.cs` is equivalent to
  ```
  SELECT "cs".* FROM "cs" INNER JOIN "bs" ON "C.c" == "B.c" WHERE "B.a" == "a"
  ```

