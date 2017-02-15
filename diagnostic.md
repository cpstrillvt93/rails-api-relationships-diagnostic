# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    Join tables are valuable when you have two entities(tables) with different attributes. Depending on the objective of your application, creating a join table between these two tables allows your app to access and use attributes of one table with those of the other. I.e. reference eachother. This is crucial when you have two databases that you want to use together to complete a task.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Profiles have many movies. Profiles have many favorites. Similarly, movies have many profiles and also belong to favorites. Favorites have many movies and belong to many profiles
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through: :favorites
    has_many :favorites
    belongs_to :movies
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favorites
    belongs_to :profiles
    belongs_to :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    has_many :movies, through: :profiles
    belongs_to: profiles

  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  The purpose of the serializer is to attach numerical value to the attribues shared between the two main resources, in the join table.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :given_name, :surname, :email, :title, :release_date, :length
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rake scaffold favorite given_name:string, surname:string, email:string, title:string, release_date:date, length:time
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    If their was a subsequent child resource created under a parent resource, we would use dependent destroy to destroy that child relationship.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  In a library...Books have many authors(one to many). Authors have many books(one to many). Books have many borrowers and borrowers have many books(Many to many). Book to borrower relationship is facilitated through loans.
  ```
