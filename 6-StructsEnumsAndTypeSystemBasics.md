# Introduction

In this module we will begin to explore Rust's type system. We will create our own types and add methods to those types. We will also take our first look at Generics and Traits in Rust. There will be much more to cover about the type system later in the course, but in this module we will learn the most fundamental and common aspects.

Rust allows users to create two different broad categories of types. First, there are Structs which are sometimes known as "product types" or "each-of types" because a struct instance requires data in each of its fields. Second, there are Enums which are sometimes known as "sum types" or "one-of types" because an enum instance is one of multiple variants.

If you come from an object oriented programming background, structs will be familiar as they are pretty similar to the data fields on a class or object. If you come from an functional programming background, both structs and enums will likely be familiar, although the enum syntax is unlike that of OCaml or Haskell.

# Structs

Structs allow packaging multiple primitive data types together into a logical package. They are very similar to the tuples we saw previously, although they make the language much more programmer friendly.

When defining a struct, you provide a name for the type, which has a capital letter by convention. Then you provide a name for each field and a type for each field.

```rust
struct Fraction {
    numerator: u32,
    denominator: u32,
}
```

To create an instance of a struct, you use the name of the type and the name of each field as before. But instead of specifying a type for each field, which has already been defined, we supply a value for each field.

```rust
let one_half = Fraction {
    numerator: 1,
    denominator: 2,
}
```

Structs, like tuples, need to have a value for each and every field. It is not valid to omit one of the fields.

```rust
let one = Fraction {
    numerator: 1,
}
```

Similarly, each field must be of the type that was declared when the struct was defined.

```rust
let one_half = Fraction {
    numerator: 1u64,
    denominator: 2u64,
}
```

Now that we know how declare and instantiate structs, let's see how to access their fields. To access the inner data of a struct, we just use a dot operator and then the name of the field.

```rust
let my_frac = Fraction {
    numerator: 2,
    denominator: 3,
}

let my_bottom_value = my_frac.denominator;
assert!(my_bottom_value == 3);
```

Notice that here we have used simple integer literals like `3` as opposed to explicitly typed ones like `3u32`. This is allowed because the Rust compiler knows that the fields of the `Fraction` type are `u32` because they were declared as such. Similarly, the compiler can infer that the type of `my_bottom_value` is u32 so we don't need an explicit annotation.

We can use the same syntax to mutate the value of a field. But, as always in Rust, to make a value mutable it must be explicitly declared as such.

```rust
let mut f1 = Fraction {
    numerator: 1,
    denominator: 2,
}

let f2 = Fraction {
    numerator: 1,
    denominator: 2,
}

// here we can mutate the denominator of f1
f1.denominator = 3;

// but this line will not compile because f2 is immutable
f2.denominator = 4;
```

# Tuple Structs

In some cases you want to give your type a name like you can with structs, but you don't want to give each field a name. One such example would be storing a point on a 2D lattice.

So far we've learned two ways to store this data, both of which work, but neither of which are exactly what we want. We could simply use a tuple, but this does not allow us
to name the type, and could lead to confusion and even bugs if tuples are used in another context in the program.
Or we could use a struct, but this require repeating field names often when the position is a well-established mathematical convention.

```rust
// We could simply use a tuple.
let point = (2u32, 3u32);

// We could use a struct
struct Point {
    x_coord: u32,
    y_coord: u32,
}
```

For cases like this, Rust has the tuple struct. This allows us to name the type while referencing the fields positionally.

```rust
struct Point(u32, u32);
```

When using tuple structs, the fields are accessed with the dot operator and an integer index exactly like they are when working with tuples.

```rust
let buried_treasure = Point(4, 5);

let treasure_x_coord = buried_treasure.0;
```

It is also possible to create tuple structs with a single element inside. You may wonder why you would ever want to do this when you could simply use the underlying type directly, but in fact this is done frequently, and is often referred to as the "new type pattern". The new type pattern allows us the leverage Rust's type system to distinguish values of different semantic units even if they are represented digitally by the same underlying primitive type.

A typical example would be something like storing Temperature data when you want to be sure that the units are in Celsius as opposed to something like Kelvin or Fahrenheit.

```rust
/// A temperature value that is stored in the celsius units.
struct Celsius(u32);
```

As a side note, notice the comment starting with a triple slash here. This is known as a doc comment or a documentation comment. Doc comments can precede data types, functions, and many other pieces of code and be used to generate automated documentation. We will not explore this in depth here, but know that it is good practice to use doc comments when creating your own types.

Finally, it is possible to create unit structs that have no fields at all. They are analogous to Rust's built-in unit type `()`. This is useful when you want a type to declare static methods on. We will cover methods later in this module.

```rust
/// A unit type that has no data fields.
struct MyUnitType;
```

# Custom types in Functions and Other Types

Now that we've created some custom types of our own, let's see some ways that they can be used.

One way we can use our custom types is as parameters to functions. Let's define a function that allows us to multiply two fractions.

```rust
fn multiply_fractions(a: Fraction, b: Fraction) -> Fraction {
    Fraction {
        numerator: a.numerator * b.numerator,
        denominator: a.denominator * b.denominator,
    }
}
```

Now we are ready to see the value of the new type pattern we discussed previously. Consider steam engine control circuit with a function that determines whether the boiler has warmed up enough to start the engine.

```rust
/// First attempt that is subject to unit confusion
fn warm_enough(temp: u32) -> bool {
    temp > 100
}
```

In this naive function we run the risk of a user entering a Fahrenheit temperature by mistake, or interfacing with a temperature probe that is incorrectly configured to report Fahrenheit temperature.

We can make this function safer by using Rust's type system to insist that a Celsius temperate is entered. This program will not even compile if a plain u32 is passed.

```rust
struct Celsius(u32);

fn warm_enough(temp: Celsius) -> bool {
    temp.0 > 100
}
```

Another place we can use our custom types is as fields of other custom types. Consider a geometry program that builds up shapes from more fundamental primitive types.

```rust
struct Point(u32, u32);

/// We define a Rectangle by its two opposite corners
struct Rectangle {
    top_left: Point,
    bottom_right: Point,
}
```

# Enums

# Methods

multiply fraction

new, setter, getter

# Generics

Many of the types we've defined in this module are more restrictive than they need to be. For example, our fraction type insists that its inner values be of type `u32`. It may be that in some cases, programmers want more precision and prefer to use `u64` instead. It would be a shame to have to re-write the entire struct and all of its methods just to change all the `u32` to `u64`.

To address this problem, Rust's Type system has a notion of Generic types, or "generics" for short. Code that uses generics and related concepts can become quite complex, and we will dive into that full complexity in due course, but for now, let's take a look at a simple use of generic types.

TODO explain this

```rust
/// A fraction type that allows arbitrarily large or small numerators and denominators
struct Fraction<T> {
    numerator: T,
    denominator: T,
}
```

TODO

```rust
let my_precise_fraction = Fraction<u128> {
    numerator: 236,
    denominator: 473,
}

let my_low_memory_fraction = Fraction<u8> {
    numerator: 1,
    denominator: 4,
}
```

We have succeeded in creating a Fraction type that allows us to use any precision integer wa want! But we have also introduced a few problems here. For example, it is now possible to create fractions with data types that don't make any sense at all. Consider this example.

```rust
let silly_fraction = Fraction<bool> {
    numerator: true,
    denominator: false,
}
```

Not only does this bool fraction not make any sense, we also won't be able to multiply bool fractions together because booleans themselves can't be multiplied. In the next video we will see how Rust's traits solve this problem.

# Traits

TODO