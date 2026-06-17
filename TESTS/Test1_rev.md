
## 1. Actions vs Calculations
Calculation:
- Pure function, does not change anything outside itself.
- Always returns the same result for the same inputs.
- Easy to test.
Example:
    ```java
    int sum(int x, int y) {
        return x + y;
    }
    ```

Action:
- Interacts with the outside world: printing, saving files, modifying global variables.
- Has side effects.
Example:
    ```java
    System.out.println("Hello");
    ```

Tip: If it changes something outside the function → it is an action.

---

## 2. Side Effects
A side effect is anything a function does besides returning a value.

Side effects include:
- printing
- saving to a file
- updating a global variable

Not a side effect:
- returning a value

---

## 3. Hidden Dependencies
A function depends on something not passed as a parameter (like a global variable).

This makes testing and maintenance harder.

Tip: Pure functions never have hidden dependencies.

---

## 4. Functional Core / Imperative Shell
Functional Core:
- only calculations
- no side effects
- easy to test

Imperative Shell:
- where actions happen
- printing, saving files, calling APIs

Core = logic  
Shell = interaction with the outside world

---

## 5. MVC — Controller
The controller coordinates interactions between the View, Model, and services.
It does not draw the UI, store data, or perform heavy logic.

---

## 6. Records (Java)
Records are the simplest way to represent data.

    ```java
    record Customer(String name, int id) {}
    ```

Immutable and ideal for representing facts.

---

## 7. Sealed Interfaces
Used to restrict which classes can implement an interface.

    ```java
    sealed interface Result permits Success, Error {}
    ```

Advantage: the compiler knows all possible types.

---

## 8. Testing
Unit test: tests a function in isolation, uses fakes.  
Integration test: tests real interaction (e.g., writing to a file).  
End-to-end: tests the whole system.

Tip: If it touches a real file → integration test.

---

## 9. Cloud Computing
Real benefits:
- high availability
- elasticity
- pay-per-use

Not a benefit:
- automatic bug reduction

AWS Region = group of Availability Zones.

---

## 10. Data vs Action vs Calculation
Data: facts (record, string, list).  
Calculation: transforms data without side effects.  
Action: interacts with the outside world.

A customer order = data (a fact).

---

## 11. Implicit Output
When a function changes something outside itself without returning.

Example:
    ```java
    globalCount++;
    ```

---

## 12. Consumer<T>
Receives a value and returns nothing.

Valid example:
    ```java
    (String s) -> System.out.println(s)
    ```

---

## 13. UUID.randomUUID()
Generates a random value → not deterministic → action.

---

# 14. Programming Paradigms (Simple Version)

## Functional
What it is:
Programming based on pure functions.

Examples:
- pure functions
- recursion

How to remember:
"Does not change anything, only calculates."

---

## Object-Oriented (OO)
What it is:
Programming based on objects and behavior.

Examples:
- encapsulation
- overriding

How to remember:
"Objects with state and behavior."

---

## Structural
What it is:
Step-by-step programming with variables that change.

Examples:
- loops
- mutable variables

How to remember:
"Code that changes values and uses loops."

---

# Final Summary for Exam
- Pure function = no side effects  
- Action = interacts with the outside world  
- Record = best way to represent data  
- Sealed interface = compiler knows all types  
- Controller = coordinates, does not render or store data  
- Integration test = touches real file or database  
- Cloud = elasticity + availability  
- Side effect = anything besides returning  
- Customer order = data  
- Functional = pure functions + recursion  
- OO = encapsulation + overriding  
- Structural = loops + mutable variables

# 1. DATA vs CALCULATIONS vs ACTIONS (The MOST important topic)

## DATA = facts
- Data does NOT do anything.
- It’s just information.

Examples:
    ```java
    record User(String name, int age) {}
    List<Integer> nums = List.of(1,2,3);
    ```

Key idea:  
**Data never causes bugs. It’s safe.**

---

## CALCULATIONS = pure functions
- Take input → return output
- No side effects
- Same input = same output
- Easy to test
- Safe to reuse anywhere

Examples:
    ```java
    int add(int a, int b) { return a + b; }
    int max(List<Integer> nums) { return ... }
    ```

