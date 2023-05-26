---
description: Design Patterns with Java Examples.
---

# Design Patterns

#### Interface Driven Patterns

**Strategy Pattern / Bridge Pattern**

This pattern is one of the incarnation of Dependency Injection.

```
class StrategyContext {
    private Strategy strategy;
    public StrategyContext(Strategy strategy) {
        this.strategy = strategy;
    }

    public boolean run(String[] args) {
        return this.strategy.run(args)
    }
}
```

Strategy can be either interface or abstract class. This pattern helps to hide the implementation details from the method callers, and it's just a simple illustration of interface-oriented programming.

Note StrategyContext doesn't implement Strategy interface.

**Proxy Pattern**

```
class StrategyProxy implements Strategy {
    private Strategy strategy;
    public StrategyProxy() {
        this.strategy = new ConcreteStrategy();
    }

    @Override
    public boolean run(String[] args) {
        return this.strategy.run(args);
    }
}
```

This pattern is quite similar to the Strategy Pattern except that here the strategy details are hidden from the proxy dependents, however in Strategy Pattern the concrete strategy is injected through the constructor.

Note both StrategyProxy and ConcreteStrategy have implement the Strategy interface.

**Decorator Pattern**

```
class ConcreteComponent implements Component {
    @Override
    public void run() {
        System.out.println("running");
    }
}

class Decorator implements Component {
    private Component component = null;
    public void setComponent(Component component) {
        this.component = component;
    }
    @Override
    public void run() {
        System.out.println("before running");
        this.component.run();
        System.out.println("after running");
    }
}
```

This pattern helps to add more functionalities without changing the original target instance.

**Adaptor Pattern**

The implementation of Adaptor Pattern is the same as that of Proxy Pattern while with different goal.

```
public interface Worker {
    void run();
}
Class BeggerAdaptor implements Worker {
    private Begger begger;
    public BeggerAdaptor() {
        this.begger = new Begger();
    }
    @Override
    public void run() {
        this.begger.beg();
    }
}
```

Note only BeggerAdaptor has implemented the Worker interface.

#### Factory Patterns

**Singleton Pattern**

There are multiple implementation methods for Singleton Pattern in Java as listed here - [Java Singleton Design Pattern Best Practices with Examples](https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples). My favorite one is Bill Pugh Singleton Implementation, because it's concise and thread-safe.

```
public class BillPughSingleton {
    private BillPughSingleton(){}
    private static class SingletonHelper{
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }
    public static BillPughSingleton getInstance(){
        return SingletonHelper.INSTANCE;
    }
}
```

**Simple Factory Pattern**

```
public class OperationFactory {
    public static Operation createOperation(String operation) {
        switch (operation) {
            case "+":
                return new OperationAdd();
            case "-":
                return new OperationMinus();    
            default:
                return null;  
        }
    }
}
```

OperationAdd and OperationMinus classes implement Operation interface.

Note that from my perspective of view **Factory Pattern** and **Abstract Factory** Pattern don't provide much value to simplify the program or make the logic clearer, so that I would prefer not to introduce them here.

**Flyweight Pattern**

This pattern is to reduce the cost of resource on recreating instances, one common example is Java String.

```
public class FlyWeight {
    private String name;
    public FlyWeight(String name) {
        this.name = name;
    }
}
class FlyWeightFactory {
    private HashMap<String, FlyWeight> flyweights = new HashMap<>();

    public FlyWeight getFlyWeight(String name) {
        if (!flyweights.keySet().contains(name)) {
            flyweights.add(new FlyWeight(name));
        }
        return flyweight.get(name);
    }
}
```

In Java the strings with the same value are actually refering to the same underlying object. Although the resource used by creating a string is not substantial, because the number of operations is large, it still impact tremendously on the program overall.

Note that in Simple Factory Pattern a new object is always returned, however in FlyWeight Pattern, only when there is no such instance will a new object be created and returned.

**Builder Pattern**

```
Class ContextBuilder {
    String color;
    Integer size;
    String metadata;
    public ContextBuilder() {
        this.color = "red";
        this.size = 2;
        this.metadata = null;
    }

    public setColor(String color) {
        this.color = color;
        return this;
    }

    public setSize(Integer size) {
        this.size = size;
        return this;
    }

    public setMetadata(String metadata) {
        this.metadata = metadata;
        return this;
    }

    public Context build() {
        return new Context(this.color, this.size, this.metadata);
    }
}

public static void main(string[] args) {
    ContextBuilder contextBuilder = new ContextBuilder();
    Context context = contextBuilder
      .setColor("blue")
      .setSize(1)
      .setMetadata("builder pattern")
      .build();
}
```

Here we set the options for the builder and finally it will create the instance based on the requirement.

**Prototype Pattern**

It's just to implement the Cloneable interface in java. Note that if you want to create a immutable class, you should follow [How to create Immutable class in Java?](https://www.geeksforgeeks.org/create-immutable-class-java/) and use deep copy instead of shallow copy.

