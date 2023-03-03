## Information Expert

nformation Expert is a pattern that suggests that an object that has the most information about a task or responsibility should be responsible for carrying out that task.

This pattern helps to promote the principle of encapsulation by minimizing the amount of information that needs to be passed between objects.

In other words, an object should be responsible for performing tasks that require access to its own information or information that it can obtain directly. This approach can lead to a more modular and maintainable design, as each object is responsible for its own behavior, and changes to one object do not necessarily require changes to other objects.

## Important notes

If an object delegates a task to another object that it could perform itself, we are not following the Information Expert pattern.

If an object has too much responsibility and performs tasks that are not related to its main responsibility, we are not following the Information Expert pattern.

If an object does not have enough information to perform a task, but another object does, we are not following the Information Expert pattern if we assign the task to the object without enough information.

## Example

For example, let's say we have a system for managing employee data. Suppose we have an Employee object that is responsible for managing an employee's personal details, such as name, address, and contact information. If we assign the task of calculating an employee's tax liability to the Employee object, we are violating the Information Expert pattern because the Employee object does not have the necessary information to perform this task. Instead, we should create a separate TaxCalculator object that has access to the necessary tax rate information and is responsible for calculating the employee's tax liability.