Key idea:  
**Calculations are the “good guys” — predictable and safe.**

---

## ACTIONS = things that affect the outside world
- Timing matters
- Order matters
- Hard to test
- Cause side effects
- Any function that CALLS an action becomes an action

Examples:
    ```java
    System.out.println("Hi");
    db.save(user);
    globalCount++;
    getCurrentTime();
    ```

Key idea:  
**Actions are “dangerous” — use them only when necessary.**

---

# 2. How to Identify Each One (FAST)

## Ask yourself:
- **Does it change something?** → Action  
- **Does it only compute something?** → Calculation  
- **Is it just information?** → Data  

This ALWAYS works.

---

# 3. Referential Transparency (Easy Definition)

A function is referentially transparent if:
**You can replace the function call with its result and nothing changes.**

Example (transparent):
    ```java
    hyp(3,4) → always 5
    ```

Example (NOT transparent):
    ```java
    hyp(3,4) prints something
    ```

Key idea:  
**If it prints, logs, saves, or reads globals → NOT transparent.**

---

# 4. Functional Core / Imperative Shell

## Functional Core
- Pure functions
- Immutable data
- Business logic
- Easy to test

## Imperative Shell
- I/O (DB, network, files)
- Sequencing
- Minimal logic

Diagram:
```
[ Imperative Shell ] → I/O, order, actions
[ Functional Core ] → pure functions, data
```

Key idea:  
**Put the “smart thinking” in the core, and the messy stuff in the shell.**

---

# 5. Extracting Calculations from Actions (VERY IMPORTANT)

Goal:  
Turn messy action code into:
- a small action wrapper  
- a clean calculation you can test  

Example (bad):
    ```java
    shoppingCartTotal = 0;
    for (var item : shoppingCart)
        shoppingCartTotal += item.price();
    ```

Example (good):
    ```java
    double calcTotal(List<Item> cart) {
        double total = 0;
        for (var item : cart) total += item.price();
        return total;
    }
    ```

Key idea:  
**Move the math into a pure function. Leave I/O outside.**

---

# 6. Implicit Inputs & Outputs (How to Spot Actions)

## Implicit input:
- reading a global variable
- reading system time

## Implicit output:
- modifying a global
- mutating a list/object

Example (impure):
    ```java
    cart.add(item); // modifies external list
    ```

Pure version:
    ```java
    List<Item> addItem(List<Item> cart, Item item) {
        var newCart = new ArrayList<>(cart);
        newCart.add(item);
        return newCart;
    }
    ```

Key idea:  
**If something changes OUTSIDE the function → it’s an action.**

---

# 7. AWS Cloud — The Simple Version

## What is Cloud Computing?
Using IT resources over the internet:
- on-demand
- pay-as-you-go
- no hardware to buy

## 6 Cloud Benefits (memorize these)
1. No upfront cost (CapEx → OpEx)  
2. Economies of scale  
3. Stop guessing capacity  
4. Faster and more agile  
5. No data center maintenance  
6. Go global in minutes  

## Service Models
- **IaaS** → you manage most things (EC2)
- **PaaS** → you deploy apps, platform manages the rest
- **SaaS** → ready-to-use apps (email, CRM)

## Deployment Models
- Cloud
- Hybrid
- On-Premises

## AWS Global Infrastructure
- Regions
- Availability Zones
- Data Centers

## Ways to Access AWS
- Console
- CLI
- SDKs

Key idea:  
**Cloud = fast, scalable, cheap, global.**

---

# 8. What the Coding Question Will Likely Be

You may need to:
- identify data/calculation/action
- convert an action into a calculation
- remove implicit inputs/outputs
- write a pure function
- use a record or enum

Example:
    ```java
    record Coupon(String code, CouponRank rank) {}
    enum CouponRank { GOOD, BEST, BAD }
    ```

---

# FINAL SUPER SUMMARY (If you remember ONLY this)

- **Data = facts**  
- **Calculations = pure functions**  
- **Actions = side effects**  

- Prefer: **Data → Calculations → Actions**  
- Pure functions = predictable, testable  
- Actions = unpredictable, contagious  
- FP = immutability + pure functions  
- AWS = on-demand, scalable, pay-as-you-go  



