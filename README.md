[![Gem Version](https://badge.fury.io/rb/schema_plus_functions.svg)](http://badge.fury.io/rb/schema_plus_functions)
[![Build Status](https://secure.travis-ci.org/SchemaPlus/schema_plus_functions.svg)](http://travis-ci.org/SchemaPlus/schema_plus_functions)
[![Coverage Status](https://img.shields.io/coveralls/SchemaPlus/schema_plus_functions.svg)](https://coveralls.io/r/SchemaPlus/schema_plus_functions)
[![Dependency Status](https://gemnasium.com/SchemaPlus/schema_plus_functions.svg)](https://gemnasium.com/SchemaPlus/schema_plus_functions)

# SchemaPlus::Functions

SchemaPlus::Functions adds support for SQL functions in ActiveRecord.

SchemaPlus::Functions is part of the [SchemaPlus](https://github.com/SchemaPlus/) family of Ruby on Rails ActiveRecord extension gems.

## Installation

<!-- SCHEMA_DEV: TEMPLATE INSTALLATION - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
As usual:

```ruby
gem "schema_plus_functions"                # in a Gemfile
gem.add_dependency "schema_plus_functions" # in a .gemspec
```

<!-- SCHEMA_DEV: TEMPLATE INSTALLATION - end -->

## Usage

### Migrations

To declare a function use `create_function`:

    create_function :test, "start date, stop date DEFAULT NULL::date", <<-END
    RETURNS integer
      LANGUAGE plpgsql
      AS $$
    DECLARE
      processed INTEGER = 0;
    BEGIN
      processed = processed + 1;  
      RETURN processed;
    END;
    $$
    END

To create an aggregate function specify the `function_type`:

    create_function :array_cat_agg, "anyarray", <<-END, function_type: :aggregate
    (SFUNC=array_cat,STYPE=anyarray)
    END

To update a function in a new migration specify the `allow_replace` property

    create_function :test, "start date, stop date DEFAULT NULL::date", <<-END, allow_replace: true
    RETURNS integer
      LANGUAGE plpgsql
      AS $$
    DECLARE
      processed INTEGER = 0;
    BEGIN
      processed = processed + 5;  
      RETURN processed;
    END;
    $$
    END

To remove a function use `drop_function` with the name and argument types:

    drop_function :test, "start date, stop date"

To remove an aggregate use `drop_function` with the name and argument types with the `function_type`:

    drop_function :array_cat_agg, "anyarray", function_type: :aggrgate

### Introspection

You can query the list of user functions at the connection level (uncached):

    connection.functions

This will return a array of arrays. The inner array containing the function_name, argument types
and possible function type.

## Compatibility

SchemaPlus::Functions is tested on:

<!-- SCHEMA_DEV: MATRIX - begin -->
<!-- These lines are auto-generated by schema_dev based on schema_dev.yml -->
* ruby **2.3.1** with activerecord **5.2**, using **postgresql**

<!-- SCHEMA_DEV: MATRIX - end -->

## History

* 0.1.0 - Initial release

## Development & Testing

Are you interested in contributing to SchemaPlus::Functions?  Thanks!  Please follow
the standard protocol: fork, feature branch, develop, push, and issue pull
request.

Some things to know about to help you develop and test:

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_DEV - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
* **schema_dev**:  SchemaPlus::Functions uses [schema_dev](https://github.com/SchemaPlus/schema_dev) to
  facilitate running rspec tests on the matrix of ruby, activerecord, and database
  versions that the gem supports, both locally and on
  [travis-ci](http://travis-ci.org/SchemaPlus/schema_plus_functions)

  To to run rspec locally on the full matrix, do:

        $ schema_dev bundle install
        $ schema_dev rspec

  You can also run on just one configuration at a time;  For info, see `schema_dev --help` or the [schema_dev](https://github.com/SchemaPlus/schema_dev) README.

  The matrix of configurations is specified in `schema_dev.yml` in
  the project root.


<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_DEV - end -->

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_PLUS_CORE - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
* **schema_plus_core**: SchemaPlus::Functions uses the SchemaPlus::Core API that
  provides middleware callback stacks to make it easy to extend
  ActiveRecord's behavior.  If that API is missing something you need for
  your contribution, please head over to
  [schema_plus_core](https://github.com/SchemaPlus/schema_plus_core) and open
  an issue or pull request.

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_PLUS_CORE - end -->

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_MONKEY - begin -->
<!-- These lines are auto-inserted from a schema_dev template -->
* **schema_monkey**: SchemaPlus::Functions is implemented as a
  [schema_monkey](https://github.com/SchemaPlus/schema_monkey) client,
  using [schema_monkey](https://github.com/SchemaPlus/schema_monkey)'s
  convention-based protocols for extending ActiveRecord and using middleware stacks.

<!-- SCHEMA_DEV: TEMPLATE USES SCHEMA_MONKEY - end -->
