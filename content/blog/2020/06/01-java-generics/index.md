---
title: 'Generics In Java'
date: "2020-06-06T02:00:32.169Z"
description: 'Short Notes About Generics In Java'
---

### Generic Terms:

| Term                      |  Example                                  |
|---------------------------|-------------------------------------------|
| Parameterized type        | List \<String\>                           |
| Actual type parameter     | String                                    |
| Generic type              | List\<E\>                                 |
| Formal type parameter     | E                                         |
| Unbounded wildcard type   | List\<?\>                                 |
| Raw type                  | List                                      |
| Bounded type parameter    | \<E extends Number\>                      |
| Recursive type bound      | \<T extends Comparable\<T\>\>             |
| Bounded wildcard type     | List\<? extends Number\>                  |
| Generic method            | static \<E\> List\<E\> <br> asList(E[] a) |
| Type token                | String.class                              |

---

>> **Covariant:** f is covariant if A ≤ B implies that f(A) ≤ f(B) <br>`Arrays are covariant.`

```
// Fails at runtime!
Object[] objectArray = new Long[1];
objectArray[0] = "I don't fit in"; // Throws ArrayStoreException
```
<br>

>> f is contravariant if A ≤ B implies that f(B) ≤ f(A)

<br>

>> **Invariant:** f is invariant if neither of the above holds <br>`Generics, by contrast, are invariant.`
```
// Won't compile!
List<Object> ol = new ArrayList<Long>(); // Incompatible types
ol.add("I don't fit in");
```
<br>

---

If method is declared by:

```
Number[] method(ArrayList<Number> list) { ... }
```

none of the following expressions will compile:

```
Integer[] result = method(new ArrayList<Integer>());
Number[] result = method(new ArrayList<Integer>());
Object[] result = method(new ArrayList<Object>());
```

but

```
Number[] result = method(new ArrayList<Number>());
Object[] result = method(new ArrayList<Number>());
```

will.

---

>> arrays are reified [JLS, 4.7]. This means that arrays know and enforce their element type at runtime.


>> Generics, by contrast, are implemented by erasure [JLS, 4.6]. This means that they enforce their type constraints 
>> only at compile time and discard (or erase) their element type information at runtime.
>> <br><br>Because of erasure, the only parameterized types that are reifiable are unbounded wildcard types such as List\<?\> and Map\<?,?\>

```
// Why generic array creation is illegal - won't compile!
List<String>[] stringLists = new List<String>[1]; // (1)
List<Integer> intList = List.of(42); // (2)
Object[] objects = stringLists; // (3)
objects[0] = intList; // (4)
String s = stringLists[0].get(0); // (5)
```

---

<i><strong>p.s:</strong> These notes were obtained from the book Effective Java 3rd Edition By Joshua Bloch and this
<a href="https://stackoverflow.com/a/8482091/2183174" target="_blank">stackoverflow answer.</a>
</i>

