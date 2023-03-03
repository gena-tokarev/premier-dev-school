# Creator

Creator is a pattern that suggests that an object should be responsible for creating other objects that it requires to do its work.

This pattern helps to promote encapsulation by allowing objects to create the other objects they need without exposing the details of their creation to the rest of the system.

## Important notes
In other words we should delegate creating new objects to objects that have the necessary knowledge to initialize them properly.

The Creator pattern is especially useful in situations where an object needs to create and manage other objects as part of its responsibilities. By delegating the creation of objects to the appropriate creator object, we can ensure that each object is created and initialized correctly and that each object has access to the information it needs to do its work.

The Creator pattern suggests that we should use a factory method to create objects. If we create objects directly using the "new" operator within an object's methods, then we are not using a factory method. Using a factory method can make it easier to change the way objects are created in the future, as we can simply modify the factory method instead of modifying the object's methods.
