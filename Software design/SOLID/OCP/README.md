# OCP - Open/Closed principle

Software entities should be open for extension but closed for modification. This means that you should be able to extend the behavior of a software entity without modifying its source code

## Examples in real life:
- In the automotive industry, car manufacturers use modular design to allow for different configurations and features without having to redesign the entire car. For example, a car manufacturer may use the same chassis and body design for multiple models, but with different engines, transmissions, and interior options. This allows for greater flexibility and cost-effectiveness, without having to completely redesign the car each time a new feature is added.

## Comments:
It correlates with other design principles (for example SRP).

If you follow SRP then you most likely to achieve OCP, since if your class is responsible for one thing only, you easily can apply OCP using some structural patterns such as Factory without changing the class code.

## Examples
