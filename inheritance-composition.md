---
title: Inheritance vs Composition
description: Choosing between composition and inheritance is hard. Composition and Inheritance are the building blocks of an application structure. It is imperative to know when to use which one.
keywords: inheritance vs composition, object oriented programming, software engineering, inheritance, composition, inheritance in java, inheritance in kotlin
datePublished: '2019-09-14'
---

# Inheritance vs Composition

With the introduction of OOPs, Inheritance and Composition entered our senses and still confusing us.

Choosing between Composition and Inheritance is not easy. Often, the decision depends upon the information which is not yet there.

The structure of an application plays an essential role in adding changes. Composition and Inheritance are the building blocks of an application structure. It is imperative to know when to use which one.

## Definition

### Inheritance

_Inheritance_ is a way of reusing code by inheriting the structure from the referred Class. The referring Class is called subclass and referred Class is called parent/super/base class.

_Inheriting the structure_ suggests that Subclass inherits API (public members) of the parent class. Subclass can also override the parent class methods or add more methods.

```kotlin
// Reference class
class Animal {
   fun walk() {
     println("animal walk");
   }
}

// Referring Class
// Cat class "inherits the structure" from the Animal class
class Cat : Animal {
}


// in main
val cat = Cat();
cat.walk() // Cat can "reuse" parent class behavior
```

### Composition

**Composition** is a way of reusing code by interacting with the referred class _without any structural imposition_.

```kotlin
// Reference Class
class Line (val x: Int, val y:Int);
  fun draw() {
     println("line drawing")
  }
}

// Refering class
class Shape (val sides: Array<Line>) {
  fun shapeDraw() {
     for(side in sides) {
       side.draw(); // code refering
     }
  }
}
```

## Decision Matrix

### Uniformity

I had a situation to ignore JsonNaming annotation¹ provided by Jackson library (Java ecosystem). There is no direct API provided by the library to facilitate the same.

A possible solution to have a new class dictating the desired functionality and injecting the new class object to the library. Luckily, the chosen library provides the API to inject custom class (Thanks to open-close design).

Should I compose or inherit from the existing Class? - *inherit*.

```java
// class for library
class JSONIntrospector {

    // .. rest code

    public Object findNamingStrategy(AnnotatedClass ac) {
        JsonNaming ann = _findAnnotation(ac, JsonNaming.class);
        return (ann == null) ? null : ann.value();
    }

    // .. rest code
}


// custom class
class IgnoreJsonNaming extends JSONIntrospector {

    @Override
    public Object findNamingStrategy(AnnotatedClass ac) {
        return null;
    }
}

// inject code
ObjectMapper mapper = new ObjectMapper();
mapper.setAnnotationIntrospector(new IgnoreJsonNaming());

```

Inheritance is more suitable in this context as it is expected to have structural similarity or uniformity.

**Composition would not have worked :**
If I preferred to go with Composition, I would have to implement unnecessary methods to maintain structural similarity.

Imagine, If in future, library developers add new behaviours to the referred Class, I would end up changing in the latest Class to have structural similarity.

### Semantics

Inheritance and Composition not only allow to reuse the code; they also define a relationship semantic between two classes.

```
is-a
has-* (* means any quantifier)
```

Apple is a Fruit. (Inheritance)
Apple has seeds. (Composition)

Human is a mammal. (Inheritance)
Human has two hands. (Composition)

These keywords assist in basic analysing the requirements.

### Software Model: OOA/D

Whenever we write software, we introduce so many concepts from Specification and Implementation point of view.

Through rigorous analysis, these concepts end up with numerous classes. Shaping these classes (object decomposition) and connecting these classes become our primary responsibility.

```
Implementation Concepts: Factory, Manager, Service, Client, Provider
e.g. RestClient, EmailService, ValidtorFactory
```

Sometimes, it is better to start with code at first, then evolve the design. You are free to play until the first release. Once the software is released, it is hard to reverse most of the decisions.

#### Multiple Concepts

Inheritance does not befit well for multi-dimensions concepts or variants or attributes. If applied, it leads to Combinatorial Explosion of classes.

Say, you are modelling Pizza (concept 1) with two allowed Toppings (concept 2) Onion and Tomato. If you choose Inheritance to connect Pizza and Topping, then you would end with four classes.

```kotlin
// with inheritance
class Pizza { // without topping
  fun getPrice() {
   return 100;
  }
}
class PizzaWithTomatoToppings : Pizza {
  fun getPrice() {
   return 120;
  }
}
class PizzaWithOnionTopping : Pizza {
  fun getPrice() {
   return 120;
  }
}
class PizzaWithOnionAndTomatoToppings : Pizza {
  fun getPrice() {
   return 140;
  }
}
```

With Composition, two classes are sufficient.

```kotlin
// with composition
class Pizza {
  var t1 : Topping? = null ;
  var t2 : Topping? = null ;

  fun getPrice() {
    val basePrice = 100;
    return basePrice + t1?.getPrice() ?: 0 + t2?.getPrice() ?: 0
  }

}
class Topping(val price: Int) {
  fun getPrice() {
    return price;
  }
}
```

## Takeaways

- In Inheritance, the structure imposition introduces tight coupling between both connected classes; it restricts their future editing.
- Inheritance is well suited for uniformity.
- Inheritance does not great for multi-dimensions concepts or variants or attributes.
- Composition does not impose any structure of any of connected Class. It is a flexible option.

---

## References

1. Disable specific annotation in Jackson
   https://github.com/FasterXML/jackson-databind/issues/133
