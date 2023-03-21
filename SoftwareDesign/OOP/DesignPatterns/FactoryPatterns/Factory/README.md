# Factory

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
class VehicleFactory {
  createVehicle(type: string): Vehicle {
    switch (type) {
      case 'car':
        return new Car();
      case 'motorcycle':
        return new Motorcycle();
      case 'bicycle':
        return new Bicycle();
      default:
        throw new Error(`Invalid vehicle type: ${type}`);
    }
  }
}

// Client code that uses the VehicleFactory to create objects
const vehicleFactory = new VehicleFactory();

const car = vehicleFactory.createVehicle('car');
car.move(); // Driving the car

const motorcycle = vehicleFactory.createVehicle('motorcycle');
motorcycle.move(); // Riding the motorcycle

const bicycle = vehicleFactory.createVehicle('bicycle');
bicycle.move(); // Riding the bicycle

```
