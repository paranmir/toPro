## 2.4.4  Constant expressions and Constexpr
----
## Constant expression
### 1. definition
- **An expression whose value cannot change and evaluated at compile time.**
- A `const` object that is initialized from a constant expression is a constant expression.
- types and the initializer determines whether an object or expression is a constant expression.
### 2. examples
	const int foo = 20; //foo is a constant expression
	int bar = 21; // bar is not a constant expression
bar is initialized from a literal, but it is pure `int`, not the `const int`.

---
	const int size = getSize(); // size is not a constant expression

value of the `size` is unknown until run time.

---

## Constexpr Variables
### 1. Definition
- **Asking** the compiler to verify if a variable is a constant expression.
###why?
In large system, it can be confusing to determine whether an initializer is a constant expression or not. We might define a constant variable with an initializer that is *NOT* a constant expression. 

### 2. examples
	constexpr int var = 20;// 20 is a constant expression
	constexpr int limit = var + 1; // var + 1 is a constant expression
	constexpr int sz = size(); // ok only if size is a constexpr function.

### 3. constexpr functions
- must be **simple enough** that the compiler can evaluate them at compile time.
- can use in the initializer of a `constexpr` variable.

####Best Paractices
Use `constexpr` for variables that you *intend to* use as constant expressions.


----

### 4. Literal Types

- Literal types are simple enough to use in a `constexpr` 
- `constexpr` objects to initialize **pointers** and **reference** are strictly limited
	+ can initialize `constexpr` pointer from the `nullptr` or 0.
<<<<<<< HEAD
	+ can point to an object : remains at a fixed address
		* ordinarily variables defined inside a function are not fixed.
		* cover in §6.1.1

- 
=======
	+ can point to ( bind to ) an object that remains at a fixed address.
		* varialbed defined *inside* a function -> address not fixed
		* object defined *outside* of any function -> constant expression
		* special local objects  ( see in § 6.1.1 ) function like an object defined outside any function, these special local objects also have fixed addresses.

---

### 5. Pointers and `constexpr`

#### Different to a pointer to const

- the `constexpr` specifier *applies to the pointer* , not the*type* to which the pointer points

	const int *p = nullptr; // p is a pointer to a const int
	constexpr int *q = nullptr; // q is a const pointer to int

- p : pointer to const
- q : constant pointer
- `constexpr` imposes a top-level const

---

#### Similar to constant pointer

- `constexpr` pointer may point to a const *or* a nonconst type

	constexpr int *np = nullptr;	// np is a onstant pointer to int
	int j = 0;
	constexpr int i = 42;		//type of i is const int
					// i and j must be defined outside any function
	constexpr const int *p = &i; 	// p is a constnt pointer to the const int i
	constexpr int *p1 = &j;		// p1 is a constant pointer to the int j




# 2.5. Dealing with Types

----

## 2.5.1. Type Aliases

### 1. Definition
- A synonym for another type
- Emphasize the purpose for which a type is used

**2 ways to do this :**

### 2. `typedef`

	typedef double wages;	// wages is a synonym for double
	typedef wages base, *p;	// base is a synonym for double, p for double*

### 3. `alias` declaration

	using SI = Sales_item;		// SI is a synonym for Sales_item

- Starts with the `using`
- then alias name and `=` 


----

### 4. Pointers, `const`, and Type Aliases

Declarations that use type aliases that represent compound types and `const` can yield *distracting* results.
Here is a example. this declarations use type `pstring` : alias for the type `char*`

	typedef char *pstring; 

	const pstring cstr = 0; // cstr is a constant pointer to char
	const pstring *ps; 	// ps is a pointer to a constant pointer to char

base type in both :  `const pstring`
 
`const` that appears in the base type modifies the given type.
the type of `pstring` : **pointer to char** 
`const pstring` : **constant pointer to char**, **not a pointer to const char**

#### confusing!

`const char* cstr` : pointer to const char.
`const pstring cstr` : constant pointer to char.

NOT correct : interpreting a type alias by *replacing * the alias type like this:

	const char* cstr = 0;	//wrong interpretation of const pstring cstr
				// cstr is a pointer to const char
Using `pstring` in a declaration, the base type of the declaration is a pointer type. Otherwise, when we rewrite the declaration using char*, the base type is char and the * is part of the declarator, const char is the base type. thus, this rewrite declares `cstr` as a pointer to `const char` rather than as a `const` pointer to `char`. 

