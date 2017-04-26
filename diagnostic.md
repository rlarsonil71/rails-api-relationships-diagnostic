# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
  A Join table is used for combining fields from two tables by using values
  common to each. A join table supports one to many AND many to many
  relationships.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  Profiles Table:
  - id (integer primary key)
  - given_name (string)
  - surname (string)
  - email (string)

  Movies Table:
  - id (integer primary key)
  - title (string)
  - release_date (date)
  - length (integer)

  Favorites Table:
  - id (integer primary key)
  - movie_id (integer foreign key with references movie(id))
  - profile_id (integer foreign key with references profiles(id))
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    /* many-to-many */
    has_many :movies, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    /* many-to-many */
    has_many :profiles, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    /* many-to-many */
    belongs_to :profile, inverse_of: :favorites
    belongs_to :movie, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
A serializer is a filter of table data that is brought back to the client.  It
is used to control what data is brought back to the client requesting the data.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname
    has_many: movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
bundle exec rails generate scaffold favorites movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
`Dependent: Destroy` is used within a model to delete an instance of that class
as well as as to delete any associated objects of that class.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
One to many:
A library (one) can have (many) books.  A book can only belong to one library.

Many to Many:
Doctors (many) can have (many) patients.
  ```
