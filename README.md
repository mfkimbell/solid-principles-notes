# solid-design-patterns-notes

# SOLID

### Single Responsibility
NO:
<img width="351" alt="Screenshot 2024-08-20 at 10 40 20 AM" src="https://github.com/user-attachments/assets/bbe79c59-02a4-4569-825d-177e3750d19d">
YES:
<img width="352" alt="Screenshot 2024-08-20 at 10 40 03 AM" src="https://github.com/user-attachments/assets/b97159a9-f722-463a-a932-8ee2d92d49fd">

![image](https://github.com/user-attachments/assets/b7314825-60bd-44eb-a51e-1dfa0735bbbb)

another benefit is that developers can work on different services, so less merge conflicts

### Open / Closed Principle
open for extension, closed for modificatuon 
- **extension methods** in C# are a good example of implementing this principle. they add functionality without altering the original file
- **inheritance** also fits with this since subclasses and extend class functionality without altering the superclass.

### Liskov Substituion Principle

<img width="698" alt="Screenshot 2024-08-20 at 10 46 20 AM" src="https://github.com/user-attachments/assets/f9e6ff9f-0c9e-4d04-88d6-355205b091d9">




(can be the fix for Liskov SP)

<img width="422" alt="Screenshot 2024-08-20 at 10 52 07 AM" src="https://github.com/user-attachments/assets/1087b074-2da0-4274-8d4d-b27fc9de837d">

### Interface Segregation Principle
- instead of having big fat interfaces, maybe split them into smaller specific intentional interfaces, so we dont have to implement methods we wont usez. avoiding this:  fly() { return no implementation }

ex: say we have a printer class we need, if we design a Device() interface that had print(), scan(), this is bad design. now we wouod have print() for all devices. better to have an interface Printable() and Scanable() with related functions

### repository pattern
-relevant
### how it related to graphQL 

### Dependency Inversion Principle
- we dont want high level code/classes to depend on the low level code (specific implementations) of its dependencies.
- for instance, if we need to use payment processing and we currently use stripe, it would be bad to write that code directly in our class. instead, we can use a PaymentProcessor(user) class with a Pay(user) method. this way, we can more easily change the payment method (to paypal), or even accept multiple payment methods (stripe or paypal) based on the user
# DRY (don't repeat yourself)

# KISS (keep it simple stupid)

# Singleton

# Factory

# polymorphism 

### overloading

### overwriting

# abstrqction

# encapsulation

# interfaces
- help with testing code and writing unit tests
- ex: our payment service relies on a banking servive we import, we can make a
Let me break it down a bit more:


An interface is like a contract that defines what methods a class should have, but it doesn't say how those methods should work. When a class implements an interface, it agrees to follow that contract by providing specific implementations of those methods.

### Why Use Interfaces?

When you write code that depends on an interface rather than a specific class, you're creating a flexible setup where your code doesn't need to know the details of how those methods work—it just knows that the methods exist. This is key for unit testing.

### How Does This Help with Unit Testing?

1. **Substituting Real Objects with Mocks**:
   - Imagine you have a class that depends on another class to do something. If your class is directly tied to that other class (without an interface), it's difficult to test your class without also involving that other class. This can make testing hard because you're not just testing one piece of code—you’re testing everything it depends on too.
   - When you use an interface, you can "swap out" the real class with a "mock" class in your test. This mock class implements the same interface but is controlled by your test code, allowing you to simulate different scenarios without relying on the real dependencies.

   **Example**: 
   Let's say you have a `PaymentService` class that relies on a `BankService` to process payments.

   ```python
   class BankService:
       def transfer(self, amount):
           # Logic to transfer money
           pass

   class PaymentService:
       def __init__(self, bank_service: BankService):
           self.bank_service = bank_service

       def make_payment(self, amount):
           return self.bank_service.transfer(amount)
   ```

   - In a test, without interfaces, you'd have to deal with the real `BankService`, which might actually try to transfer money, something you definitely don't want during testing.
   - If `BankService` was an interface instead, you could create a mock version of `BankService` in your tests:

   ```python
   class MockBankService(BankService):
       def transfer(self, amount):
           return "Mock Transfer Success"
   ```

   Now, when you test `PaymentService`, you can use `MockBankService`, which doesn't actually move money around but lets you test how `PaymentService` works when a payment is "successful."

2. **Isolating the Code Under Test**:
   - By using interfaces, you ensure that when you test a class, you're only testing that class's logic, not the logic of its dependencies. This isolation is crucial for unit tests, which are meant to test small, self-contained pieces of functionality.

   For example, in the case above, if you test `PaymentService` with `MockBankService`, you only focus on whether `PaymentService` behaves correctly when it calls the `transfer` method, without worrying about what the real `BankService` does behind the scenes.

3. **Testing Various Scenarios Easily**:
   - Using interfaces makes it easier to create different versions of a service for testing purposes. For instance, you could have another mock that simulates a failed transfer. This way, you can test how your class handles both success and failure cases without needing to change the actual code.

   ```python
   class FailingBankService(BankService):
       def transfer(self, amount):
           raise Exception("Transfer Failed")
   ```

   Now you can test how `PaymentService` handles a failed transfer, again without touching the real `BankService`.

### Summary

Interfaces allow you to replace real dependencies with controlled test versions (mocks) during testing. This makes it easier to:
- Isolate the class you're testing from its dependencies.
- Create predictable and controlled test scenarios.
- Focus your tests on the behavior of the class under test, without worrying about external dependencies.

This approach simplifies testing, makes your tests more reliable, and ensures that you're only testing one thing at a time.

