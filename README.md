# Clean architecture example with Python

## Resources

- [The clean architecture](http://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Clean architectures in Python](https://leanpub.com/clean-architectures-in-python)
- [Clean architecture example](https://github.com/mattia-battiston/clean-architecture-example)
- [Real life clean architecture](https://www.slideshare.net/mattiabattiston/real-life-clean-architecture-61242830)
- [A Guided Tour inside a clean architecture code base](https://proandroiddev.com/a-guided-tour-inside-a-clean-architecture-code-base-48bb5cc9fc97?gi=eba71cf04e7)

## Description of the layers (from inside to outside)

![The clean architecture (cleancoder.com)](CleanArchitecture.jpg)

### Entities

Representation of the domain models

> It contains classes, with methods that simplify the interaction with them.

> These models are not connected with a storage system, so they cannot be directly saved or queried using methods of their classes, they don’t contain methods to dump themselves to JSON strings, they are not connected with any presentation layer. They are so-called lightweight models.

> Entities have a mutual knowledge since they live in the same layer, so the architecture allows them to interact directly. This means that one of your Python classes can use another one directly, instantiating it and calling its methods. Entities don’t know anything that lives in outer layers, however. For example, entities don’t know details about the external interfaces, and they only work with interfaces.

Example :

- **/entities/room.py** : `class Room`

### Use cases

> Use cases are the processes that happen in your application, where you use you domain models to work on real data. Examples can be a user logging in, a search with specific filters being performed, or a bank transaction happening when the user wants to buy the content of the cart.

> A use case should be as small a possible. It is very important to isolate small actions in use cases, as this makes the whole system easier to test, understand and maintain. Use cases know the entities, so they can instantiate them directly and use them. They can also call each other, and it is common to create complex use cases that put together other simpler ones.

The use cases layer dictates the contract the upper layers must follow (since they can be used with dependency injection)

Example :

- **/use_cases/list_rooms.py** : `class ListRooms`

### Interfaces

Controllers, gateways, presenters, data providers, entrypoints

> A set of adapters that convert data from the format most convenient for the Use Cases and Entities to the format most convenient for an external agency such as a database.

Conversion of data format :

- use cases and entities format => external systems format (ex : Database, API)
- external systems format => use cases and entities format

> No code inward of this circle should know anything at all about the database. If the database is a SQL database, then all the SQL should be restricted to this layer, and in particular to the parts of this layer that have to do with the database.

Example :

- **/interfaces/room_repository.py** : `class RoomRepository`

### External systems

> Generally you don’t write much code in this layer other than glue code that communicates to the next circle inwards.

- Configuration (database…)
- APIs (used or exposed)
- Web framework
- Storage

Examples :

- **/data_providers/mem_room_repository.py** : `class MemRoomRepository`
- **/data_providers/mongo_room_repository.py** : `class MongoRoomRepository`
- **/data_providers/postgres_room_repository.py** : `class PostgresRoomRepository`