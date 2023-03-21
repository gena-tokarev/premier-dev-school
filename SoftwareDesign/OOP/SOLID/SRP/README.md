# SRP - Single responsibility principle

Software entities (classes, functions) should have only one reason to change. They should be responsible only for one thing, and it shouldn't have multiple responsibilities that could cause it to change for different reasons.

## If you don't follow:
- **Difficult to understand and maintain.** If you mix multiple things together you obviously make code more difficult to understand and maintain.
- **Unintended changes.** If you were to change the code for one responsibility you might unintentionally change the other. It might cause bugs.
- **Difficulty in reusing code and code duplication.** If you tie together more than one responsibility and want to use one of it, it might be challenging because you'll have to refactor the entity to separate this responsibility from it. In the end you may just duplicate this responsibility code when you need to use it.
- **Difficulty in testing.** When a class has multiple responsibilities, it can be difficult to test each responsibility separately.

## Examples in real life:
- A car that can also sail. If you want to drive only, you'll have to carry the boat functionality around with you despite you don't need it. In this case you better make one car and one boat.
- Chef who does micromanagement instead of delegating things to his subordinates.
