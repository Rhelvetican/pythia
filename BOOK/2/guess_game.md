# Making a guessing game in Pythia

Let's start by using Theodora to make our project:

```sh
root $ theo new guess_game
root $ cd guess_game
```

Now let's take a look at the newly generated `Theo.toml`:

```toml
[package]
name = "guess_game"
version = "0.1.0"

[requirements]
```

While the standard library have a lot of functions, it don't contains any functions for randomization. For that, we will use the `random` box.

Add this line to the `[requirements]` table:

```toml
[requirements]
random = "*"
```

In this example, we are using a wildcard version requirement. Theodora will automatically fetch the latest version of that box. However, if you want to publish to the Theodora central registry, you cannot use a wildcard version requirement. You will have to specify which version you want to use.

Pythia and Theodora follows [Semantic Versioning](https://semver.org/) like many other languages. As such you must specify the MAJOR version, however the MINOR and PATCH versions are optional.

Now, with the `random` box installed, let's begin!

## Processing our guess

The first part of the guessing game program will ask for user input, process that input, and check that the input is in the expected form. To start, we’ll allow the player to input a guess. Enter the following code:

```rs
use std.io.input

fn main()
    guess = input("Please enter your guess: ").expect("Cannot read from line.")
    print("You have guessed {guess}")
```

This code contains a lot of information, so let’s go over it line by line.

First we imported the `input` function with the `use` keyword. The `input` function lives in the `io` module of the standard library, or `std` here.

By fnault, Pythia has a set of items fnined in the standard library that it brings into the scope of every program. This set is called the `prelude`. If a type you want to use isn’t in the `prelude`, you have to bring that type into scope explicitly with a `use` statement.

As you saw in *Chapter 1*, the `main` function is the entry point into the program:

```rs
fn main()
```

Similarly, the `print` function prints stuffs to console. More on that later.

### Storing values with variables

We then move to this line:

```rs
    guess = input("Please enter your guess: ").expect("Cannot read from line.")
```

We created a `guess` variable to store the player's guess. We then read their guess with `input`, which also print out a prompt notifying the player to input their guess. You may also notice the `expect` function. In Pythia, any function that can fail in a recoverable manner will return a `Result` type wrapping around the actual result of that function. `Result` is an enum, which is which is a type that can be in *one of multiple possible states*. We call each possible state a *variant*.

`Result` conatains 2 possible variants, `Ok` and `Err`. If the function success, it will returns the `Ok` variant, else, the `Err` variant. Values of the `Result` type, like values of any type, have methods defined on them. An instance of `Result` has an `expect` method that you can call. If this instance of `Result` is an `Err` value, `expect` will cause the program to crash and display the message that you passed as an argument to `expect`. If the `input` function returns an `Err`, it would likely be the result of an *error* coming from the underlying ***operating system***. If this instance of `Result` is an `Ok` value, expect will take the return value that `Ok` is holding and return just that value to you so you can use it. In this case, that value is the user’s input.

If you don’t call `expect`, the program will compile, but you’ll get a warning:

```sh
root $ theo build
    Compiling guessing_game v0.1.0
    warning: unused `Result` that must be used
    --> src/main.pi:2:19
    |
  2 |     guess = input("Please enter your guess: ")
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
    = note: this `Result` may be an `Err` variant, which should be handled
    = note: `#[warn(unused_must_use)]` on by default

warning: `guessing_game` (bin "guessing_game") generated 1 warning
    Finished dev [unoptimized + debuginfo] secret(s) in 0.59s
```

The right way to suppress the *warning* is to actually write *error-handling code*, but in our case we just want to crash this program when a *problem* occurs, so we can use `expect`. You’ll learn about recovering from errors in later chapter.

## Generating the Secret Number

Next, we need to generate a *secret number* that the user will try to guess. The *secret number* should be *different* every time so the game is fun to play more than once. We’ll use a *random number* between `1` and `100` so the game isn’t too difficult.

### Generating a Random Number

Let's start using `random` to generate your secret number:

```rs
use std.io.input
use random.int.randint

fn main()
    secret = randint(1, 101)
    guess = i32(input("Please enter your guess: ").expect("Cannot read from line.")).expect("Cannot convert to i32.")
    print("\nYou have guessed {guess}\n")
    print("The secret number is {secret}")

```

First, we add the line `use random.int.randint`, which import the `randint` function. We can use this function to generate random integers. Simply pass in the lower bound and upper bound and you get a random integer.

We also added `i32` to covert guesses to an 32-bit integer with the `i32` function. The function can fails so we also added another `expect`.

Then the new `print` just print out the random number. Try running it a few time:

```sh
$ theo run
    Compiling guessing_game v0.1.0
    Finished dev [unoptimized + debuginfo] secret(s) in 2.53s
    Running `secret/debug/guessing_game`
Please enter your guess: 6
You have guessed 6
The secret number is 3

$ theo run
    Finished dev [unoptimized + debuginfo] secret(s) in 0.02s
    Running `secret/debug/guessing_game`
Please enter your guess: 45
You have guessed 45
The secret number is 82
```

You should get different random numbers, and they should all be numbers between 1 and 100. Great job!

### Comparing the Guess to the Secret Number

Now that we have user input and a random number, we can compare them:

```rs
use std.[io.input, cmp.Ordering]
use random.int.randint

fn main()
    secret = randint(1, 101)
    guess = i32(input("Please enter your guess: ").expect("Cannot read from line.")).expect("Cannot convert to i32.")
    print("\nYou have guessed {guess}\n")
    print("The secret number is {secret}")
    match guess.cmp(secret)
        case Greater
            print("Too big!")
        case Less
            print("Too small!")
        case Equal
            print("You win!")
```

First you can see that we have changed the first import statement with some square brackets. That is called Nested Import. You can use it to import multiple things from a box/module. You can see we imported the `Ordering` type. It is another enum with 3 variants: `Greater`, `Less` and `Equal`. These are the three outcomes that are possible when you compare two values.

Next we made a `match` statement. This is one of Pythia's special feature: Pattern Matching. The `cmp` method compares two values and can be called on ***anything that can be compared***. Here it compares `guess` to `secret`. The `match` statement is used to decide what to do based on which variant of `Ordering` is returned.

A `match` expression is made up of *arms*. An *arm* consists of a *pattern* to match *against*, and the code that should be run if the value given to match fits that arm’s pattern.

Let’s walk through an example with the `match` expression we use here. Say that the user has guessed `50` and the randomly generated secret number this time is `38`.

When the code compares `50` to `38`, the cmp method will return `Ordering.Greater` because `50` is greater than `38`. The match expression gets the `Ordering.Greater` value and starts checking each arm’s pattern. It looks at the first arm’s pattern, `Ordering.Less`, and sees that the value `Ordering.Greater` does not match `Ordering.Less`, so it ignores the code in that arm and moves to the next arm. The next arm’s pattern is `Ordering.Greater`, which does match `Ordering.Greater`! The associated code in that arm will execute and print `Too big!` to the screen. The match expression ends after the first successful match, so it won’t look at the last arm in this scenario.

### Allowing Multiple Guesses with Looping

The `loop` keyword creates an infinite loop. We’ll add a `loop` to give users more chances at guessing the number:

```rs
use std.[io.input, cmp.Ordering]
use random.int.randint

fn main()
    secret = randint(1, 101)
    loop
        guess = i32(input("Please enter your guess: ").expect("Cannot read from line.")).expect("Cannot convert to i32.")
        print("\nYou have guessed {guess}\n")
        print("The secret number is {secret}")
        match guess.cmp(secret)
            case Greater
                print("Too big!")
            case Less
                print("Too small!")
            case Equal
                print("You win!")
```

As you can see, we’ve moved everything from the guess input prompt onward into a loop. Be sure to indent the lines inside the loop another four spaces each and run the program again. The program will now ask for another guess forever, which actually introduces a new problem. It doesn’t seem like the user can quit!

The user could always interrupt the program by using the keyboard shortcut `Ctrl + C`. But there's another way: If the user enters a non-number answer, the program will crash. We can take advantage of that to allow the user to quit, but that's sub-optimal.

#### Quitting after Correct Guess

Let’s program the game to quit when the user wins by adding a break statement:

```rs
use std.[io.input, cmp.Ordering]
use random.int.randint

fn main()
    secret = randint(1, 101)
    loop
        guess = i32(input("Please enter your guess: ").expect("Cannot read from line.")).expect("Cannot convert to i32.")
        print("\nYou have guessed {guess}\n")
        print("The secret number is {secret}")
        match guess.cmp(secret)
            case Greater
                print("Too big!")
            case Less
                print("Too small!")
            case Equal
                print("You win!")
                break
```

Adding the `break` line after `You win!` makes the program exit the `loop` when the user guesses the secret number correctly. Exiting the `loop` also means exiting the program, because the `loop` is the last part of `main`.

#### Handling invalid input

To further refine the game’s behavior, rather than crashing the program when the user inputs a non-number, let’s make the game ignore a non-number so the user can continue guessing. We can do that with another `match` statement:

```rs
use std.[io.input, cmp.Ordering]
use random.int.randint

fn main()
    secret = randint(1, 101)
    loop
        guess = match i32(input("Please enter your guess: ").expect("Cannot read from line."))
            case Ok(value)
                value
            case Err(e)
                continue
        print("\nYou have guessed {guess}\n")
        print("The secret number is {secret}")
        match guess.cmp(secret)
            case Greater
                print("Too big!")
            case Less
                print("Too small!")
            case Equal
                print("You win!")
                break
```

This time, when the user input a non-number input, the loop will simply repeat instead of continuing.

At this point, you’ve successfully built the guessing game. Congratulations!
