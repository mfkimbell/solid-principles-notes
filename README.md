# solid-design-patterns-notes

# SOLID

### Single Responsibility
* a class or module should have only one reason to change, meaning it should have only one responsibility or job
* intended to reduce side effects
* the logic for certain parts of the car need to be encapsulated in a class, such as an Engine() class and a Tire() class.
  
NO:

<img width="351" alt="Screenshot 2024-08-20 at 10 40 20 AM" src="https://github.com/user-attachments/assets/bbe79c59-02a4-4569-825d-177e3750d19d">

<img width="638" alt="Screenshot 2025-02-20 at 7 10 57 PM" src="https://github.com/user-attachments/assets/e0d43c21-e0c3-4fdc-a3c2-0323dfa56ed6" />

YES:

<img width="352" alt="Screenshot 2024-08-20 at 10 40 03 AM" src="https://github.com/user-attachments/assets/b97159a9-f722-463a-a932-8ee2d92d49fd">

<img width="947" alt="Screenshot 2025-02-20 at 7 11 42 PM" src="https://github.com/user-attachments/assets/421e1dae-df10-429e-9099-f1120c873cfe" />

#### This does NOT MEAN you class should do only one thing, it means a class's functionality should be VERY closely related. We are preventing "class bloat" and making code more reusable

* i would need to import the car and tire class if the files were separate. 

* services are a good example of splitting up logic to into modular functionality
  
![image](https://github.com/user-attachments/assets/b7314825-60bd-44eb-a51e-1dfa0735bbbb)

* another added benefit is that developers can work on different services, so less merge conflicts

### Open / Closed Principle
open for extension, closed for modificatuon 
- **extension methods** in C# are a good example of implementing this principle. they add functionality without altering the original file
- extension method are a good example of code i've seen used in my experience at regions, we might have a base class for logging, but we add extension methods to tailor it to the individual class or project to add extra functionality
- **inheritance** also fits with this since subclasses and extend class functionality without altering the superclass.

* If we have unit tests that we have already written for methods, adding more functionality will require us to change those unit tests, BAD

#### I USED THE DECORATOR PATTERN IN MY SAAS DEVOPS PROJECT: added functionality without changing the original code

#### Another example my "GetTransactions()" at work, i pass in a list of filters, and we apply all filters in that list, so a certain client can call transactions and get filtered based on their specifications, and FUTURE filters can be added by passing in the data

* We also use some extension methods to do csv vs pdf generation i believe

* you can also use inheritance

### Liskov Substituion Principle
* objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.
* **"a child class should be able to do anything a parent class can"**
<img width="698" alt="Screenshot 2024-08-20 at 10 46 20 AM" src="https://github.com/user-attachments/assets/f9e6ff9f-0c9e-4d04-88d6-355205b091d9">

* because an electric car can't shift, therefore we can't use an electric car in place of all vehicle instances
* Derived classes should extend the functionality of base classes without changing their behavior.

<img width="422" alt="Screenshot 2024-08-20 at 10 52 07 AM" src="https://github.com/user-attachments/assets/1087b074-2da0-4274-8d4d-b27fc9de837d">

* this is an example of implementing Liskov SP)
* following this principle can make your code less error prone and more modular, I can put all kinds of cars in a place where "vehicle" can exist and my code won't break (unlike before with the electric car's introduciton)

### Interface Segregation Principle

Here is an example with the repository pattern

<img width="816" alt="Screenshot 2025-02-20 at 7 37 55 PM" src="https://github.com/user-attachments/assets/7675d712-c292-4e85-ac83-6360614d350a" />

You can split it into a reader, writer, and remover

<img width="921" alt="Screenshot 2025-02-20 at 7 39 09 PM" src="https://github.com/user-attachments/assets/2abb5e88-a2ee-4a25-9f48-d9eaa885b0ba" />

if you need all, you can have a MAIN interface that inherits from all the others

* clients/classes should not be forced to depend on interfaces they do not use.
* Therefore: Interfaces should be small, specific, and focused on what a client actually needs.
* it follows the idea that interfaces should be designed FOR classes/clients
* It encourages creating smaller, more specific interfaces that are focused on a particular responsibility.
* instead of having big fat interfaces, maybe split them into smaller specific intentional interfaces, so we dont have to implement methods we wont usez.
* for instance, say we have an interface person() and it has work() sleep() eat(), this won't work for babies, since they can't work, so maybe an interface called Alive() with eat() and sleep() and then Workable() with work(). Originally we would have had to put a `return NoImplementation() erroor` for the baby's Work() function. 

BAD

```python
class Machine:
    def print_document(self, doc): pass
    def scan_document(self, doc): pass
    def fax_document(self, doc): pass
```

```python
class OldPrinter(Machine):
    def print_document(self, doc):
        print(f"Printing {doc}")

    def scan_document(self, doc):
        raise NotImplementedError("This printer can't scan.")

    def fax_document(self, doc):
        raise NotImplementedError("This printer can't fax.")
```

GOOD

