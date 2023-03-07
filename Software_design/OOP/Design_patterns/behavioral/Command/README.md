# Command pattern

Here is a nice explanation:
https://www.youtube.com/watch?v=UfGD60BYzPM

## Example

```typescript
// Define an interface for the Command object
interface Command {
  execute(): void;
}

// Define some concrete Command objects that implement the interface
class TurnOnCommand implements Command {
  constructor(private light: Light) {}

  execute(): void {
    this.light.turnOn();
  }
}

class TurnOffCommand implements Command {
  constructor(private light: Light) {}

  execute(): void {
    this.light.turnOff();
  }
}

// Define the Receiver class that will be receiving the commands
class Light {
  public isOn = false;

  turnOn(): void {
    this.isOn = true;
    console.log('The light is now on.');
  }

  turnOff(): void {
    this.isOn = false;
    console.log('The light is now off.');
  }
}

// Define the Invoker class that will be executing the commands
class RemoteControl {
  private commands: Record<string, Command> = {};

  setCommand(name: string, command: Command): void {
    this.commands[name] = command;
  }

  pressButton(name: string): void {
    const command = this.commands[name];

    if (command) {
      command.execute();
    }
  }
}

// Usage
const light = new Light();
const turnOnCommand = new TurnOnCommand(light);
const turnOffCommand = new TurnOffCommand(light);

const remote = new RemoteControl();
remote.setCommand('on', turnOnCommand);
remote.setCommand('off', turnOffCommand);

remote.pressButton('on'); // Output: The light is now on.
remote.pressButton('off'); // Output: The light is now off.

```

In this example, we have a `Command` interface that defines the execute method. We also have two concrete `Command` objects: `TurnOnCommand` and `TurnOffCommand`. These objects encapsulate the actions that need to be performed on the Light object, which is our receiver.

The `RemoteControl` class acts as the invoker, which executes the commands. It maintains a dictionary of named `Command` objects, and when a button is pressed, it looks up the corresponding command by name and executes it.

This pattern allows us to decouple the requester (the remote control) from the receiver (the light), and to encapsulate requests as objects. This makes it easy to add new commands in the future, and to parameterize requests with different arguments.

## Important comments
Without using the Command Pattern, the `RemoteControl` would need to have direct access to the `Light` object in order to perform the actions of turning it on or off. This would tightly couple the `RemoteControl` and `Light` classes, and make it more difficult to change the behavior of either class independently.
So this way if we want to add more buttons to the remote control such as TurnOnHeat and TurnOffHeat we'd need to have direct access to the `Heat` object as well. Or if we'd like to change the behavior of the existing buttons we'd need to change the code in the `RemoteControl` class which violates the Open/Close principle.
