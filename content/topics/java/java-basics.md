---
title: "Java Fundamentals: Getting Started"
date: 2024-01-18T10:00:00+08:00
tags: ["java", "basics", "programming"]
---

Java is one of the most popular programming languages in the world, powering everything from mobile apps to enterprise systems. This guide will get you started with Java fundamentals.

## What is Java?

Java is a high-level, object-oriented programming language developed by Sun Microsystems (now Oracle) in 1995. It's designed to be platform-independent, meaning Java programs can run on any system that has the Java Virtual Machine (JVM) installed.

## Setting Up Your Development Environment

### Installing Java Development Kit (JDK)

1. **Download JDK**: Visit [Oracle's official website](https://www.oracle.com/java/technologies/downloads/) or use OpenJDK
2. **Install JDK**: Follow the installation wizard for your operating system
3. **Verify Installation**: Open terminal/command prompt and run:
   ```bash
   java -version
   javac -version
   ```

### Choosing an IDE

Popular Java IDEs include:
- **IntelliJ IDEA**: Professional-grade IDE with excellent features
- **Eclipse**: Free, open-source IDE with extensive plugin ecosystem
- **Visual Studio Code**: Lightweight editor with Java extensions

## Your First Java Program

Let's create a simple "Hello World" program:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### Understanding the Code

- `public class HelloWorld`: Defines a public class named HelloWorld
- `public static void main(String[] args)`: The main method - entry point of the program
- `System.out.println()`: Prints text to the console

## Java Syntax Basics

### Variables and Data Types

Java is a strongly-typed language. Here are the primitive data types:

```java
// Integer types
int age = 25;
long population = 7800000000L;

// Floating-point types
float price = 19.99f;
double pi = 3.14159265359;

// Character and boolean
char grade = 'A';
boolean isActive = true;

// String (reference type)
String name = "John Doe";
```

### Control Structures

#### Conditional Statements

```java
int score = 85;

if (score >= 90) {
    System.out.println("Grade: A");
} else if (score >= 80) {
    System.out.println("Grade: B");
} else if (score >= 70) {
    System.out.println("Grade: C");
} else {
    System.out.println("Grade: F");
}
```

#### Loops

```java
// For loop
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}

// While loop
int count = 0;
while (count < 3) {
    System.out.println("While count: " + count);
    count++;
}

// Enhanced for loop (for arrays/collections)
int[] numbers = {1, 2, 3, 4, 5};
for (int number : numbers) {
    System.out.println("Number: " + number);
}
```

## Object-Oriented Programming Concepts

### Classes and Objects

```java
public class Car {
    // Instance variables (attributes)
    private String brand;
    private String model;
    private int year;
    
    // Constructor
    public Car(String brand, String model, int year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
    }
    
    // Methods (behaviors)
    public void start() {
        System.out.println(brand + " " + model + " is starting...");
    }
    
    public void displayInfo() {
        System.out.println(year + " " + brand + " " + model);
    }
    
    // Getters and setters
    public String getBrand() {
        return brand;
    }
    
    public void setBrand(String brand) {
        this.brand = brand;
    }
}
```

### Using the Class

```java
public class Main {
    public static void main(String[] args) {
        // Creating objects
        Car myCar = new Car("Toyota", "Camry", 2022);
        Car friendCar = new Car("Honda", "Civic", 2021);
        
        // Using methods
        myCar.start();
        myCar.displayInfo();
        
        friendCar.start();
        friendCar.displayInfo();
    }
}
```

## Arrays and Collections

### Arrays

```java
// Declaring and initializing arrays
int[] numbers = new int[5];  // Array of 5 integers
String[] names = {"Alice", "Bob", "Charlie"};

// Accessing array elements
numbers[0] = 10;
numbers[1] = 20;

System.out.println("First name: " + names[0]);
System.out.println("Array length: " + names.length);
```

### ArrayList (Dynamic Array)

```java
import java.util.ArrayList;

ArrayList<String> fruits = new ArrayList<>();
fruits.add("Apple");
fruits.add("Banana");
fruits.add("Orange");

// Accessing elements
System.out.println("First fruit: " + fruits.get(0));

// Iterating through ArrayList
for (String fruit : fruits) {
    System.out.println("Fruit: " + fruit);
}
```

## Exception Handling

Java uses try-catch blocks to handle exceptions:

```java
public class ExceptionExample {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // This will cause ArithmeticException
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Error: Cannot divide by zero!");
        } finally {
            System.out.println("This block always executes");
        }
    }
}
```

## Best Practices

### Naming Conventions

- **Classes**: Use PascalCase (e.g., `StudentRecord`, `BankAccount`)
- **Methods and Variables**: Use camelCase (e.g., `calculateTotal`, `firstName`)
- **Constants**: Use UPPER_SNAKE_CASE (e.g., `MAX_SIZE`, `DEFAULT_COLOR`)

### Code Organization

- One public class per file
- File name should match the class name
- Use meaningful variable and method names
- Add comments for complex logic
- Follow consistent indentation

## Next Steps

Now that you understand Java basics, you're ready to:

1. **Explore Advanced OOP**: Learn about inheritance, polymorphism, and interfaces
2. **Study Collections Framework**: Master List, Set, Map, and other data structures
3. **Learn Spring Framework**: Build enterprise applications with Spring
4. **Practice with Projects**: Create real-world applications to solidify your knowledge

## Common Beginner Mistakes

- Forgetting semicolons at the end of statements
- Not handling exceptions properly
- Using `==` instead of `.equals()` for string comparison
- Not following naming conventions
- Creating overly complex methods instead of breaking them down

Java's strong typing system and comprehensive error checking help catch many mistakes at compile time, making it an excellent language for beginners to learn programming fundamentals.

Remember: practice is key to mastering Java. Start with simple programs and gradually work your way up to more complex applications.