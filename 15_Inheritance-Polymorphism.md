# 🎓 Inheritance & Polymorphism

---

## 🤔 Why Inheritance?

Imagine modeling different types of trucks in a program:

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE PROBLEM                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Truck             FireTruck           ConcreteTruck            │
│  ┌──────────┐      ┌──────────┐        ┌──────────┐             │
│  │ weight   │      │ weight   │        │ weight   │             │
│  │ fuelType │      │ fuelType │        │ fuelType │             │
│  │ length   │      │ length   │        │ length   │             │
│  │ height   │      │ height   │        │ height   │             │
│  ├──────────┤      ├──────────┤        ├──────────┤             │
│  │ Forward()│      │ Forward()│        │ Forward()│             │
│  │ Stop()   │      │ Stop()   │        │ Stop()   │             │
│  └──────────┘      │ waterCap │        │ concCap  │             │
│                    ├──────────┤        ├──────────┤             │
│       ↑            │StartSiren│        │PourConc()│             │
│       │            │StopSiren │        └──────────┘             │
│       │            └──────────┘                                 │
│       │                                                         │
│   Same code        Same code!          Same code!               │
│   copied 3x! 😫                                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Problem:** Copy-pasting the same attributes and behaviors for each truck type!

**Solution:** **Inheritance** — Put common stuff in a base class, let specialized classes inherit it.

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE SOLUTION: INHERITANCE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                      Truck (Base Class)                         │
│                      ┌──────────────┐                           │
│                      │ weight       │                           │
│                      │ fuelType     │                           │
│                      │ length       │                           │
│                      │ height       │                           │
│                      ├──────────────┤                           │
│                      │ Forward()    │                           │
│                      │ Stop()       │                           │
│                      └──────┬───────┘                           │
│                             │                                   │
│              ┌──────────────┴──────────────┐                    │
│              │ inherits                    │ inherits           │
│              ▼                             ▼                    │
│     FireTruck (Derived)          ConcreteTruck (Derived)        │
│     ┌──────────────┐             ┌──────────────┐               │
│     │ waterCapacity│             │ concreteCapacity│            │
│     ├──────────────┤             ├──────────────┤               │
│     │ StartSiren() │             │ PourConcrete()│              │
│     │ StopSiren()  │             └──────────────┘               │
│     └──────────────┘                                            │
│                                                                 │
│     "FireTruck IS A Truck"   "ConcreteTruck IS A Truck"         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📝 Inheritance Syntax

### Basic Syntax

```cpp
class Base {
    // Base class members
};

class Derived : public Base {  // Derived "is a" Base
    // Derived class members (additions/specializations)
};
```

### The Truck Example

```cpp
#include <string>

// Base class: Generic Truck
class Truck {
public:
    Truck(double weight, std::string fuel, double length, double height)
        : weight_(weight), fuel_type_(fuel), length_(length), height_(height) {}
    
    void Forward() { /* move forward */ }
    void Stop()    { /* stop */ }
    
protected:  // Accessible to derived classes
    double weight_;
    std::string fuel_type_;
    double length_;
    double height_;
};

// Derived class: FireTruck IS A Truck
class FireTruck : public Truck {
public:
    // Call base constructor to set up base portion
    FireTruck(double weight, std::string fuel, double length, 
              double height, double water_cap)
        : Truck(weight, fuel, length, height),  // Initialize base
          water_capacity_(water_cap) {}          // Initialize derived
    
    void StartSiren() { /* wee-woo! */ }
    void StopSiren()  { /* silence */ }
    
private:
    double water_capacity_;  // Specialization
};
```

### Using Inheritance

```cpp
int main() {
    FireTruck big_red(36000, "diesel", 52, 11, 500);
    
    // Inherited from Truck
    big_red.Forward();  // ✅ Works! Inherited
    big_red.Stop();     // ✅ Works! Inherited
    
    // FireTruck specializations
    big_red.StartSiren();  // ✅ Works! FireTruck-specific
    
    return 0;
}
```

