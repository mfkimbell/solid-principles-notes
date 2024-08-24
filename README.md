# solid-design-patterns-notes

# SOLID

### Single Responsibility
* a class or module should have only one reason to change, meaning it should have only one responsibility or job
* intended to reduce side effects
* 
NO:
<img width="351" alt="Screenshot 2024-08-20 at 10 40 20 AM" src="https://github.com/user-attachments/assets/bbe79c59-02a4-4569-825d-177e3750d19d">
YES:
<img width="352" alt="Screenshot 2024-08-20 at 10 40 03 AM" src="https://github.com/user-attachments/assets/b97159a9-f722-463a-a932-8ee2d92d49fd">


* services are a good example of splitting up logic to into modular functionality
![image](https://github.com/user-attachments/assets/b7314825-60bd-44eb-a51e-1dfa0735bbbb)
* another added benefit is that developers can work on different services, so less merge conflicts

### Open / Closed Principle
open for extension, closed for modificatuon 
- **extension methods** in C# are a good example of implementing this principle. they add functionality without altering the original file
- **inheritance** also fits with this since subclasses and extend class functionality without altering the superclass.

### Liskov Substituion Principle

<img width="698" alt="Screenshot 2024-08-20 at 10 46 20 AM" src="https://github.com/user-attachments/assets/f9e6ff9f-0c9e-4d04-88d6-355205b091d9">
-because an electric car can't shift, all subclasses must implement ALL methods


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
- dependency injection is a way of implementing this principle
- dependency injection allows us to alter different dependencies in a modular way without changing the code that uses those dependencies

- Focus your tests on the behavior of the class under test, without worrying about external dependencies.

This approach simplifies testing, makes your tests more reliable, and ensures that you're only testing one thing at a time.

# class types, static, protecged etc...
