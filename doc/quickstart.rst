===========
Quickstart
===========

We'll be using a subset of the Chinook test database as an example. 
Create one file with the following contents (which I'll call ``runserver.py``)::

    from sandman.model import register, Model
   
    class Artist(Model):
        __tablename__ = 'Artist'

    class Album(Model):
        __tablename__ = 'Album'

    class Playlist(Model):
        __tablename__ = 'Playlist'

    class Genre(Model):
        __tablename__ = 'Genre'

    register((Artist, Album, Playlist))
    register(Genre)

    from sandman import app, db
    app.config['SQLALCHEMY_DATABASE_URI'] = '<your database connection string (using SQLAlchemy)'
    app.run()

Then simply run::

    python runserver.py

and try curling your new REST API service!

A Quick Guide to REST APIs
--------------------------

Before we discuss the example code above in more detail, we should discuss some
REST API basics first. The most important concept is that of a *resource*.
Resources are sources of information, and the API is an interface to this information. 
That is, resources are the actual "objects" manipulated by the API. In sandman, each 
row in a database table is considered a resource. 
Even though the example above is short, let's walk through it step by step.

Creating models
```````````````

A *Model* represents a table in your database. You control which tables to
expose in the API through the creation of classes which inherit from 
:class:`sandman.model.Model`. The only attribute you must define in your 
class is the ``__tablename__`` attribute. sandman uses this to map your
class to the corresponding database table. From there, sandman is able to divine
all other properties of your tables. Specifically, sandman creates the
following:

- an ``__endpoint__`` attribute that controls resource URIs for that class.

In the code above, we created 4 :class:`sandman.models.Model` classes that
correspond to tables in our database. 
In a "real" project, you should divide the code into at least two files: one with the 
"Model" definitions (``models.py``) and the other with the configuration 
and ``app.run()`` call (``runserver.py``). 

Or you can come up with your own scheme. Whatever.
