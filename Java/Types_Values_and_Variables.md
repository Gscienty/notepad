The Java programming language is a *statically typed* language, which means that every variable and every expression has a type that is known at compile time.

The Java programming language is also a strongly typed language, bacause types limit the values that a variable can hold or that an expression can produce, limit the operations supported on those values, and determine the meaning of the operations. Strong static typing helps detect errors at compile time.

**NOTE: 是否可以在Java中做“模板元编程”?**

The types of the Java programming language are divided into two categories:**primitive types** and **reference types**.  
**primitive types**: boolean type and the numeric types.  
**reference types**: class types, interface types, array types.  
**null type**: it is impossible to declare a variable of the null type or to cast to the null type. It can always be assigned or cast to any reference type. 

The values of a reference type are references to objects. All objects, including arrays, support the methods of class *Object*.

##Primitive Types and Values##

Primitive values do not share state with other primitive values.

The string concatenation operator +, which, when given a String operand and an integral operand, whill convert the integral operand to a String representing its value in decimal form, and then produce a newly created String that is the concatenation of the two strings.

###Integral Types and Values###

- *byte*, from -128 to 127, inclusive
- *short*, from -32768 to 32767, inclusive
- *int*, from -2147483648 to 2147483647, inclusive
- *long*, from -9223372036854775808 to 9223372036854775807, inclusive
- *char*, from '\u0000'to '\uffff'inclusive

###Integer Operations###

If an integer operator other than a shift operator has at least one operand of type *long*, then the operation is carried out using 64-bit precision, and the result of the numerical operator is of type *long*. If the other operand is not *long*, it is first widened to type long by numeric promotion.

###Floating-Point Types, Formats, and Values###

The floating-point types are *float* and *double*, which are conceptually associated with the single-precision 32-bit and double-precision 64-bit format IEEE 754 values and operations as specified in IEEE Standard for Binary Floating-Point Arithmetic, ANSI/IEEE Standard 754-1985(IEEE, New York).

The IEEE 754 standard includes not only positive and negative numbers that consist of a sign and magnitude, but also positive and negative zeros, positive and negative *infinities*, and special *Not-a-Number* values. A NaN value is used to represent the result of certain invalid operations.

Positive zero and negative zero compare equal.

###The Boolean Type and Boolean Values###

An Integer or floating-point expression *x* can be converted to a boolean value, following the C language convention that any nonzero value is true.

An object reference *obj* can be converted to a boolean value, following the C language convertion that any reference other than *null* is true.

##Reference Types and Values##

There are four kinds of reference types:

- class types
- interface types
- type variables
- array types

###Objects###

An object is a class instance or an array.

The reference values are pointers to these objects, and a special null reference, which refers to no object.

There may be many references to the same object. Most objects have state, stored in the fields of objects that are instances of classes or in the variables that are the components of an array object.  
If two variables contain references to the same object, the state of the object can be modified using one variable's reference to the object, and then the altered state can be observed through the reference in the other variable.

Each object is associated with a monitor, which is used by *synchronized* methods and the *syncchronized* statement to provide control over concurrent access to state by multiple threads.

###The Class *Object*###

The class Object is a superclass of all other classes.

- The method *clone* is used to make a duplicate of an object.
- The method *equals* defines a notion of object equality, which is based on value, not reference, comparison.
- The method *finalize* is run just before an object is destroyed.
- The method *getClass* returns the *Class* object that represents the class of the object.  
A *Class* object exists for each reference type. It can be used, for example, to discover the fully qualified name of a class, its members, its immediate superclass, and any interfaces that it implements.  
A class method that is declared *synchronized* synchronizes on the monitor associated with the *Class* object of the class.
- The method *hashCode*.
- The methods *wait,* *notify*, and *notifyAll* are used in concurrent programming using threads.
- The method *toString* returns a *String* representation of the object.

###When Reference Types Are the Same###

Two reference types are the *same compile-time type* if they have the same binary name and their type arguments, if any, are the same, applying this definition recursively.

Two reference types are the *same run-time type* if:

- They are both class or both interface types, are defined by the same class loader, and hav the same binary name, in which case they are sometimes said to be the *same run-time class* or the *same run-time interface*.
- They are both array types, and their component types are the same run-time type.

##Parameterized Types##

A class or interface declaration that is generic defines a set of parameterized types.

