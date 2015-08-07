# Rails

## Database
* ``rails g scaffold table_name attr1:type1 attr2:type2 ...``
create a migrate file that create the table with corresponding attributes, and views, controllers.
* ``rails g migration AddAttr1AndAttr2ToTable Attr1:type1 Attr2:type2``
create a migration that adds columns. The following are options you can put in `migration/date_time_add_attr1_and_attr2_to_table.rb`

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