#### Changing Context

**Chain of Responsibility Pattern**

```
public interface Handler {
    void setSuccessor(Handler hanlder);
    void handle(int i);
}

class AtaskHandler implements Handler {
    private Handler successor = null;
    @Override
    public void setSuccessor(Handler handler) {
        this.successor = handler;
    }
    @Override
    public void handle(int i) {
        if (this.successor) {
            this.successor.handle(i + 1);
        }
    }
}
```

And there is another more common pattern which is widely used in servlet,

```
public interface Handler {
    void handle(int i);
}
class Processor implements Handler {
    private List<Handler> handlers = new ArrayList<>();
    public void register(Handler handler) {
        handlers.add(handler);
    }

    @Override
    public void handle(int i) {
        for (int j = 0; j < this.handlers.size(); j++) {
            h.handler(i + j);
        }
    }
}
```

In this way the handlers don't have to the successor of itself.

**States Pattern**

Please check this awesome article about [Deterministic Finite Automation (DFA)](https://leetcode.com/problems/string-to-integer-atoi/solution/) from Leetcode.

```
public interface State {
    void handle(Context context);
}
class StateStarted implements State {
    @Override
    public void handle(Context context) {
        // business logic here
        context.state = new StateB();
    }
}
class StateB implements State {
    @Override
    public void handle(Context context) {
        // business logic here
        context.state = new StateFinished();
    }
}
class StateFinished implements State {
    @Override
    public void handle(Context context) {
        // business logic here
        System.out.println("finished");
    }
}
class Context {
    private State state; 
    public Context() {
        this.state = new StateStarted();
    }

    public State getState() {
        return this.state;
    }

    public void request() {
        this.state().handle(this); // go to next state
    }
}

public static void main(String[] args) {
    Context context = new Context();
    context.request();
    context.request();
    context.request();

}
```

In this way the business logic for different states is implemented in the related State, and the correlation between different business units is reduced.

other implementations：

* [state patern](https://refactoring.guru/design-patterns/state/java/example)
* redux

Example for the 3 implementation of State Pattern - [Example](https://github.com/damonYuan/Redux).

**Interpreter Pattern**

```
class Context {
    private String value;
    public Context(String v) {
        this.value = v;
    }
}
public interface Expression {
    void interpret(Context context);
}
```

Here the concrete `Expression` classes will interpret the context accordingly. This pattern can be used in syntax tree (I don't quite understand this, anyone can give an illustration?).

**Visitor Pattern**

```
public interface Visitor {
    void visit(Element e);
}
public interface Element {
    void accept(Visitor v);
}
public class ConcreteVisitor implements Visitor {
    @Override
    public void visit(Element e) {
        // TODO:
    }
}
public class ConcreteElement implements Element {
    @Override
    public void accept(Visitor v) {
        v.visit(this);
    }
}
class Entirety implements Element {
    private List<Element> elements = new ArrayList<>();

    public void add(Element e) {
        elements.add(e);
    }

    public void remove(Element e) {
        elements.remove(e);
    }

    @Override
    public void accept(Visitor v) {
        for (Element e : this.elements) {
            e.accept(v);
        }
    }
}
public static void main(String[] args) {
    Visitor v = new ConcreteVisitor();
    Entirety e = new Entirety();
    e.add(new ConcreteElement());
    e.accept(v);
}
```

This pattern encapsulates the elements inside an `Entirety` object and allows the `Visitor` instances to visit it's internal elements.

#### Others

**Template Pattern**

```
abstract class AbstractClass {
    public void templateMethod {
        System.out.println("I am the stupid template...");
    }
}
# or use interface in Java 8 or above
public interface Template {
    default void templateMethod() {
        System.out.println("I am the stupid template...");
    }
}
```

The target for this pattern is DRY - Don't Repeat Yourself.

**Facade Pattern**

```
class Facade {
    SubSystemOne one;
    SubSystemTwo two;
    Validation validation;

    public Facade () {
        this.one = new SubSystemOne();
        this.two = new SubSystemTwo();
        this.three = new Validation();
    }

    public void taskOne() {
        this.one.run();
        this.two.run();
    }

    public void taskTwo() {
        this.validation.run();
        this.two.run();
    }
}
```

This pattern is to reduce the complexity of the interface that an external dependent needs to know.

**Observer Pattern**

```
class Subject {
    private ArrayList<Listener>() listeners = new ArrayList();
    public void addListener(Listener listener) {
        this.listeners.add(listener);
    }
    public void removeListener(Listener listener) {
        this.listeners.remove(listener);
    }
    private void run() {
        for (Listener l : this.listeners) {
            l.notify("to run");
        }
        System.out.println("run");
    }
}
public interface Listener {
    void notify(String event);
}
```

**Memo Pattern**

This pattern is quite common in frontend application when preparing the data to be submitted to the backend.

```
class Data {
    private String username;
    private int amount;
    public Data(String username, int amount) {
        this.username = username;
        this.amount = amount;
    }
    public String getUsername() {
        return this.username;
    }
    public setUsername(String username) {
        this.username = username;
    }
    public int getAmount() {
        return this.amount;
    }
    public setAmount(int amount) {
        this.amount = amount;
    }
    public Data createMemo() {
        return new Data(this.username, this.amount);
    }
}

public static void main(String[] args) {
    Data data = new Data("Alice", 15);
    Data memo = data.createMemo();
    data.setUsername("Bob");
    if (submit(data) == false) {
        // revert the data to the original state
        data.setUsername(memo.getUsername());
        // or we can just replace the object with the memo which 
        // depends on the context
    }
}
```

**Composite Pattern**

Composite Pattern combines objects into a tree structure to represent the "part-whole" hierarchy.

```
public interface Component {
    void add(Component c);
    void remove(Component c);
    void display(int depth);
}

class Leaf implements Component {
    private String name;

    public Leaf(String name) {
        this.name = name;
    }

    @Override
    public void add(Component c) {
        // do nothing since it's a leaf node
    }

    @Override
    public void remove(Component c) {
        // do nothing since it's a leaf node
    }

    @Override
    public void display(int depth) {
        System.out.println(new String(new char[depth]).replace("\0", "-") + " " + name);
    }
}

class Composite implements Component {
    private List<Component> list = new ArrayList<>();
    private String name;

    public Composite(String name) {
        this.name = name;
    }

    @Override
    public void add(Component c) {
        list.add(c);
    }

    @Override
    public void remove(Component c) {
        list.remove(c);
    }

    @Override
    public void display(int depth) {
        System.out.println(new String(new char[depth]).replace("\0", "-") + " " + name);
        for (Component c : list) {
            c.display(depth);
        }
    }
}

public static void main(String[] args) {
    Composite root = new Composite("root");
    root.add(new Leaf("Left"));
    root.add(new Leaf("Right"));

    root.display(1);
}

```

Actually it's a preorder tree traversal which can be implemented as

```
class Node {
    private List<Node> leaves = new ArrayList<>();
    private String name;

    public Node(String name) {
        this.name = name;
    }

    public void add(Node node) {
        this.leaves.add(node);
    }

    public void remove(Node node) {
        this.leaves.remove(node);
    }

    public void visit(int depth) {
        System.out.println(new String(new char[depth]).replace("\0", "-") + " " + name);
        for (Node n : this.leaves) {
            n.visit(depth + 1);
        }
    }
}
```

**Iterator Pattern**

In Java there are `Iterator<T>` interface and `Iterable<T>` interface. The `Iterable<T>` requires an implementation of `Iterator<T> iterator();` to return an `Iterator<T>` object. The `Iterator<T>` requires the implementation of `boolean hasNext();` and `T next();` methods.

```
class Ledger implements Iterable<String> {
    private String[] records = new String[] { "1st record", "2nd record", "3rd record" };

    @Override
    public Iterator<String> iterator() {
        return new RecordIterator(0);
    }

    private class RecordIterator implements Iterator<String> {
        private int pos;

        RecordIterator(int pos) {
            this.pos = pos;
        }

        @Override
        public boolean hasNext() {
            return this.pos < records.length - 1;
        }

        @Override
        public String next() {
            // thread-unsafe here
            return records[++this.pos];
        }
    }
}

public static void main(String[] args) {
    Ledger ledger = new Ledger();
    Iterator<String> iterator = ledger.iterator();
    while(iterator.hasNext()) {
        String record = iterator.next();
        System.out.println(record);
    }
}
```

**Mediator Pattern**

```
public interface Participant {
    void send(String msg);
    void receive(String msg);
}

class Mediator {
    private Participant[] participants;

    public Mediator(Participant a, Participant b) {
        this.participants = new Participant[] { a, b };
    }

    public void send(String msg, Participant sender) {
        if (sender == this.participants[0]) {
            this.participants[1].recieve(msg);
        } else {
            this.participants[0].recieve(msg);
        }
    }
}

class Person implements Participant {
    private Mediator mediator;
    public Person(Mediator mediator) {
        this.mediator = mediator;
    }
    @Override
    public void send(String msg) {
        this.mediator.send(msg, this);
    }

    @Override
    public void receive(String msg) {
        System.out.println(msg);
    }
}

```

Here the implementation is one to one, obviously the business case might be many-to-many, and then the logic wrapped inside the mediator would be more complicated.

#### Principles

**SRP**

Single Responsibility Principle.

**Open-Close Principle**

* Open to extension
* Closed to modification

**Dependency Inversion Principle**

Interface-oriented programming.

**LoD Principle**

The Law of Demeter (LoD) or principle of least knowledge is a design guideline for developing software, particularly object-oriented programs.
