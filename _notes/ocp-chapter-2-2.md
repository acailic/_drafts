---
title: OCP Review 2.2 - Functional Programming intro
layout: post
tags: [java, ocp, functional, design]
date: 2018-09-07
---

# Functional Programming

- no side effects
- data in, data out
- no mutability (no state)
- state is bad
- shared mutable state is the root of all evil

---

## Functional Interface

- an interface with just one method

```java
interface Add {
    int add(int n1, int n2);
}
```

---

## Functional Interface

- we can add the annotation `@FunctionalInterface`

```java
@FunctionalInterface
interface Add {
    int add(int n1, int n2);
}

@FunctionalInterface
interface Substract {
    int substract(int n1, int n2);
}
```

---

## Just one method, please

```java
@FunctionalInterface
interface Add {
    int add(int n1, int n2);
    void m1();  // wrong: two methods
}

@FunctionalInterface
interface Add {
    int add(int n1, int n2);
    static void m1();  // right: just one method
}
```

---

## No Sh*t, sherlock, just one method

```java
@FunctionalInterface
interface Add {
    int add(int n1, int n2);
}

@FunctionalInterface
interface Substract {
    int substract(int n1, int n2);
}

@FunctionalInterface            // ERROR
interface Operation extends Add, Substract {
    // add & substract are copied here
}
```

---

## Lambda expressions

- a way to _consume_ functional interfaces
- a way to _implement_ those interfaces

```java
(String instance) -> { return instance.legth(); }

// Lambda expression for a Functional Interface 
// that declares a method taking one input param of type String, and returns an int


// shorthand
s -> s.length;
```

---

## Lambda expressions

- can omit () if no type present and just one argument
- can omit `return` and `;`

---

## Example: calculator

- a calculator can +, -, *, /, %, ...
- restricted (for now) to `int` type
- we can use an enum for operations and a Switch:

```java
class OldCalculator {
    enum OldOperations {
        ADD, SUBSTRACT, MULTIPLY, DIVIDE
    }

    int calculate(int n1, int n2, OldOperations operation) {
        int result = 0;
        switch (operation) {
            case ADD:
                result = n1 + n2; break;
            case SUBSTRACT:
                result = n1 - n2; break;
            case MULTIPLY:
                result = n1 * n2; break;
            case DIVIDE:
                result = n1 / n2; break;
        }
        return result;
    }
}

OldCalculator oldCalculator = new OldCalculator();
int r = oldCalculator.calculate(1,2, OldCalculator.OldOperations.ADD);
System.out.println("r: " + r);
```

---

## Example: calculator

- problem: we want to add another operation
    - change the `enum`
    - touch the `switch`
    - recompile
    - if code is copied in other projects...


---

## Example: calculator

- Functional solution: all these are arithmetic operations between two numbers. Let's say that:

```java
@FunctionalInterface
interface Operation  {
    int operate(int n1, int n2);
}
```

- and create a `Calculator` class with a method that takes two numbers and an `Operation`
- as all `Operation` references have `calculate` we can call it

```java
class Calculator {
    int calculate(int n1, int n2, Operation op) {
        return op.operate(n1, n2);
    }
}
```

---

## Example: calculator

- using it: we're passing two numbers and _what we want to do with these two numbers_ (the code block)

```java
Calculator c = new Calculator();
int i = c.calculate(1, 2, (n1, n2) -> n1 * n2);
    i = c.calculate(2, 2, (int n1, int n2) -> n1 * n2);
    i = c.calculate(2, 2, (int n1, int n2) -> n1 * n2);
    i = c.calculate(2, 2, (int n1, int n2) -> { return n1 * n2; });

```

---

## Example: calculator

- Lambda expressions can be passed around, or stored...

```java
final Operation multiply = (n1, n2) -> n1 * n2;
```

so we can do:

```java
class Calculator {
    static final Operation multiply = (n1, n2) -> n1 * n2;
    static final Operation add = (n1, n2) -> n1 + n2;
    static final Operation substract = (n1, n2) -> n1 - n2;
    static final Operation divide = (n1, n2) -> n1 / n2;

    int calculate(int n1, int n2, Operation op) {
        return op.operate(n1, n2);
    }
}

// using it

i = c.calculate(40, 40, Calculator.divide);
```

---


## Example: calculator

- we can return a Lambda expression from a method

```java
Operation sum() {
    return ((n1, n2) -> n1 + n2);
}
```

---

## Example: calculator

- even return a random operation each time

```java
Operation randomOperation() {
    Operation[] operations = {
            multiply, add, substract, divide
    };

    Random r = new Random();
    return operations[abs(r.nextInt()) % operations.length];
}

```

---

## Write correct Functional Interfaces for the following Lambda Expressions

```java
() -> 10;
(a) -> a.size();
```

---

## Answers

```java
L1 l1 = () -> 10;

interface L1 {
    int m();
}

L3 l3 = a -> a.size();

@FunctionalInterface
interface L3 {
    int m(Collection c);
}
```

---

## Write correct Functional Interfaces for the following Lambda Expressions

```java
(a, b, c) -> a.size() + b.length() + c.length;
```

---

## 

```java
interface L4 {
    int m(Collection a, String b, Object[] c);
}
```
---

## Write correct Functional Interfaces for the following Lambda Expressions

```java
() -> () -> 10;
```

---


```java

L2 = () -> () -> 10;

interface L2 {
    L1 m();
}

```

---

## Useful lambdas

- Without lambdas...

```java
Runnable task1 = new Runnable() {
	@Override
	public void run() {
		System.out.println("Something!");
	}
};
task1.run();

```

- With Lambdas...

```java
Runnable task2 = () -> { System.out.println("Yeah!"); };
task2.run();
```


---

## Useful lambdas: repeating codeblocks

```java
interface CodeBlock {
    void execute();
}

CodeBlock cb = () -> {
    System.out.println("Hello");
};

cb.execute();
cb.execute();

```

---

## Useful lambdas: forEach

```java
List<String> streets = Arrays.asList("Torricelli", "Sierpes", "Constitución");

for (String street : streets) {
    System.out.println(street);
}

streets.forEach((String x) -> {System.out.println(x);});
streets.forEach((String x) -> System.out.println(x));
streets.forEach(x -> System.out.println(x));
streets.forEach(System.out::println);

```

---

## Lambdas capture the semantic scope

```java
int i = 4;

CodeBlock cb = () -> {
    System.out.println("Hello " + i);
};

cb.execute();
```

---

## Final vs EffectivelyFinal

- `final`: a constant with a explicit `final` keyword. Can't change after being set
- Effectively final: the compiler knows you have defined a variable, but never changed it (usually, all method parameters). We can use it from a lamda expression without the need to mark it `final`
    - but we can't change the value of that constant
    - it's a constant, but now we don't have to explicitly mark it as `final`