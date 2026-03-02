# 🔹 Encapsulation

## 🔹 Definition

Encapsulation is the process of grouping data members and corresponding methods into a single unit.

It combines data hiding and controlled access.

---

## 🔹 Encapsulation = Data Hiding + Abstraction

- Data hiding → private variables
- Abstraction → controlled access via methods

---

## 🔹 Example

```java
class Employee {

    private double salary;

    public void setSalary(double s) {
        salary = s;
    }

    public double getSalary() {
        return salary;
    }
}
```

---

## 🔹 Benefits of Encapsulation

- Improved security
- Better maintainability
- Controlled data access
- Flexibility to change implementation

---

## 🔹 Important Note

If we declare variables as private
but do not provide public methods,
then it is only data hiding.

If we provide controlled access,
then it becomes proper encapsulation.
