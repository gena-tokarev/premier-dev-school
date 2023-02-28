# LSP - Liskov Substitution Principle
Objects of a superclass should be able to be replaced with objects of its subclasses without affecting the correctness of the program. This principle ensures that the behavior of the program is consistent, regardless of which subclass is used.

## Examples in real life:
For example, let's say a customer of a car rental agency is looking for a car with at least five seats, a spacious trunk, and good gas mileage for their road trip. You could offer them a sedan, an SUV, or a sports car that meets their requirements, and they should be able to consider any of the cars without changing their requirements.
But you can't offer them a car with 2 seats for example.

## Examples in code:

### Example 1
Here's an example of how we could break the Liskov substitution principle:
```typescript
// Define a parent interface
interface Animal {
  name: string;
  speak(): void;
}

// Define a child class that implements the Animal interface
class Dog implements Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  // This implementation of the speak method violates the Liskov substitution principle
  speak(): string {
    return "Woof!";
  }
}

// Define a function that takes an Animal object and calls its speak method
function makeAnimalSpeak(animal: Animal) {
  console.log(`${animal.name} says`);
  animal.speak();
}

// Create an instance of the parent interface
const animal: Animal = { name: "Generic animal", speak: () => {} };

// Create an instance of the child class
const dog = new Dog("Fido");

// Call the function with both objects
makeAnimalSpeak(animal); // Prints "Generic animal says"
makeAnimalSpeak(dog); // TypeError: Type 'string' is not assignable to type 'void'.

```
In this example, the implementation of the speak method in the Dog class has been changed to return a string instead of void. This violates the contract of the parent interface Animal, which requires the speak method to have a void return type. When we pass an instance of the Dog class to the makeAnimalSpeak function, it expects the speak method to return void but instead it gets a string, which results in a type error.

This violates the Liskov substitution principle, because the Dog class no longer behaves like a subtype of Animal. Code that is designed to work with objects that implement the Animal interface may not work correctly with instances of the Dog class, because the return type of the speak method is not compatible.

### Example 2
However, it's possible to break the Liskov substitution principle **WITHOUT GETTING A TYPE ERROR** by using a conditional statement to check the type of the object and behaving differently based on the type. Here's an example:

```typescript
// Define a parent interface
interface Animal {
  name: string;
  speak(): string;
}

// Define a child class that implements the Animal interface
class Dog implements Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  speak(): string {
    return "Woof!";
  }

  wagTail(): void {
    console.log(`${this.name} is wagging its tail!`);
  }
}

// Define another child class that implements the Animal interface
class Cat implements Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  speak(): string {
    return "Meow!";
  }

  purr(): void {
    console.log(`${this.name} is purring!`);
  }
}

// Define a function that takes an Animal object and interacts with it based on its type
function interactWithAnimal(animal: Animal) {
  console.log(`${animal.name} says ${animal.speak()}`);

  if (animal instanceof Dog) {
    animal.wagTail();
  } else if (animal instanceof Cat) {
    animal.purr();
  }
}

// Create instances of the child classes
const dog = new Dog("Fido");
const cat = new Cat("Whiskers");

// Call the function with both objects
interactWithAnimal(dog); // Prints "Fido says Woof!" and "Fido is wagging its tail!"
interactWithAnimal(cat); // Prints "Whiskers says Meow!" and "Whiskers is purring!"
```
This violates the Liskov substitution principle because it means that instances of Dog and Cat are not fully substitutable for instances of Animal. If a calling code relies on the behavior of `interactWithAnimal`, and passes in an object that is not a Dog or Cat instance but still implements the Animal interface, it may not behave correctly because it does not expect the additional methods to be called.
TypeScript did not give us an error because the `interactWithAnimal` function expects an `Animal` instance, which the `Dog` and `Cat` instances are subclasses of.

However, the problem with violating the Liskov substitution principle is not one of type safety, but rather one of correctness. Even though TypeScript allows us to pass a `Cat` or `Dog` instance to the `interactWithAnimal` function, the behavior of the program may not be what we expect if the `Cat` or `Dog` instance has additional behavior that is not part of the `Animal` class.

We can modify the `interactWithAnimal` function to adhere to the Liskov substitution principle by removing the conditional statements that check the type of the object and instead relying on the behavior defined in the `Animal` interface. Here's an updated version of the function:
```typescript
function interactWithAnimal(animal: Animal) {
  console.log(`${animal.name} says ${animal.speak()}`);
}
```

If we want to interact with the Dog or Cat objects in a specific way, we can define additional functions that take these specific types as arguments and call the appropriate methods. For example:
```typescript
function interactWithDog(dog: Dog) {
  console.log(`${dog.name} says ${dog.speak()}`);
  dog.wagTail();
}

function interactWithCat(cat: Cat) {
  console.log(`${cat.name} says ${cat.speak()}`);
  cat.purr();
}

```

## Examples in react:

Here is how we can break the LSP principle:

```typescript jsx
type Props = HTMLAttributes<HTMLInputElement> & {
  onUpdate: (value: string) => void
}

const CustomInput = ({ onUpdate, ...props }: Props) => {
  const onChange = (event) => {
    /// ... some logic
    onUpdate(event.target.value)
  }

  return <input {...props} onChange={onChange} />
}
```
Here we break the interface and now CustomInput can't be substituted with `input` or other input component's modifications. In this case we'll have to change the prop name we pass to it.

Instead, we should do this:
```typescript jsx
type Props = HTMLAttributes<HTMLInputElement>

const CustomInput = ({ onChange, ...props }: Props) => {
    const handleChange = (event) => {
        /// ... some logic
        onChange(event)
    }

    return <input {...props} onChange={handleChange} />
}
```