---

## 🔐 Access Specifiers in Inheritance

### Three Levels

```cpp
class Base {
public:      // Accessible everywhere
    int x;
    
protected:   // Accessible in class AND derived classes
    int y;
    
private:     // Accessible ONLY in this class
    int z;
};
```

### Visibility Chart

```
┌────────────────────────────────────────────────────────────────┐
│                   ACCESS SPECIFIERS                            │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│   In Base Class    │ public │ protected │ private             │
│   ─────────────────┼────────┼───────────┼─────────             │
│   Accessible from: │        │           │                     │
│     - Base class   │   ✅   │    ✅     │   ✅                │
│     - Derived class│   ✅   │    ✅     │   ❌                │
│     - Outside      │   ✅   │    ❌     │   ❌                │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Example

```cpp
class Animal {
public:
    std::string name;      // Everyone can access
    
protected:
    int age;               // Only Animal and derived can access
    
private:
    std::string dna;       // Only Animal can access
};

class Cat : public Animal {
public:
    void Birthday() {
        age++;            // ✅ OK: protected is accessible
        // dna = "xxx";   // ❌ ERROR: private not accessible
    }
};

int main() {
    Cat bubby;
    bubby.name = "Bubby";  // ✅ OK: public
    // bubby.age = 5;      // ❌ ERROR: protected
    // bubby.dna = "xyz";  // ❌ ERROR: private
}
```

---

## 🏗️ Modes of Inheritance

### Public Inheritance (Most Common)

Models **"is-a"** relationship. Preserves access levels.

```cpp
class FireTruck : public Truck {
    // public stays public
    // protected stays protected
    // private stays inaccessible
};
```

### Protected Inheritance

Models **"implemented-in-terms-of"** relationship. Everything becomes protected.

```cpp
class FireTruck : protected Truck {
    // public becomes protected
    // protected stays protected
    // private stays inaccessible
};
```

### Private Inheritance

Models **"implemented-in-terms-of"** relationship. Everything becomes private.

```cpp
class FireTruck : private Truck {
    // public becomes private
    // protected becomes private
    // private stays inaccessible
};
```

### Inheritance Mode Summary

```
┌─────────────────────────────────────────────────────────────────┐
│            INHERITANCE MODES                                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Base Member    │ public      │ protected   │ private         │
│   Access         │ inheritance │ inheritance │ inheritance     │
│   ───────────────┼─────────────┼─────────────┼────────────      │
│   public         │ public      │ protected   │ private         │
│   protected      │ protected   │ protected   │ private         │
│   private        │ (no access) │ (no access) │ (no access)     │
│                                                                 │
│   Use Case:      │ "is-a"      │ "impl. in   │ "impl. in       │
│                  │ relationship│  terms of"  │  terms of"      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> 💡 **Rule:** Use `public` inheritance for "is-a" relationships (FireTruck IS A Truck)

---

## 🔨 Construction & Destruction Order

### Construction: Base First, Then Derived

```cpp
class Parent {
public:
    Parent() { std::cout << "Parent constructed\n"; }
    ~Parent() { std::cout << "Parent destroyed\n"; }
};

class Child : public Parent {
public:
    Child() { std::cout << "Child constructed\n"; }
    ~Child() { std::cout << "Child destroyed\n"; }
};

int main() {
    Child c;
}
```

**Output:**
```
Parent constructed    ← Base first
Child constructed     ← Then derived
Child destroyed       ← Derived first
Parent destroyed      ← Then base
```

### Memory Layout

```
┌─────────────────────────────────────────────────────────────────┐
│            OBJECT MEMORY LAYOUT                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Child object "c":                                             │
│   ┌─────────────────────────────────┐                           │
│   │   Parent portion (base)         │ ← Constructed FIRST       │
│   │   - parent data members         │                           │
│   ├─────────────────────────────────┤                           │
│   │   Child portion (derived)       │ ← Constructed SECOND      │
│   │   - child data members          │                           │
│   └─────────────────────────────────┘                           │
│                                                                 │
│   Destruction goes in REVERSE order!                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Calling Base Constructor

```cpp
class Parent {
public:
    Parent(int value) : data_(value) {
        std::cout << "Parent(" << value << ")\n";
    }
private:
    int data_;
};

