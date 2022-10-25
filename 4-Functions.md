# Rust Functions

In Rust, we define a function by entering `fn` followed by the function name and a set of parentheses. 
The curly brackets tell the compiler where the function body begins and ends. We have seen a function
already in the hello world program - the main() function.

## main() function - the entry point of a Rust program

Rust

```
fn main() {

}
```

These lines define a function named main. The main function is special: it is always the first code that 
runs in every executable Rust program. Here, the first line declares a function named main that has no parameters 
and returns nothing. If there were parameters, they would go inside the parentheses (). Having no parameters in 
Rust main() function is different from some other languages, eg Java, where CLI arguments are passed to main as 
an array of strings or the typical C++ two-parameter form of the main function that allow arbitrary multibyte 
character strings to be passed from the execution environment.

Java

```
public static void main(String[] args) { 

}
```

C++

```
int main (int argc, char *argv[]) {

}
```

Coming back to Rust, the function body is wrapped in {}. Rust requires curly brackets around all function bodies. 


## Rust function parameters 

```
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

In this example, `x`is the parameter of `i32` type for the `another_function`.
We can define functions to have parameters, which are special variables that are part of a function’s signature. When a 
function has parameters, you can provide it with concrete values for those parameters, which are called as arguments. 
For instance, in the example above, the integer `5` is passed into `another_function` as an argument. When this program
is compiled and executed, `another_function` is called with `5` as an input argument. Then, inside `another_function` 
the `println!` macro puts `5` where the pair of curly brackets containing `x` which formats it as a string and prints
it as a line on the command line prompt.

## Rust function signatures and parameter types

```
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

If we try to pass an argument of different type, say a string into `another_function`, the program would not even compile.
In function signatures, you must declare the type of each parameter. This is a deliberate decision in Rust’s design: requiring 
type annotations in function definitions means the compiler almost never needs you to use them elsewhere in the code to figure 
out what type you mean. The compiler is also able to give more helpful error messages if it knows what types the function expects.

## Writing Rust functions with return values

### Rust is an expression based language

### statements vs expressions