```python
class Printer:
    def print_document(self, doc): pass

class Scanner:
    def scan_document(self, doc): pass

class Fax:
    def fax_document(self, doc): pass
```

```python
class SimplePrinter(Printer):
    def print_document(self, doc):
        print(f"Printing {doc}... Done.")

class OfficeScanner(Scanner):
    def scan_document(self, doc):
        print(f"Scanning {doc}... Done.")

class MultiFunctionMachine(Printer, Scanner, Fax):
    def __init__(self, printer: Printer, scanner: Scanner, fax: Fax):
        self.printer = printer
        self.scanner = scanner
        self.fax = fax

    def print_document(self, doc):
        self.printer.print_document(doc)

    def scan_document(self, doc):
        self.scanner.scan_document(doc)

    def fax_document(self, doc):
        self.fax.fax_document(doc)

```

### Dependency Inversion Principle
* **High-level modules should not depend on low-level modules: Both should depend on abstractions (interfaces or abstract classes).**
* Abstractions should not depend on details: Details (concrete implementations) should depend on abstractions.
- we dont want high level code/classes to depend on the low level code (specific implementations) of its dependencies.
- for instance, if we need to use payment processing and we currently use stripe, it would be bad to write that code directly in our class. instead, we can use a PaymentProcessor(user) class with a Pay(user) method. this way, we can more easily change the payment method (to paypal), or even accept multiple payment methods (stripe or paypal) based on the user
- dependency injection is a way of implementing this principle
- dependency injection allows us to alter different dependencies in a modular way without changing the code that uses those dependencies

- Focus your tests on the behavior of the class under test, without worrying about external dependencies.

### one exmaple
another good example is the use of the Ilogger interface in C#, and we have a class logger.cs that implements the logic, and we inject that into the constructor of our controllers. now we can change logging logic without alerting the individual files

```C#
// High-level module that depends on ILogger abstraction
public class PaymentProcessor
{
    private readonly ILogger _logger;

    // Constructor injection of ILogger
    public PaymentProcessor(ILogger logger)
    {
        _logger = logger;
    }
```

### Second example
Let's consider your example of a PaymentProcessor class:

Without DIP and Dependency Injection:
``` java
// High-level class directly depends on a specific low-level implementation (Stripe).
public class PaymentProcessor {
    public void processPayment(User user, double amount) {
        // Directly using Stripe payment API
        StripePaymentProcessor stripeProcessor = new StripePaymentProcessor();
        stripeProcessor.charge(user, amount);
    }
}
```
In this non-compliant example:

Problem: PaymentProcessor directly depends on StripePaymentProcessor, making it tightly coupled to the Stripe payment system.
Issues:
Changing to another payment system (e.g., PayPal) requires modifying the PaymentProcessor class, violating the Open-Closed Principle.
Testing PaymentProcessor becomes challenging because it's tightly coupled to Stripe's implementation.
Applying DIP with Dependency Injection:
java
```java
// High-level class depends on an abstraction (PaymentGateway) instead of a specific implementation.
public class PaymentProcessor {
    private PaymentGateway paymentGateway;

    // Constructor injection of PaymentGateway abstraction.
    public PaymentProcessor(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public void processPayment(User user, double amount) {
        // Using the abstraction (PaymentGateway)
        paymentGateway.charge(user, amount);
    }
}

// Abstraction (interface) representing a payment gateway.
public interface PaymentGateway {
    void charge(User user, double amount);
}

// Concrete implementation for Stripe payment processing.
public class StripePaymentProcessor implements PaymentGateway {
    @Override
    public void charge(User user, double amount) {
        // Stripe specific implementation
        // Stripe API calls
    }
}

// Another concrete implementation for PayPal payment processing.
public class PayPalPaymentProcessor implements PaymentGateway {
    @Override
    public void charge(User user, double amount) {
        // PayPal specific implementation
        // PayPal API calls
    }
}
```
In this compliant example:

Solution: PaymentProcessor depends on the PaymentGateway abstraction (interface), allowing it to work with any implementation of PaymentGateway (Stripe, PayPal, etc.) without modifying PaymentProcessor itself.

```java
public class PaymentProcessor {
    private PaymentGateway stripeGateway;
    private PaymentGateway paypalGateway;

    // Constructor injection of the available gateways
    public PaymentProcessor(PaymentGateway stripeGateway, PaymentGateway paypalGateway) {
        this.stripeGateway = stripeGateway;
        this.paypalGateway = paypalGateway;
    }

    public void processPayment(User user, double amount) {
        PaymentGateway selectedGateway = selectGatewayForUser(user);
        selectedGateway.charge(user, amount);
    }

    private PaymentGateway selectGatewayForUser(User user) {
        // Determine which gateway to use based on the user's preference
        switch (user.getPreferredPaymentType()) {
            case "Stripe":
                return stripeGateway;
            case "PayPal":
                return paypalGateway;
            default:
                throw new IllegalArgumentException("Unsupported payment type: " + user.getPreferredPaymentType());
        }
    }
}
```