class Child : public Parent {
public:
    // MUST call Parent constructor in initializer list
    Child(int val, std::string name) 
        : Parent(val),      // ← Initialize base portion FIRST
          name_(name) {     // ← Then derived portion
        std::cout << "Child(" << name << ")\n";
    }
private:
    std::string name_;
};
```

---

## 🙈 Name Hiding

When derived class defines a member with the same name as base, it **hides** the base version.

### Scope Lookup Rules

```cpp
int i = 7;           // Outer scope
{
    int i = 8;       // Inner scope HIDES outer
    std::cout << i;  // Prints: 8
}
std::cout << i;      // Prints: 7 (outer scope)
```

### Same Happens with Inheritance

```cpp
class Animal {
public:
    std::string Speak() { return "Generic animal noise"; }
};

class Parrot : public Animal {
public:
    std::string Speak() { return "Polly wants a cracker!"; }  // HIDES Animal::Speak
};

int main() {
    Parrot polly;
    std::cout << polly.Speak();  // "Polly wants a cracker!"
    
    // To call hidden base version:
    std::cout << polly.Animal::Speak();  // "Generic animal noise"
}
```

### Copy Assignment Operator Example

```cpp
class Parent {
public:
    Parent& operator=(const Parent& rhs) {
        std::cout << "Parent operator=\n";
        // copy parent members
        return *this;
    }
};

class Child : public Parent {
public:
    Child& operator=(const Child& rhs) {
        std::cout << "Child operator=\n";
        
        // IMPORTANT: Call parent's operator= explicitly!
        Parent::operator=(rhs);  // ← Don't forget this!
        
        // copy child members
        return *this;
    }
};
```

---

## ✂️ Object Slicing

When you copy a derived object **by value** to a base type, the derived portion is **sliced off**.

```cpp
class Animal {
public:
    std::string name;
};

class Cat : public Animal {
public:
    std::string favorite_toy;  // This gets SLICED OFF!
};

int main() {
    Cat bubby;
    bubby.name = "Bubby";
    bubby.favorite_toy = "yarn";
    
    // SLICING! Creates copy with only Animal portion
    Animal a = bubby;  // ⚠️ favorite_toy is LOST!
    
    // No slicing with references/pointers
    Animal& ref = bubby;      // ✅ Just a different view
    Animal* ptr = &bubby;     // ✅ Points to full Cat object
}
```

```
┌─────────────────────────────────────────────────────────────────┐
│            OBJECT SLICING                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Cat bubby:                    Animal a = bubby:               │
│   ┌─────────────────────┐       ┌─────────────────────┐         │
│   │ Animal portion      │  ──►  │ Animal portion      │         │
│   │ name = "Bubby"      │       │ name = "Bubby"      │         │
│   ├─────────────────────┤       └─────────────────────┘         │
│   │ Cat portion         │       (Cat portion SLICED OFF!)       │
│   │ favorite_toy="yarn" │                                       │
│   └─────────────────────┘                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Reference/Pointer: No Slicing

```cpp
void PassByValue(Animal a) {
    // a is SLICED copy - only Animal portion
}

void PassByReference(Animal& a) {
    // a is REFERENCE to full object - no slicing
}

void PassByPointer(Animal* a) {
    // a POINTS to full object - no slicing
}
```

---

## 🔄 Virtual Functions & Function Overriding

### The Problem Without `virtual`

```cpp
class Animal {
public:
    std::string Speak() { return "Generic noise"; }
};

class Cat : public Animal {
public:
    std::string Speak() { return "Meow!"; }
};

int main() {
    Cat bubby;
    Animal& animal_ref = bubby;  // View cat as animal
    
    std::cout << bubby.Speak();       // "Meow!" ✅
    std::cout << animal_ref.Speak();  // "Generic noise" 😢
    // We want "Meow!" here too!
}
```

