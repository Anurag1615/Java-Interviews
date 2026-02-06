# Functional Interface

Interface which contains exactly ONE abstract method.

Also called:
SAM Interface (Single Abstract Method)

---

## Example

```java
interface Interf {
    void m1();
}
```

Valid Functional Interface ✔

---

## Invalid Example

```java
interface Interf {
    void m1();
    void m2();
}
```

Not Functional ❌

---

## @FunctionalInterface Annotation

Used to restrict interface to only one abstract method.

If we add second abstract method → Compile time error.

```java
@FunctionalInterface
interface Interf {
    void m1();
}
```

---

## Important Rule

Functional interface can contain:

| Member Type | Allowed |
|------|------|
| Abstract Method | Only one |
| Default Method | Any number |
| Static Method | Any number |

---

### Example

```java
@FunctionalInterface
interface Interf {
    void m1();

    default void m2() {}
    static void m3() {}
}
```

Valid ✔

---

## Why Lambda requires Functional Interface?

Lambda provides implementation for only one method.

So multiple abstract methods → confusion for compiler.
