# Abstract factory

## Example
```typescript
// Abstract Product A
interface Button {
  render(): void;
}

// Concrete Product A1
class MacButton implements Button {
  render(): void {
    console.log("Rendering Mac Button...");
  }
}

// Concrete Product A2
class WindowsButton implements Button {
  render(): void {
    console.log("Rendering Windows Button...");
  }
}

// Abstract Product B
interface InputField {
  render(): void;
}

// Concrete Product B1
class MacInputField implements InputField {
  render(): void {
    console.log("Rendering Mac Input Field...");
  }
}

// Concrete Product B2
class WindowsInputField implements InputField {
  render(): void {
    console.log("Rendering Windows Input Field...");
  }
}

// Abstract Factory
interface GUIFactory {
  createButton(): Button;
  createInputField(): InputField;
}

// Concrete Factory 1
class MacGUIFactory implements GUIFactory {
  createButton(): Button {
    return new MacButton();
  }

  createInputField(): InputField {
    return new MacInputField();
  }
}

// Concrete Factory 2
class WindowsGUIFactory implements GUIFactory {
  createButton(): Button {
    return new WindowsButton();
  }

  createInputField(): InputField {
    return new WindowsInputField();
  }
}

// Client code
class Application {
  private guiFactory: GUIFactory;
  private button: Button;
  private inputField: InputField;

  constructor(guiFactory: GUIFactory) {
    this.guiFactory = guiFactory;
    this.button = this.guiFactory.createButton();
    this.inputField = this.guiFactory.createInputField();
  }

  run(): void {
    this.button.render();
    this.inputField.render();
  }
}

// Client code using MacGUIFactory
const macFactory = new MacGUIFactory();
const macApplication = new Application(macFactory);
macApplication.run(); // Rendering Mac Button... Rendering Mac Input Field...

// Client code using WindowsGUIFactory
const windowsFactory = new WindowsGUIFactory();
const windowsApplication = new Application(windowsFactory);
windowsApplication.run(); // Rendering Windows Button... Rendering Windows Input Field...

```

In this example, we have two abstract products: `Button` and `InputField`. We have two concrete products for each abstract product: `MacButton`, `WindowsButton`, `MacInputField`, and `WindowsInputField`.

We also have an abstract factory `GUIFactory`, which provides interfaces for creating `Button` and `InputField` objects. We have two concrete factories for each abstract factory: `MacGUIFactory` and `WindowsGUIFactory`.

Finally, we have the `Application` class that uses a `GUIFactory` to create a `Button` and an `InputField` object, and then calls the `render()` method on each object.

By using the Abstract Factory pattern, we can create families of related objects (buttons and input fields) without having to specify their concrete classes. The client code can use different factories to create objects with different implementations, which makes the code more flexible and extensible.

The abstract factory can be thought of as a blueprint for creating objects, and the concrete factories implement that blueprint to create the actual objects.