### Solution: `virtual` Keyword

```cpp
class Animal {
public:
    virtual std::string Speak() { return "Generic noise"; }
    //  ↑ Magic keyword!
};

class Cat : public Animal {
public:
    std::string Speak() override { return "Meow!"; }
    //                   ↑ Optional but recommended
};

int main() {
    Cat bubby;
    Animal& animal_ref = bubby;
    
    std::cout << bubby.Speak();       // "Meow!" ✅
    std::cout << animal_ref.Speak();  // "Meow!" ✅ Now works!
}
```

### How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│            VIRTUAL FUNCTION DISPATCH                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Without virtual:                                              │
│   ┌────────────────────────────────┐                            │
│   │ Compiler decides at COMPILE   │                            │
│   │ time based on pointer/ref TYPE │                            │
│   └────────────────────────────────┘                            │
│   Animal& ref = cat; ref.Speak() → Animal::Speak()              │
│                                                                 │
│   With virtual:                                                 │
│   ┌────────────────────────────────┐                            │
│   │ Runtime decides based on       │                            │
│   │ ACTUAL object type             │                            │
│   └────────────────────────────────┘                            │
│   Animal& ref = cat; ref.Speak() → Cat::Speak()                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### The `override` Keyword (C++11)

```cpp
class Cat : public Animal {
public:
    // 'override' tells compiler: "I'm overriding a virtual function"
    std::string Speak() override { return "Meow!"; }
    
    // If base doesn't have this virtual function, compiler ERROR!
    // Prevents typos like:
    // std::string Speek() override { }  // ❌ Compiler catches this!
};
```

---

## 🌈 Polymorphism

**Poly** = many, **Morph** = forms

Objects of different types responding differently to the same message.

### Example: Animal Collection

```cpp
#include <iostream>
#include <vector>
#include <string>

class Animal {
public:
    virtual std::string Speak() { return "..."; }
    virtual ~Animal() = default;  // Important! (explained later)
};

class Cat : public Animal {
public:
    std::string Speak() override { return "Meow!"; }
};

class Dog : public Animal {
public:
    std::string Speak() override { return "Woof!"; }
};

class Parrot : public Animal {
public:
    std::string Speak() override { return "Polly wants a cracker!"; }
};

int main() {
    // Collection of different animals
    std::vector<Animal*> zoo;
    zoo.push_back(new Cat());
    zoo.push_back(new Dog());
    zoo.push_back(new Parrot());
    
    // Each speaks according to its ACTUAL type!
    for (Animal* animal : zoo) {
        std::cout << animal->Speak() << std::endl;
    }
    // Output:
    // Meow!
    // Woof!
    // Polly wants a cracker!
    
    // Cleanup
    for (Animal* animal : zoo) {
        delete animal;
    }
}
```

```
┌─────────────────────────────────────────────────────────────────┐
│            POLYMORPHISM: MANY FORMS                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                      Animal* → Speak()                          │
│                           │                                     │
│              ┌────────────┼────────────┐                        │
│              ▼            ▼            ▼                        │
│           Cat          Dog        Parrot                        │
│         "Meow!"      "Woof!"    "Polly..."                      │
│                                                                 │
│   Same method call → Different behavior based on actual type   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎭 Abstract Base Classes

### What is an "Animal"?

An "animal" is an abstract concept — you never see a generic "animal" walking around, only specific types like cats, dogs, parrots.

### Pure Virtual Functions

Make a function **pure virtual** by adding `= 0`:

```cpp
class Animal {
public:
    // Pure virtual: NO implementation, MUST be overridden
    virtual std::string Speak() = 0;  // ← "= 0" makes it pure
    
    virtual ~Animal() = default;
};
```

### Abstract Class Rules

```cpp
class Animal {
public:
    virtual std::string Speak() = 0;  // Pure virtual
};

