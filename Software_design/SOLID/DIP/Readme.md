# DIP - dependency inversion principle
Suggests that high-level modules should not depend on low-level modules. Instead, both should depend on abstractions.

## Examples in real life
Suppose you are a chef running a restaurant, and you need to order fresh ingredients every week from a local supplier. You could simply call the supplier and place your order directly with them every week, but this would create a tight coupling between your restaurant and the supplier, making it difficult to switch to a different supplier if needed.

To apply the principle of inversion of dependency, you could use an intermediary such as a food distributor, who would act as a middleman between your restaurant and the supplier. The distributor would be responsible for placing orders with the supplier, storing the ingredients in their warehouse, and delivering them to your restaurant as needed.

This way, your restaurant would depend on the distributor instead of the supplier, and the supplier would depend on the distributor instead of your restaurant. This creates a more modular and maintainable supply chain, since you can easily switch to a different distributor or supplier without affecting the rest of the chain.

In this example, the distributor is acting as the abstraction layer between your restaurant and the supplier, allowing you to apply the principle of inversion of dependency and decouple your restaurant from the supplier. By doing so, you create a more flexible and resilient supply chain that can adapt to changes in the market or your business needs.

## Examples

### Example 1

Let's say we have a class Customer that represents a customer in a retail application. The `Customer` class has a method `getOrders()` that returns a list of the customer's orders.

Without inversion of dependencies, the `Customer` class would depend on a `Database` class to fetch the customer's orders from the database. This creates a tight coupling between the `Customer` class and the `Database` class.

If the `Database` class changes in any way, the `Customer` class will also need to change to accommodate the changes. For example, if the database schema changes, the `Customer` class may need to be updated to reflect the changes. This can create maintenance issues and make the application harder to update.

With inversion of dependencies, the `Customer` class would depend on an abstraction, such as an `OrderRepository` interface. This interface would provide a method for fetching a customer's orders. The `Database` class would implement the `OrderRepository` interface, and the `Customer` class would depend on the interface instead of the `Database` class.

This makes it easier to switch to a different data source, such as an API or a different database, without changing the `Customer` class. It also makes it easier to mock the `OrderRepository` interface for testing purposes.


### Examples in code

Suppose we have a simple `UserController` that handles HTTP requests for user-related actions such as retrieving a user's details and creating a new user. We want to follow the inversion of dependencies principle by ensuring that the `UserController` does not depend directly on the database module, but instead relies on an abstraction.

First, we define an interface for our database module:

```typescript
interface UserRepository {
  findById(id: string): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
  create(user: User): Promise<User>;
}

```

Then, we create a concrete implementation of this interface using the pg library to connect to a PostgreSQL database:

```typescript
import { Pool } from 'pg';

class PgUserRepository implements UserRepository {
  constructor(private pool: Pool) {}

  async findById(id: string): Promise<User | null> {
    const query = 'SELECT * FROM users WHERE id = $1';
    const { rows } = await this.pool.query(query, [id]);
    return rows[0] || null;
  }

  async findByEmail(email: string): Promise<User | null> {
    const query = 'SELECT * FROM users WHERE email = $1';
    const { rows } = await this.pool.query(query, [email]);
    return rows[0] || null;
  }

  async create(user: User): Promise<User> {
    const query = `
      INSERT INTO users (id, name, email, password_hash)
      VALUES ($1, $2, $3, $4)
      RETURNING *
    `;
    const values = [user.id, user.name, user.email, user.passwordHash];
    const { rows } = await this.pool.query(query, values);
    return rows[0];
  }
}
```

Note that the `PgUserRepository` depends on the `pg` library, but the `UserController` will not depend directly on `PgUserRepository`.

Instead, we use the `inversify` library to create an instance of the `UserRepository` interface and inject it into the `UserController`:

```typescript
import { inject, injectable } from 'inversify';

@injectable()
class UserController {
  constructor(@inject('UserRepository') private userRepository: UserRepository) {}

  async getUserById(req: Request, res: Response) {
    const userId = req.params.id;
    const user = await this.userRepository.findById(userId);
    if (user) {
      res.json(user);
    } else {
      res.status(404).json({ message: 'User not found' });
    }
  }

  async createUser(req: Request, res: Response) {
    const user = req.body as User;
    const createdUser = await this.userRepository.create(user);
    res.status(201).json(createdUser);
  }
}

```

Here, we use the `@injectable` decorator to indicate that `UserController` is a candidate for dependency injection, and the `@inject` decorator to indicate that the `UserRepository` instance should be injected into the constructor.

Finally, we create an instance of the `PgUserRepository` and bind it to the `UserRepository` interface using the inversify container:

```typescript
import { Container } from 'inversify';

const container = new Container();
container.bind<UserRepository>('UserRepository').to(PgUserRepository);

const userController = container.resolve<UserController>(UserController);

```

Here, we create an instance of the `Container` class and bind the `UserRepository` interface to the `PgUserRepository` class


**NOTE!**

When we bind the `UserRepository` interface to the `PgUserRepository` class in the container using `container.bind<UserRepository>('UserRepository').to(PgUserRepository)`, we are telling the `inversify` library that whenever we request an instance of `UserRepository`, it should create and return an instance of `PgUserRepository`.
