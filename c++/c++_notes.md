## 2.4 Constant expressions
- **An expression whose value cannot change and evaluated at compile time.**
- A `const` object that is initialized from a constant expression is a constant expression.
- types and the initializer determines whether an object or expression is a constant expression.
### 2.4.1 examples
	const int foo = 20; //foo is a constant expression
	int bar = 21; // bar is not a constant expression
bar is initialized from a literal, but it is pure `int`, not the `const int`.


---
	const int size = getSize(); // size is not a constant expression

value of the `size` is unknown until run time.

---

## 2.5 Constexpr Variables

- **Asking** the compiler to verify if a variable is a constant expression.
###why?
In large system, it can be confusing to determine whether an initializer is a constant expression or not. We might define a constant variable with an initializer that is *NOT* a constant expression. 

### 2.5.1 examples
	constexpr int var = 20;// 20 is a constant expression
	constexpr int limit = var + 1; // var + 1 is a constant expression
	constexpr int sz = size(); // ok only if size is a constexpr function.

### 2.5.2 constexpr functions
- must be **simple enough** that the compiler can evaluate them at compile time.
- can use in the initializer of a `constexpr` variable.

####Best Paractices
Use `constexpr` for variables that you *intend to* use as constant expressions.


----

### 2.5.3 Literal Types

- Literal types are simple enough to use in a `constexpr` 
- `constexpr` objects to initialize **pointers** and **reference** are strictly limited
	+ can initialize `constexpr` pointer from the `nullptr` or 0.
	+ can point to an object : remains at a fixed address
		* ordinarily variables defined inside a function are not fixed.
		* cover in §6.1.1

- 