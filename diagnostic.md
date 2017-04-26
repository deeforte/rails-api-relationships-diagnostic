# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
  Join tables are valuable to keep tables to a manageable size and streamline tables so that each one only has fields that contain information directly about that particular table.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  profiles('id,''given_name', 'surname', 'email')
  movies('id','title', 'release_date', 'length')
  favorites('profiles_id','movies_id')

  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profile, inverse_of: :favorites
    belongs_to :movies, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  A serializer formats the way our database is viewed when shown on the localhost url after the GET.   In many to many relationships we are able to show the related database information.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :email, :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
  bin/rails generate scaffold favorite movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    dependent destroy is an attribute in a model that when a particular record is destroyed, the dependent record is destroyed.   For example in our profile/movie/favorite database if profile id of 3 was destroyed, the associated favorites pointing to profile id 3 would be deleted as well.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    If both the one to many and many to many relationships are important to our application both should exist.  For example in a clinic's database one person has one primary care physician who is a doctor, but they also have many other doctors.  And in turn each of the doctors have many patients.  Both the one-many and many-many relationships exist.
    Primary Care Physician (1) to people(m).
    Doctors(m) to people(m) have appointments.
  ```
