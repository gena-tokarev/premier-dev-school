# ISP - Interface Segregation Principle
Suggests that a software system should be broken down into smaller, more focused interfaces rather than one large interface. This is intended to avoid forcing clients to depend on methods they don't use and to prevent coupling between unrelated parts of the system

## Examples in real life:

### Example 1:
Consider a car that has several subsystems, such as engine management, braking, and steering. Each subsystem would have its own interface that exposes the methods necessary to perform its specific functions.

For example, the engine management subsystem interface might include methods such as "start engine", "adjust throttle", and "monitor engine performance". The braking subsystem interface might include methods such as "apply brakes", "release brakes", and "adjust brake pressure". The steering subsystem interface might include methods such as "turn left", "turn right", and "adjust steering angle".

### Example 2:
An example of the Interface Segregation Principle would be a media player application that has different types of media, such as audio and video. Instead of having a single "MediaPlayer" interface that includes methods for playing both audio and video, the application can split the interface into two separate interfaces: "AudioPlayer" and "VideoPlayer". This way, clients that only need to play audio can depend on the "AudioPlayer" interface, and clients that only need to play video can depend on the "VideoPlayer" interface. This reduces the amount of unnecessary code and dependencies, and makes the application easier to maintain and extend.

## Examples in code:

Example of a car with its different part that has their own interfaces.

```typescript
interface CarPart {
  getName(): string;
}

interface Engine extends CarPart {
  start(): void;
  stop(): void;
  setThrottle(throttle: number): void;
}

interface Steering extends CarPart {
  turnLeft(): void;
  turnRight(): void;
}

interface Braking extends CarPart {
  applyBrakes(): void;
  releaseBrakes(): void;
}

class Car {
  private engine: Engine;
  private steering: Steering;
  private braking: Braking;

  constructor(engine: Engine, steering: Steering, braking: Braking) {
    this.engine = engine;
    this.steering = steering;
    this.braking = braking;
  }

  start() {
    this.engine.start();
  }

  stop() {
    this.engine.stop();
  }

  accelerate(throttle: number) {
    this.engine.setThrottle(throttle);
  }

  turnLeft() {
    this.steering.turnLeft();
  }

  turnRight() {
    this.steering.turnRight();
  }

  applyBrakes() {
    this.braking.applyBrakes();
  }

  releaseBrakes() {
    this.braking.releaseBrakes();
  }
}

class GasolineEngine implements Engine {
  getName() {
    return "Gasoline Engine";
  }

  start() {
    // start the gasoline engine
  }

  stop() {
    // stop the gasoline engine
  }

  setThrottle(throttle: number) {
    // adjust the throttle of the gasoline engine
  }
}

class ElectricSteering implements Steering {
  getName() {
    return "Electric Steering";
  }

  turnLeft() {
    // turn the car left using electric steering
  }

  turnRight() {
    // turn the car right using electric steering
  }
}

class HydraulicBrakes implements Braking {
  getName() {
    return "Hydraulic Brakes";
  }

  applyBrakes() {
    // apply the hydraulic brakes to stop the car
  }

  releaseBrakes() {
    // release the hydraulic brakes to allow the car to move
  }
}

```

## Comments:

The Interface Segregation Principle (ISP) and the Single Responsibility Principle (SRP) are related and often correlated.

The SRP states that a class or module should have only one reason to change, meaning that it should have only one responsibility. This helps to keep code maintainable, testable, and extensible.

The ISP, on the other hand, states that clients should not be forced to depend on interfaces they do not use. This helps to reduce the coupling between components and makes the code more modular and flexible.

When applying the ISP, we often create smaller and more specific interfaces that have a single responsibility. This helps to ensure that clients only depend on the methods they need, and that each interface has a clear and distinct purpose.

Therefore, the ISP and SRP are closely related because both principles aim to create more modular and maintainable code by ensuring that each component has a clear and specific responsibility.