// Animal is now ABSTRACT - cannot create objects directly!
Animal a;  // ❌ ERROR: cannot instantiate abstract class

// But CAN use pointers/references
Animal* ptr;     // ✅ OK
Animal& ref = ...;  // ✅ OK
```

### Derived Classes MUST Implement

```cpp
class Cat : public Animal {
public:
    // MUST implement Speak() or Cat is also abstract!
    std::string Speak() override { return "Meow!"; }
};

Cat bubby;  // ✅ OK: Cat is concrete (implements all pure virtuals)
```

### Complete Example

```cpp
// Abstract base class
class Shape {
public:
    virtual double Area() = 0;      // Pure virtual
    virtual double Perimeter() = 0; // Pure virtual
    virtual ~Shape() = default;
};

// Concrete derived class
class Rectangle : public Shape {
public:
    Rectangle(double w, double h) : width_(w), height_(h) {}
    
    double Area() override { return width_ * height_; }
    double Perimeter() override { return 2 * (width_ + height_); }
    
private:
    double width_, height_;
};

// Another concrete derived class
class Circle : public Shape {
public:
    Circle(double r) : radius_(r) {}
    
    double Area() override { return 3.14159 * radius_ * radius_; }
    double Perimeter() override { return 2 * 3.14159 * radius_; }
    
private:
    double radius_;
};
```

---

## ⚠️ Virtual Destructors

### The Problem

```cpp
class Animal {
public:
    virtual std::string Speak() = 0;
    ~Animal() { std::cout << "~Animal\n"; }  // NOT virtual!
};

class Cat : public Animal {
public:
    Cat() { data_ = new int[100]; }  // Allocates memory
    ~Cat() { 
        delete[] data_;  // Cleanup
        std::cout << "~Cat\n"; 
    }
    std::string Speak() override { return "Meow"; }
private:
    int* data_;
};

int main() {
    Animal* pet = new Cat();  // Cat allocated
    delete pet;               // Only ~Animal called! MEMORY LEAK!
}
```

**Output:**
```
~Animal    ← Only base destructor called!
           ← Cat's destructor NEVER runs!
           ← data_ is leaked!
```

### The Solution: Virtual Destructor

```cpp
class Animal {
public:
    virtual std::string Speak() = 0;
    virtual ~Animal() { std::cout << "~Animal\n"; }  // ← VIRTUAL!
    //  ↑ Add virtual!
};

int main() {
    Animal* pet = new Cat();
    delete pet;  // Now calls ~Cat THEN ~Animal
}
```

**Output:**
```
~Cat       ← Derived destructor called first
~Animal    ← Then base destructor
```

### Rule of Thumb

> 🔑 **If a class has ANY virtual function, it should have a virtual destructor!**

```cpp
class Base {
public:
    virtual void SomeMethod() = 0;
    virtual ~Base() = default;  // ← Always add this!
};
```

---

## 💼 Real-World Production Examples

### Example 1: Payment Processing System

```cpp
// Abstract base class
class PaymentProcessor {
public:
    virtual bool ProcessPayment(double amount) = 0;
    virtual std::string GetProviderName() = 0;
    virtual ~PaymentProcessor() = default;
};

// Concrete implementations
class CreditCardProcessor : public PaymentProcessor {
public:
    bool ProcessPayment(double amount) override {
        // Connect to credit card network
        // Validate card, process transaction
        return true;
    }
    std::string GetProviderName() override { return "Visa/Mastercard"; }
};

class PayPalProcessor : public PaymentProcessor {
public:
    bool ProcessPayment(double amount) override {
        // Connect to PayPal API
        // Authenticate, process payment
        return true;
    }
    std::string GetProviderName() override { return "PayPal"; }
};

class CryptoProcessor : public PaymentProcessor {
public:
    bool ProcessPayment(double amount) override {
        // Connect to blockchain
        // Validate wallet, send transaction
        return true;
    }
    std::string GetProviderName() override { return "Bitcoin"; }
};

