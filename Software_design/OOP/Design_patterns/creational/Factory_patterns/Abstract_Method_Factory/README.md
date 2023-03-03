# Abstract method factory

## Example

```typescript
// Define a Vehicle interface
interface Vehicle {
  move(): void;
}

// Define a Car class that implements the Vehicle interface
class Car implements Vehicle {
  move(): void {
    console.log('Driving the car');
  }
}

// Define a Motorcycle class that implements the Vehicle interface
class Motorcycle implements Vehicle {
  move(): void {
    console.log('Riding the motorcycle');
  }
}

// Define a Bicycle class that implements the Vehicle interface
class Bicycle implements Vehicle {
  move(): void {
    console.log('Riding the bicycle');
  }
}

// Define a VehicleFactory class that has a createVehicle() method
abstract class VehicleFactory {
  abstract createVehicle(): Vehicle;

  // Some other methods that are common to all VehicleFactory subclasses
  // ...
}

// Define a CarFactory subclass that implements the VehicleFactory class
class CarFactory extends VehicleFactory {
  createVehicle(): Vehicle {
    return new Car();
  }
}

// Define a MotorcycleFactory subclass that implements the VehicleFactory class
class MotorcycleFactory extends VehicleFactory {
  createVehicle(): Vehicle {
    return new Motorcycle();
  }
}

// Define a BicycleFactory subclass that implements the VehicleFactory class
class BicycleFactory extends VehicleFactory {
  createVehicle(): Vehicle {
    return new Bicycle();
  }
}

// Client code that uses the VehicleFactory to create objects
const carFactory = new CarFactory();
const motorcycleFactory = new MotorcycleFactory();
const bicycleFactory = new BicycleFactory();

const car = carFactory.createVehicle();
car.move(); // Driving the car

const motorcycle = motorcycleFactory.createVehicle();
motorcycle.move(); // Riding the motorcycle

const bicycle = bicycleFactory.createVehicle();
bicycle.move(); // Riding the bicycle

```

In this example, the VehicleFactory class provides an interface for creating Vehicle objects. The createVehicle() method is an abstract method that is implemented by its subclasses, which determine the type of Vehicle object that will be created. The client code can use the VehicleFactory to create objects of different types without knowing the concrete class of the objects being created.

