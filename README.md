# design-patterns
Experiments with design patterns


## Behaviroal patterns
Core Behavioral Patterns
1. Strategy Pattern
Purpose: Define a family of algorithms, encapsulate each one, and make them interchangeable

Senior Perspective:

Use with dependency injection frameworks (Spring, Guice)

Enables runtime algorithm selection

Great for replacing complex conditional logic

```java
public interface PaymentStrategy {
    void pay(double amount);
}

public class CreditCardPayment implements PaymentStrategy { /* ... */ }
public class PayPalPayment implements PaymentStrategy { /* ... */ }

public class PaymentService {
    private PaymentStrategy strategy;
    
    public void setStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void executePayment(double amount) {
        strategy.pay(amount);
    }
}
```
2. Observer Pattern
Purpose: Define a one-to-many dependency between objects

Senior Perspective:

Modern implementations using ```'s built-in Observable and Observer

Better alternatives with reactive programming (Rx```, Project Reactor)

Event-driven architectures

```java
public class Order {
    private List<OrderObserver> observers = new ArrayList<>();
    
    public void addObserver(OrderObserver observer) {
        observers.add(observer);
    }
    
    public void complete() {
        observers.forEach(o -> o.onOrderCompleted(this));
    }
}
```
3. Command Pattern
Purpose: Encapsulate a request as an object

Senior Perspective:

Foundation for undo/redo operations

Useful in distributed systems (command-query separation)

Microservices command pattern implementation

```java
public interface Command {
    void execute();
    void undo();
}

public class CommandProcessor {
    private Stack<Command> history = new Stack<>();
    
    public void executeCommand(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }
    
    public void undoLastCommand() {
        if (!history.isEmpty()) {
            history.pop().undo();
        }
    }
}
```
4. Template Method
Purpose: Define the skeleton of an algorithm in a method

Senior Perspective:

Avoid duplication in framework code

Hook methods for customization

Contrast with Strategy pattern (inheritance vs composition)

```java
public abstract class ReportGenerator {
    // Template method
    public final Report generateReport() {
        Data data = fetchData();
        ProcessedData processed = processData(data);
        return formatReport(processed);
    }
    
    protected abstract Data fetchData();
    protected abstract ProcessedData processData(Data data);
    protected abstract Report formatReport(ProcessedData data);
}
```
5. Chain of Responsibility
Purpose: Pass requests along a chain of handlers

Senior Perspective:

Used in Servlet filters

Spring Security filter chain

Middleware patterns

```java
public interface Handler {
    void setNext(Handler handler);
    void handle(Request request);
}

public abstract class AbstractHandler implements Handler {
    private Handler next;
    
    public void setNext(Handler handler) {
        this.next = handler;
    }
    
    protected void passToNext(Request request) {
        if (next != null) {
            next.handle(request);
        }
    }
}
```
Advanced Behavioral Patterns
6. Visitor Pattern
Purpose: Separate algorithm from object structure

Senior Perspective:

Double dispatch technique

Useful for complex object structures (ASTs, document models)

Compiler design applications

```java
public interface Visitor {
    void visit(ElementA element);
    void visit(ElementB element);
}

public interface Element {
    void accept(Visitor visitor);
}

public class ElementA implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```
7. State Pattern
Purpose: Allow an object to alter its behavior when its internal state changes

Senior Perspective:

Alternative to large state machines

Finite state machine implementations

Thread state management

```java
public interface State {
    void handle(Context context);
}

public class Context {
    private State state;
    
    public void setState(State state) {
        this.state = state;
    }
    
    public void request() {
        state.handle(this);
    }
}
```

8. Mediator Pattern
Purpose: Define an object that encapsulates how objects interact

Senior Perspective:

Reduces coupling between components

Centralized control logic

Message brokers as mediators

```java
public interface Mediator {
    void notify(Component sender, String event);
}

public class ConcreteMediator implements Mediator {
    private ComponentA componentA;
    private ComponentB componentB;
    
    public void notify(Component sender, String event) {
        if (sender == componentA && event.equals("click")) {
            componentB.doSomething();
        }
    }
}
```