// Usage: Works with ANY payment type!
void Checkout(PaymentProcessor* processor, double total) {
    std::cout << "Processing via " << processor->GetProviderName() << "\n";
    if (processor->ProcessPayment(total)) {
        std::cout << "Payment successful!\n";
    }
}
```

### Example 2: GUI Widget Framework

```cpp
// Abstract base class for all UI elements
class Widget {
public:
    virtual void Draw() = 0;
    virtual void HandleClick(int x, int y) = 0;
    virtual ~Widget() = default;
    
protected:
    int x_, y_, width_, height_;
};

class Button : public Widget {
public:
    void Draw() override {
        // Render button graphics
    }
    void HandleClick(int x, int y) override {
        // Execute button action
    }
};

class TextBox : public Widget {
public:
    void Draw() override {
        // Render text input field
    }
    void HandleClick(int x, int y) override {
        // Focus text input, show cursor
    }
private:
    std::string text_;
};

class CheckBox : public Widget {
public:
    void Draw() override {
        // Render checkbox
    }
    void HandleClick(int x, int y) override {
        checked_ = !checked_;  // Toggle
    }
private:
    bool checked_ = false;
};

// Render ALL widgets polymorphically
void RenderUI(std::vector<Widget*>& widgets) {
    for (Widget* w : widgets) {
        w->Draw();  // Each draws itself correctly!
    }
}
```

### Example 3: Game Enemy System

```cpp
class Enemy {
public:
    virtual void Attack() = 0;
    virtual void Move() = 0;
    virtual int GetDamage() = 0;
    virtual ~Enemy() = default;
    
protected:
    int health_;
    int x_, y_;
};

class Zombie : public Enemy {
public:
    void Attack() override { /* Slow melee attack */ }
    void Move() override { /* Shuffle slowly */ }
    int GetDamage() override { return 10; }
};

class Dragon : public Enemy {
public:
    void Attack() override { /* Breathe fire! */ }
    void Move() override { /* Fly */ }
    int GetDamage() override { return 50; }
};

class Archer : public Enemy {
public:
    void Attack() override { /* Shoot arrows */ }
    void Move() override { /* Run and dodge */ }
    int GetDamage() override { return 20; }
};

// Game loop processes ALL enemies the same way
void GameUpdate(std::vector<Enemy*>& enemies) {
    for (Enemy* e : enemies) {
        e->Move();    // Each moves differently
        e->Attack();  // Each attacks differently
    }
}
```

---

## 🔑 Key Takeaways

### Inheritance Checklist

| Concept | Key Point |
|---------|-----------|
| **Inheritance** | Reuse code, model "is-a" relationships |
| **public** | Most common, preserves access levels |
| **protected** | Accessible in derived classes only |
| **private** | Accessible only in defining class |
| **Construction** | Base first, then derived |
| **Destruction** | Derived first, then base |
| **Name hiding** | Derived hides base members of same name |
| **Object slicing** | Copying derived to base loses derived part |

### Polymorphism Checklist

| Concept | Key Point |
|---------|-----------|
| **`virtual`** | Enables runtime dispatch |
| **`override`** | Documents intent, catches errors |
| **`= 0`** | Makes function pure virtual (abstract) |
| **Abstract class** | Has at least one pure virtual function |
| **Virtual destructor** | Required if class has any virtual functions |

### Common Mistakes to Avoid

```cpp
// ❌ Forgot virtual destructor
class Base {
    virtual void Foo() { }
    ~Base() { }  // Should be: virtual ~Base() = default;
};

// ❌ Forgot to call base class operator=
Child& operator=(const Child& rhs) {
    // Missing: Parent::operator=(rhs);
    return *this;
}

// ❌ Object slicing
void Process(Animal a) { }  // Pass by VALUE slices!
void Process(Animal& a) { } // Pass by REFERENCE - correct!

// ❌ Forgot override keyword
class Cat : public Animal {
    std::string Speek() { }  // Typo! Won't override, no error!
    std::string Speek() override { }  // Compiler catches typo!
};
```

