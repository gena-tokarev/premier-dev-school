# OCP - Open/Closed principle

Software entities should be open for extension but closed for modification. This means that you should be able to extend the behavior of a software entity without modifying its source code

## Examples in real life:
- In the automotive industry, car manufacturers use modular design to allow for different configurations and features without having to redesign the entire car. For example, a car manufacturer may use the same chassis and body design for multiple models, but with different engines, transmissions, and interior options. This allows for greater flexibility and cost-effectiveness, without having to completely redesign the car each time a new feature is added.

## Comments:
It correlates with other design principles (for example SRP).

If you follow SRP then you most likely to achieve OCP, since if your class is responsible for one thing only, you easily can apply OCP using some structural patterns such as Factory without changing the class code.

## Examples in code:

### Example 1:

Suppose we have the following Product and ProductList classes that calculate the total cost of a list of products in a given currency using a currency factor:
```typescript
class Product {
    public name: string;
    public price: number;

    constructor(name: string, price: number) {
        this.name = name;
        this.price = price;
    }
}

class ProductList {
    private products: Product[];
    private currencyFactor: number;

    constructor(products: Product[], currencyFactor: number) {
        this.products = products;
        this.currencyFactor = currencyFactor;
    }

    public getTotalCost(): number {
        let totalCost = 0;
        for (const product of this.products) {
            totalCost += product.price * this.currencyFactor;
        }
        return totalCost;
    }
}
```

Suppose we want to introduce a new currency conversion strategy that uses a different formula to calculate the total cost. For example, we want to apply a discount to the total cost for products that have a price above a certain threshold.

With the current implementation that uses the currencyFactor parameter, we would have to modify the ProductList class to accommodate the new currency conversion strategy:

```typescript
class ProductList {
    private products: Product[];
    private currencyFactor: number;
    private discountThreshold: number;
    private discountFactor: number;

    constructor(products: Product[], currencyFactor: number, discountThreshold: number, discountFactor: number) {
        this.products = products;
        this.currencyFactor = currencyFactor;
        this.discountThreshold = discountThreshold;
        this.discountFactor = discountFactor;
    }

    public getTotalCost(): number {
        let totalCost = 0;
        for (const product of this.products) {
            if (product.price > this.discountThreshold) {
                totalCost += (product.price - this.discountThreshold) * this.discountFactor;
            } else {
                totalCost += product.price * this.currencyFactor;
            }
        }
        return totalCost;
    }
}
```

With this modification, the ProductList class can now handle the new currency conversion strategy that applies a discount to the total cost for products above a certain threshold.

However, this modification violates the open/closed principle, as we had to modify the existing ProductList class to add the new currency conversion strategy. This can be error-prone and time-consuming, especially if we have to introduce more complex currency conversion strategies.

To follow OCP we can use the CurrencyConverter interface and separate classes to implement the currency conversion logic provides more flexibility and maintainability in the long run. To implement the new currency conversion strategy that applies a discount, we can create a new DiscountConverter class that implements the CurrencyConverter interface:

```typescript
interface CurrencyConverter {
    convert(price: number): number;
}

class Product {
    public name: string;
    public price: number;

    constructor(name: string, price: number) {
        this.name = name;
        this.price = price;
    }
}

class ProductList {
    private products: Product[];
    private currencyConverter: CurrencyConverter;

    constructor(products: Product[], currencyConverter: CurrencyConverter) {
        this.products = products;
        this.currencyConverter = currencyConverter;
    }

    public getTotalCost(): number {
        let totalCost = 0;
        for (const product of this.products) {
            totalCost += this.currencyConverter.convert(product.price);
        }
        return totalCost;
    }
}

class CurrencyFactorConverter implements CurrencyConverter {
    private currencyFactor: number;

    constructor(currencyFactor: number) {
        this.currencyFactor = currencyFactor;
    }

    public convert(price: number): number {
        return price * this.currencyFactor;
    }
}

class DiscountConverter implements CurrencyConverter {
    private discountThreshold: number;
    private discountFactor: number;

    constructor(discountThreshold: number, discountFactor: number) {
        this.discountThreshold = discountThreshold;
        this.discountFactor = discountFactor;
    }

    public convert(price: number): number {
        if (price > this.discountThreshold) {
            return (price - this.discountThreshold) * this.discountFactor;
        } else {
            return price;
        }
    }
}
```

With this implementation, we can create a new ProductList object with an array of Product objects as its parameter, and a CurrencyConverter object that uses the new DiscountConverter class to calculate the total cost using the new currency conversion strategy:

```typescript
const products = [new Product("Product 1", 10), new Product("Product 2", 20)];
const discountConverter = new DiscountConverter(15, 0.8);
const productList = new ProductList(products, discountConverter);
console.log(productList.getTotalCost()); // expected output: 22

```

### Example 2:

Suppose we have Shape interface and different shapes implementing it. To follow OCP we can use the Factory pattern:

```typescript
interface Shape {
    draw(): void;
}

class Circle implements Shape {
    constructor(private radius: number) {}
    draw() {
        console.log(`Drawing a circle with radius ${this.radius}`);
    }
}

class Square implements Shape {
    constructor(private sideLength: number) {}
    draw() {
        console.log(`Drawing a square with side length ${this.sideLength}`);
    }
}

class Triangle implements Shape {
    constructor(private base: number, private height: number) {}
    draw() {
        console.log(`Drawing a triangle with base ${this.base} and height ${this.height}`);
    }
}

abstract class ShapeFactory {
    abstract createShape(...args: any[]): Shape;
    validate(...args: any[]): void {}
}

class CircleFactory extends ShapeFactory {
    createShape(radius: number): Shape {
        console.log(`Creating a circle with radius ${radius}`);
        return new Circle(radius);
    }
}

class SquareFactory extends ShapeFactory {
    createShape(sideLength: number): Shape {
        console.log(`Creating a square with side length ${sideLength}`);
        if (sideLength <= 0) {
            throw new Error("Side length must be positive");
        }
        return new Square(sideLength);
    }
}

class TriangleFactory extends ShapeFactory {
    createShape(base: number, height: number): Shape {
        console.log(`Creating a triangle with base ${base} and height ${height}`);
        return new Triangle(base, height);
    }
}

const circleFactory = new CircleFactory();
const squareFactory = new SquareFactory();
const triangleFactory = new TriangleFactory();

```

In this example, the SquareFactory class includes the validation logic directly in the createShape() method, which ensures that the Square object is only created with a positive side length. This approach achieves the Open/Closed Principle by allowing the Square object creation process to be extended without modifying the existing code. If you needed to add validation to other shapes, you could create a separate factory class with the validation logic for that shape, without modifying the existing CircleFactory or TriangleFactory classes.
