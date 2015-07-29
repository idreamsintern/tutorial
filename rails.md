# Rails

## Database
* ``rails g scaffold table_name attr1:type1 attr2:type2 ...``
create a migrate file that create the table with corresponding attributes, and views, controllers.
* ``rails g migration migration_name new_column1:type1 new_column2:type2``
create a migration that adds columns
* ``rake db:schema:dump``
Dump current schema for future setup
* ``rake db:setup``
Creat database from schema dumped by ``rake db:schema:dump``
